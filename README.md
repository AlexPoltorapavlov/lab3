# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
- Полторапавлов Александр Алексеевич
- 2093937
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity

## Задание 1
### Создать ML-агент и тренировать нейросеть, задача которой будет заключаться в управлении шаром. Задача шара заключается в том, чтобы оставаясь на плоскости находить кубик, смещающийся в заданном случайном диапазоне координат.

Ход работы:
- Создали новый пустой 3D проект на Unity.
- Скачали папку с ML агентом.
- В созданный проект добавили ML Agent, выбрав Window - Package Manager - Add Package from disk. Последовательно добавили .json – файлы:
    o ml-agents-release_19 / com,unity.ml-agents / package.json
    o ml-agents-release_19 / com,unity.ml-agents.extensions / package.json
Во вкладке с компонентами (Components) внутри Unity появилась строка ML Agent. 
- Запустили Anaconda Prompt для возможности запуска команд через консоль.
- Прописали среию команд для создания и активации нового ML- агента, а также для скачивания необходимых библиотек:
    o mlagents 0.28.0;
    o torch 1.7.1;
- Создали на сцене плоскость, куб и сферу так, как показано в методичке. Создали простой C# скрипт-файл и подключили его к сфере. Объекту «сфера» добавили компоненты Rigidbody, Decision Requester, Behavior Parameters и настроили их согласно методичке.
- В корень проекта добавили файл конфигурации нейронной сети, доступный в папке с файлами проекта по ссылке из методички.
- Запустили работу ml-агента в консоли:
- Запустили сцену в проекте Unity, так там работал ML-Agent:
![image](https://github.com/AlexPoltorapavlov/lab3/blob/main/GIFs/1_Ball.gif)
https://github.com/AlexPoltorapavlov/lab3/blob/main/GIFs/1_Ball.gif
- Сделал 3, 9, 27 копий модели «Плоскость-Сфера-Куб», запустил симуляцию сцены и наблюдал за результатом обучения модели и замедлениями работы компьютера:
![image](https://github.com/AlexPoltorapavlov/lab3/blob/main/GIFs/9_balls.gif)
https://github.com/AlexPoltorapavlov/lab3/blob/main/GIFs/9_balls.gif

![image](https://github.com/AlexPoltorapavlov/lab3/blob/main/GIFs/9_balls.gif)
https://github.com/AlexPoltorapavlov/lab3/blob/main/GIFs/9_balls.gif

### Вывод: 
Научился учить шар преследовать объекты. Изучил код C#, изучил yaml файл. Возможно из-за ограниченных ресурсов компьютера, но особой разницы между скоростью обучения нейронной сети при одном, трех, девяти и двадцати семи полях я не увидел. Чем больше было полей, тем медленнее работал Unity. Так как все делалол строго по методичке, никакого кода не прикреплял. Нет смысла, ведь код не менялся

## Задание 2
### Подробно описать каждую строку файла конфигурации нейронной сети, доступного в папке с файлами проекта по ссылке из методички. Самостоятельно найти информацию о компонентах Decision Requester, Behavior Parameters, добавленных на сфере.

Описание файла конфигурации представлено ниже в виде кода. Информацию искал в официальной докуентации и сторонних ресурсах:

```yaml
behaviors: # К какому компоненту обращаемся.
  RollerBall: # имя компонента.
    trainer_type: ppo # тип тренера. ppo означает proximal policy optomozation. Есть еще SAC.
    # гиперпараметры, общие для и ppo, и sac.
    hyperparameters:  
      batch_size: 10 # кол-во опыта(предпринятых действий, вознаграждений и тд), передаваемых на каждой итерации градиентного спуска.
      buffer_size: 100 # размер буффера.
      learning_rate: 3.0e-4 # коэффициент скорости обучения, позволяет управлять величиной коррекции весов на каждой итерации.
      learning_rate_schedule: linear # режим обучения.
    # Специфические гиперпараметры конкретно для ppo.
      beta: 5.0e-4 # коеффициент энтропии.
      epsilon: 0.2 # ограничение.
      lambd: 0.99 # параметр GAE (автокодера графов). В диапазоне от 0.9 до 1.
      num_epoch: 3 # количество эпох, на сколько я понял, полных циклов обработки обучающих данных.
    # настройка нейронной сети
    network_settings: 
      normalize: false # следует ли нормализировать преимущество.
      hidden_units: 128 # Количество units в скрытых слоях нейронной сети. Чем сложнее взаимодействие, тем больше это кол-во.
      num_layers: 2 # Количество скрытый слоев в нейронной сети.
    reward_signals: # определение сигналов вознаграждения.
      extrinsic: # внешний сигнал вознаграждения. В данном случае вознаграждение от среды игнорируется.
        gamma: 0.99 # чем больше значение, тембольше агента волнует возможное вознаграждение в отдаленном будущем.
        strength: 1.0 # на что можно умножить необработанное вознаграждение. От 0 до 1.
    max_steps: 500000 # кол-во шагов моделирования, умноженных на частоту кадров, выполняемых в процессе обучения. Чем сложнее задача, тем больше должно быть число.
    time_horizon: 64 # как много шагов опыта нужно собрать прежде, чем добавлять его в буффер опыта. Должно таким, чтобы охватывать все важное поведение агента в последовательности его действий.
    summary_freq: 10000 # как часто в шаге сохранять статистику тренировок.
```
DecisionRequester - автоматически запрашивает решения для экземпляра агента через регулярные промежутки времени.
Behavior Parameters - компонент для настройки поведения экземпляра агента и свойств мозга.

## Выводы:
Научился основам работ с ml-agent`ами, решил множество проблем, чтобы проект заработал, ознакомился с официальной документацией, онакомился с гиперпараметрами, настройками нейронных сетей и многое другое. Для бОльшего результата следует подробно изучить документацию по ml-agents и попробовать сделать что-то более сложное, чем преследование шариком квадрата. 

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
