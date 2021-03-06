.. include:: ../../defs.rst

Сортирорвки
*******************************************************

.. container:: left-col

    * QuickSort_

    * `Пирамидальная сортировка (HeapSort)`_

    * MergeSort_

    * TimSort_

    * RadixSort_

    * GravitySort_

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Сортировки <http://neerc.ifmo.ru/wiki/index.php?title=%D0%A1%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B8>`_

    .. container:: defs-block

        Устойчивая (стабильная) сортировка — сортировка, которая не меняет относительный порядок сортируемых элементов, имеющих одинаковые ключи, по которым происходит сортировка.



QuickSort
===========
.. container:: left-col

    1. Находится опорная точка (первая / последняя / середина) по значению

    2. Относительно нее на текущем обрабатываемом участке массива происходит перестановка для всех элементов (>=) нее - вправо, и соответственно влево для (<=).
       При этом находится середина, относительно которой справа будут большие (или равные) элементы чем опорная точка, а слева меньшие (или равные).

    3. Сортировка повторяется для двух участков, разделенных найденной серединой

    .. code-block:: java

        algorithm quicksort(A, lo, hi) is
            if lo < hi then
                p := partition(A, lo, hi)
                quicksort(A, lo, p)
                quicksort(A, p + 1, hi)

        algorithm partition(A, lo, hi) is
            pivot := A[lo + (hi - lo) / 2]
            i := lo - 1
            j := hi + 1
            loop forever
                do i := i + 1 while A[i] < pivot

                do j := j - 1 while A[j] > pivot

                if i >= j then return j

                swap A[i] with A[j]

    .. code-block:: java

        // Находится опорная точка
        pivot := A.pop()

        // Создается массив с меньшими элементами чем опорная точка от исходного
        lA := A.filter(where e < pivot)  

        // Создается массив с большими элементами чем опорная точка от исходного
        rA := A.filter(where e >= pivot) 

        // вернуть массив состоящий из отсортированной левой части, опорного и отсортированной правой части
        return quicksort(lA) + [pivot] + quicksort(rA)	

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Википедия <https://ru.wikipedia.org/wiki/%D0%91%D1%8B%D1%81%D1%82%D1%80%D0%B0%D1%8F_%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0>`__

    .. figure:: ../../_static/Sorting_quicksort_anim.gif

Пирамидальная сортировка (HeapSort)
======================================
.. container:: left-col

    Строится пирамида (двоичная куча), последовательно удаляются узлы, начиная с вершины. гарантированная сложность Θ(n log n)

MergeSort
==================
.. container:: left-col

    1. Сортируемый массив разбивается на две части примерно одинакового размера.

    2. Каждая из получившихся частей сортируется отдельно, например — тем же самым алгоритмом.

    3. Два упорядоченных массива половинного размера соединяются в один.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Википедия <https://ru.wikipedia.org/wiki/%D0%A1%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0_%D1%81%D0%BB%D0%B8%D1%8F%D0%BD%D0%B8%D0%B5%D0%BC>`__

    .. figure:: ../../_static/Merge-sort-example-300px.gif

TimSort
=========
.. container:: left-col

    Гибридный алгоритм сортировки, сочетающий сортировку вставками и сортировку слиянием (MergeSort_)

    1. По специальному алгоритму входной массив разделяется на подмассивы.

    2. Каждый подмассив сортируется сортировкой вставками.

    3. Отсортированные подмассивы собираются в единый массив с помощью модифицированной сортировки слиянием.

RadixSort
==============
.. container:: left-col

    Исходный массив: ``a``. 
    Максимальное число разрядов: ``k``. 
    Число различных цифр в пределах одного разряда ``d``

    1. Создается дополнительная структура (Массив массивов) ``b`` для каждой цифры (размерностью ``d``).

    2. Для каждого разряда (``k_cur`` : ``k``), начиная с наименьшего:

        2.1. Занести все число ``a[i]`` в массив ``b`` под индексом ``a[i].get(k_cur)`` (число в текущем разряде ``k_cur``)::

            b.add( a[i].get(k_cur) ) = a[i]

        2.2. После прохода по всему массиву ``a``, выполняется проход по сформировавшемуся массиву ``b``, начиная от наименьшего числа, выписывая все элементы в массив ``a`` начиная с первого записанного

    3. После всех проходов, полученный результат сортировки будет храниться в массиве ``a``
    
    ::

        [40, 34, 32, 2, 14, 24]     k = 2    d = 5  (0, 1, 2, 3, 4)

        Для младшего разряда:

        {
            0: 40, 
            1: 
            2: 32, 2
            3: 
            4: 34, 14, 24
        }

        [40, 32, 2, 34, 14, 24]

        Для старшего разряда:

        {
            0: 2, 
            1: 14
            2: 24
            3: 32, 34
            4: 40
        }

        [2, 14, 24, 32, 34, 40]
    

    Алгоритм сортировки выполняется за линейное время.

.. container:: right-col

    .. raw:: html

        <iframe width="560" height="315" src="https://www.youtube.com/embed/toAlAJKojos" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

GravitySort
============
.. container:: left-col

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        https://en.wikipedia.org/wiki/Bead_sort