---
source: https://habr.com/ru/articles/847360/
tags: [debian,centos,selinux]
habs: [Информационная безопасность,Серверное администрирование,Системное программирование]
---
# Небезопасность Debian
В июне 2023 года Red Hat приняла спорное решение изменить способ распространения исходного кода Red Hat Enterprise Linux (RHEL). В социальных сетях разгорелись бурные обсуждения, оставившие многих в недоумении относительно последствий этого решения. Возникло множество вопросов о будущей жизнеспособности вторичных сборок RHEL, затрагивающих такие дистрибутивы, как Rocky Linux, AlmaLinux, Oracle Linux и другие. Каждый из них впоследствии сделал заявления, пытаясь успокоить свои сообщества.

Тем не менее, многие в сообществе открытого ПО расценили решение Red Hat как откровенно подлый ход.

Всё больше людей заявляют, что они перейдут (или уже перешли) на Debian, ища убежища от того, что они считают жадным корпоративным влиянием. Я полностью понимаю это чувство. Однако есть проблема, о которой я хочу поговорить: безопасность.

Горькая правда заключается в том, что обеспечение безопасности — сложная задача. Это утомительно, неприятно и требует большой работы, чтобы сделать всё правильно.

Debian недостаточно делает для защиты пользователей.

Давно Red Hat внедрила использование SELinux (Security\-Enhanced Linux). И они пошли дальше простого включения этой функции в своё ядро. Они проделали кропотливую работу по созданию политик SELinux по умолчанию для своего дистрибутива.

Эти политики поставляются включенными по умолчанию в их дистрибутиве. Политики помогают защитить различные демоны, работающие по умолчанию в RHEL, а также многие из наиболее популярных демонов, которые обычно используются на серверах.

Apache, nginx, MariaDB, PostgreSQL, OpenSSH и т. д. — все они охватываются политиками SELinux, поставляемыми в дистрибутивах RHEL.

Защита распространяется даже на контейнеры. Контейнеры становятся всё более предпочтительным методом развертывания программного обеспечения для разработчиков, включая меня. Распространенное заблуждение заключается в том, что если вы запускаете что\-то в контейнере, это по своей сути безопасно. Это совершенно неверно. Сами по себе контейнеры не решают проблему безопасности. Они решают проблему распространения программного обеспечения. Они создают ложное впечатление безопасности у тех, кто их использует.

В дистрибутивах на основе Red Hat вы можете использовать podman — альтернативу Docker, которая позволяет запускать контейнеры без демона (в отличие от Docker) и предоставляет другие преимущества, например, возможность полностью безрутового запуска. Но Red Hat идет еще дальше и применяет строгие политики SELinux по умолчанию, которые отделяют контейнер от основной ОС и даже от других контейнеров!

Было множество примеров возможности выхода из контейнера и доступа к основной ОС или другим контейнерам. Вот где вступают в игру такие инструменты, как SELinux. Применение политик SELinux к контейнеру позволяет создать укрепленный «саркофаг» для вашего приложения, который снижает риск неизвестных будущих эксплоитов. И это практически не требует усилий в RHEL.

Red Hat понимала, что если они не проделают работу над этими политиками по умолчанию, их пользователи просто не будут использовать эту технологию, и миллионы серверов останутся уязвимыми. Потому что, будем реалистами, SELinux — сложная штука. Язык политик и инструменты громоздки, неинтуитивны и примерно так же привлекательны, как заполнение налоговых деклараций. Честно говоря, это ужасно в использовании — если вы вручную создаете свои собственные политики.

Но благодаря проделанной Red Hat работе, использование SELinux в RHEL в основном прозрачно и обеспечивает реальные преимущества безопасности для своих пользователей.

### Подход Debian

Debian, оплот сообщества открытого исходного кода, почитается за свою стабильность и обширную библиотеку программного обеспечения. Я являюсь поклонником и ежегодно жертвую проекту (вам тоже следует!), хотя и не использую его в производственной среде.

Тем не менее, его система безопасности по умолчанию оставляет желать лучшего. Решение Debian включить AppArmor по умолчанию, начиная с версии 10, знаменует собой позитивный шаг к повышению безопасности, однако оно не достигает цели из\-за неполноценной реализации в системе.

Опора Debian на AppArmor и его конфигурации по умолчанию выявляет системную проблему с его подходом к безопасности. Хотя AppArmor способен обеспечить надежную безопасность при правильной настройке, настройки Debian «из коробки» не используют весь его потенциал:

* **Ограниченные профили по умолчанию:** Debian поставляется с минимальным набором профилей AppArmor, оставляя многие критически важные службы незащищенными.
* **Реактивная, а не проактивная позиция:** Модель безопасности Debian часто полагается на пользователей в реализации более строгих политик, вместо предоставления безопасной по умолчанию конфигурации.
* **Непоследовательное применение:** В отличие от SELinux в системах Red Hat, который применяется ко всей системе единообразно, AppArmor в Debian применяется фрагментарно, что приводит к потенциальным брешам в безопасности.
* **Нехватка ресурсов:** Debian как сообщественный проект не имеет ресурсов для разработки и поддержки комплексных политик безопасности, сравнимых с теми, которые предоставляет Red Hat.

Очень часто пользователи запускают контейнерные рабочие нагрузки в Debian через Docker, который автоматически генерирует и загружает профиль AppArmor по умолчанию для контейнеров с именем `docker-default`. К сожалению, это не очень надежный профиль с точки зрения безопасности, он излишне разрешителен.

Этот профиль, хотя и обеспечивает некоторую защиту, оставляет значительные поверхности атаки открытыми. Например:


```
  network,
  capability,
  file,
  umount,
  # Host (privileged) processes may send signals to container processes.
  signal (receive) peer=unconfined,
  # runc may send signals to container processes (for "docker stop").
  signal (receive) peer=runc,
  # crun may send signals to container processes (for "docker stop" when used with crun OCI runtime).
  signal (receive) peer=crun,
  # dockerd may send signals to container processes (for "docker kill").
  signal (receive) peer={{.DaemonProfile}},
  # Container processes may send signals amongst themselves.
  signal (send,receive) peer={{.Name}},
  deny @{PROC}/* w,   # deny write for all files directly in /proc (not in a subdir)
  # deny write to files not in /proc/<number>/** or /proc/sys/**
  deny @{PROC}/{[^1-9],[^1-9][^0-9],[^1-9s][^0-9y][^0-9s],[^1-9][^0-9][^0-9][^0-9/]*}/** w,
  deny @{PROC}/sys/[^k]** w,  # deny /proc/sys except /proc/sys/k* (effectively /proc/sys/kernel)
  deny @{PROC}/sys/kernel/{?,??,[^s][^h][^m]**} w,  # deny everything except shm* in /proc/sys/kernel/
  deny @{PROC}/sysrq-trigger rwklx,
  deny @{PROC}/kcore rwklx,
  deny mount,
  deny /sys/[^f]*/** wklx,
  deny /sys/f[^s]*/** wklx,
  deny /sys/fs/[^c]*/** wklx,
  deny /sys/fs/c[^g]*/** wklx,
  deny /sys/fs/cg[^r]*/** wklx,
  deny /sys/firmware/** rwklx,
  deny /sys/devices/virtual/powercap/** rwklx,
  deny /sys/kernel/security/** rwklx,
```
Правило **network** разрешает все сетевые системные вызовы без ограничений.

Правило **capability**, без конкретных запретов, разрешает большинство возможностей по умолчанию.

Правило **file** предоставляет широкие права доступа к файлам, полагаясь на конкретные правила запрета для защиты.

### AppArmor против SELinux

Фундаментальное различие между AppArmor и SELinux заключается в их подходе к МАС (Mandatory Access Control \- принудительный контроль доступа). AppArmor работает на основе модели, основанной на путях, в то время как SELinux использует значительно более сложную систему принудительного управления доступом на основе типов. Это различие становится особенно очевидным в контейнерных средах.

SELinux применяет тип к каждому объекту в системе \- файлам, процессам, портам, всему, что угодно. Когда вы запускаете контейнер в системе RHEL с поддержкой SELinux, ему немедленно назначается тип **container\_t** \- строгий механизм контроля доступа. Тип **container\_t** эффективно изолирует контейнер, предотвращая его взаимодействие с любым объектом, не помеченным явно для использования контейнером.

Но SELinux не ограничивается принудительным контролем типов. Он выводит изоляцию контейнеров на новый уровень с помощью меток MCS (Multi\-Category Security \- многокатегорийная безопасность). Эти метки функционируют как дополнительный уровень разделения, гарантируя, что даже контейнеры одного типа (**container\_t**) остаются изолированными друг от друга. Каждый контейнер получает свою собственную уникальную метку MCS, создавая то, что по сути является частной «песочницей» внутри более широкой среды **container\_t**.

AppArmor, напротив, не заботится о типах или категориях. Он фокусируется на ограничении возможностей конкретных программ на основе предопределенных профилей. Эти профили указывают, к каким файлам программа может обращаться и какие операции она может выполнять. Хотя этот подход проще в реализации и понимании, ему не хватает детализации и системной согласованности принудительного контроля типов SELinux. Почти ни один из основных дистрибутивов Linux не распространяет по умолчанию исчерпывающие профили AppArmor для всех распространенных сетевых демонов.

Практические последствия этих различий весьма существенны. В среде SELinux скомпрометированный контейнер сталкивается со значительными препятствиями при доступе к хостовой системе или другим контейнерам или воздействии на них благодаря двойным барьерам принудительного контроля типов и меток MCS.

Это не значит, что один универсально лучше другого. SELinux предлагает более надежную изоляцию, но ценой повышенной сложности и потенциальных накладных расходов на производительность. AppArmor предоставляет более простую, более доступную модель безопасности, которая, тем не менее, может быть весьма эффективной при правильной настройке. Суть моего рассуждения в том, что Red Hat проделала работу, чтобы сделать использование SELinux и контейнеров бесшовным и легким для своих пользователей. Вас не оставляют разбираться самостоятельно.

В конечном счете, выбор между Debian и Red Hat — это не просто выбор между корпоративным влиянием и разработкой, управляемой сообществом. Это также выбор между системой, которая предполагает лучшее, и системой, которая готовится к худшему. К сожалению, в современном взаимосвязанном мире пессимизм необходим.

Только зарегистрированные пользователи могут участвовать в опросе. [Войдите](/kek/v1/auth/habrahabr/?back=/ru/articles/847360/&hl=ru), пожалуйста.Должен ли Линукс\-дистрибутив быть по умолчанию настроен на безопасность, даже в ущерб простоте и удобств пользования?25\.95% Да759\.69% Не уверен2848\.79% Хотелось бы согласиться, но в реальной жизни это неудобно14115\.57% Нет45 Проголосовали 289 пользователей. Воздержались 30 пользователей. 