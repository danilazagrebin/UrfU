# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил:
- Шлыков Олег Петрович
- РИ212702
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.


## Цель работы
Познакомиться с работой перцептрона на практике

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления:
-OR
-AND
-NAND
-XOR

Ход работы:
- Создать проект и добавить скрипт-файл с перцептроном 
Для каждого логического оператора создаём массив ts и подбираем необходимое количество эпох обучения.

Для OR достаточно 4 эпох обучения, после чего Total Error = 0
 
![image](https://user-images.githubusercontent.com/114522298/204098084-edaf9cc7-3362-4d00-9bb3-21a6fb6bc96c.png)

Для AND достаточно 7 эпох обучения, после чего Total Error = 0

![image](https://user-images.githubusercontent.com/114522298/204098155-d88c15fc-5c44-44d9-8136-866a5f3a52cf.png)

Для NAND достаточно 7 эпох обучения, после чего Total Error = 0

![image](https://user-images.githubusercontent.com/114522298/204098199-b9278142-0919-436c-9e5a-6c99e5fa14c0.png)

Для XOR недостаточно 20 эпох обучения, т.к. перцептрон не способен решить эту задачу

## Выводы

В лабораторной работе познакомились с предком нейроной сети, - перцетроном и проверили обучение базовым логически оператором.
