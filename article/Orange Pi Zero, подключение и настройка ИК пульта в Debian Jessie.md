---
tags: [linux,orange pi]
source: https://elchupanibrei.livejournal.com/42272.html
---
Orange Pi Zero, подключение и настройка ИК пульта в Debian Jessie.
==================================================================

Продолжаем ковырять [Orange Pi Zero](https://elchupanibrei.livejournal.com/39639.html). Сегодня прикрутим ИК пульт дистанционного управления. Научимся управлять общей громкостью и выключать апельсинку не вставая с дивана.  
  
Загружаем PuTTY, вписываем IP и устанавливаем SSH соединение на порту 22. Вводим логин и пароль.  
  
Загружаем УДОБНОЕ текстовое меню:  
  
*root@orangepizero:~\# **sudo armbian-config***  
  
Попадаем в меню:  
  

![](https://ic.pics.livejournal.com/elchupanibrei/30212434/84207/84207_original.jpg)  
*текстовое меню здорового человека*

  
Идем в **Network**, выбирем **wlan** и кликаем на **Install IR**. Утилита сама подгрузит у установит [LIRC](https://en.wikipedia.org/wiki/LIRC) пакет.  
  
Теперь загружаем ИК модуль ядра, который по умолчанию отключен в Debian от Armbian:  
  
*root@orangepizero:~\# **sudo modprobe sunxi\_cir***  
  
Посмотреть загрузился ли модуль можно командой выводящей список всех активных модулей ядра:  
  
*root@orangepizero:~\# **sudo lsmod***  
  
Проверим нашли ли друг друга модуль ядра и LIRC:  
  
*root@orangepizero:~\# **sudo evtest***  
  
Выбираем **sunxi-ir**, в моем случае он под цифрой 2 и тыкаем кнопки пульта. Если в терменале запрыгали цыфры и буквы все отлично.  
  

![](https://ic.pics.livejournal.com/elchupanibrei/30212434/84521/84521_original.jpg)  
*прыгают цифры*

  
Добавляем ИК модуль ядра в автозагрузку. Если этого не сделать то после reboot все придется повторять заново. Загружаем [Midnight Commander](https://elchupanibrei.livejournal.com/41913.html) и идем в папку **etc/modules-load.d/** открываем файл **@modules.conf** на запись командой **F4** и добавляем строку **sunxi\_cir** как на картинке.  
  

![](https://ic.pics.livejournal.com/elchupanibrei/30212434/84902/84902_original.jpg)  
*открытый @modules.conf*

  
Теперь сконфигурируем LIRC. Идем в папку **etc/lirc/** открываем файл **hardware.conf**, удаляем все и копируем текст:  
  
***\# /etc/lirc/hardware.conf  
\#  
\# Arguments which will be used when launching lircd  
LIRCD\_ARGS=""  
  
\# Don't start lircmd even if there seems to be a good config file  
START\_LIRCMD=false  
  
\# Don't start irexec, even if a good config file seems to exist  
[\#START\_IREXEC](https://www.livejournal.com/rsearch/?tags=%23START_IREXEC)=false  
  
\# Start irexec  
START\_IREXEC=true  
  
\# Try to load appropriate kernel modules  
LOAD\_MODULES=true  
  
\# Run "lircd --driver=help" for a list of supported drivers.  
[\#DRIVER](https://www.livejournal.com/rsearch/?tags=%23DRIVER)="UNCONFIGURED"  
DRIVER="default"  
  
\# Usually /dev/lirc0 is the correct setting for systems using udev  
DEVICE="/dev/lirc0"  
MODULES="sunxi\_cir"  
  
\# Default configuration files for your hardware if any  
LIRCD\_CONF=""  
LIRCMD\_CONF=""***  
  
Сохраняем и закрываем. Следующий этап обучить LIRC понимать пульт. Идем по адресу <http://lirc.sourceforge.net/remotes/>, ищем свой пульт и кидаем файл **lircd.conf** в папку **etc/lirc/**. Если не нашли то удаляем старый конфиг пульта командой:  
  
*root@orangepizero:~\# **sudo rm /etc/lirc/lircd.conf***  
  
Останавливаем LIRC чтоб не мешал:  
  
*root@orangepizero:~\# **sudo service lirc stop***  
  
Запускаем процесс обучения LIRC новому пульту:  
  
*root@orangepizero:~\# **sudo irrecord --disable-namespace -H default -d /dev/lirc0 /etc/lirc/lircd.conf***  
  
Для определения header, gap и bit mask пульта нажимаем на все кнопки пока irrecord-у не надоест. Повторяем процедуру 2 раза. В появившемся приглашении пишем название клавиши, на пример **KEY\_CH-**. Нажимаем и удерживаем клавишу "**CH-**" до следующего приглашения. Повторяем пока не закончатся клавиши.  
  
Я купил пульт на eBay и создал для него **lircd.conf**. Качаем [тут](https://drive.google.com/open?id=19X73_-bOC3qm-L425PGpRBsvr7ujiE09). Продают за $1.3. В довесок идет ИК приемник [HX1838/VS1838](http://dalincom.ru/datasheet/AX-1838HS.pdf). Батарейку CR2025 продавец зажал. Дома нашлась и подошла CR2032.  
  

![](https://ic.pics.livejournal.com/elchupanibrei/30212434/85260/85260_original.jpg)  
*дешманский пульт с eBay*

  
После того как файл пульта скопирован или создан перезапускаем LIRC:  
  
*root@orangepizero:~\# **sudo service lirc start***  
  
Для проверки конфига пульта запускаем команду:  
  
*root@orangepizero:~\# **sudo irw /dev/lircd***  
  
Нажимаем кнопки на пульте. В терминале дожна появится комнда и имя котрое ей присвоенно, а так же марка пульта если она прописана.  
  

![](https://ic.pics.livejournal.com/elchupanibrei/30212434/85752/85752_original.jpg)  
*тест конфига ИК пульта*

  
Для дистанционного управления [Logitech Media Server](https://elchupanibrei.livejournal.com/39698.html) через Command Line Interface нам понабится telnet:  
  
*root@orangepizero:~\# **sudo apt-get install expect telnet***  
  
Создаем файл **lircrc** в папке **etc/lirc/**. В нем описывается как и какому приложению реагировать на нажатие клавиш. Я уже все написал за вас. Копируем в **lircrc** текст и сохраняем (если у вас другой пульт, то поля **remote** и **button** придется заменить на свои):  
  
***\# telnet is required to control Logitech Media Server via Command Line Interface  
\# sudo apt-get install expect telnet  
  
begin  
prog = irexec  
remote = CAR\_MP3  
button = KEY\_CH  
repeat = 0  
delay = 2000  
config = sudo shutdown -h now  
end  
  
begin  
prog = irexec  
remote = CAR\_MP3  
button = KEY\_VOL-  
repeat = 0  
delay = 0  
config = echo "mixer volume -2.5" | telnet orangepizero 9090  
end  
  
begin  
prog = irexec  
remote = CAR\_MP3  
button = KEY\_VOL+  
repeat = 0  
delay = 0  
config = echo "mixer volume +2.5" | telnet orangepizero 9090  
end  
  
begin  
prog = irexec  
remote = CAR\_MP3  
button = KEY\_EQ  
repeat = 0  
delay = 0  
config = echo "mixer muting toggle" | telnet orangepizero 9090  
end  
  
begin  
prog = irexec  
remote = CAR\_MP3  
button = KEY\_PLAY\_PAUSE  
repeat = 0  
delay = 0  
config = echo "pause" | telnet orangepizero 9090  
end  
  
begin  
prog = irexec  
remote = CAR\_MP3  
button = KEY\_PREV  
repeat = 0  
delay = 0  
config = echo "playlist index -1" | telnet orangepizero 9090  
end  
  
begin  
prog = irexec  
remote = CAR\_MP3  
button = KEY\_NEXT  
repeat = 0  
delay = 0  
config = echo "playlist index +1" | telnet orangepizero 9090  
end***  
  
На всякий случай перезагружаем Orange Pi Zero:  
  
*root@orangepizero:~\# **sudo reboot***  
  
Кнопка "**CH**" выключение апельсина, "**VOL+**" увеличение громокости, "**VOL-**" уменьшение громкости, "**EQ**" отключить-включить звук, "**PLAY/PAUSE**" проигрование и пауза, "**PREV**" предыдущий трек в текущем плейлисте и "**NEXT**" следующий трек в плейлисте.  
  
Остальной список команд смотреть в тут:  
  
***хттп://IP вашего LMS:9000/html/docs/cli-api.html[\#playlistcontrol](https://www.livejournal.com/rsearch/?tags=%23playlistcontrol)***  
  
PROFIT!!!  
