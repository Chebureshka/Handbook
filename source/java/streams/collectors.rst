.. include:: ../../defs.rst


Свертка элементов стрима
========================


reduce
------

`Optional<T> reduce(BinaryOperator<T> accumulator) <https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-java.util.function.BinaryOperator->`_

`T reduce(T identity, BinaryOperator<T> accumulator) <https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-T-java.util.function.BinaryOperator->`_

`U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner) <https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-U-java.util.function.BiFunction-java.util.function.BinaryOperator->`_

collect
-------

`<R> R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner) <https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BiConsumer->`_

`<R,A> R collect(Collector<? super T,A,R> collector) <https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.stream.Collector->`_

Collector
---------

.. raw:: html

    <iframe width="640" height="360" frameborder="0" src="https://mega.nz/embed/iAtl3Iob#sQ8N2EW0_obO_9ZXyh_3KnBNwzOdykNNnWgQbvFAyB4!3749s" allowfullscreen ></iframe>

|br|


Collectors
----------

.. raw:: html

    <iframe width="640" height="360" frameborder="0" src="https://mega.nz/embed/qE1VRILI#SK69Guct1sOqTuGagS8AjPIWRHfU4Gwk6hL_riwwYbI" allowfullscreen ></iframe>
