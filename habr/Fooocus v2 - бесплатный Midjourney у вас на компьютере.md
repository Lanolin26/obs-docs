---
tags:
- stable diffusion
- дизайн
- нейросети
- midjourney
- sdxl
- Fooocus
- исскуственный интеллект
- инструкция
habs: [Графический дизайн,Будущее здесь,Искусственный интеллект]
source: https://habr.com/ru/articles/774908/
---
# Fooocus v2 — бесплатный Midjourney у вас на компьютере. Подробная инструкция по установке и использованию нейросети
Друзья, всем привет! Сегодня я хочу рассказать вам про самую простую и доступную для понимания нейросеть, которая создает изображения по вашему текстовому описанию. Она называется **Fooocus** и основана на знаменитой **Stable Diffusion XL**. Это идеальное решение в качестве вашей первой нейросети, и необходимый инструмент для любого дизайнера или контент мейкера.

![Fooocus v2 — бесплатный Midjourney у вас на компьютере.](picture/4b38867d8677e88fc7271463048e3784.gif "Fooocus v2 — бесплатный Midjourney у вас на компьютере.")

Fooocus v2 — бесплатный Midjourney у вас на компьютере.

Автор **Fooocus** не случайный разработчик, а сам создатель **ControlNet**, очень важной подсистемы для **Stable Diffusion**, которая изменила все в мире генерации изображений, позволив художникам и дизайнерам полностью контролировать создаваемый арт. Создатель сравнивает свой проект с **Midjourney** по качеству арта и удобству использования. И действительно порог входа в эту нейросеть очень низкий, а результаты отличные с первой генерации. Установим, изучим, сделаем выводы, поехали.

Что нам понадобится:

1. Компьютер или ноутбук с видеокартой минимум на **8GB** видеопамяти.
2. Около **25GB** свободного места на диске для одного режима и **40GB** для всех трех.

Или **Google** аккаунт для запуска в облаке.

**Fooocus** пока еще не забанен в **Google Colab,** а это значит, что если у вас нет подходящего компьютера вы можете запустить приложение на серверах гугла совершенно **бесплатно**. ПК бояре могут спускаться к следующему заголовку. Поговорим про запуск в облаке.

#### Запуск в Google Colab

![](picture/fd71621c1ecaf3ffc9d3370f6583a795.png)Открываем вот [эту ссылку](https://colab.research.google.com/github/lllyasviel/Fooocus/blob/main/fooocus_colab.ipynb), и нажимаете на кнопку плей, соглашаетесь с гуглом и жмите кнопку **Выполнить**. Ждите пока произойдёт скачивание и установка на сервер **Google Colab**, это может занять до 10 минут.

![](picture/a9c00177ce88d3010089fb462bb62760.png)Вы поймете что установка завершена и программа готова к работе когда внизу консоли увидите **App started successful**.и рядом будет ссылка вида [https://какие\-то\-цифры.gradio.live](https://e40d78454674ca6975.gradio.live/), вот на неё и надо будет кликнуть. Программа откроется готовая к работе.

![](picture/e88a952f0e54c45535b0b15d077c1d2a.png)Если вы хотите запустить в режиме **Realistic** или в режиме **Anime** замените строку кода *!python entry\_with\_update.py \-\-share* на строку *!python entry\_with\_update.py \-\-preset anime \-\-share* для режима **Аниме***,* или на *!python entry\_with\_update.py \-\-preset realistic \-\-share* для режима **Реализма.** Про режимы я еще расскажу ниже.

Помните, что **Google Colab** еще весной прикрыл возможность использовать свои мощности для генерации в **Automatic 1111**, другом интерфейсе нейросети, скорее всего скоро прикроют и этот, поэтому не рассчитывайте на него слишком сильно. Кроме того по итогам моих тестов, вижу что контейнер с фокусом вылетает если сильно грузить его, например если несколько раз подряд отправлять изображение на аутпеинтинг каждый раз с увеличением разрешения. Так, что только локальная версия вас не подведет, к ней и перейдем.

#### Локальная установка

Если у вас ПК на **Windows** и видео карта **NVidia**, все что вам нужно сделать, это скачать архив с [этой страницы](https://github.com/lllyasviel/Fooocus#windows), нажав на **\>\>\> Click here to download \<\<\<.** Архив распакуйте в любую удобную папку не содержащую в путях кириллицы.

![](picture/a4cff0d9baa47f671a2e073f64b653c9.jpeg)После того как архив распакован у вас в папке будет три файла **run.bat**, **run\_anime.bat** и **run\_realistic.bat**, каждый из файлов запускает соответствующий режим, про режимы я покажу наглядно чуть ниже, а пока можете выбрать то, к чему больше душа лежит, я запущу режим по умолчанию \- **run.bat**.

Для установки на **Mac**, **AMD**, **Linux** и т.д. переходите на [гитхаб проекта](https://github.com/lllyasviel/Fooocus) и изучайте способы самостоятельно, поддержка заявлена, но у меня протестировать не на чем, а рассказывать о том, что я сам не протестировал я по понятным причинам не могу.

![](picture/2b4a5264707d2eb09baba2c51d92a0a0.png)Если вы все сделали правильно, не важно локально или в гугле, то у вас уже открыт интерфейс фокуса и выглядит он примерно так, попробуем написать какой\-нибудь простенький промпт и посмотрим что получится. У меня это будет *"Leonardo DiCaprio as a mechanic in a garage with oil effect in a rugged style*". Первая генерация будет дольше чем последующие, потому что еще скачиваются дополнительные файлы. Вот что получилось у меня.

![Leonardo DiCaprio as a mechanic in a garage with oil effect in a rugged style](picture/6ad83f56b005f93272e518807721af63.png "Leonardo DiCaprio as a mechanic in a garage with oil effect in a rugged style")

Leonardo DiCaprio as a mechanic in a garage with oil effect in a rugged style

По моему отличный результат, кстати, если у вас так же как у меня не выбирается автоматически темная тема, просто добавьте в конце адреса в адресной строке *?\_\_theme\=dark*, тогда будет установлена темная тема. Работает и локально и в гугл коллабе.

![Wonder Woman in the style of Babs Tarr with pop art effect. Согласитесь, темная тема гораздо приятнее](picture/bef19d8ea329d73581dbc9ebc64361f3.png "Wonder Woman in the style of Babs Tarr with pop art effect. Согласитесь, темная тема гораздо приятнее")

Wonder Woman in the style of Babs Tarr with pop art effect. Согласитесь, темная тема гораздо приятнее

#### Как писать запросы

Чтобы нейросеть вас понимала, важно научиться правильно писать запросы. В фокусе у нас работают **SDXL** модели, которые отлично понимают человеческий язык, а дополнительный GPT движок улучшает ваши текстовые запросы самостоятельно, поэтому каких\-то особых знаний вам не понадобится. Просто опишите то что хотите видеть следуя такой структуре: Вид изображения, объект, описание внешности, дополнительные элементы, место, эффект, стиль.

Например: Фотография красивой девушки 28 лет, красные волосы заплетенные в косы, большие голубые глаза. Одета в красивое голубое платье с белыми цветами. День, лето, сидит в кафе, пьет кофе. Современная цифровая иллюстрация, рекламный постер. Затем я просто перевожу текст в любом переводчике и получаю отличный результат, который соответствует моим ожиданиям. Вот что вышло у меня.

![Photo of a beautiful girl 28 years old, red hair braided in braids, big blue eyes. Dressed in a beautiful blue dress with white flowers. Day, summer, sitting in a cafe, drinking coffee. Modern digital illustration, advertising poster.](picture/23e647518578defe050e56e942e4b5be.png "Photo of a beautiful girl 28 years old, red hair braided in braids, big blue eyes. Dressed in a beautiful blue dress with white flowers. Day, summer, sitting in a cafe, drinking coffee. Modern digital illustration, advertising poster.")

Photo of a beautiful girl 28 years old, red hair braided in braids, big blue eyes. Dressed in a beautiful blue dress with white flowers. Day, summer, sitting in a cafe, drinking coffee. Modern digital illustration, advertising poster.

По моему отличный результат, но не расстраивайтесь, если у вас что\-то не вышло сразу, написание запросов \- это навык, потренируйтесь всего недельку и у вас будет получаться уже гораздо лучше.

В этом руководстве я использую готовые запросы из моего списка [**100 промптов для новичков**](https://boosty.to/neuro_art/posts/900f35b5-55c9-4367-94bb-48a97628cb28), по которым всегда получается хороший результат, подписчики могут скачать список запросов на [**Бусти**](https://boosty.to/neuro_art)**.** Подпишитесь и вы,ведь на [**Бусти**](https://boosty.to/neuro_art) видео выходят раньше и много эксклюзивных материалов, записи обучающий стримов, а так же доступ в наш секретный чат.Только благодаря поддержке подписчиков у меня есть возможность создавать такие исчерпывающее инструкции и все свое время посвящать изучению нейросетей, чтобы потом делиться информацией с вами друзья. А мы продолжаем изучать **Fooocus** и переходим к режимам.

#### Режимы запуска

Режимы отличаются значительно, в разных режимах используются разные модели (в моделях содержится информация обо всем что может создать нейросеть), подходящие под эти модели настройки, разные дополнительные лоры (дополнительные мини\-модели) и различные стили включены по умолчанию, ниже я перечислил основные отличия и сгенерировал изображения с одинаковым сидом и запросом, но в разных режимах, чтобы вы лучше понимали разницу и смогли выбрать подходящий для себя. Но обязательно попробуйте их все. Дополнительно я указал ссылки на модели и лоры, на сайте civitai, так вы сможете самостоятельно посмотреть изображения которые на них можно создать и запросы к ним.

#### Режим General

![Cat with a bowtie in a coffee shop with steam effect in a cozy style](picture/3e25097f657524a252890ac1d04e4741.png "Cat with a bowtie in a coffee shop with steam effect in a cozy style")

Cat with a bowtie in a coffee shop with steam effect in a cozy style

![Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller](picture/a4ff2c91b54e7f92b17100a244bbcf37.png "Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller")

Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller

Универсальный режим подойдет для всего и для арта и для реалистичных работ, хорошо следует стилям.

**Модель**: [Juggernaut XL](https://civitai.com/models/133005/juggernaut-xl)

**Лора**: [SDXL Offset Example Lora](https://civitai.com/models/137511/sdxl-offset-example-lora)

**Стили по умолчанию:** Fooocus V2, Fooocus Enhance, Fooocus Sharp.

#### Режим Realistic

![Cat with a bowtie in a coffee shop with steam effect in a cozy style](picture/e2843f9664e92b5a00a7ef997af5ee8d.png "Cat with a bowtie in a coffee shop with steam effect in a cozy style")

Cat with a bowtie in a coffee shop with steam effect in a cozy style

![Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller](picture/fbcf27274de0e34b37e127eb5149f442.jpeg "Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller")

Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller

Идеален для близких портретов людей в фотореализме, генерации реалистичных пейзажей или предметов.

**Модель**: [Realistic Stock Photo](https://civitai.com/models/139565/realistic-stock-photo)

**Лора**: [SDXL Film Photography Style](https://civitai.com/models/158945/sdxl-film-photography-style)

**Стили по умолчанию:** Fooocus V2, Fooocus Photograph, Fooocus Negative

**Негативный запрос:** unrealistic, saturated, high contrast, big nose, painting, drawing, sketch, cartoon, anime, manga, render, CG, 3d, watermark, signature, label

#### Режим Anime

![Cat with a bowtie in a coffee shop with steam effect in a cozy style](picture/6a24eb7d00cd0ea8ad08952c8bb5f3fd.png "Cat with a bowtie in a coffee shop with steam effect in a cozy style")

Cat with a bowtie in a coffee shop with steam effect in a cozy style

![Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller](picture/0d50cdb6cf81814ba50593fbb53bb0ad.jpeg "Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller")

Harley Quinn as a waitress in a diner with hammer effect in a playful style, photographed by Juergen Teller

Режим подойдет для Аниме и художественного арта. Обратите внимание, что запрос всегда начинается с 1girl, корректируйте если требуется, а то будете получать анимешных девочек.

**Модель**: [blue\_pencil\-XL](https://civitai.com/models/119012/bluepencil-xl)

**Refiner**: [DreamShaper 8](https://civitai.com/models/4384/dreamshaper)

**Лора**: [SDXL Offset Example Lora](https://civitai.com/models/137511/sdxl-offset-example-lora)

**Стили по умолчанию:** Fooocus V2, Fooocus Masterpiece, SAI Anime, SAI Digital Art, SAI Enhance, SAI Fantasy Art.

**Позитивный запрос:** 1girl,

**Негативный запрос:** (embedding:unaestheticXLv31:0\.8\), low quality, watermark

**Негативный эмбединг**: [unaestheticXL](https://civitai.com/models/119032/unaestheticxl-or-negative-ti)

Надеюсь теперь вы лучше понимаете на что способен Фокус в каждом из режимов и сможете сознательно выбирать режим под задачу. А я же останусь сидеть на режиме **General**, на мой взгляд самый универсальный.

#### Дополнительные настройки

![The Joker in the style of Greg Capullo with ink effect](picture/f889980de7cfbad7da28a18bcfc25e40.png "The Joker in the style of Greg Capullo with ink effect")

The Joker in the style of Greg Capullo with ink effect

Если вы думали, что в самом простом интерфейсе для создания изображений с помощью **SDXL** моделей больше нет настроек, он же простой, то вы ошибаетесь, настроек много. Скрывают их две галочки. Начнем с галочки **Advanced.**

#### Раздел Setting

В этой вкладке находится все, что непосредственно касается настроек генерации.

**Performance** \- позволяет задать производительность, на выбор три режима **Speed** \- 30 шагов,   
**Quality** \- 60 шагов и **Extreme Speed**, между первыми двумя режимами вы разницу скорее всего даже не заметите, а вот последний режим появился совсем недавно, он конечно делает качество хуже, но работает невероятно быстро за счёт использования новой технологии рендеринга **LCM**. Меня обычно устраивает режим **Speed.**

**Aspect Ratios** \- соотношение сторон, позволяет вам выбрать разрешение для вашего изображения, выбор фиксированный не случайно, тут только те разрешения на которых обучались **SDXL** модели, а значит вы при всем желание не сможете сделать что\-то не правильно. Первая цифра это ширина, вторая высота. Для удобства рядом еще написано соотношение сторон. Можно сделать как ультра широкое изображение, например **1728×576**, в стиле кино\-кадров.

![The Joker in the style of Greg Capullo with ink effect](picture/6bfbca6365c92eead3278523dcf0fda7.png "The Joker in the style of Greg Capullo with ink effect")

The Joker in the style of Greg Capullo with ink effect

Так и ультра высокое, например в **704×1408**, в обоих случаях результат отличный, так что выбирайте размер под ваши задачи.

![The Joker in the style of Greg Capullo with ink effect](picture/6a0486ce0bdf8bce4a08775ed653b1d0.png "The Joker in the style of Greg Capullo with ink effect")

The Joker in the style of Greg Capullo with ink effect

**Image Number** \- позволяет задать количество изображений которые нужно сгенерировать, по умолчанию **2**, но вы можете указать вплоть до **32** изображений, но конечно это займет длительное время.

**Negative Prompt** \- негативная подсказка позволяет указать то, чего на изображении быть не должно.

**Seed** \- все изображения создаются из белого шума, как помехи в телевизоре, **Seed** и есть ид конкретного уникального шума, по умолчанию стоит галочка **Random**, задавая случайный шум для каждой генерации, но если вы её снимите, то увидите ид по которому была создана текущая картинка. Использовать один и тот же **Seed** бывает полезно если вы экспериментируете с запросом, или проверяете как работают разные лоры, или просто хотите воспроизвести то изображение, которое уже создавали ранее.

**History Log** \- содержит информацию обо всем, что вы ранее создавали, тут как раз можно увидеть **Seed** для каждого изображения, запрос и другие настройки. В отличии от **Automatic 1111**, **ComfyUI** и прочих Фокус не хранит информацию о генерации внутри самого изображения, а значит вы не сможете воспроизвести информацию о генерации через **png info**. Сохраняйте лог генераций или промпты отдельно. А мы переходим на следующую вкладку.

#### Раздел Style

![Owl with glasses in a library with book effect in a scholarly style](picture/2edecc3499b9ff06f678e056657ebb1f.png "Owl with glasses in a library with book effect in a scholarly style")

Owl with glasses in a library with book effect in a scholarly style

По умолчанию всегда включено несколько стилей, **Fooocus V2**, это тот самый стиль который активирует **GPT** модель улучшающую ваши запросы, имейте это ввиду, когда будете переключать стили. Стилей очень много, поэтому можно воспользоваться поиском. Для примера я выключу два стиля следующие за **Fooocus V2**, и вместо них включу **Steampunk 2** и **SAI Fantasy Art**, не изменяя промпт и даже **Seed**. И получаю отличную фентези сову.

![Owl with glasses in a library with book effect in a scholarly style](picture/fb1405801260485c322332b022ec1f26.png "Owl with glasses in a library with book effect in a scholarly style")

Owl with glasses in a library with book effect in a scholarly style

Или например мне нужна сова с книгами в Киберпанк стиле, для этого выключаете все стили и включаете **Game Cyberpunk Game.**

![Owl with glasses in a library with book effect in a scholarly style](picture/6ef910c4f38888718784d720fc7a1f8c.png "Owl with glasses in a library with book effect in a scholarly style")

Owl with glasses in a library with book effect in a scholarly style

А возможно вам нужная черно\-белая драматичная сова? Тоже не проблема, для примера ниже я выбрал стили **Photo Film Noir**, **Dark Fantasy**, **Dark Moody Atmosphere** и **SAI Line Art**. Мне результат очень нравится.

![Owl with glasses in a library with book effect in a scholarly style](picture/e25cb9ede189dcce2ab019030be82b15.png "Owl with glasses in a library with book effect in a scholarly style")

Owl with glasses in a library with book effect in a scholarly style

Экспериментируйте со стилями и комбинируйте их, в Фокусе работа со стилями улучшена по сравнению с **A1111** и другими, это позволяет применять одновременно 3\-5 стилей для получения отличного результата, а не парочку как в аналогах. А мы двигаемся в следующую вкладку.

#### Раздел Model

![Wonder Woman as a barista in a coffee shop with steam effect in a retro style, photographed by Annie Leibovitz](picture/356ed7772f3a483c39895ecda0416acc.png "Wonder Woman as a barista in a coffee shop with steam effect in a retro style, photographed by Annie Leibovitz")

Wonder Woman as a barista in a coffee shop with steam effect in a retro style, photographed by Annie Leibovitz

На вкладке **Model** можно переключить модель, выбрать рефайнер, или добавить дополнительные лоры. Сила лор может регулироваться от **\-2** до **2**, в большинстве случаев оптимально ставить **0\.5**, всего можно добавить до пяти лор.

Скачиваем лоры и модели с https://civitai.com, лоры кладем в папку *Fooocus\\models\\loras*. Модели кладем в папку *Fooocus\\models\\checkpoints.* Какие лоры могут вам понадобиться и зачем? Смотрите в моем [большом обзоре сервисных лор для SDXL](https://youtu.be/E4fd7OffQ_Q?si=UINEG0_Ws7l6Vfm7) на YouTube, я сравнил 12 самых популярных, рассказал что они делают и как их использовать.

Если у вас уже есть своя папка с моделями или лорами, например в **A1111**, то вы можете подключить её отредактировав пути до папки с моделями в файле *Fooocus\\config.txt,* кстати, там же в конфиге можно указать и настройки по умолчанию, с которыми будет запускаться Фокус. Используйте файл *config\_modification\_tutorial.txt* в качестве пособия по возможным настройкам, он лежит рядом.

#### Раздел Advanced

На вкладке **Advanced** находится всего пара настроек, первая **Sampling Sharpness** отвечает за добавочный шум при создании изображения, чем больше шума, тем больше деталей будет на вашем изображении, но избыток шума может привести к артефактам и замусоренности, это отлично видно на гифке ниже. Мне обычно нравится значение **5**\-**7**.

![Raccoon with a mask in a trash can with garbage effect in a mischievous style.](picture/d3eeba5bddccb50c9ae2c12b180782d2.gif "Raccoon with a mask in a trash can with garbage effect in a mischievous style.")

Raccoon with a mask in a trash can with garbage effect in a mischievous style.

**Guidance Scale** отвечает за то, насколько сильно нейросеть должна пытаться следовать запросу, высокое значение приведет к артефактом, а на низком все будет блеклое, смотрите рекомендуемое значение **CFG** в описании модели, или оставляйте по умолчанию.

**Developer Debug Mode** открывает меню для тонкой настройки, но настройки там настолько тонкие, что покрутить их и ничего не сломать, а сделать лучше у вас вряд ли получится, так что этот раздел исследовать не будем.

Друзья, поскольку количество медиа файлов в этом руководстве уже переваливает за 20, а для рассказа про оставшуюся галочку **Input Image** мне нужно еще как минимум столько же, я сделаю это в следующей публикации.

Из второй части вы узнаете как в Фокусе работают **вариации**, чтобы создать похожее изображение на то, что вы загружаете. Узнаете как работает качественное **увеличение** ваших изображений. Расскажу про местную вариацию **ControlNet** которая позволяет **скопировать** и **стиль** и **содержимое** с любого изображения добавив в вашу генерацию. И про местный **дипфейк**, который позволяет перенести ваше лицо на создаваемое изображение. И конечно же про **инпеинтинг** и **аутпеинтинг**, с помощью которого можно расширить или изменить любое изображение как в тех роликах с фотошопом, генеративной заливкой и мемами.

![close-up of baby Groot bye-bye hand shake in the space, surrounded with firefly and blue sparkles](picture/5bfb12309ec61a371276d3982f70a3a8.png "close-up of baby Groot bye-bye hand shake in the space, surrounded with firefly and blue sparkles")

close\-up of baby Groot bye\-bye hand shake in the space, surrounded with firefly and blue sparkles

А на сегодня у меня все, вы узнали про нейросеть **Fooocus**, которая создает изображения по текстовому запросу и научились в ней работать. Теперь вы знаете за что отвечает каждая из настроек и сможете осмысленно создавать красивый арт который пригодится в работе или учебе, и конечно, порадует друзей и близких. Генерация изображений с помощью нейросетей очень интересный и увлекательный процесс, делитесь своими работами в нашем [чате](https://t.me/neuroart0) с такими же увлеченными энтузиастами.

Я рассказываю больше про нейросети у себя на [YouTube](https://www.youtube.com/@nerual_dreming/), в [телеграм](https://t.me/neuro_art0), на [Бусти](https://boosty.to/neuro_art), буду рад вашей подписке и поддержке. До скорого.

