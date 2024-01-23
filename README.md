# rockdice

- Start App launcher - https://surikov.github.io/rockdice/main.html
- Source - https://github.com/surikov/rockdice

# Автогенерация музыки без AI

![bender](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/fd496baf-9c29-4a28-95c6-9878866a2b9e)


Автоматическое создание музыки по заданным параметрам.

- Открыть в браузере - https://surikov.github.io/rockdice/main.html
- Исходники - https://github.com/surikov/rockdice
- Андроидная версия - https://play.google.com/store/apps/details?id=rockdice.chord.progression

Используется Web Audio API, TypeScript, Android WebView, таблично-волновой синтез и элементарная теория музыки.

---

Содержание:

- [Описание работы](#Описание%20работы)
  - [Похожие проекты](#Похожие%20проекты)
  - [Настройки](#Настройки)
  - [История](#История)
  - [Публикация и экспорт](#Публикация%20и%20экспорт)
- [Описание реализации](#Описание%20реализации)
  - [Воспроизведение звука](#Воспроизведение%20звука)
  - [Модуляция фрагментов](#Модуляция%20фрагментов)
  - [Специфика инструментов](#Специфика%20инструментов)
  - [Хранение состояний](#Хранение%20состояний)
  - [Разметка ссылок](#Разметка%20ссылок)
  - [Android](#Android)

# Описание работы

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/027520ce-06f2-45ea-abac-f19e590b04ac)


В основном окне можно слайдером выбрать прогрессию. Последовательность аккордов определяет настроение мелодии (слева минорные, справа мажорные, посередине джазовые септаккорды).
Круглыми переключателями можно выбрать риффы для стандартных 4-х слоёв мелодии
- Drums - задает ритм
- Bass - гармоническая основа и ритм
- Lead - создаёт мелодический рисунок
- Pad - контрапункт, протяжные ноты показывающие функцию аккорда

По голубой кнопке с игральным кубиком выбираются рандомные значения.

Приложение делает модуляцию выбранных фрагментов под заданные аккорды. Если покрутить переключатели и прослушать несколько мелодий, можно убедиться что при любом сочетании аккордов и инструментов звучание результата получается вполне "человеческое".

## Похожие проекты

Встроенные функции автогенерации аранжировок появились ещё в первых синтезаторах. Пример исполнения на Cassio MT-100 из 80-х прошлого века: 

https://www.youtube.com/watch?v=Aepm6V4yvhw

Современное приложение Captain Plugins Epic, подойдёт только профессионалам: 

https://www.youtube.com/watch?v=ZuB_t1DBwp8

Web-приложение BandLab SongStarter, никаких настроек не поддерживает, просто генерирует мелодии в одном стиле: 

https://www.youtube.com/watch?v=EDRPy8KtY0c

## Настройки

По кнопке с шестерёнкой открывается окно настроек.
Можно редактировать громкость по слоям, скорость, менять аккорды, транспонировать:

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/013b2923-a28c-47bb-9841-b20b4f9e602c)


Для каждого трека можно выбрать рифф из списка:

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/2618be75-69d0-4be6-8713-8c4e0dd06f3d)


Также можно выбрать аккордовую последовательность:

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/287b9a8f-bafb-421b-9ddc-c76d0263e037)


## История

Возможность отменить прошлое действие и вернуться в предидущее состояние это типовое требование для любых приложений.
По кнопке с иконкой Undo (справа вверху) открывается окно с историей подбора мелодий:

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/a0c72210-9fcb-4b32-82c4-bd9f2a8173c7)


## Публикация и экспорт

Как и любое музыкальное приложение, RockDice позволяет экспортировать созданную мелодию в MIDI-файл или Wav-файл (кнопки в нижней панели).
Полученные файлы можно редактировать в других музыкальных приложениях.
Пример треков импортированных в BandLab MixEditor (любой трек можно форкнуть и открыть в редакторе):

https://www.bandlab.com/sss1024/tracks

![bandlabtracks.png](img/bandlabtracks.png)

### Совместная работа

Ссылка на мелодию открывается в приложении точно в том виде как её отправил автор.

По кнопке со стандартной иконкой Share открывается окно предпросмотра. В нём
можно отослать ссылку на мелодию через e-mail, мессенджер или соц. сети:

![rockdiceshare.png](img/rockdiceshare.png)

В большинстве соц. сетей ссылка корректно распознаётся с описанием и изображением выбранных инструментов:

![rockdicevk.png](img/rockdicevk.png)

# Описание реализации

## Воспроизведение звука

Для воспроизведения звука используется библиотека WebAudioFont:

https://github.com/surikov/webaudiofont

В библиотеке содержатся более 2000 оцифрованных инструментов и основные фильтры для обработки звука:

- 10-полосный эквалайзер для настройки тембра
- эхо для объёмного звучания
- динамический компрессор для выделения голосов

## Модуляция фрагментов

Для выбранных (или введённых вручную) аккордов в функции extractMode вычисляется тоника и лад, см.

https://github.com/surikov/rockdice/blob/main/ts/code/zvoogharmonizer.ts#L1156

Для воспроизведения музыки в проект добавлено около двухсот риффов с разными инструментами, их можно посмотреть в папке

https://github.com/surikov/rockdice/tree/main/patterns

Каждый паттерн кроме нот также содержит описание лада и тоники.

Выбранные риффы в функции  morphSchedule транспонируются в тонику выбранных аккордов и проводится модуляция в лад аккордовой прогрессии, см.

https://github.com/surikov/rockdice/blob/main/ts/code/zvoogharmonizer.ts#L377

- Транспонирование мелодии это перенос всех нот на равное количество полутонов. По-простому - сделать звук выше или ниже.
- Модуляция мелодии это сдвиг определённых ступеней лада. По-простому - из минора в мажор и т.п.

## Специфика инструментов

При воспроизведении музыкальныз фрагментов учитывается специфика исполнения на конкретных инструментах. Наример:

- при игре на гитаре с дисторшном обычно используют как игру на открытых струнах, так и игру на прижатых струнах (Palm Mute). Т.е. инструемент один, но для реалистичного звучания необходимо два отдельных набора сэмплов [для открытых](https://surikov.github.io/webaudiofontdata/sound/0300_LesPaul_sf2.html) и [приглушенных](https://surikov.github.io/webaudiofontdata/sound/0290_LesPaul_sf2.html) струн
- на аккустической гитаре удары по струнам вниз и вверх ощутимо различаются, см. [пример](https://surikov.github.io/webaudiofont/examples/strum.html)
- наборы нот похожих аккордов (например G и Gm) могут зажиматься на совершенно разных ладах, это нужно учитывать при модуляции фрагментов
- и т.п.

В результате получается сделать звучание сгенерированной музыки менее однообразным.

## Документация:https://surikov.github.io/webaudiofont/npm/src/docs/index.html
