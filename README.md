# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
- Шлыков Олег Петрович
- РИ212702
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

## Цель работы
Узнать об основах машинного обучения и обучить простейшую нейронную сеть.

## Задание 1
### Реализовать систему машинного обучения
Ход работы:
- Создать новый проект на Unity, добавить в него MlAgenta, после чего настроить его через консоль.
- Создать и настроить объекты на сцене в Unity
- Создать скрипт для Roller Agent'а:

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8 - 4, 0.5f, Random.value * 8 - 4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if (distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}

```

- Создать конфигурационный файл нейросети и провести обучение

```c#
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
    
```
- Применить полученную сеть для проверки результата


https://user-images.githubusercontent.com/114469030/197031643-584d8c0c-db01-4ce0-87d1-1aea6db34705.mov



## Задание 2
### Подробно опишите каждую строку файла конфигурации нейронной сети. Самостоятельно найдите информацию о компонентах Decision Requester, Behavior Parameters, добавленных на сфере.

```c#
behaviors:
  RollerBall:   //Название объекта
    trainer_type: ppo   //Тип тренировки
    hyperparameters:
      batch_size: 10    //Кол-во опытов в одной иттреации
      buffer_size: 100  //Кол-во опытов, которые необходимо собрать перед обновлением модели
      learning_rate: 3.0e-4 //Начальная скорость обучения
      beta: 5.0e-4  //Hегуляризации энтропии, которая делает политику более случайной
      epsilon: 0.2  //Cкорость изменения политики
      lambd: 0.99   //Параметр регуляризации используемый при расчете обобщенной оценки преимущества
      num_epoch: 3  //Уменьшение этого параметра обеспечит более стабильные обновления за счет более медленного обучения
      learning_rate_schedule: linear    //Определяет, как скорость обучения изменяется с течением времени
    network_settings:  //Настройки сети
      normalize: false  //Нормализация полезна в случаях со сложными задачами непрерывного управления, но вредна для более простых задач дискретного управления
      hidden_units: 128 //Соответствуют количеству единиц в каждом полносвязном слое нейронной сети
      num_layers: 2 //Количество скрытых слоев в нейронной сети
    reward_signals:    
      extrinsic:
        gamma: 0.99 //Фактор скидки для будущих вознаграждений, поступающих из окружающей среды
        strength: 1.0   //Коэффициент, на который умножается вознаграждение, данное средой
    max_steps: 500000   //Общее количество шагов которые необходимо выполнить в среде перед завершением процесса обучения
    time_horizon: 64    //Сколько шагов опыта необходимо собрать для каждого агента, прежде чем добавить его в буфер опыта
    summary_freq: 10000 //Количество опытов, которое необходимо собрать перед созданием и отображением статистики обучения
    
```
Decision Requester - Данный компонент автоматически запрашивает решения для экземпляра агента через регулярные промежутки времени.
Behavior Parameters - Компонент для настройки поведения экземпляра агента и свойств мозга ИИ

## Выводы

Системы машинного обучения позвоялют сделать игру более интересной для игроков. В частности это позволяет сделать перестрелки с противниками под управлением ИИ более зрелещными и интересными для игроков. Так же машинное обучение позволяет регулировать сложность противников, и если для игроков  становится сложно играть против ИИ, то можно изменить некоторые настройки нейросети, что позволит сделать виртуальных противников слабее.
