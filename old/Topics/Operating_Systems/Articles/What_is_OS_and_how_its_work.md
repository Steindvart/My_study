# Что такое операционная система и как она работает?

> Основа конспекта, лекция - https://www.youtube.com/watch?v=hb9CTGSJm88&list=PLlb7e2G7aSpRgsZVTYYbpqiFrIcIpf8kp<br>
> Автор немного переработал и дополнил некоторые моменты: структуру и повествование, постарался дать более глубокое и последовательное разъясление, добавил как скаченные, так и созданные им лично схемы.

Цель конспекта — последовательно рассмотреть и объяснить принципы устройства и функционирования операционной системы, её основных компонентов и абстракций. 
<hr>
Операционные системы окружают нас повсюду – персональные компьютеры, серверы, мобильные устройства (телефон, планшет), сетевые устройства (роутеры, коммутаторы) и даже современные автомобили (борт-компьютер), телевизоры и прочее. Перечислять можно очень долго, ведь они требуются практически в каждой компьютерной системе.
<br>

## Введение <br>
Любой компьютер представляет собой связанную совокупность: **процессора, памяти и устройств ввода-вывода**.

<img src="..\img\basic_architecture.png" width="450px" hidth="450px">
<details> <summary>Более подробная структура ПК</summary>
    <img src="..\img\Motherboard_diagram_ru.jpg" width="575px" hidth="300px">
</details>

Сама по себе, аппаратура умеет делать только очень простые, базовые операции, и получается, что непосредственное *создание, выполнение и управление* сложными процессами (приложениями) на аппаратуре становится крайне неэффективным и неудобным.

Например, **процессор** может выполнять только три базовых типа инструкции: 

- *Чтение инструкций/данных из памяти (read)*
- *Выполнение интрукции (execute)*
- *Запись результата в память (write)*

Возникает вопрос — **Как заставить всё это слаженно и эффективно работать, сделав пользование компьютером удобным как для обычного человека, так и для прикладного программиста?**.

Чтобы ответить на него более полседовательно, заглянем немного туда, где всё начиналось.

### *Немного истории*
> На заре компьютерной эпохи, первые компьютеры представляли собой огромные блоки (занимавшие большие комнаты), в которых размещались основные его компоненты: **процессор, память и устройства ввода-вывода**. <br>
> Любое взаимодействие с компьютером подразумевало **ручное изменение или считывание** состояния памяти (непосредственно или с помощью перфокарт). <br>
> И всего можно было выделить два состояния, в котором, в реальном времени находится компьютерная система:
> - Ввод/Вывод
> - Вычисление
>
>
> **Важная идея!**<br>
> Так как **вычисления производятся быстрее**, чем непосредственный ввод-вывод данных, разработчикам пришла идея о том, что к ресурсам можно допускать не одного пользователя (**процесс**), а множество, предоставляя им способ независимо друг от друга загружать (ввод) и получать (вывод) данные, чтобы более эффективно использовать ресурсы компьютера и вычислительные модули не простаивали в ожидании.
>
> Идея *многопользовательского режима* в использовании ресурсов компьютера нашла свою реализацию в понятии **процесс**. То есть, **каждый процесс - это пользователь ресурсов компьютера**.<br>
> Этот принцип положило начало созданию такой системы, которую мы теперь называем *операционной* - программной системы, которая осуществляет доступ к ресурсам компьютера и, следовательно, **управляет процессами**.

> Далее, термины: *процесс, приложение* идут как синонимы термину *пользователь ресурсов*.

## Зачем нужна Операционная Система?<br>

Чтобы управлять доступом пользователей (процессов) к ресурсам компьютера и осуществлять другие *системные функции*, и были разработаны **операционные системы**.

***Фунции ОС:***
- **Совместное использование вычислительный ресурсов группой приложений — центральная функция ОС, которая является базой для разных системных архитектур**
- Абстракция оборудования для удобства и переносимости
    - то есть реализация единого интерфейса для разного, но схожего по функциям оборудования.
- Изоляция ошибок приложений друг от друга (и от ядра ОС)
- Переносимость данных между приложениями (процессами)
    - файлы и файловая система, Inter Process Communication (IPC)

***Основные абстракции ОС***
- Процессы и потоки
- Файлы и файловые системы
- Адресное пространство и память
- Сокеты, протоколы, устройства

## Положение ОС в уровневой иерархии организации компьютера

<img src="..\img\GeneralizedLayeredComputerStructure_OS.png" width="650px" hidth="1150px">

Современный компьютер можно представить в виде **иерархии уровней** (от двух и более), где на каждом уровне выделяются свои абстракции и набор возможных функций.

Операционная система является одним из таких уровней и представляет собой **интерфейс** ("прослойку") между пользователем ресурсов компьютера и самими ресурсами, обеспечивающий управление взаимодействием между *пользователь-ресурс*, так и взаимодействием *пользователь-пользователь* и *устройство-устройство*.

В целом, наиболее *общей схемой* это можно отобразить так:<br>
<img src="..\img\OS_monolit-OS_1.png" width="450px" hidth="150px">

## Как приложения взаимодействуют с ОС?
Взаимодействие процессов с ОС осуществляется с помощью **системных вызовов**.<br>
**Механизм системных вызовов** — это интерфейс, который предоставляет ядро ОС (**kernel space**) пользовательским процессам (**user space**).<br>
**Системный вызов** – обращение *пользовательского* процесса к *ядру* операционной системы для выполнения какой-либо операции.

> **Интерфейс** — способ взаимодействия; некий набор правил и средств взаимодействия двух систем.<br>
> **Kernel space** — адресное пространство ядра ОС, в котором процессы имеют привилегированный доступ к ресурсам компьютера и другим процессам.<br>
> **User space** — адресное пространство, отведённое для пользовательских процессов (программ).

Например, чтобы выполнить обычное действие, с точки зрения прикладного программиста, – вывод строки в консоль, необходимо загрузить исполнимый код в память компьютера и передать его процессору. С помощью *системных вызовов*, всё это делает **запускающий** процесс (уже запущенный процесс, из которого вызывается новый процесс; одни процессы порождают другие) и передаёт управление. 

То есть с помощью **системных вызовов** выполняются те рутинные действия, которые раньше осуществлялись вручную, — загрузка кода программы в память, передача его на исполнение процессору и прочее.

*Схема организации ОС расширяется добавлением интерфейса для взаимодействия приложений с ядром ОС - **механизмом системных вызовов**:*<br>
<img src="..\img\OS_monolit-OS_2.png" width="450px" hidth="150px">

## Как оборудование (hardware) взаимодействует с ОС?

Одной из функций ОС является — **абстрагирование оборудования**.

*Что это значит?*<br>
У каждого оборудования есть свой фиксированный интерфейс. Например, операции с флешкой, жестким диском и сетевой платой будут похожи по своему типу - "записать/считать данные". Но у каждого устройства для этого, тем не менее, будет свой интерфейс.

ОС должка выполнять одни и те же операции над разными типами устройств. И чтобы она выполняла их *однообразно* — нужно чтобы был **общий интерфейс**. Реализацией этого *общего интерфейса* занимаются специальные программы - **драйверы устройств**. То есть, ОС обращается к *драйверам устройств* используя однотипные команды *"отправить команду/считать/записать"*, а *драйвера* уже превращает эти команды в то, что понимает конкретное устройство.

*Схема организации ОС расширяется добавлением интерфейса взаимодействия ОС и оборудования - **специальные программы "драйвера"**::*<br>
<img src="..\img\OS_monolit-OS_3.png" width="450px" hidth="150px">

## Что делает ядро ОС?

**Ядро ОС** – центральная часть операционной системы. Причём, это **реакционный механизм**, то есть его работа заключается исключительно в *реакции* на какие-либо события для их последующей обработки.

*Что именно?*
- Обрабывает запросы приложений
    - *системные вызовы*
- Обрабывает запросы оборудования
    - *прерывания*
- Обеспечивает диспетчеризацию процессов (scheduling)
    - реализация *многопользовательского режима* доступа к ресурсам
        - **время работы процессора делится на фрагменты и они распределяются по процессам**
- Обрабатывает исключительные ситуации

## Сервисы ОС

Функции ОС заключены в её сервисах (модулях). Реализация организации которых зависит от **архитектуры ядра**. Рассмотрим на примере [**монолитного ядра**](https://ru.wikipedia.org/wiki/%D0%9C%D0%BE%D0%BD%D0%BE%D0%BB%D0%B8%D1%82%D0%BD%D0%BE%D0%B5_%D1%8F%D0%B4%D1%80%D0%BE):

<img src="..\img\OS_monolit-All.png" width="450px" hidth="150px">

- ### Управление процессами (Process scheduler)
    - Запуск (помещение на процессор, выделение процессорного времени)
    - "Заморозка"
    - Остановка
    - Изменение приоритета
- ### Управление памятью (Memory manager)
    - Создание иллюзии уникальности адресного пространства для каждого процесса
    - Механизм **виртуальной памяти**
- ### Межпроцессное взаимодействие (IPC)
    - Общая память для нескольких процессов
    - Способы обмена данными через те или иные механизмы (file, pipe)
    - Механизмы предотвращения коллизий и синхронизации (семафоры, мьютексы)
- ### Файловая система (File system)
- ### Модель безопасности
    - Пользователи ('юзеры') и их группы
    - Права доступа
- ### Разное
    - Интерфейс ввода-вывода (I/O Interface)
    - Сетевой интерфейс (Network Interface)

## Основные абстракции ОС
### Процессы и потоки