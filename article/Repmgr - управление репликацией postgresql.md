---
tags:
-   postgresql
-   db
-   reliability
source: https://prudnitskiy.pro/post/2018-08-22-repmgr/
---
Repmgr: управление репликацией postgresql
=========================================

PostgreSQL - это мощная и очень развитая база данных, функциональная и дружелюбная. В комплект входит надежный и очень удобный механизм потоковой репликации (я писал о нем [здесь](/2018-01-05-pgsql-replica)). Не смотря на мощь и удобство – этот инструмент сложен в настройке и не всегда понятен, особенно, если серверов баз много. Все становится еще хуже, если у вас сложная схема репликации с каскадами (master &gt; slave &gt; slave of slave). Чтобы облегчить жизнь DBA в таких ситуациях – известные специалисты по консалтингу Postgres, компания 2ndQuadrant придумали repmgr – специальный инструмент для управления настройками репликации для PostgreSQL.

Repmgr может:

-   облегчить создание новых серверов
-   облегчить переключение на другой сервер (promote)
-   автоматизировать переключение на новый сервер при отказе старого (failover)
-   вести аудит событий репликации в кластере (event flow)
-   официально repmgr поддерживает мультимастер с помощью механизма bidirectional replication. Это жуткий грязный хак, и я очень не рекомендую его использовать

Repmgr требует (ограничения):

-   postgresql 9 или 10 (так как только в 9 версии появилась потоковая репликаця)
-   доступ с каждого узла кластера на каждый узел кластера по протоколам SSH (непривелигированый) и postgresql (5432)
-   одинаковую версию Postgres (только major)
-   одинаковую архитектуру сервера (вы не сможете реплицировать данные с ARM на AMD64)
-   одинаковую версию repmgr на всех узлах кластера (одинаковость должна быть *полной*)
-   пользователя с правами SUPERUSER, от имени которого repmgr будет проводить операции. Пароль от этого пользователя хранится в файловой системе кластера в открытом виде.

Установка  [](#%d1%83%d1%81%d1%82%d0%b0%d0%bd%d0%be%d0%b2%d0%ba%d0%b0) 
---------------------------------------------------------------------

Repmgr поставляется в виде пакета и входит в стандартный для всех пользователей postgresql репозиторий PGDG. В данной статье я продемонстрирую настройку для debian, но для centos он настраивается практически так же – меняются только пути доступа к файлам. Добавим репозиторий:

    sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
    apt-get update

Теперь можно установить необходимые для работы пакеты:

    apt-get install postgresql-10 postgresql-10-repmgr -y

Для repmgr важно, чтобы все хосты видели друг друга по hostname. Я рекомендую иметь внутренний DNS для решения этой задачи, но если у вас по какой-то причине его нет – придется добавить имена и адреса серверов кластера в `/etc/hosts`. Например:

    192.168.0.17    pg1.lab.office    pg1
    192.168.0.18    pg2.lab.office    pg2
    192.168.0.19    pg3.lab.office    pg3

Для того, что бы repmgr мог копировать базу с одного сервера на другой – пользователю repmgrd нужен доступ по ssh без ввода пароля. Для этого на каждом из серверов нужно сгенерировать ssh public key. Этот public key нужно положить на все остальные сервера кластера в файл `~/.ssh/authorized_keys`. Это нужно сделать для пользователя, от которого будет запущен repmgr. Обычно это тот же пользователь, от имени которого запущен postgres. Создадим ключи на каждом сервере:

    sudo -u postgres ssh-keygen -t rsa

    Generating public/private rsa key pair.
    Enter file in which to save the key (/var/lib/postgresql/.ssh/id_rsa):
    Created directory '/var/lib/postgresql/.ssh'.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /var/lib/postgresql/.ssh/id_rsa.
    Your public key has been saved in /var/lib/postgresql/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:oqFJ3/Gr8oeBgQpAsKSlImbUOLroa/YaFbJ8AxlsBng postgresql@pg1

Публичный ключ из файла `/var/lib/postgresql/.ssh/id_rsa.pub` надо добавить на все остальные сервера кластера, в файл `~/.ssh/authorized_keys`. Заодно поправим права:

    #на сервере pg1
    echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoavDuAETBMZBD0NwRvSEDYL1avCIkSLzxMG50L6b7nIeasrfv90AGjARxID9THkUXDNkdKhfRIu+WGFYxlgZ6zqPQCyZyQvKjcJr325pbo9it474LpLpeHuPrXdeMSzSilxvAKvYX/ml7L9KtOnYMDusFK1XdGeV25qcj2OSLWBY168riW5vvGWFYTCdU6q9eQ+JN2zCpoZzXKNqhh+dpItt1QiKRw84u7EtUW6U02tw1V5nmO+HGyG2A50S5/JNS7lbj/7IYAXwIgtlBrf3mzCPCIoHbjlSny/V6sp3S7QWNrxynpkI7o+oMvJq5frAEpn0syiUmtOz56Qnw67GP postgres@pg2" >> /var/lib/postgresql/.ssh/authorized_keys
    echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDfl8Rg47u97kGUPf7OwF4jeGhcIxVtWEVDk1AKJt5o3Y65v5jzrjoI5F0YrboEzr+oPu5BV24M6dOI5u3ysRBX/osQI2fBlt+hotAIWXPiP8UUy9CgIdQH59h/MJcp3jPH0KYQTwF8WJDr1skUcUzKGswuofBaElm5TpME+Oz2vygXEl2vL9Pfo5kfdsk9ov58cUJNlDGtxTo/Rzw9XFRnkBimzwvem/gmdpYBFb45ulsbLVmdBcv+QTU7PQ+knqIyERboTecS8wBYoKnlCTA0LZscvyeHjKwILSl9ZFfir3CRdYtxNqx4Zk/hMphx4Bt7hn96KUXRiMf3ODpd2yp1 postgres@pg3" >> /var/lib/postgresql/.ssh/authorized_keys
    chown postgres /var/lib/postgresql/.ssh/authorized_keys
    chmod 600 /var/lib/postgresql/.ssh/authorized_keys

Убедимся, что доступ есть:

    root@pg1:~# sudo -u postgres ssh pg2.lab.office -x 'w'
    09:04:07 up 3 days, 23:48,  1 user,  load average: 0.00, 0.00, 0.00
    USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
    logan    pts/0    192.168.0.70     14:28    1:19   0.22s  0.02s sshd: logan [priv]

Для остальных серверов делаем по аналогии.

Для того, чтобы процесс repmgr мог сам перезапускать постгрес - ему нужны соответствующие права. Чтобы их дать - создадим файл `/etc/sudoers.d/repmgr` и впишем туда:

    Cmnd_Alias PGRE = /bin/systemctl status postgresql, \
        /bin/systemctl start postgresql, \
        /bin/systemctl stop postgresql, \
        /bin/systemctl restart postgresql, \
        /bin/systemctl reload postgresql

    postgres ALL=(ALL) NOPASSWD: PGRE

Это позволит пользователю postgres перезапускать процесс сервера postgres.

Теперь нам надо настроить сам postgresql. Обязательный минимум:

-   доступность из сети (директива `listen_addresses`)
-   репликация (`wal_level`, `archive_mode`)
-   лимит `max_wal_senders` - как минимум на 1 больше количества серверов в кластере
-   `hot_standby` - для серверов в режиме slave.
-   права на репликацию в hba

Если вы строите кластер на debian - настройки надо скопировать на все сервера кластера. Для centos это не обязательно, так как файл настроек лежит прямо в data directory и при клонировании repmgr вытащит файлы.

Пример настроек - `/etc/postgresql/10/main/postgresql.conf`:

    # тут приведены не все настройки, а только то, что я поменял
    # часть настроек в файле закомментирована, а в части указаны другие значения.
    # Пользуйтесь поиском.
    listen_addresses = '*'

    wal_level = replica

    archive_mode = on
    archive_command = 'cp %p /var/lib/pg-arch/%f'

    max_wal_senders = 4

    hot_standby = on

В данном примере мы не удаляем архивированные сегменты, а перемещаем их папку `/var/lib/pg-arch/`. Это позволит восстановить “отставший” slave. Подробнее я писал [здесь](/2018-01-05-pgsql-replica). Эту папку нужно создать (владелец - postgres, права доступа - 700). Папку нужно периодически чистить – postgres сам не очищает архивы. В упомянутой выше статье вы найдете детальное описание.

Пример настроек hba. В данном примере пользователь БД называется `repmgr`. Служебная база repmgr - `repmgrdb`:

    # Не менять! сломаются локальные операции!
    local   all             postgres                                peer

    # TYPE  DATABASE        USER            ADDRESS                 METHOD
    local   all             all                                     md5
    host    all             all             127.0.0.1/32            md5
    host    all             all             ::1/128                 md5

    # replication settings
    local   replication     all                                     peer
    host    replication     all             127.0.0.1/32            md5
    host    replication     all             ::1/128                 md5
    host    repmgrdb        repmgr          192.168.0.0/24          md5
    host    replication     repmgr          192.168.0.0/24          md5

    #remote access to server cluster. any DB, any user, any host, password required
    host    all             all             0.0.0.0/0               md5

С настройкой postgres закончили, перезапускаем сервер и создаем служебную базу и пользователя. Пароль для пользователя лучше сделать посложнее:

    service postgresql status
    sudo -u postgres psql

    psql (10.4 (Debian 10.4-2.pgdg90+1))
    Type "help" for help.

    postgres=# create user repmgr with superuser;
    postgres=# alter role repmgr with password 'Ahn7yaechie6hoe0av8eF0ei';
    postgres=# create database repmgrdb owner repmgr;
    postgres=# \q

Чтобы repmgr мог обращаться к серверу БД и ему не требовалось вводить пароль – нужно создать файл `/var/lib/postgresql/.pgpass`. Владельцем файла должен быть пользователь postgres, права - 0600 (иначе он игнорируется). Структура файла - `IP:port:DB:user:password`. `*` означает “любое”. К сожалению pgpass не в состоянии работать с CIDR, то есть задать адрес как `192.168.0.*` можно, а `192.168.0.0/24` – нельзя. Пример файла:

    *:5432:repmgrdb:repmgr:Ahn7yaechie6hoe0av8eF0ei
    *:5432:replication:repmgr:Ahn7yaechie6hoe0av8eF0ei

Вторая запись нужна для самого процесса репликации.

Теперь настроим repmgr. Его настройки в debian лежат в файле `/etc/repmgr.conf`. Пример с комментариями:

    # ID узла (сервера). В рамках кластера обязательно уникальный
    node_id=1
    # hostname. Остальные узлы должны иметь возможность найти этот именно по этому имени.
    node_name='pg1.lab.office'
    # строка подключения к БД
    conninfo='host=pg1.lab.office user=repmgr dbname=repmgrdb connect_timeout=2'
    # директория с данными postgres.
    data_directory='/var/lib/postgresql/10/main/'
    # режим репликации. Пока что поддерживается только этот
    replication_type=physical

    # log file. Не забудьте создать папку для него.
    log_file='/var/log/repmgr/repmgr.log'
    # записывать статус каждые 5 минут (300 секунд)
    log_status_interval=300

    # где находятся bin-файлы postgres.
    pg_bindir='/usr/lib/postgresql/10/bin/'

    # не использовать password из conninfo (строки выше)
    # мы храним пароль в .pgpass, это безопаснее. Потому - false
    use_primary_conninfo_password=false
    ssh_options='-q -o ConnectTimeout=10'

    # режим failover
    failover=manual
    # очередность выборов мастера в случае отказа
    # эта настройка и последующие применимы только если failover - auto
    priority=100
    reconnect_attempts=3
    reconnect_interval=5
    promote_command='/usr/bin/repmgr -f /etc/repmgr.conf standby promote --log-to-file'
    follow_command='/usr/bin/repmgr standby follow -f /etc/repmgr.conf --log-to-file --upstream-node-id=%n'

    # команды запуска, остановки и перезапуска сервиса. Должны соответствовать тому, что мы вписали в sudo.
    service_start_command = 'sudo -n /bin/systemctl start postgresql'
    service_stop_command =  'sudo -n /bin/systemctl stop postgresql'
    service_restart_command = 'sudo -n /bin/systemctl restart postgresql'
    service_reload_command = 'sudo -n /bin/systemctl reload postgresql'

Теперь можно зарегистрировать первый сервер в кластере:

    sudo -u postgres repmgr -f /etc/repmgr.conf primary register

Проверим, что он там появился:

    root@pg1:~# sudo -u postgres repmgr -f /etc/repmgr.conf cluster show
     ID | Name           | Role    | Status    | Upstream       | Location | Connection string
    ----+----------------+---------+-----------+----------------+----------+-------------------------------------------------------------------
     1  | pg1.lab.office | primary | * running |                | default  | host=pg1.lab.office user=repmgr dbname=repmgrdb connect_timeout=2

Все ок. Теперь настроим остальные два сервера. Они настраиваются по аналогии с первым сервером. Только в конфиге repmgr.conf нужно поменять node\_id, node\_name и conninfo. На 2 и 3 серверах запускать postgres не нужно.

Удалим существующий data-dir и склонируем базу с мастера:

    root@pg2:~# service postgresql stop
    root@pg2:~# rm -rf /var/lib/postgresql/10/main/*
    root@pg2:~# sudo -u postgres repmgr -f /etc/repmgr.conf -h pg1.lab.office -U repmgr -d repmgrdb standby clone

    NOTICE: destination directory "/var/lib/postgresql/10/main/" provided
    NOTICE: starting backup (using pg_basebackup)...
    HINT: this may take some time; consider using the -c/--fast-checkpoint option
    NOTICE: standby clone (using pg_basebackup) complete
    NOTICE: you can now start your PostgreSQL server
    HINT: for example: sudo -n /bin/systemctl start postgresql
    HINT: after starting the server, you need to register this standby with "repmgr standby register"

Запускаем сервер, регистрируемся:

    root@pg2:~# service postgresql start
    root@pg2:~# sudo -u postgres /usr/bin/repmgr standby register
    NOTICE: standby node "pg2.lab.office" (id: 2) successfully registered

Проверяем, что изменилось:

    root@pg1:~# sudo -u postgres repmgr -f /etc/repmgr.conf cluster show
     ID | Name           | Role    | Status    | Upstream       | Location | Connection string
    ----+----------------+---------+-----------+----------------+----------+-------------------------------------------------------------------
     1  | pg1.lab.office | primary | * running |                | default  | host=pg1.lab.office user=repmgr dbname=repmgrdb connect_timeout=2
     2  | pg2.lab.office | standby |   running | pg1.lab.office | default  | host=pg2.lab.office user=repmgr dbname=repmgrdb connect_timeout=2

Третий сервер запускаем по аналогии со вторым.

Failover  [](#failover) 
----------------------

Итак, мастер сломался и надо переключится на slave:

    root@pg2:~# sudo -u postgres repmgr -f /etc/repmgr.conf cluster show
     ID | Name           | Role    | Status        | Upstream       | Location | Connection string
    ----+----------------+---------+---------------+----------------+----------+-------------------------------------------------------------------
     1  | pg1.lab.office | primary | ? unreachable |                | default  | host=pg1.lab.office user=repmgr dbname=repmgrdb connect_timeout=2
     2  | pg2.lab.office | standby |   running     | pg1.lab.office | default  | host=pg2.lab.office user=repmgr dbname=repmgrdb connect_timeout=2
     3  | pg3.lab.office | standby |   running     | pg1.lab.office | default  | host=pg3.lab.office user=repmgr dbname=repmgrdb connect_timeout=2

    WARNING: following issues were detected
      - when attempting to connect to node "pg1.lab.office" (ID: 1), following error encountered :
    "could not connect to server: Connection refused
        Is the server running on host "pg1.lab.office" (192.168.0.17) and accepting
        TCP/IP connections on port 5432?"
      - node "pg1.lab.office" (ID: 1) is registered as an active primary but is unreachable

Прежде чем продолжить – обязательно убедитесь, что старый мастер (pg1) отключен и не “оживет” в самый неподходящий момент. Repmgr не умеет работать с fencing-ом и вы можете потерять часть данных.

Повышаем pg2 до мастера:

    root@pg2:~# sudo -u postgres repmgr -f /etc/repmgr.conf standby promote
    NOTICE: promoting standby to primary
    DETAIL: promoting server "pg2.lab.office" (ID: 2) using "/usr/lib/postgresql/10/bin/pg_ctl  -w -D '/var/lib/postgresql/10/main/' promote"
        waiting for server to promote.... done
    server promoted
    NOTICE: STANDBY PROMOTE successful
    DETAIL: server "pg2.lab.office" (ID: 2) was successfully promoted to primary

Проверяем статус:

    root@pg2:~# sudo -u postgres repmgr -f /etc/repmgr.conf cluster show
     ID | Name           | Role    | Status    | Upstream       | Location | Connection string
    ----+----------------+---------+-----------+----------------+----------+-------------------------------------------------------------------
     1  | pg1.lab.office | primary | - failed  |                | default  | host=pg1.lab.office user=repmgr dbname=repmgrdb connect_timeout=2
     2  | pg2.lab.office | primary | * running |                | default  | host=pg2.lab.office user=repmgr dbname=repmgrdb connect_timeout=2
     3  | pg3.lab.office | standby |   running | pg1.lab.office | default  | host=pg3.lab.office user=repmgr dbname=repmgrdb connect_timeout=2

pg3 по-прежнему ждет данных от pg1. Его нужно переключить на новый мастер:

    root@pg3:~# sudo -u postgres /usr/bin/repmgr standby follow -f /etc/repmgr.conf --upstream-node-id=2
    NOTICE: setting node 3's primary to node 2
    NOTICE: restarting server using "sudo -n /bin/systemctl restart postgresql"
    NOTICE: STANDBY FOLLOW successful
    DETAIL: node 3 is now attached to node 2

Проверим состояние кластера:

    root@pg2:~# sudo -u postgres repmgr -f /etc/repmgr.conf cluster show
     ID | Name           | Role    | Status    | Upstream       | Location | Connection string
    ----+----------------+---------+-----------+----------------+----------+-------------------------------------------------------------------
     1  | pg1.lab.office | primary | - failed  |                | default  | host=pg1.lab.office user=repmgr dbname=repmgrdb connect_timeout=2
     2  | pg2.lab.office | primary | * running |                | default  | host=pg2.lab.office user=repmgr dbname=repmgrdb connect_timeout=2
     3  | pg3.lab.office | standby |   running | pg2.lab.office | default  | host=pg3.lab.office user=repmgr dbname=repmgrdb connect_timeout=2

    WARNING: following issues were detected
      - when attempting to connect to node "pg1.lab.office" (ID: 1), following error encountered :
    "could not connect to server: Connection refused
        Is the server running on host "pg1.lab.office" (192.168.0.17) and accepting
        TCP/IP connections on port 5432?"

Теперь мы можем убрать старый сервер из конфигурации:

    root@pg2:~# sudo -u postgres repmgr -f /etc/repmgr.conf primary unregister --node-id=1

Проверяем еще раз:

    root@pg2:~# sudo -u postgres repmgr -f /etc/repmgr.conf cluster show
     ID | Name           | Role    | Status    | Upstream       | Location | Connection string
    ----+----------------+---------+-----------+----------------+----------+-------------------------------------------------------------------
     2  | pg2.lab.office | primary | * running |                | default  | host=pg2.lab.office user=repmgr dbname=repmgrdb connect_timeout=2
     3  | pg3.lab.office | standby |   running | pg2.lab.office | default  | host=pg3.lab.office user=repmgr dbname=repmgrdb connect_timeout=2

Теперь можно спокойно чинить pg1, без риска что он внезапно вернется в сеть и будет конфликт записи в slave.

Возврат мастера  [](#%d0%b2%d0%be%d0%b7%d0%b2%d1%80%d0%b0%d1%82-%d0%bc%d0%b0%d1%81%d1%82%d0%b5%d1%80%d0%b0) 
----------------------------------------------------------------------------------------------------------

Самый простой способ вернуть старый сервер - удалить его datadir и зарегистрировать заново как slave:

    root@pg1:~# rm -rf /var/lib/postgresql/10/main/*
    root@pg1:~# sudo -u postgres repmgr -f /etc/repmgr.conf -h pg2.lab.office -U repmgr -d repmgrdb standby clone
    NOTICE: destination directory "/var/lib/postgresql/10/main/" provided
    NOTICE: starting backup (using pg_basebackup)...
    HINT: this may take some time; consider using the -c/--fast-checkpoint option
    NOTICE: standby clone (using pg_basebackup) complete
    NOTICE: you can now start your PostgreSQL server
    HINT: for example: sudo -n /bin/systemctl start postgresql
    HINT: after starting the server, you need to register this standby with "repmgr standby register"
    root@pg1:~# service postgresql start
    root@pg1:~# sudo -u postgres /usr/bin/repmgr standby register
    WARNING: --upstream-node-id not supplied, assuming upstream node is primary (node ID 2)
    NOTICE: standby node "pg1.lab.office" (id: 1) successfully registered

Заключение  [](#%d0%b7%d0%b0%d0%ba%d0%bb%d1%8e%d1%87%d0%b5%d0%bd%d0%b8%d0%b5) 
----------------------------------------------------------------------------

Repmgr - простой, понятный инструмент, который сильно облегчает операции в master-slave конфигурациях. Он не позволит полностью автоматизировать защиту от отказов (для этого нужны другие инструменты), но поможет в простых конфигурациях. Я очень рекомендую использовать его – самостоятельно или в сочетании с pgpool-II. В таком варианте pgpool отвечает за балансировку запросов, фенсинг и инициирует failover, когда это необходимо. Repmgr отвечает за сам низкоуровневый процесс failover (и это намного лучше, чем рекомендуемый pgpool набор жутковатых скриптов!).

При этом я очень не рекомендую:

-   использовать автоматический failover средствами repmgr. Строго говоря – он не работает. Для работы repmgr нужен работающий сервер postgresql, и если postgresql master упал – repmgr не в состоянии самостоятельно переключится (из-за упавшего мастера)
-   использовать bidirectional multimaster. Эта функция заявлена в описании, но работает крайне плохо - сервера теряют связь друг с другом и часто трут конфликтующие данные. Проверено до версии **4.0.5**

