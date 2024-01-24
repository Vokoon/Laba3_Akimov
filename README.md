# Автоматическая генерация музыкальных композиций с помощью дообучения MuseGAN

MuseGAN — проект по созданию музыки. Короче говоря, мы стремимся создавать полифоническую музыку из нескольких треков (инструментов). Предлагаемые модели способны генерировать музыку как с нуля, так и аккомпанируя априори заданному пользователем треку.

Мы обучем модель с помощью данных, собранных из   Lakh Pianoroll, для генерации  поп-песен, состоящих из треков баса, ударных, гитары, фортепиано и струнных.


## Условия

### Установить зависимости

- Использование  pipenv (рекомендуется)

  ```sh
  # Установите зависимости
  pipenv install
  # Активируйте виртуальную среду
  pipenv shell
  ```

  ```sh
  # Install the dependencies
  pip install -r requirements.txt
  ```

### Подготовьте данные для самостоятельного до обучения


Данные для обучения собираются из набора данных Lakh Pianoroll (LPD), нового многодорожечного набора данных для фортепиано.

```sh
# Загрузите данные обучения
./scripts/download_data.sh
# Сохраняйте данные обучения в общей памяти.
./scripts/process_data.sh
```

Вы также можете загрузить данные обучения вручную
([train_x_lpd_5_phr.npz](https://docs.google.com/uc?export=download&id=14rrC5bSQkB9VYWrvt2IhsCjOKYrguk3S)).

## Обучите новую модель

1. Запустите следующую команду, чтобы настроить новый эксперимент с настройками по умолчанию.

   ```sh
   # Настройте новый эксперимент
   ./scripts/setup_exp.sh "./exp/my_experiment/" "Some notes on my experiment"
   ```

2. Измените файлы конфигурации и параметров модели для  настроек.

3. Вы можете обучить модель:

     ```sh
     # Обучим модель
     ./scripts/run_train.sh "./exp/my_experiment/" "0"
     ```

### Собирайте данные обучения

Запустите следующую команду, чтобы собрать обучающие данные из MIDI-файлов.

  ```sh
  # Собирайте данные обучения
  ./scripts/collect_data.sh "./midi_dir/" "data/train.npy"
  ```

### Используйте предварительно обученные модели  

1. Загрузите предварительно обученные модели

   ```sh
   # Загрузите предварительно обученные модели
   ./scripts/download_models.sh
   ```

2. Вы можете выполнить вывод из обученной модели:

   ```sh
   # Выполните вывод из предварительно обученной модели
   ./scripts/run_inference.sh "./exp/default/" "0"
   ```

## Выводы

образцы будут создаваться одновременно с обучением. Вы можете отключить это поведение, установив save_samples_stepsнулевое значение в файле конфигурации ( config.yaml). По умолчанию сгенерированные данные будут храниться в следующих трех форматах.

- `.npy`: необработанные массивы numpy
- `.png`: файлы изображений
- `.npz`: многодорожечные файлы фортепианороллов

Вы можете отключить сохранение в определенном формате , установив save_image_samples и save_pianoroll_samplesв False файле конфигурации.

Созданные фортепианные ролики сохраняются в формате .npz для экономии места и времени обработки. Вы можете использовать следующий код для записи их в MIDI-файлы.

```python
from pypianoroll import Multitrack

m = Multitrack('./test.npz')
m.write('./test.mid')
```

## Примеры результатов


Some sample results can be found in `./exp/` directory. More samples can be
downloaded from the following links.

- [`sample_results.tar.gz`](https://docs.google.com/uc?export=download&id=1BsNtc8_mpLK5l2F5jncIkHbTcJqtZu2w) (54.7 MB):
  sample inference and interpolation results
- [`training_samples.tar.gz`](https://docs.google.com/uc?export=download&id=1pZk0YCElcHHSBfhbV8j_zaRr1zhEQUzN) (18.7 MB):
  sample generated results at different steps
