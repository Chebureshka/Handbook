.. include:: ../../defs.rst

Реализации изменяемых коллекций
************************************
.. container:: left-col

    .. raw:: html

        <object data="../../_static/collections-mutable-diagram-213.svg" type="image/svg+xml" style="width: 100%;"></object>

.. container:: right-col

    .. container:: links-block

      .. rubric:: Ссылки:

      `scala.collection.mutable <https://docs.scala-lang.org/ru/overviews/collections-2.13/concrete-mutable-collection-classes.html>`_

Seq
=======


ArraySeq
----------
.. container:: left-col

    Последовательные массивы - это изменяемые массивы со свойствами последовательности фиксированного размера, которые хранят свои элементы внутри ``Array[Object]``

    Вам стоит использовать **ArraySeq**, если вам нужен массив из-за его показателей производительности, но вы дополнительно хотите использовать обобщеные экземпляры последовательности, 
    в которых вы не знаете тип элементов и у которого нет **ClassTag** который будет предоставлен непосредственно во время исполнения.

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

ArrayDeque
------------
.. container:: left-col

    ArrayDeque это последовательность, поддерживающая эффективное добавление элементов как спереди, так и сзади. Реализован на основе массива с изменяемым размером.

    Double-ended queue that internally uses a resizable circular buffer

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

StringBuilder
--------------
.. container:: left-col

    Так же, как буферизированный массив полезен для создания массивов, а буферизированный список полезен для построения списков, **StringBuilder** полезен для создания строк.

    Обертка над ``java.lang.StringBuilder``

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

Stack
------------
.. container:: left-col

    LIFO, основан на ArrayDeque_

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

Queue
------------
.. container:: left-col

    FIFO, основан на ArrayDeque_

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

ArrayBuffer
------------
.. container:: left-col

    ArrayBuffer - буферизированный массив в своем буфере хранит массив и его размер. Большинство операций с буферизированным массивом выполняются с той же скоростью, что и с массивом, 
    так как операции просто обращаются и изменяют исходный массив. Кроме того он может эффективно добавлять данные к своему концу. Присоединение элемента к такому массиву занимает амортизированное константное время.

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq, Buffer

ListBuffer
-----------
.. container:: left-col

    Похож на буферизированный массив, за исключением того, что он базируется на связанном списке, а не массиве

.. container:: right-col

    .. container:: remark-block

        Наследуется от Buffer

PriorityQueue
==============
.. container:: left-col

    This class implements priority queues using a heap


Map
================


HashMap
-----------
.. container:: left-col

    Основываются на хэш-таблицах


WeakHashMap
------------
.. container:: left-col

    Ослабленный хэш-мап это специальный вид хэш-мапы, при которой сборщик мусора не ходит по ссылкам с мапы к её ключам. 
    Это означает, что ключ и связанное с ним значение исчезнут из мапы, если на ключ не будет никаких ссылок. 


ListMap
-----------
.. container:: left-col

    Ассоциативная коллекция выраженный связанным списоком ключей и значений.

TreeMap
-----------
.. container:: left-col

    Ассоциативная коллекция на красно-черном дереве. С ключами в качестве узлов дерева и элементами в качетве листьев.


LinkedHashMap
---------------
.. container:: left-col

    Помимо хранения ключ-значение, как в обычной HashMap_ добавляется гарантия на порядок добавления, при итерировании, за счет хранения ссылок на предыдущий элемент и последующий в элементах значений мапы.
    
.. container:: right-col

    .. container:: remark-block

        Наследуется от SeqMap

Set
================


HashSet
-----------
.. container:: left-col

    Основываются на хэш-таблицах


LinkedHashSet
--------------
.. container:: left-col

    Реализован на основе LinkedHashMap_


BitSet
-----------
.. container:: left-col

    Изменяемый набор типа ``mutable.BitSet`` практически такойже как и неизменяемый набор, за исключением того, что он при изменении сам меняется.

.. container:: right-col

    .. container:: remark-block

        Наследуется от SortedSet