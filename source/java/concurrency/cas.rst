.. include:: ../../defs.rst

CAS
********************************

.. container:: left-col

    lock-free 
        Могут присутствовать потоки, которые по CAS операциям всегда проигрывают (естественно без локов на другие потоки)

    wait-free 
        Каждый из потоков должен быть завершен за конечное число шагов (естественно без локов на другие потоки)

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        https://en.wikipedia.org/wiki/Compare-and-swap

        `CAS <https://ru.wikipedia.org/wiki/%D0%A1%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5_%D1%81_%D0%BE%D0%B1%D0%BC%D0%B5%D0%BD%D0%BE%D0%BC>`_
        
        `VarHandle <https://docs.oracle.com/javase/9/docs/api/java/lang/invoke/VarHandle.html>`_


Atomics
------------------
.. container:: left-col

    .. thumbnail:: ../../_static/atomics.PNG


ABA-problem
-------------

.. container:: left-col

    |br|

.. container:: right-col

    .. container:: remark-block

        То что решается с помощью AtomicMarkableReference и AtomicStampedReference