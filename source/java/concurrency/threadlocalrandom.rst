.. include:: ../../defs.rst

ThreadLocalRandom
******************
.. container:: left-col

    Генератор случайных чисел, изолированный в текущем потоке.

    Like the global Random generator used by the Math class, a ThreadLocalRandom is initialized with an internally generated seed that may not otherwise be modified.

    .. container:: code-markup

        static ThreadLocalRandom ``current()``
            Returns the current thread's ThreadLocalRandom.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ThreadLocalRandom <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/ThreadLocalRandom.html>`_

Методы для получения Stream
=============================
.. container:: left-col

    .. container:: code-markup

        ``DoubleStream doubles(...)`` |br| ``IntStream    ints(...)`` |br| ``LongStream   longs(...)``
            Возвращает соответствующий стрим чисел.

            Поддерживает 4 способа вызова:

                * Без параметров - для *double* - от 0 до 1, для *int* и *long* - вся область определения.
                * (randomNumberOrigin, randomNumberBound) - интевал [randomNumberOrigin .. randomNumberBound).
                * (long streamSize) - стрим ограниченной длинны.
                * ​(long streamSize, randomNumberOrigin, randomNumberBound) - стрим ограниченной длинны с числами в интервале [randomNumberOrigin .. randomNumberBound).

