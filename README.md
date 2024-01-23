
# Использование глубокого обучения для создания инструмента автоматической генерации музыкальных композиций.

![bender](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/fd496baf-9c29-4a28-95c6-9878866a2b9e)


Автоматическое создание музыки по заданным параметрам.
Для начала откроем вебсайт:

https://surikov.github.io/rockdice/main.html .

в котором реализованы большая часть инструментов для создание музыки, дальнейшии инструкции будут по работе в этом вебприложении.

---

Содержание:

- [Описание работы](#Описание%20работы)
  - [Настройки](#Настройки)
  - [История](#История)
- [Описание реализации](#Описание%20реализации)
  - [Воспроизведение звука](#Воспроизведение%20звука)
  - [Модуляция фрагментов](#Модуляция%20фрагментов)
  - [Специфика инструментов](#Специфика%20инструментов)

# Описание работы

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/027520ce-06f2-45ea-abac-f19e590b04ac)


В основном окне можно слайдером выбрать прогрессию. Последовательность аккордов определяет настроение мелодии (слева минорные, справа мажорные, посередине джазовые септаккорды).
Круглыми переключателями можно выбрать риффы для стандартных 4-х слоёв мелодии
- 1 - задает ритм
- 2 - гармоническая основа и ритм
- 3 - создаёт мелодический рисунок
- 4 - контрапункт, протяжные ноты показывающие функцию аккорда.

Можете немного по эксперементировать и послушать что полчучилось прежде чем продвигатся дальше.

По нажатию на ![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/189fea1d-a676-4fba-968f-7ac30f9df5d6) выбираются случайные инструменты.


## Настройки

По кнопке с ![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/d80785f3-e405-45f5-acbb-53e389101437) открывается окно настроек.
Можно редактировать громкость по слоям, скорость, менять аккорды:

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/013b2923-a28c-47bb-9841-b20b4f9e602c)


Для каждого трека можно выбрать рифф из списка:

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/2618be75-69d0-4be6-8713-8c4e0dd06f3d)


Также можно выбрать аккордовую последовательность:

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/287b9a8f-bafb-421b-9ddc-c76d0263e037)


## История
Что делать, если вы создали отличную мелодию но потом куда то нажали несколько раз, а вы не запомнили инструменты которые были использованы. В этом случаи вам поможет История по кнопке с иконкой Undo (справа вверху) открывается отдельное окно с вашей историей подбора мелодий и даже если вы закроете сайт, история не очистится и останется.

![image](https://github.com/Vokoon/Laba3_Akimov/assets/120046709/b12f4a32-3873-4c4e-a485-359162e76861)


# Описание реализации

## Воспроизведение звука

Для воспроизведения звука используется библиотека WebAudioFont: В библиотеке содержатся более 2000 оцифрованных инструментов и основные фильтры для обработки звука

## Модуляция фрагментов

Для выбранных (или введённых вручную) аккордов в функции extractMode вычисляется тоника и лад, см.

https://github.com/Vokoon/Laba3_Akimov/blob/main/zvoogharmonizer

## Специфика инструментов

При воспроизведении музыкальныз фрагментов учитывается специфика исполнения на конкретных инструментах. Наример:

- при игре на гитаре с дисторшном обычно используют как игру на открытых струнах, так и игру на прижатых струнах (Palm Mute). Т.е. инструемент один, но для реалистичного звучания необходимо два отдельных набора сэмплов [для открытых](https://surikov.github.io/webaudiofontdata/sound/0300_LesPaul_sf2.html) и [приглушенных](https://surikov.github.io/webaudiofontdata/sound/0290_LesPaul_sf2.html) струн
- на аккустической гитаре удары по струнам вниз и вверх ощутимо различаются, см. [пример](https://surikov.github.io/webaudiofont/examples/strum.html)
- наборы нот похожих аккордов (например G и Gm) могут зажиматься на совершенно разных ладах, это нужно учитывать при модуляции фрагментов
- и т.п.

В результате получается сделать звучание сгенерированной музыки менее однообразным.
Вот что получилось у меня : https://mzxbox.ru/RockDice/share.php?seed=%7B"drumsSeed"%3A9%2C"bassSeed"%3A12%2C"leadSeed"%3A34%2C"padSeed"%3A9%2C"drumsVolume"%3A111%2C"bassVolume"%3A99%2C"leadVolume"%3A66%2C"padVolume"%3A77%2C"chords"%3A%5B"Cm"%2C"2%2F1"%2C"Ebm"%2C"2%2F1"%5D%2C"tempo"%3A130%2C"mode"%3A"Ionian"%2C"tone"%3A"D%23"%2C"version"%3A"v2.89"%2C"comment"%3A""%2C"ui"%3A"web"%7D

## Документация:https://surikov.github.io/webaudiofont/npm/src/docs/index.html
