.. include:: ../../defs.rst

JMM
********************************
JMM устанавливает правила использования потоками разделяемой памяти

.. raw:: html

    <iframe width="640" height="360" frameborder="0" src="https://mega.nz/embed/bNkjTDRB#_eYTedHnq4NaAfm3hl1hb0jMSG_I9uwRJ5Z4U4ydlvE" allowfullscreen ></iframe>


.. raw:: html

    <iframe width="640" height="360" frameborder="0" src="https://mega.nz/embed/7V1n2bzL#byi2Y8w1RLtG3VRTqk2KZazuUsoxdZKLGR4ey02Df9Y" allowfullscreen ></iframe>


|br|

Модели памяти процессоров
===========================
Сильная модель 
    Все видят одинаковые значения.

Слабая модель 
    Предпологает наличие барьеров памяти.

    1. Flush - Инструкция на сброс данных (отчистка кэша (сброс) в основную память).
    2. Инвалидация - объявление данных в кэше недействительными (значения впредь берутся из основной памяти).


Критическая секция
==================
Критическая секция - участок кода, где используется доступ к общему ресурсу.

Не должно в критической секции быть более одного потока в один момент времени (~ или же могут быть несколько читателей).

Решает проблемы Race Condition.


Cache coherence protocol 
========================
The protocol CPUs use to transfer cache lines between main memory and other CPUs



Правила конструирования объектов
================================
Если объект правильно построен (без утечек ссылок в конструкторе объекта и без исключений) - все потоки будут видеть корректное состояние его final полей

*  Объекты, достижимые через данные (инициализируемые) final поля так же будут гарантированно корректно отображаться для всех потоков.
   (например присвоение final полю ссылки на массив - все элементы массива будут независимо от потока одинаково видны без дополнительной синхронизации).

   Но если после проставления в final поле ссылки - объект по этой ссылке будет изменен - нет гарантий на корректное отображение.
   Он может быть изменен даже в том же конструкторе: 

   .. code-block:: java

        data = new int[]{1,2}; data[1] = 10; 
        // после правильного конструирования data != null, 
        // НО (в других потоках) data[1] может быть == 2
  

*  Любое поле (в том числе final) может быть некорректно отображаемое, при публикации ссылки (**досрочная публикация**) на this в конструкторе (в т.ч. запись его в static поля), даже в текущем потоке, из-за перестановки.
        
   Только после завершения конструктора можно гарантировать видимость проставленного в final поле значения
   Любой анонимный объект созднный в конструкторе имеет неявную ссылку на текущий, оборачивающий объект.
   Так же при запуске потока из конструктора присутствует **досрочная публикация**.


*  Сначала происходит построение объекта - потом присваивание ссылки на него.

Все значения, выставленне при объявлении статических полей или статическом инициализаторе выдны любому потоку, получившему доступ к этому классу, без какой либо синхронизации.


volatile
========
volatile - используется для данных, которые могут храниться только в heap (~~~)

*  Обращение не в кэш процессора за значением, а напрямую в память (visibility).

   Кэш хранит копию часто используемых данных из RAM, обычно не больших блоков. Могут быть как из heap, так и из стека.

   cashline - то что считывается в кэш при доступе к определенному участку памяти.


*  Атомарные операции чтения и записи для long и double (на 32 разрядных процессорах).


*  Переупорядочивание volatile переменных не происходит. 

   Volatile переменные являются memory барьерами, в отношении любой операции чтения, произошедшей после чтения volaile, и любой операции записи, до записи в volaile переменную.

   .. code-block:: java

        a = 1; volatile = 2; read(volatile); read(a) 

   Гарантируется Program Order: :math:`setA[X] \Rightarrow setVolatile[X] \Rightarrow showVolatile[X] \Rightarrow showA[X]`



Правила межпоточного взаимодействия. Happens-before
====================================================

.. raw:: html

    <iframe width="560" height="315" src="https://www.youtube.com/embed/puYPHysBN7U?start=6581" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

|br|

* Отношение Happens-before (->): |br|
  :math:`Operation1[Thread1] \rightarrow Operation2[Thread2]`

  Означает, что все изменения, выполненные Потоком1 до момента Операции1, а также изменения, которые повлекла эта операция - будут видны Потоку2 на Операции2 и после.
  Happens-before обеспечивает Flush кэша Потока1 по достижению Операции2


* Happens-before обладает свойством транзитивности: |br|
  :math:`A[X] \rightarrow B[Y] \rightarrow C[Z] \sim A[X] \rightarrow C[Z]`


* Отношение Program Order (=>): |br|
  :math:`A[X] \rightarrow B[X] \sim A[X] \Rightarrow B[X]`

  Описывает порядок Program Order, который определяет отношение Happens-before только на явных зависимых элементах, которые нальзя переставить друг с другом:


* Race Condition: |br|
  :math:`SetVal[X] \rightarrow \begin{Bmatrix} SetVal[Y] \Rightarrow ShowVal[Y] \\ SetVal[Z] \Rightarrow ShowVal[Z] \end{Bmatrix}`


Освобождение монитора
---------------------
(Освобождение монитора) hb (каждого последующего заполучения того же самого монитора)

.. math:: (EnterMonitor[X] \Rightarrow SetVal(myVal)[X] \Rightarrow ExitMonitor[X]) \rightarrow  ... \\ ... \rightarrow  (EnterMonitor[Y] \Rightarrow ShowVal[Y] \Rightarrow ExitMonitor[Y])
.. centered:: *Выведет myVal, при том, что поле может быть не volatile!, так как на каждом hb выполняется flush (~~~)*

|br|

.. math:: SetVal(myVal)[X] \Rightarrow (EnterMonitor[X] \Rightarrow ExitMonitor[X]) \rightarrow ... \\ ... \rightarrow  (EnterMonitor[Y] \Rightarrow ExitMonitor[Y]) \Rightarrow ShowVal[Y]
.. centered:: *Выведет myVal*

Все что случилось до hb и после (до выхода из монитора и после входа) - может восприниматься, как операции, происходящие в одном потоке с точки зрения JMM

.. math:: lock.unlock[X] \rightarrow  \begin{bmatrix} lock.lock[Y] \\ if \, lock.tryLock[Y] \end{bmatrix}
.. centered:: *То же самое, что и с захватом монитора*


Операция записи в volatile-поле
-------------------------------
(Операция записи в volatile-поле) hb (каждой последующей операции чтения того же самого поля)

.. math:: SetVolatile1(1)[X] \Rightarrow SetVolatile2(2)[X] \rightarrow ... \\ ... \rightarrow  when \, IsVolatile2(== 2)[Y] \Rightarrow ShowVolatile1[Y]
.. centered:: *выведет 1*

Program Order (Happens-before) между записью в volatile поля гарантируется из-за модификатора volatile, а именно из-за того, что Volatile2 является таковым, т.е. memory барьером

.. math:: SetVal(1)[X] \Rightarrow SetVolatile2(2)[X] \rightarrow when \, IsVolatile2(== 2)[Y] \Rightarrow ShowVal[Y] 
.. centered:: *выведет 1*


Состояния потоков
--------------------------------

.. math:: X.run[X] \rightarrow  \begin{bmatrix} X.join[Y] \\ if \, (X.isAlive == false)[Y] \end{bmatrix}

|br|

.. math:: X.interrupt[Y] \rightarrow \begin{bmatrix} catch \, throwInterruptedException[X] \\ if \, X.isInterrupted[Z] \\ if \, interrupted[X]) \end{bmatrix}

|br|

.. math:: SetValue[Y] \Rightarrow  X.interrupt[Y] \rightarrow if \, interrupted[X] \Rightarrow  ShowValue[X]
.. centered:: *как и в примерах выше с отношениями hb*


Конструирование объекта
--------------------------------
.. math:: CorrectConstructed[X] \rightarrow  finalize[Y]

|br|

.. math:: StartConstructor[X] \Rightarrow SetVal[X] \Rightarrow CorrectConstructed[X] \rightarrow ... \\ ... \rightarrow   finalize[Y] \Rightarrow ShowVal[Y]


CAS
---
Обычный CAS имеет семантику volatile read+write. В частности, это означает, что между двумя CAS-ами одной переменной всегда существует ребро happens-before