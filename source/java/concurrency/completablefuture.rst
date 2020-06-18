.. include:: ../../defs.rst

CompletableFuture
*******************************************************

.. contents::
    :local:

.. container:: left-col

    **CompletableFuture** используется для построения пайплайна (сколь угодно ветвящегося) при обработке ассинхронных событий, во избежание `callback hell <http://callbackhell.com/>`_. 

    **CompletableFuture** наследуется от CompletionStage и Future, но в отличии от Future - CompletableFuture может быть явно закомпличена. 
    
    **CompletableFuture** - фреймворк для обработки асинхронных событий, объекты которого используются часто и во внутренних библиотеках java, как возвращаемое значения выполнеения какого нибудь ассинхронного действия.

    .. container:: code-markup

        T ``get(...)``
            См. :ref:`FutureRef`

        T ``join()``
            Так же как и ``get`` ожидает завершения и возвращает значение, но в отличие от ``get`` кидает *unchecked exceptions* (**CompletionException** - if this future completed exceptionally or a completion computation threw an exception).

        T ``getNow​(T valueIfAbsent)``
            Если задача еше не была выполнена - возвращается valueIfAbsent.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `CompletableFuture <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/CompletableFuture.html>`_

        `CompletionStage <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/CompletionStage.html>`_

    .. container:: remark-block

        Добавлено в Java 8


Способы создания CompletableFuture
=======================================
.. container:: left-col

    .. container:: code-markup

        ``new``
            Создает незакомпличенное CompletableFuture.

            .. toggle-header::
                :header: Что бы закомплитить явно, нужно вызвать один из методов:

                .. container:: code-markup
    
                    boolean ``cancel``
                        If not already completed, completes this CompletableFuture with a CancellationException.
    
                    boolean ``complete​(T value)``
                        If not already completed, sets the value returned by ``get`` and related methods (``join``...) to the given value.
    
                    boolean ``completeExceptionally​(Throwable ex)``
                        If not already completed, causes invocations of ``get`` and related methods (``join``...) to throw the given exception.
    
                        (См. :ref:`ExHandlRef`)
    
                    CompletableFuture<T> ``completeAsync​(Supplier<out T> supplier[, Executor executor])``
                        Completes this CompletableFuture with the result of the given Supplier function invoked from an asynchronous task.
    
                        Returns this CompletableFuture.
    
                    CompletableFuture<T> ``completeOnTimeout​(T value, long timeout, TimeUnit unit)``
                        Completes this CompletableFuture with the given value if not otherwise completed before the given timeout.
    
                        Returns this CompletableFuture.

        static ``completedFuture​(U value)``
            Возвращает новое **CompletableFuture<U>**, которое уже закомпличено заданным значением.

        static ``supplyAsync​(Supplier<U> supplier[, Executor executor])``
            Returns a new **CompletableFuture<U>** которая комплитится, как только выполнится переданное действие (асинхронно).

        static ``runAsync​(Runnable runnable[, Executor executor])``
            Returns a new **CompletableFuture<Void>** которая комплитится, как только выполнится переданное действие (асинхронно).


    .. note::
        Сейчас и далее для всех асинхронных операций (\*Async - напр ``thenApplyAsync​``): если Executor не указан - выбирается для исполнения *commonPool* (а там все потоки демоны).

.. container:: right-col

    .. raw:: html

        <iframe width="640" height="360" frameborder="0" src="https://mega.nz/embed/XR8WjIAa#mDecucV08h-TIrqDG_1S4RREtnncP-yUFdlAQifEZbk!4921s" allowfullscreen ></iframe>


Объединение и пересечение результатов
-------------------------------------------------
.. container:: left-col

    .. container:: code-markup

        static CompletableFuture<Void> ``allOf​(CompletableFuture<?>... cfs)``
            Возвращает новое CompletableFuture, которое комплитится после завершения всех заданных CompletableFutures.

        static CompletableFuture<Object> ``anyOf​(CompletableFuture<?>... cfs)``
            Возвращает новое CompletableFuture, которое комплитится, когда завершается хоть одно из заданных CompletableFutures, с тем же результатом.


CompletionStage
==================================================
.. container:: left-col

    CompletionStage - Этап асинхронного (возможно) вычисления, выполняющееся, когда завершается другой CompletionStage.

    Async функции (\*Async - напр ``thenApplyAsync​``) выполняются в другом (указанном или *commonPool*) executor'е, нежели в том потоке, где происходило предыдущее действие.
    В противоположность не async-функциям, которые выполняются в том же потоке, где действие происходит. Но если действие уже было завершено - выполнение происходит в **текущем потоке** (в том откуда вызвана функция на CompletionStage).

    На один и тот-же CompletionStage можно прицепить несколько обработчиков, при этом если они **не async** - они будут выпоняться последовательно в последнем потоке обработки (если действие описанное в обработчике не завершилось)
    или в **текущем потоке** (если предыдущее действие завершилось). После выполнения всех действий - проверяется, нет ли еще обработчиков, которые нужно в данном потоке прогнать, и если их нет - работа в этом потоке завершается.
    Поэтому если выполнение завершено и добавляется еще действие в пайплайне - то это действие уже будет происходить в **текущем потоке**.

    .. tip::
        Стоит помнить, что при модификациях разделяемой памяти из разных витков CompletionStage - модифицируется все равно один и тот же объект:

        .. code-block:: java

    .. note::
        Для всех ниже перечисленных методов на CompletionStage доступны парные async-методы (кроме ``exceptionally​``) с опциональным параметром (**Executor** executor), 
        выполняемые асинхронно, либо в указаном **Executor**'е, либо в *FJP.commonPool* .

.. container:: right-col

    .. raw:: html

        <iframe width="640" height="360" frameborder="0" src="https://mega.nz/embed/KNlVTSIS#af25bdAOXLV428KHyVOy9HETe2tTpSEW3K2mguXjpGA" allowfullscreen ></iframe>


Подписка
----------------------------------
.. container:: left-col

    .. container:: code-markup

        ``thenRun​(Runnable action)``
            Выполняет специфицированное действие, при завершении выполнения предыдущего в пайплайне. Возвращает **CompletionStage<Void>**.

        ``thenAccept​(Consumer<? super T> action)``
            Выполняет специфицированное действие принимая значение полученное из предыдущего в пайплайне. Возвращает **CompletionStage<Void>**.

        

Трансформация (map)
------------------------------------
.. container:: left-col

    .. container:: code-markup

        ``thenApply​(Function<? super T,? extends U> fn)``
            Преобразует оборачиваемое значение внутри CompletionStage.
    

Комбинация (reduce)
------------------------------------
.. container:: left-col

    .. container:: code-markup

        ``thenCombine​(CompletionStage<out U> other, BiFunction<in T,in U,out V> fn)``
            Ожидает выполнения двух этапов (CompletionStage<T> и CompletionStage<U>) и склеивает их функцией ``fn``.



Композиция (flatMap)
------------------------------------
.. container:: left-col

    .. container:: code-markup

        ``thenCompose​(Function<in T,out CompletionStage<U>> fn)``
            Преобразует один CompletionStage в зависимый от него другой CompletionStage (возвращает новый CompletionStage, который завершается тем же значением, что и CompletionStage, возвращаемый данной функцией).

.. _ExHandlRef:

Обработка ошибок 
------------------------------------
.. container:: left-col

    ``join`` запихнет ошибку в **CompletionException**, а ``get`` в **ExecutionException**.

    .. note::
        Ошибка пробрасывается по цепочкам, т.е. обработчики ожидающие значение, а принимающие ошибку - не будут выполняться, а лишь пробросят ошибку далее.

    .. container:: code-markup

        ``exceptionally​(Function<Throwable,out T> fn)``
            Функция ``fn`` применяется, если на одном из предыдущих обработчиков в пайплайне было брошено исключение. 
            При этом от момента исключения до метода *exceptionally​*, обработчики не будут выполняться (ошибка пробрасывается по цепочкам), и наоборот *exceptionally​* не будет выполняться, если не было получено исключение.

        ``handle​(BiFunction<? super T,Throwable,? extends U> fn)``
            Так же как *exceptionally​* принимает исключение, но наряду с ним так же принимает предыдущее значение из пайплайна. 
            Не трудно догадаться что либо исключение, либо значение от пред. обработчика будет == *null*.
            