.. include:: ../../defs.rst

CAS
********************************

.. container:: left-col

    Compare-and-Set (Compare-and-Swap) - атомарная инструкция, сравнивающая значение в памяти с одним из аргументов, и в случае успеха записывающая второй аргумент в память.

    .. code-block:: c

        // Псевдокод работы инструкции, возвращающей булево значение в синтаксисе языка C
        // Очевидно конкретно данная реализация в многопоточной среде не будет работать корректно.
        bool cas( int* addr, int old, int new ) {
          if ( *addr != old )
            return false;

          *addr = new;
          return true;
        }

    .. code-block:: java

        /* Пример использования CAS с AtomicInteger */
        void incr() {
            int v;

            do {
                v = ai.get();
            } while (!ai.compareAndSet(v, v + 1))
        }

    Compare-And-Exchange - атомарная инструкция, так же как и CAS позволяет обновить значение в памяти, но вместо boolean возвращающая предыдущее значение.

    Fetch-and-Add - атомарная инструкция, временно сохраняющая значение из памяти, увеличивающая его на указанное число и записывающая обратно.

    lock-free 
        Могут присутствовать потоки, которые по CAS операциям всегда проигрывают (естественно без локов на другие потоки)

    wait-free 
        Каждый из потоков должен быть завершен за конечное число шагов (естественно без локов на другие потоки)


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `CAS <https://ru.wikipedia.org/wiki/%D0%A1%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5_%D1%81_%D0%BE%D0%B1%D0%BC%D0%B5%D0%BD%D0%BE%D0%BC>`_

        `Сравнение Lock-free алгоритмов — CAS и FAA на примере JDK 7 и 8 <https://habr.com/ru/post/319036/>`_


LL/SC и weakCAS
===============
.. container:: left-col

    LL/SC - алгоритм, позволяющий использовать CAS операции, и при этом не поддерживать инструкций CAS на стороне процессора, 
    а вместо этого использовать 2 инструкции **load-linked** и **store-conditional**.

    При загрузке **load-linked** из *location* - запоминается что данное значение был считано, далее проверяется, что загруженное значение равно *expected*, 
    и далее выполняется **store-conditional** - если во время этой записи ничего не поменялось с момента вызова **load-linked** - изменяет значение в памяти, иначе он говорит, что не получилось к примеру возвращая false.

    LL/SC может пофейлиться на реальном хардваре (например *spurious failure*). Если несколько потоков делают LL/SC, может ни один не вернуть true (а CAS должен позволить хотя бы одному потоку изменить), так как 
    **store-conditional** возвращает false. Поэтому необходимо в данный алгоритм добавить *while*-цикл. Но так как зачастую CAS используется вместе с циклической проверкой на то что, получилось ли изменить
    или нет (см выше Пример использования CAS с AtomicInteger) - то можно выделить 2 алгоритма реализации CAS:

        * обычный CAS через LL/SC с циклом, для предотвращения *spurious failure* и того, что для всех потоков может вернуться false.
        * weak CAS без цикла, но с возможностью получить false для всех потоков. 

    .. code-block:: java

        /* Эмуляция обычного CAS через LL/SC */
        boolean CAS(location, expected, update) {
            while (true) {
                int cur = load-linked(location);

                if (cur != expected)
                    return false;

                if (store-conditional(location, update))
                    return true;
            }
        }

        /* Эмуляция weakCAS через LL/SC, но с spurious failure */
        boolean weakCAS(location, expected, update) {
            int cur = load-linked(location);

            if (cur == expected) {
                return store-conditional(location, update);
            } else {
                return false;
            }
        }

    weakCAS:

        * Может откатиться (ничего не изменить и вернуть false) даже если значение expected совпадает с текущим значением в ячейке памяти (другими словами -- может откатиться "без причины")

        * Не создает (=не обязан создавать) ребра happens-before

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `weakCompareAndSet <https://dev.cheremin.info/2011/07/weakcompareandset.html>`_

    .. raw:: html

        <iframe width="560" height="315" src="https://www.youtube.com/embed/ESs0bZw8hsA?start=1656" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Atomics
=============
.. container:: left-col

    .. thumbnail:: ../../_static/atomics.PNG

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:
        
        `Faster Atomic*FieldUpdaters for Everyone <https://shipilev.net/blog/2015/faster-atomic-fu/>`_


ABA-problem
===============
.. container:: left-col

    В многозадачных вычислениях проблема ABA возникает при синхронизации, когда ячейка памяти читается дважды, оба раза прочитано одинаковое значение, и признак «значение одинаковое» трактуется как «ничего не менялось».

        * Процесс :math:`{\displaystyle P_{1}}` читает значение A из разделяемой памяти,
        * :math:`{\displaystyle P_{1}}` вытесняется, позволяя выполняться :math:`{\displaystyle P_{2}}`,
        * :math:`{\displaystyle P_{2}}` меняет значение A на B и обратно на A перед вытеснением,
        * :math:`{\displaystyle P_{1}}` возобновляет работу, видит, что значение не изменилось, и продолжает…

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ABA-problem <https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%BE%D0%B1%D0%BB%D0%B5%D0%BC%D0%B0_ABA>`_

    .. container:: remark-block

        ABA-problem решается с помощью AtomicMarkableReference и AtomicStampedReference.



VarHandle
==========
.. container:: left-col

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Java 9 Variable Handles <https://www.baeldung.com/java-variable-handles>`_
        
        `VarHandle <https://docs.oracle.com/javase/9/docs/api/java/lang/invoke/VarHandle.html>`_