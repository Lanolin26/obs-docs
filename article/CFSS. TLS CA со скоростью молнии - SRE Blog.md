---
tags: [ssl,tls,security]
source: https://prudnitskiy.pro/post/2018-12-14-cfssl/
---
CFSS: TLS CA со скоростью молнии
================================

TLS - один из самых распространенных стандартов шифрования и аутентификации в современном интернете. Используется исключительно широко - взаимодействие с пользователем (HTTPS), межпрограммное общение (RPC over HTTPS, gRPC), даже VPN (OpenVPN) и телефония (SIPoTLS). Он обеспечивает шифрование (симметричным ключом), авторизацию (PKI), проверку целостности переданной информации (HMAC). Важная часть TLS - PKI (инфраструктура публичных ключей). Любой публичный ключ в TLS должен быть подписан (публичный ключ с подписью и набором определенных атрибутов называется сертификатом). Центр сертификации – критически важная часть работы TLS, так как он управляет доверием к приложению или сервису (цепочки доверия). В этой статье я расскажу о том, как запустить свой собственный CA на основе CFSSL. Введение получилось неожиданно большим, так что если вам нужна практика - вам сюда

Цепочки доверия  [](#%d1%86%d0%b5%d0%bf%d0%be%d1%87%d0%ba%d0%b8-%d0%b4%d0%be%d0%b2%d0%b5%d1%80%d0%b8%d1%8f) 
----------------------------------------------------------------------------------------------------------

В ассиметричном шифровании каждый сервис имеет два ключа, открытый (публичный) и закрытый (приватный). Из приватного ключа можно легко получить публичный, но обратная операция практически невозможна. Из публичного ключа, добавив в него несколько текстовых полей (атрибутов) - можно создать запрос на сертификат (CSR). Центр сертификации подписывает CSR и таким образом CSR становится сертификатом. Подписью центр сертификации (CA) подтверждает, что:

-   сервис, использующий этот сертификат – достоин доверия с точки зрения CA
-   цифровая подпись действительна в определенный момент времени (у нее есть ограничения по сроку жизни)
-   подписанный CA сертификат можно использовать определенным образом. Возможности использования описаны атрибутами в сертификате. Так, как атрибуты подписываются вместе с самим публичным ключом – их нельзя поменять, это разрушит цифровую подпись.

Сертификат может быть отозван по определенной причине (например, приватный ключ сервиса был украден). Для того, чтобы приложения (пользователи) узнали о факте отзыва – существует CRL (certificate revocation list). Устроен он сравнительно просто: это текстовый файл, где перечислены уникальные серийные номера сертификатов (SSN) и дата отзыва. Содержимое файла подписано цифровой подписью CA, что исключает подделку.

Чтобы клиент (приложение) мог доверять центру сертификации – у него должен быть сертификат центра сертификации. Этому сертификату клиент (приложение) доверяет безусловно, а используя этот сертификат – может проверить цельность цифровой подписи любого сертификата, подписанного доверенным центром сертификации.

Очевидно, что потеря (компрометация) ключа доверенного центра сертификации – это крайне плохо – нужно создать новый ключ после чего заново подписать все сертификаты. Плюс – разослать всем клиентам новый сертификат, а старый пометить, как не подлежащий доверию. Тот, кто украдет ключ центра сертификации – сможет выписывать себе произвольные сертификаты и соединение с таким сервисом будет считаться совершенно безопасным.

Чтобы избежать этого – IEEE придумали цепочки сертификатов. Сначала создается “корневая пара” из приватного ключа и сертификата. Именно этому сертификату будут доверять клиенты безусловно. Корневой сертификат подписывает сам себе. Затем создается “промежуточная пара”. Ключ промежуточной пары подписывается корневым ключом. Теперь корневой ключ можно положить сейф, залить сейф бетоном и закопать в основании небоскреба – в обозримом будущем он нам не потребуется. Клиентские сертификаты будут подписаны промежуточной парой. Клиент, подключаясь, проверит, что сертификат подписан промежуточным центром сертификации, а сертификат промежуточного центра подписан сертификатом, которому клиент доверяет. Такая конструкция называется цепочкой доверия или цепочкой сертификатов (certificate chain), а обладатель промежуточной пары – промежуточным центром сертификации (intermediate center of authorities). Если произойдет ужасное, и ключ промежуточного центра утечет – мы просто откопаем сейф, вынем из него корневой ключ, подпишем этим ключом новую корневую пару и будем подписывать новые сертификаты уже новой корневой парой. Сертификаты, подписанные скомпрометированным ключом – придется перевыпускать (то есть – подписать заново, новым ключом), но хотя бы не придется заставлять всех клиентов менять рутовый сертификат.

Что делает CA и из чего его можно собрать  [](#%d1%87%d1%82%d0%be-%d0%b4%d0%b5%d0%bb%d0%b0%d0%b5%d1%82-ca-%d0%b8-%d0%b8%d0%b7-%d1%87%d0%b5%d0%b3%d0%be-%d0%b5%d0%b3%d0%be-%d0%bc%d0%be%d0%b6%d0%bd%d0%be-%d1%81%d0%be%d0%b1%d1%80%d0%b0%d1%82%d1%8c) 
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Как я уже говорил выше – CA - это пара “приватный ключ + публичный ключ”, в котором приватный ключ создает для сертификатов цифровые подписи. Доступный всем желающим публичный ключ можно использовать для проверки цифровой подписи. При подписании СА должен внести в сертификат уникальный серийный номер подписанного сертификата, а так же – указать срок действия подписи. Хороший центр сертификации должен вести учет выданных сертификатов (то есть - хранить серийные номера всех сертификатов, которые он подписал), а так же – при необходимости создавать CRL – список отозванных сертификатов. Так как TLS сейчас буквально везде – способов создать CA существует множество:

### OpenSSL ca  [](#openssl-ca) 

OpenSSL - это классика реализации SSL (TLS) в xNIX. Состоит из библиотеки (libssl) и утилиты командной строки (openssl). Штука фантастически, невероятно, исключительно сложная. Алгоритмы криптографии в принципе не подарок, а в случае openssl сложнейший С-код набит различными оптимизациями для ускорения работы (шифрование должно работать быстро!). Плюс OpenSSL поддерживается для кучи различных платформ (ОС и процессоров). Ориентация в коде OpenSSL – сложнейшее дело. В 2014 году небольшая правка в коде вызывала уязвимость, известную как “heartbleed” – отправляя специально сформированные пакеты на любой сервис, использующий TLS - можно было читать память, доступную процессу, принимающему пакеты. Чтение было медленным, но результат – сокрушительным: heartbleed позволял вытаскивать внутренние данные работающих программ, читать данные других пользователей, даже воровать приватные ключи приложения.

Пользовательская сторона OpenSSL тоже не подарок – умеет она много, а документирована при этом просто отвратительно. Достаточно просто сказать, что список только доступных команд к утилите openssl - это 3 страницы текста. А руководства, в общем-то – нет. В случае работы с OpenSSL очень несложно сделать ошибку, из-за которой ваш CA станет небезопасным, а потому я не рекомендую использовать openssl для этого.

### EasyRSA / EasyRSAv2  [](#easyrsa--easyrsav2) 

По сути это вообще не утилита. Это набор shell-скриптов, которые используют все тот же OpenSSL. Это резко облегчает настройку и снижает вероятность ошибки, но – уменьшает гибкость настройки. Плюс скрипты могут ломаться (у них бывает, увы), и в таком случае вам будет очень трудно понять, что именно у вас сломалось, и где. Ибо за милым и простым фасадом из easy-rsa вас ждет кровожадное лицо OpenSSL.

### Hasicorp Vault  [](#hasicorp-vault) 

Мощная, удобная утилита для создания и хранения секретов. Секретов самых разных - паролей, токенов. Умеет работать с центрами сертификации в том числе. Прекрасный выбор, если вам нужно построить большую и сложную систему централизованного управления секретами и давать в эту систему ограниченный доступ. Для новичка – очень сложно. Требует поднятия отдельного сервера (где будет работать сервис vault), поднятия отдельного хранилища данных, написания политик доступа в vault. Если вам нужно быстро запустить CA (или несколько, но работать с самим СА будет буквально пара человек) - это явный перебор

### CFSSL  [](#cfssl) 

То, ради чего статья затевалась. Набор из простых, небольших, понятных утилит для создания и управления CA. Код написан целиком на golang – он простой и легко читается. Так как утилита сравнительно молодая - в ней минимум legacy-кода. Изначально написана и поддерживается компанией CloudFlare - огромным CDN-провайдером. Так как CF активно использует шифрование внутри своей сети – сертификатов ему нужно море, по этому они и написали собственный инструментарий для работы с ними

Самый простой случай: Bare CA  [](#%d1%81%d0%b0%d0%bc%d1%8b%d0%b9-%d0%bf%d1%80%d0%be%d1%81%d1%82%d0%be%d0%b9-%d1%81%d0%bb%d1%83%d1%87%d0%b0%d0%b9-bare-ca) 
---------------------------------------------------------------------------------------------------------------------------------------------------------

Чтобы установить свежую версию cfssl (если у вас нет ее в пакетах) – потребуется go версии 1.10 или выше. Ставим:

    go get -u github.com/cloudflare/cfssl/cmd/cfssl

Проверяем установку:

    > cfssl version
    Version: 1.3.2
    Revision: dev
    Runtime: go1.10.2

Все ок, можно делать CA. Создаем папку для CA, переходим туда и создаем конфиг для создания ключа самого CA:

    {
      "CN": "Test Root CA",
      "key": {
        "algo": "rsa",
        "size": 2048
      },
      "ca": {
        "expiry": "87600h"
      },
      "names": [
        {
          "C": "RU",
          "L": "Saint Petersburg",
          "O": "Test Company",
          "OU": "Internal systems unit",
          "ST": "Saint Petersburg"
        }
      ]
    }

В данном примере мы используем ключ RSA в 2048 бит размером. CFSSL поддерживает как RSA таки ECDSA ключи. Назовем файлик csr.json. Поле expiry задает срок жизни CA. CA не может подписывать ключ на срок больший его собственной жизни. Точнее – может, но смысла в этом нет – когда срок существования цифровой подписи самого CA закончится – все созданные им цифровые подписи не будут считаться легитимными.

Сгенерируем ключ:

    > cfssl gencert -initca csr.json | cfssljson -bare ca
    2018/12/14 19:56:56 [INFO] generating a new CA key and certificate from CSR
    2018/12/14 19:56:56 [INFO] generate received request
    2018/12/14 19:56:56 [INFO] received CSR
    2018/12/14 19:56:56 [INFO] generating key: rsa-2048
    2018/12/14 19:56:57 [INFO] encoded CSR
    2018/12/14 19:56:57 [INFO] signed certificate with serial number 594378542753634129370457275219291351654652931156

Посмотрим в папку:

    > tree
    .
    ├── csr.json
    ├── ca-key.pem
    ├── ca.csr
    └── ca.pem

    0 directories, 4 files

Где:

-   ca-key.pem - приватный ключ, которым подписываются сертификаты
-   ca.pem - сам сертификат
-   ca.csr - запрос на сертификат (публичный ключ без подписи).

Лично я рекомендую для общего удобства работы ключи класть в отдельную папку, ее можно, к примеру, назвать keys:

    mkdir keys
    mv ca-key.pem ca.csr ca.pem keys/

Чтобы в Теперь создадим конфиг для создания клиентских ключей и сертификатов. Назовем его, например, ca.json:

    {
      "signing": {
        "profiles": {
          "server": {
            "expiry": "17520h",
            "usages": [
              "digital signature",
              "key encipherment",
              "server auth"
            ]
          },
          "client": {
            "expiry": "8760h",
            "usages": [
              "signing",
              "client auth"
            ]
          }
        }
      }
    }

В этом примере у нас два профиля – сертификаты для сервера (со сроком жизни в 2 года) и для клиента (на год). Сертификат клиента может использоваться для авторизации клиентского подключения (например, в OpenVPN), но такой сертификат нельзя выдать серверу.

Чтобы мочь использовать конфиг для CSR (и не вводить заново страну, регион и прочее) – его нужно подправить, убрав из него секцию CA:

    {
      "CN": "Test Root CA",
      "key": {
        "algo": "rsa",
        "size": 2048
      },
      "names": [
        {
          "C": "RU",
          "L": "Saint Petersburg",
          "O": "Test Company",
          "OU": "Internal systems unit",
          "ST": "Saint Petersburg"
        }
      ]
    }

Теперь сгенерируем серверный ключ:

    > cfssl gencert -ca=keys/ca.pem \
      -ca-key=keys/ca-key.pem \
      -config=ca.json \
      -profile="server" \
      -cn="test.server.local" \
      -hostname="test.server.local,test,192.168.1.1" \
      csr.json | cfssljson -bare keys/server

    2018/12/14 20:28:47 [INFO] generate received request
    2018/12/14 20:28:47 [INFO] received CSR
    2018/12/14 20:28:47 [INFO] generating key: rsa-2048
    2018/12/14 20:28:48 [INFO] encoded CSR
    2018/12/14 20:28:48 [INFO] signed certificate with serial number 53158698715503305792227475745544378560981646495

Аргумент cfssljson -bare keys/server позволяет положить ключ в папку keys, и называться файлы будут с server

Посмотрим, что получилось:

    > tree
    .
    ├── ca.json
    ├── csr.json
    └── keys
        ├── ca-key.pem
        ├── ca.csr
        ├── ca.pem
        ├── server-key.pem
        ├── server.csr
        └── server.pem

Проверим ключ сервера:

    > openssl x509 -text -noout -in keys/server.pem
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                09:4f:b7:ef:17:c4:b5:f5:06:f6:66:bb:0f:85:de:f0:b6:8a:3c:9f
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=Test Root CA
            Validity
                Not Before: Dec 14 17:24:00 2018 GMT
                Not After : Dec 13 17:24:00 2020 GMT
            Subject: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=test.server.local
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (2048 bit)

Теперь создадим сертификат для клиента (скажем, для OpenVPN):

    > cfssl gencert -ca=keys/ca.pem \
      -ca-key=keys/ca-key.pem \
      -config=ca.json \
      -profile="client" \
      -cn="prudnitskiy" \
      -hostname="Paul Rudnitskiy" \
      csr.json | cfssljson -bare "keys/prudnitskiy"

    2018/12/14 20:35:56 [INFO] generate received request
    2018/12/14 20:35:56 [INFO] received CSR
    2018/12/14 20:35:56 [INFO] generating key: rsa-2048
    2018/12/14 20:35:57 [INFO] encoded CSR
    2018/12/14 20:35:57 [INFO] signed certificate with serial number 82476622684473156560886280203356252095924099426

Проверим сертификат клиента:

    > openssl x509 -text -noout -in keys/prudnitskiy.pem
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                0e:72:61:32:19:eb:71:30:43:ee:66:6d:8d:8c:e7:e5:bd:57:59:62
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=Test Root CA
            Validity
                Not Before: Dec 14 17:31:00 2018 GMT
                Not After : Dec 14 17:31:00 2019 GMT
            Subject: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=prudnitskiy
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (2048 bit)

                [.....]

                X509v3 Subject Alternative Name:
                    DNS:Paul Rudnitskiy

Здесь видно, что у клиента сертификат на год, а у сервера (чуть выше) - на два года

Подпись чужого сертификата  [](#%d0%bf%d0%be%d0%b4%d0%bf%d0%b8%d1%81%d1%8c-%d1%87%d1%83%d0%b6%d0%be%d0%b3%d0%be-%d1%81%d0%b5%d1%80%d1%82%d0%b8%d1%84%d0%b8%d0%ba%d0%b0%d1%82%d0%b0) 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

В примере выше мы создавали и ключ и сертификат прямо там, где работает центр сертификации. Вообще это не правильно – приватный ключ не должен покидать места своего использования. В этом примере мы создадим приватный ключ, запрос на подпись сертификата и подпишем этот запрос на другом сервере. Создаем ключ:

    server$ openssl genrsa -aes256 -out client.key 4096
    Generating RSA private key, 4096 bit long modulus
    ...................................................++
    ................................................................................................................................................................................++
    e is 65537 (0x10001)
    Enter pass phrase for client.key:
    Verifying - Enter pass phrase for client.key:

Теперь создаем запрос на сертификат:

    server$ openssl req -new -key client.key -out client.csr
    Enter pass phrase for client.key:
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:RU
    State or Province Name (full name) [Some-State]:Saint Petersburg
    Locality Name (eg, city) []:Saint Petersburg
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:Test Company
    Organizational Unit Name (eg, section) []:International section
    Common Name (e.g. server FQDN or YOUR name) []:test2.ssign.local
    Email Address []:

    Please enter the following 'extra' attributes
    to be sent with your certificate request
    A challenge password []:
    An optional company name []:

У нас есть приватный ключ и запрос на сертификат:

    server$ tree
    .
    ├── client.csr
    └── client.key

Запрос отправим на сервер и подпишем:

    > cfssl sign -ca keys/ca.pem \
    -ca-key keys/ca-key.pem \
    -config=ca.json \
    -profile="server" \
    client.csr | cfssljson -bare "keys/signed"

Проверим, что получилось:

    > openssl x509 -text -noout -in keys/signed.pem
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                06:08:a0:e6:77:a0:a3:f2:15:82:6c:8a:da:9c:9a:73:df:a4:aa:5c
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=Test Root CA
            Validity
                Not Before: Dec 14 17:45:00 2018 GMT
                Not After : Dec 13 17:45:00 2020 GMT
            Subject: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=International section, CN=test2.ssign.local
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (4096 bit)

keys/signed.pem – это и есть наш подписанный сертификат – его можно возвращать на сервер и использовать.

Intermediate center of authorities  [](#intermediate-center-of-authorities) 
--------------------------------------------------------------------------

InterimCA - это центр сертификации, подписанный другим центром сертификации. Его можно использовать для большей безопасности или разграничения возможности подписывать разные сертификаты для разных ситуаций (например, сделать CA, который подписывает только соединение с базами данных и ничего более). Сделаем для его отдельную папку и положим туда два конфига:

l2init.json:

    {
      "signing": {
        "default": {
          "usages": [
            "cert sign",
            "crl sign"
          ],
          "expiry": "43800h",
          "ca_constraint": {
            "is_ca": true,
            "max_path_len": 0,
            "max_path_len_zero": true
          },
          "crl_url": "https://server.com/pki/crl"
        }
      }
    }

В этом примере CA будет действовать 5 лет (root CA действует 10)

l2csr.json:

    {
      "CN": "Level2 CA",
      "key": {
        "algo": "ecdsa",
        "size": 384
      },
      "names": [
        {
          "C": "RU",
          "L": "Saint Petersburg",
          "O": "Test Company",
          "OU": "Internal systems unit",
          "ST": "Saint Petersburg"
        }
      ]
    }

В этом примере мы используем ключ на эллиптических кривых, 384 бита

Сгенерируем ключ для L2CA:

    > cfssl gencert -initca l2/l2csr.json | cfssljson -bare "l2/l2ca"
    2018/12/14 21:09:11 [INFO] generating a new CA key and certificate from CSR
    2018/12/14 21:09:11 [INFO] generate received request
    2018/12/14 21:09:11 [INFO] received CSR
    2018/12/14 21:09:11 [INFO] generating key: ecdsa-384
    2018/12/14 21:09:11 [INFO] encoded CSR
    2018/12/14 21:09:11 [INFO] signed certificate with serial number 113401202328445156746253039479389987233218778440

Теперь подпишем его ключом Root CA:

    > cfssl sign -ca keys/ca.pem \
    -ca-key keys/ca-key.pem \
    -config=l2/l2init.json l2/l2ca.csr | cfssljson -bare "l2/l2ca"
    2018/12/14 21:09:21 [INFO] signed certificate with serial number 716266849684210842961608276575977912635137621329

Проверим:

    > openssl x509 -text -noout -in l2/l2ca.pem
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                7d:76:84:30:b6:71:47:f6:51:0a:f1:40:4d:26:86:21:0c:da:7d:51
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=Test Root CA
            Validity
                Not Before: Dec 14 18:04:00 2018 GMT
                Not After : Dec 13 18:04:00 2023 GMT
            Subject: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=Level2 CA

Здесь хорошо видно, что ключ подписан Test Root CA (Issuer)

Теперь можно создавать ключи, подписаные L2CA - как в прошлом примере:

    > cfssl gencert -ca=l2/l2ca.pem \
      -ca-key=l2/l2ca-key.pem \
      -config=ca.json \
      -profile="server" \
      -cn="test.l2.server.local" \
      -hostname="test.l2.server.localtest,172.16.0.1" \
      l2/l2csr.json | cfssljson -bare l2/server

Здесь мы используем ca.json из root ca (там хранятся профили подписи - время жизни и key usage), а конфигурацию CSR - уже из L2 (тип ключа, секция names).

Проверим сгенерированый сертификат:

    > openssl x509 -text -noout -in l2/server.pem
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                30:a0:58:e2:3c:0e:f3:ec:73:64:b0:e1:25:0a:86:26:46:43:21:e2
        Signature Algorithm: ecdsa-with-SHA384
            Issuer: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=Level2 CA
            Validity
                Not Before: Dec 14 18:11:00 2018 GMT
                Not After : Dec 13 18:11:00 2020 GMT
            Subject: C=RU, ST=Saint Petersburg, L=Saint Petersburg, O=Test Company, OU=Internal systems unit, CN=test.l2.server.local

Подписант – Level2CA

Создадим цепочку доверенных сертификатов. Цепочка читается “снизу вверх”, то есть в самом низу цепочки у нас находится root CA:

    > cat l2/l2ca.pem keys/ca.pem > chain.pem

Проверим, что сертификат сервера - действительный:

    > openssl verify -CAfile chain.pem l2/server.pem
    l2/server.pem: OK

Выводы  [](#%d0%b2%d1%8b%d0%b2%d0%be%d0%b4%d1%8b) 
------------------------------------------------

TLS - основа современной сети. При правильной настройке TLS обеспечивает надежную и безопасное шифрование с проверкой всех участников процесса. CFSSL – простой, быстрый и удобный инструмент для запуска SSL CA. Он позволяет избежать само-подписанных сертификатов и помогает построить инфраструктуру обмена ключами для шифрованных соединений. Так, как большинство серверов работает в сети, которой по определению нельзя доверять – использовать шифрование необходимо, а благодаря CFSSL – это несложно.
