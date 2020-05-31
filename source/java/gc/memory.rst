.. include:: ../../defs.rst

Общие сведения
********************************


.. raw:: html

    <div style="text-align:right; font-style: italic">
        Господи, дай мне места для размещения того, что пока еще нужно, <br>
        Дай мне смелости удалить то, что больше не пригодится, <br>
        И дай мне мудрости, чтобы отличить одно от другого.
    </div>


Гипотеза о поколениях
===============================

Вероятность смерти как функция от возраста снижается очень быстро. Ее приложение к сборке мусора в частности означает, что подавляющее большинство объектов живут крайне недолго.

.. figure:: ../../_static/9054ad5f8fb541a4acf4095d0847e1b8.png

Вот тут и возникает идея разделения объектов на младшее поколение (young generation) и старшее поколение (old generation). 
В соответствии с этим разделением и процессы сборки мусора разделяются на малую сборку (minor GC), затрагивающую только младшее поколение, и полную сборку (full GC), которая может затрагивать оба поколения. 





Организация памяти
================================

+----------------------------------------------------------+-------------------------------------------------------------------------------+
|                     Heap Space                           |                               Non-Heap Space                                  |
+=============================================+============+=======================================+=======================================+
|                 `Young Gen`_                | Old Gen    |               Method Area |br|        |           `Native Area`_ |br|         |
+-------------------------------+-------------+            |               `Metaspace`_            |           Code Cache                  |
|      `Survivor Space`_        |             |            |                                       |                                       |
+---------------+---------------+             +------------+---------------+-------------+---------+-------------+-------------+-----------+
| Survivor 0    | Survivor 1    |`Eden Space`_| `Tenured`_ | Runtime       | Field &     |  Code   | Thread      |  Compile    |  Native   |
| |br| (To)     | |br| (From)   |             | Generation | Constant Pool | Method Data |         | Stack, PC,  |             |           |
|               |               |             |            |               |             |         | NativeStack |             |           |
+---------------+---------------+-------------+------------+---------------+-------------+---------+-------------+-------------+-----------+

Young Gen
----------
По умолчанию младшее поколение занимает одну треть всей кучи


Eden Space
-----------
Выделятся память под большую часть создаваемых из программы объектов.

Большая часть объектов живет недолго (итераторы, временные объекты, используемые внутри методов и т.п.), и удаляются при выполнении сборок мусора это области памяти, 
не перемещаются в другие области памяти. 

Когда данная область заполняется (т.е. количество выделенной памяти в этой области превышает некоторый заданный процент), GC выполняет быструю (minor collection) сборку мусора. 
По сравнению с полной сборкой мусора она занимает мало времени, и затрагивает только эту область памяти — очищает от устаревших объектов Eden Space и перемещает выжившие объекты в следующую область.


Survivor Space
---------------
Сюда перемещаются объекты из предыдущей, после того, как они пережили хотя бы одну сборку мусора. Время от времени долгоживущие объекты из этой области перемещаются в Tenured Space.

Один из двух регионов Survivor всегда пустой, именно он выбирается для переноса объектов из Eden.

Каждый регион Survivor по умолчанию занимает одну десятую младшего поколения


Tenured
---------
Tenured ([ˈtenjʊəd] Old) Generation:
Здесь скапливаются долгоживущие объекты (крупные высокоуровневые объекты, синглтоны, менеджеры ресурсов и проч.). 

Когда заполняется эта область, выполняется полная сборка мусора (full, major collection), которая обрабатывает все созданные JVM объекты.

Так же может быть использована для создаваемых объектов, размер которых достаточно велик (объекты-акселераты) для Eden Space, и целесообразнее помещается в Tenured.


Metaspace
----------
https://www.artima.com/insidejvm/ed2/jvm5.html

Здесь хранится метаинформация, используемая JVM (используемые классы, методы и т.п.).
Может динамически расширятся, до определенного момента (OutOfMemoryError).
The method area is created on virtual machine start-up

- Type Information
    Fully qualified name, Fully qualified name of superclass, Whether or not the type is a class or an interface, The type's modifiers, An ordered list of the fully qualified names of any direct superinterfaces

    - The Constant Pool
        For each type it loads, a Java virtual machine must store a constant pool. 
        A constant pool is an ordered set of constants used by the type, including literals (string, integer, and floating point constants) and symbolic references to types, fields, and methods.

    - Field Information
        The field's name, The field's type, The field's modifiers (some subset of public, private, protected, static, final, volatile, transient)
        Order must be recorded

    - Method Information
        The method's name, The method's return type, The number and types (in order) of the method's parameters, 
        The method's modifiers (some subset of public, private, protected, static, final, synchronized, native, abstract)

        For not abstract, not native: 
        The method's bytecodes, The sizes of the operand stack and local variables sections of the method's stack frame, An exception table

        Order must be recorded

    - Class Variables

    - A Reference to Class ClassLoader

    - A Reference to Class Class

- Method Tables
    Array of direct references to all the instance methods that may be invoked on a class instance, including instance methods inherited from superclasses


Native Area
------------
- Stack
    Область хранящая состояние стека, program counter, native стека для каждого потока
    Может бросится OutOfMemoryError, при создании потока, при нехватки места

- Compile 
    Эта область используется JVM, когда включена JIT-компиляция, в ней кешируется скомпилированный платформенно-зависимый код.


Терминология
==============

Максимальная задержка 
    максимальное время, на которое сборщик приостанавливает выполнение программы для выполнения одной сборки. Такие остановки называются stop-the-world (или STW).

Пропускная способность 
    отношение общего времени работы программы к общему времени простоя, вызванного сборкой мусора, на длительном промежутке времени.

Потребляемые ресурсы 
    объем ресурсов процессора и/или дополнительной памяти, потребляемых сборщиком.