.. include:: ../../defs.rst

Реализации неизменяемых коллекций
************************************
.. container:: left-col

    .. raw:: html

        <object data="../../_static/collections-immutable-diagram-213.svg" type="image/svg+xml" style="width: 100%;"></object>

.. container:: right-col

    .. container:: links-block

      .. rubric:: Ссылки:

      `scala.collection.immutable <https://docs.scala-lang.org/ru/overviews/collections-2.13/concrete-immutable-collection-classes.html>`_

Seq
======

Vector
-----------
.. container:: left-col

    Вектора обладают хорошим балансом между быстрой случайной выборкой и быстрым случайным обновлением элементов, они используются в качестве реализации неизменяемых индексированных последовательностей.

    Вектора представленны деревьями с высоким уровнем ветвления. Каждый узел дерева содержит до 32х элементов вектора или содержит до 32х других узлов. 
    Вектора с размером до 32х элементов могут быть представленны одним узлом. Вектора 32 * 32 = 1024 элементы могут быть представлены одним витком. Для векторов с 2^15 элементами достаточно двух переходов от 
    корня дерева до конечного элемента узла и пяти переходов для 2^30 элементами. Таким образом, для всех векторов разумных размеров выбор элемента включает до 5 простых выборок массивов.

    Также как и доступ к элементу, операция обновления в векторах занимает “практически” постоянное время. 
    Добавление элемента в середину вектора может быть выполнено через копирование узла содержащего этот элемент и каждого ссылающегося на него узла, начиная от корня дерева. 
    Это означает, что процесс обновления элемента создает от одного до пяти узлов, каждый из которых содержит до 32 элементов или поддеревьев. 
    Это, конечно, дороже, чем просто обновление элемента в изменяемом массиве, но все же намного дешевле, чем копирование вообще всего вектора.

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

ArraySeq
-----------
.. container:: left-col

    ArraySeqs хранят свои элементы в приватном Массиве. Таким образом достигается компактное представление и обеспечивается быстрый индексированный доступ к элементам, 
    но обновление или добавление одного элемента занимает линейное время, так как требует создания другого массива и копирования всех элементов исходного массива.

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

NumericRange
--------------
.. container:: left-col

    Generic версия Range

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

String
-----------
.. container:: left-col

    Как и массивы, строки не являются непосредственно последовательностями, но могут быть преобразованы в них, а также поддерживают все операции которые есть у последовательностей.

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

Range
-----------
.. container:: left-col

    Диапазон представляет собой упорядоченную последовательность целых чисел, которые отделены друг от друга одинаковыми размерами.
    Диапазоны занимают константный размер, потому что они могут быть определены только тремя цифрами: их началом, концом и значением шага.

.. container:: right-col

    .. container:: remark-block

        Наследуется от IndexedSeq

List
-----------
.. container:: left-col

    List представляет из себя конечную неизменяемую последовательность. Он обеспечивает быстрый (за постоянное время) доступ как к первому элементу, так и к остальному списку, 
    а также быструю операцию добавления нового элемента в начало списка. Большинство оставшихся операции занимают линейное время исполнения.

.. container:: right-col

    .. container:: remark-block

        Наследуется от LinearSeq

LazyList
-----------
.. container:: left-col

    LazyList похож на список, за исключением того, что его элементы вычисляются лениво. Поэтому ленивый список может быть бесконечно длинным. 
    Обрабатываются только те элементы, которые запрашиваются. В остальном, у ленивых списков те же параметры производительности, что и обычных.

.. container:: right-col

    .. container:: remark-block

        Наследуется от LinearSeq

Queue
-----------
.. container:: left-col

    Queue is implemented as a pair of Lists, one containing the in elements and the other the out elements. Elements are added to the in list and removed from the out list. 
    When the out list runs dry, the queue is pivoted by replacing the out list by in.reverse, and in by Nil.

    Adding items to the queue always has cost O(1). Removing items has cost O(1), except in the case where a pivot is required, in which case, a cost of O(n) is incurred, where n is the number of elements in the queue.
    When this happens, n remove operations with O(1) cost are guaranteed. Removing an item is on average O(1).

.. container:: right-col

    .. container:: remark-block

        Наследуется от LinearSeq

Map
=====


HashMap
--------
.. container:: left-col

    Хэш деревья - это стандартный способ эффективного создания неизменяемых множеств и ассоциативных массивов (мап). 

    **Compressed Hash-Array Mapped Prefix-trees** - это специальные хэш деревья для JVM, которые улучшают локальность и обеспечивают компактную и элегантную реализацию деревьев. 
    Их представление очень похоже на реализацию векторов, которые также являются деревьями, где каждый узел имеет либо 32 элемента либо 32 поддерева. Но в данном случае ключ выбирается на основе хэш-кода. 
    Например, чтобы найти ключ на мапе, сначала берут хэш-код ключа. Затем самые младшие 5 бит хэш-кода используются для выбора первого поддерева, за которым следуют следующие 5 бит и так далее. 
    Выбор прекращается, когда для всех битов будут найдены ключи.

TreeMap
-----------
.. container:: left-col
    
    Красно-черные деревья представляют собой разновидность сбалансированного двоичного дерева, где одни узлы помечаются как “красные”, а другие - как “черные”. 
    Как и любое сбалансированное двоичное дерево, операции над ним занимают по времени логарифм от количества элементов дерева.

.. container:: right-col

    .. container:: remark-block

        Наследуется от SortedMap

ListMap
---------
.. container:: left-col

    ListMap представляет собой мапу в виде связанного списка пар ключ-значение. В общем, операции на связанной мапе могут потребовать обхода по всему связанному списку. 
    Таким образом, время выполнении обхода на связанной мапе линейно зависит от размера мапы.

.. container:: right-col

    .. container:: remark-block

        Наследуется от SeqMap

VectorMap
----------
.. container:: left-col

    VectorMap представляет собой мапу, использующую и Vector ключей и HashMap. У него есть итератор, который возвращает все записи в порядке их вставки.

.. container:: right-col

    .. container:: remark-block

        Наследуется от SeqMap

Set
=====


HashSet
--------
.. container:: left-col

    Базируется на Compressed Hash-Array Mapped Prefix-trees (реализация схожа с HashMap_)


ListSet
--------
.. container:: left-col

    Так же как и ListMap_ использует связанный список элементов, а следовательно требует линейное время для вставки элементов.


TreeSet
--------
.. container:: left-col

    Базируется на красно-черном дереве (реализация схожа с TreeMap)

.. container:: right-col

    .. container:: remark-block

        Наследуется от SortedSet

BitSet
--------
.. container:: left-col

    BitSet представляет собой набор маленьких целых чисел в виде набора битов большего целого числа. 
    Например, набор битов, содержащий 3, 2 и 0, будет представлен как целое число 1101 в двоичном виде, т.е. 13 в десятичном.

.. container:: right-col

    .. container:: remark-block

        Наследуется от SortedSet