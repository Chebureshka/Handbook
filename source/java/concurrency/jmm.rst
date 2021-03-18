.. include:: ../../defs.rst

JMM
********************************

.. container:: left-col

  JMM устанавливает правила использования потоками разделяемой памяти

.. container:: right-col

  .. container:: links-block

    .. rubric:: Ссылки:

    `Квест по внутренностям JMM: solution <https://dev.cheremin.info/2013/02/jmm-solution.html>`_

  .. toggle-header::
      :header: Лекции (EPUM)

      .. raw:: html

        <iframe width="320" height="180" frameborder="0" src="https://mega.nz/embed/bNkjTDRB#_eYTedHnq4NaAfm3hl1hb0jMSG_I9uwRJ5Z4U4ydlvE" allowfullscreen ></iframe>


      .. raw:: html

        <iframe width="320" height="180" frameborder="0" src="https://mega.nz/embed/7V1n2bzL#byi2Y8w1RLtG3VRTqk2KZazuUsoxdZKLGR4ey02Df9Y" allowfullscreen ></iframe>

Модели памяти процессоров
===========================
.. container:: left-col

  Сильная модель 
      Все видят одинаковые значения.

  Слабая модель 
      Предпологает наличие барьеров памяти.

      1. Flush - Инструкция на сброс данных (отчистка кэша (сброс) в основную память).
      2. Инвалидация - объявление данных в кэше недействительными (значения впредь берутся из основной памяти).


Критическая секция
==================
.. container:: left-col

  Критическая секция - участок кода, где используется доступ к общему ресурсу.

  Не должно в критической секции быть более одного потока в один момент времени (~ или же могут быть несколько читателей).

  Решает проблемы Race Condition.


Cache coherence protocol 
========================
.. container:: left-col

  Cache coherence - Это (применительно к многопроцессорным системам с кэш-памятью) свойство согласованности данных, вообще говоря разбросанных по различным кэшам.
  Очевидно, что если у нас одни и те же данные могут быть скэшированы в кэш-памяти разных уровней, 
  и в локальных блоках кэш-памяти разных процессоров/ядер - при модификации этих данных легко может случиться, что существует множество несогласованных версий одних и тех же данных.

  Производители процессоров обычно реализуют какой-либо аппаратный механизм поддержания согласованности данных различных кэшей между собой, и с основной памятью; когерентность кэша довольно тесно связана с аппаратной моделью памяти соответствующей системы.

  Подслушивание (snooping)
    Есть общая шина, к которой подключены кэши всех ядер, и к ней же подключен контроллер основной памяти. 
    Любые запросы отправляются бродкастом на шину, и видны всем подключенным к шине сразу.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Cache-coherency: Basics, MSI <https://dev.cheremin.info/2012/01/cache-coherency-basics-msi.html>`_

        `Cache-coherency: от MSI к MESI и далее к звездам (MESIF, MOESI) <https://dev.cheremin.info/2012/01/cache-coherency-msi-mesi-mesif-moesi.html>`_

        `Cache coherency #3: false sharing <https://dev.cheremin.info/2012/07/cache-coherency-3-false-sharing.html>`_
    
    .. container:: remark-block

      The protocol CPUs use to transfer cache lines between main memory and other CPUs.

MSI
---
.. container:: left-col

  MSI - базовый *Cache coherence protocol* - работает в многопроцессорных системах.
  Как и в других протоколах когерентности кэша, абревиатура протокола идентифицируют возможные состояния, в которых может быть строка (**cache line**):

    * M (**Modified**) - строка была изменена в кеше. Данные в кэш-памяти теперь несовместимы с хранилищем (например, памятью). 
      Кэш со строкой в состоянии «M» отвечает за запись строки в хранилище, когда она убирается из кэша (evicted), т.е. происходит flush.

    * S (**Shared**) - Эта строка не изменена и существует в состоянии только для чтения по крайней мере в одном кэше. Кэш может удалить данные, не записывая их в хранилище.

    * I (**Invalid**) Эта строка либо отсутствует в текущем кэше, либо была аннулирована запросом шины (напр. строка по этому же адресу перешла в состояние Modified. Инвалидация), и должена быть извлечена из памяти или другого кэша, если строка должна храниться в этом кэше.

  .. thumbnail:: ../../_static/State_diagram_for_processor_transactions.png

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `MSI protocol <https://en.wikipedia.org/wiki/MSI_protocol>`_


Fasle Sharing
-------------
.. container:: left-col

  Fasle Sharing - ситуация, при которой системы с реализованными протоколами когерентности кешей, ухудшают свою производительность, из-за того что не связанные переменные входят в один и тот же cache-line.

  .. note::
    cache-line имеет разный размер на разных процессорах -- от 32 до 128 байт.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Fasle Sharing, padding <https://dev.cheremin.info/2011/09/false-sharing.html>`_


Правила конструирования объектов
================================
.. container:: left-col

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
.. container:: left-col

  volatile - используется для данных, которые могут храниться только в heap (~~~)

  *  Обращение не в кэш процессора за значением, а напрямую в память (visibility).

     Кэш хранит копию часто используемых данных из RAM, обычно не больших блоков. Могут быть как из heap, так и из стека.

     cashline - то что считывается в кэш при доступе к определенному участку памяти.


  *  Атомарные операции чтения и записи для long и double (на 32 разрядных процессорах).


  *  Переупорядочивание volatile переменных не происходит. 

     Volatile переменные являются memory барьерами, в отношении любой операции чтения, произошедшей после чтения volaile, и любой операции записи, до записи в volaile переменную.

    .. container:: code-markup

      ``a = 1; volatile = 2; read(volatile); read(a)`` 
          Гарантируется Program Order: |br| :math:`setA[X] \Rightarrow setVolatile[X] \Rightarrow showVolatile[X] \Rightarrow showA[X]`

.. container:: right-col

  .. container:: remark-block

    `Out-Of-Order Excecution <https://ru.wikipedia.org/wiki/%D0%92%D0%BD%D0%B5%D0%BE%D1%87%D0%B5%D1%80%D0%B5%D0%B4%D0%BD%D0%BE%D0%B5_%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5>`_
    (Внеочередное исполнение) машинных инструкций — исполнение машинных инструкций не в порядке следования в машинном коде (как было при выполнении инструкций по порядку (англ. in-order execution)), а в порядке готовности к выполнению. Реализуется с целью повышения производительности вычислительных устройств

Правила межпоточного взаимодействия. Happens-before
====================================================
.. container:: left-col

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

  |br|

.. container:: right-col

  .. raw:: html

      <iframe width="560" height="315" src="https://www.youtube.com/embed/puYPHysBN7U?start=6581" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



Освобождение монитора
---------------------
.. container:: left-col

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
.. container:: left-col

  (Операция записи в volatile-поле) hb (каждой последующей операции чтения того же самого поля)

  .. math:: SetVolatile1(1)[X] \Rightarrow SetVolatile2(2)[X] \rightarrow ... \\ ... \rightarrow  when \, IsVolatile2(== 2)[Y] \Rightarrow ShowVolatile1[Y]
  .. centered:: *выведет 1*

  Program Order (Happens-before) между записью в volatile поля гарантируется из-за модификатора volatile, а именно из-за того, что Volatile2 является таковым, т.е. memory барьером

  .. math:: SetVal(1)[X] \Rightarrow SetVolatile2(2)[X] \rightarrow when \, IsVolatile2(== 2)[Y] \Rightarrow ShowVal[Y] 
  .. centered:: *выведет 1*


Состояния потоков
--------------------------------
.. container:: left-col

  .. math:: X.run[X] \rightarrow  \begin{bmatrix} X.join[Y] \\ if \, (X.isAlive == false)[Y] \end{bmatrix}

  |br|

  .. math:: X.interrupt[Y] \rightarrow \begin{bmatrix} catch \, throwInterruptedException[X] \\ if \, X.isInterrupted[Z] \\ if \, interrupted[X]) \end{bmatrix}

  |br|

  .. math:: SetValue[Y] \Rightarrow  X.interrupt[Y] \rightarrow if \, interrupted[X] \Rightarrow  ShowValue[X]
  .. centered:: *как и в примерах выше с отношениями hb*


Конструирование объекта
--------------------------------
.. container:: left-col

  .. math:: CorrectConstructed[X] \rightarrow  finalize[Y]

  |br|

  .. math:: StartConstructor[X] \Rightarrow SetVal[X] \Rightarrow CorrectConstructed[X] \rightarrow ... \\ ... \rightarrow   finalize[Y] \Rightarrow ShowVal[Y]


CAS
---
.. container:: left-col

  Обычный CAS имеет семантику volatile read+write. В частности, это означает, что между двумя CAS-ами одной переменной всегда существует ребро happens-before