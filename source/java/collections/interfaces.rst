.. include:: ../../defs.rst

Иерархия интерфейсов 
********************************

.. container:: left-col

  * Iterable

    * `Collection`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html>`__ 

      * **AbstractCollection**

      * `List`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/List.html>`__

        * **AbstractList**

          * **AbstractSequentialList**

        * `Queue`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html>`__

          * **AbstractQueue**

          * `BlockingQueue`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html>`__

            * `TransferQueue`_ `📖 <https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/TransferQueue.html>`__

          * `Deque`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html>`__

            * `BlockingDeque`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingDeque.html>`__

      * `Set`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Set.html>`__

        * **AbstractSet**

        * `SortedSet`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html>`__

          * `NavigableSet`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html>`__

  * `Map`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Map.html>`__

    * **AbstractMap**

    * `SortedMap`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html>`__

      * `NavigableMap`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html>`__

    * `ConcurrentMap`_ `📖 <https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html>`__

      * `ConcurrentNavigableMap`_ `📖 <https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ConcurrentNavigableMap.html>`__



Collection
===================================================================================================================

.. container:: left-col

  Интерфейс находится в составе JDK c версии 1.2 и определяет основные методы работы с простыми наборами элементов, которые будут общими для всех его реализаций (например ``size()``, ``isEmpty()``, ``add(E e)`` и др.). 

  Методы для работы с лямбдами (такие как ``stream()``, ``parallelStream()``, ``removeIf(Predicate<? super E> filter)`` и др.) были добавлены только с Java 8.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html>`__ 

    .. container:: remark-block

      .. rubric:: Методы:

      ``add``, ``addAll``, ``remove``, ``removeAll``, ``removeIf``, ``retainAll``, ``size``, ``clear``, ``contains``, ``containsAll``, ``stream``, ``parallelStream``, ``iterator``, ``spliterator``, ``toArray``, ``toArray(T[] a)``
    


List
-----------------------

.. container:: left-col

  Реализации этого интерфейса представляют собой упорядоченные коллекции. Кроме того, разработчику предоставляется возможность доступа к элементам коллекции по индексу и по значению (так как реализации позволяют хранить дубликаты, результатом поиска по значению будет первое найденное вхождение).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/List.html>`__ 

    .. container:: remark-block

      .. rubric:: Методы:

      ``indexOf``, ``get``, ``add``/``remove``/``set(index, )``, ``sort``, ``sublist``


Queue
-----------------------

.. container:: left-col

  Этот интерфейс описывает коллекции с предопределённым способом вставки и извлечения элементов, а именно — очереди FIFO (first-in-first-out). 

  Помимо методов, определённых в интерфейсе Collection, определяет дополнительные методы для извлечения и добавления элементов в очередь. 

  Большинство реализаций данного интерфейса находится в пакете java.util.concurrent

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html>`__ 

BlockingQueue
-----------------------


.. container:: left-col

  Интерфейс предоставляющий реализации очередей с возможностью блокировки при добалвении/получении элемента. 

  Помимо возможности задавать размер queue, добавились новые методы, которые реагируют по-разному на незаполнение или переполнение queue. 
  Так, например, при добавлении элемента в переполненную queue, один метод кинет IllegalStateException, другой вернет false, третий заблокирует поток, пока не появится место, четвертый же заблокирует поток с таймаутом и вернет false, если место так и не появится (см `Операции на очереди`_). 

  Также стоит отметить, что блокирующие очереди не поддерживают null значения, т.к. это значение используется в методе poll как индикатор таймаута.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html>`__ 

        `Блокирующие очереди пакета concurrent <http://java-online.ru/concurrent-queue-block.xhtml>`__ 

TransferQueue
-----------------------
.. container:: left-col

  В отличие от реализации очередей интерфейса BlockingQueue, где потоки могут быть блокированы при чтении, если очередь пустая, либо при записи, если очередь полная, очереди интерфейса TransferQueue блокируют поток записи до тех пор, пока другой поток не извлечет элемент. Для этого следует использовать метод transfer.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/TransferQueue.html>`__ 

Deque
-----------------------
.. container:: left-col

  Двусторонняя очередь (Double ended queue), позволяет работать с элементами коллекции, как со стеком и как с очередью.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html>`__ 

BlockingDeque
-----------------------
.. container:: left-col

  Двустороняя BlockingQueue.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingDeque.html>`__ 

Set
-----------------------
.. container:: left-col

  Представляет собой неупорядоченную коллекцию, которая не может содержать дублирующиеся данные. Является программной моделью математического понятия «множество».

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Set.html>`__ 

SortedSet
-----------------------
.. container:: left-col

  Интерфейс позволяющий обходить (и хранить) элементы множества в определенном порядке.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html>`__ 

NavigableSet
-----------------------
.. container:: left-col

  Интерфейс предоставляющий дополнительные методы для получения специфических элементов (первый, последний).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html>`__ 

Map
=======================
.. container:: left-col

  Данный интерфейс также находится в составе JDK c версии 1.2 и предоставляет разработчику базовые методы для работы с данными вида «ключ — значение». 

  В версии Java 8 появились дополнительные методы для работы с лямбдами, а также методы, которые зачастую реализовались в логике приложения (``getOrDefault(Object key, V defaultValue)``, ``putIfAbsent(K key, V value)``).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/Map.html>`__ 

    .. container:: remark-block

      .. rubric:: Методы:

      ``put``, ``putAll``, ``remove``, ``keySet``, ``entrySet``, ``clear``, ``containsValue``, ``containsKey``, ``get``, ``replace``, ``values``

SortedMap
-----------------------
.. container:: left-col

  Интерфейс позволяющий обходить (и хранить) элементы мапы в определенном порядке.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html>`__ 

NavigableMap
-----------------------


.. container:: left-col

  Интерфейс предоставляющий дополнительные методы для получения специфических элементов (первый, последний).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html>`__ 

ConcurrentMap
-----------------------


.. container:: left-col

  Map с несколькими дополнительными атомарными операциями + улучшенные реализации HashMap, TreeMap с лучшей поддержкой многопоточности и масштабируемости.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html>`__ 

ConcurrentNavigableMap
-----------------------


.. container:: left-col

  Расширяет интерфейс NavigableMap и вынуждает использовать ConcurrentNavigableMap объекты в качестве возвращаемых значений. 

  Все итераторы декларируются как безопасные к использованию и не кидают ConcurrentModificationException.


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Документация 📖 <https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ConcurrentNavigableMap.html>`__ 



Операции на очереди
===================================================================================================================

.. container:: left-col

  .. table::
    :widths: auto

    +----------+----------------------+-------------------+-------------------+------------------------------+
    |          |Queue |br|            |Queue |br|         |BlockingQueue |br| |BlockingQueue |br|            |
    |          |Вызывает Exception    |Чтение значения    |Блокировка         |Чтение с задержкой            |
    +==========+======================+===================+===================+==============================+
    |Insert    |add(e)                |offer(e): boolean  |put(e)             |offer(e, time, unit)          |
    +----------+----------------------+-------------------+-------------------+------------------------------+
    |Remove    |remove()              |poll(): E | null   |take()             |poll(time, unit)              |
    +----------+----------------------+-------------------+-------------------+------------------------------+
    |Проверка  |element()             |peek(): E | null   |не применимый      |не применимый                 |
    +----------+----------------------+-------------------+-------------------+------------------------------+
    |          |                      |**TransferQueue**: |**TransferQueue**: |**TransferQueue**: |br|       |
    |Transfer  | ---                  ||br| tryTransfer(e)||br| transfer(e)   |tryTransfer(e, timeout, unit) |
    +----------+----------------------+-------------------+-------------------+------------------------------+



Функциональные Операции на Map
===================================================================================================================

.. container:: left-col

  .. container:: code-markup

    ``V putIfAbsent(K key, V value)``
    	It inserts the specified value with the specified key in the map only if it is not already specified.

    ``V compute(K key, (K, V) -> V remappingFunction)``
    	remappingFunction для key, или null, если отсвутствует данный элемент.
    	It is used to compute a mapping for the specified key and its current mapped value (or null if there is no current mapping).

    ``V computeIfAbsent(K key, K -> V mappingFunction)``
    	Расчитать новое значение по данному ключу функцией mappingFunction.
    	It is used to compute its value using the given mapping function, if the specified key is not already associated with a value (or is mapped to null), and enters it into this map unless null.

    ``V computeIfPresent(K key, (K, V) -> V remappingFunction)``
    	remappingFunction для key, если key есть и не null.
    	It is used to compute a new mapping given the key and its current mapped value if the value for the specified key is present and non-null.

    ``forEach((K, V) -> Void action)``	
    	It performs the given action for each entry in the map until all entries have been processed or the action throws an exception.

    ``V getOrDefault(Object key, V defaultValue)``
    	It returns the value to which the specified key is mapped, or defaultValue if the map contains no mapping for the key.

    ``V merge(K key, V value, (V, V) -> V remappingFunction)``
    	If the specified key is not already associated with a value or is associated with null, associates it with the given non-null value.

    ``void replaceAll((K, V) -> V function)``
    	It replaces each entry's value with the result of invoking the given function on that entry until all entries have been processed or the function throws an exception.