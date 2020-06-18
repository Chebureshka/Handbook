.. include:: ../../defs.rst

Executors
********************************

.. contents::
    :local:

ExecutorService
===============

.. container:: left-col

    **Executor** - Объект, который выполняет переданные **Runnable** задачи.

    .. container:: code-markup

        void ``execute​(Runnable command)``
            Выполняет переданную команду в контексте данного **Executor**.

    **ExecutorService** - **Executor**, который предоставляет методы для управления завершением и методы, 
    которые могут создать **Future** для отслеживания хода выполнения одной или нескольких асинхронных задач.

    .. container:: code-markup

        Future<?> ``submit​(Runnable task)`` |br| Future<T> ``submit​(Runnable task, T result)`` |br| Future<T> ``submit​(Callable<T> task)``
            Передает задачу (**Runnable**/**Callable**) для выполнения и возвращает Future_, представляющий эту задачу. 
            Метод ``future.get()`` вернет данный результат после успешного завершения.

            **RejectedExecutionException** - Если задача не может быть добавлена для выполнения.

            **NullPointerException** - If the task is null.

        void ``shutdown()``
            Инициирует *orderly shutdown*, при котором выполняются ранее отправленные задачи, но новые задачи не принимаются. 
            
            Вызов ``shutdown()`` не имеет никакого дополнительного эффекта, если он уже вызывался.

        List<Runnable> ``shutdownNow()``
            Пытается остановить все активно выполняющиеся задачи, останавливает обработку ожидающих задач и возвращает список задач, ожидающих выполнения.

            Этот метод не ожидает завершения активного выполнения задач. Данные задачи остановятся в конечном счете (``Thread.interrupt()``)

        boolean ``isShutdown()``
            Returns true if this executor has been shut down.

        boolean ``isTerminated()``
            Возвращает true, если все задачи были выполнены после завершения работы.

            .. note::
                Note that ``isTerminated`` is never true unless either ``shutdown`` or ``shutdownNow`` was called first.


        boolean ``awaitTermination​(long timeout, TimeUnit unit)``
            Блокируется до тех пор, пока все задачи не завершат выполнение после запроса на выключение, или не истечет время ожидания, или текущий поток не будет прерван, в зависимости от того, что произойдет раньше.

        List<Future<T>> |br| |dl| ``invokeAll​(Collection<? extends Callable<T>> tasks)`` |br| |dl| ``invokeAll​(Collection<? extends Callable<T>> tasks, long timeout, TimeUnit unit)``
            Executes the given tasks, returning a list of Futures holding their status and results when all complete or the timeout expires, whichever happens first.

        T |br| |dl| ``invokeAny​(Collection<? extends Callable<T>> tasks)`` |br| |dl| ``invokeAny​(Collection<? extends Callable<T>> tasks, long timeout, TimeUnit unit)``
            Executes the given tasks, returning the result of one that has completed successfully (i.e., without throwing an exception), if any do before the given timeout elapses.



.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Interface Executor <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/Executor.html>`_

        `Interface ExecutorService <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/ExecutorService.html>`_

    .. thumbnail:: ../../_static/executors.PNG

    |br|

    .. container:: remark-block

        .. toggle-header::
            :header: Пример

            .. code-block:: java

                // https://repl.it/@Enota/ExecutorService?lite=true

                public static void main(String[] args) {

                  ExecutorService executor = Executors.newFixedThreadPool(5);

                  // Runnable, return void, nothing, submit and run the task async
                  executor.submit(() -> System.out.println("I'm Runnable task."));

                  // Callable, return a future, submit and run the task async
                  Future<Integer> futureTask1 = executor.submit(() -> {
                    System.out.println("I'm Callable task.");
                    return 1 + 1;
                  });

                  try {

                    otherTask("Before Future Result");

                    // block until future returned a result,
                    // timeout if the future takes more than 5 seconds to return the result
                    Integer result = futureTask1.get(5, TimeUnit.SECONDS);

                    System.out.println("Get future result : " + result);

                    otherTask("After Future Result");

                  } catch (InterruptedException e) {// thread was interrupted
                    e.printStackTrace();
                  } catch (ExecutionException e) {// thread threw an exception
                    e.printStackTrace();
                  } catch (TimeoutException e) {// timeout before the future task is complete
                    e.printStackTrace();
                  } finally {

                    // shut down the executor manually
                    executor.shutdown();
                  }
                }

                private static void otherTask(String name) {
                  System.out.println("I'm other task! " + name);
                }

.. _FutureRef:

Future
========
.. container:: left-col

    Интерфейс для получения результатов работы асинхронной операции. 
    
    Ключевым методом здесь является метод ``get``, который блокирует текущий поток (с таймаутом или без) 
    до завершения работы асинхронной операции в другом потоке. 
    
    Также, дополнительно существуют методы для отмены операции и проверки текущего статуса. 

    В качестве имплементации часто используется класс `FutureTask <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/FutureTask.html>`_.

    .. container:: code-markup

        boolean ``cancel​(boolean mayInterruptIfRunning)``
            Пытается остановить выполнение таски.

            Эта попытка потерпит неудачу, если задача уже выполнена, уже отменена или не может быть отменена по какой-либо другой причине, в таком случае функция вернет ``false``.

            ``mayInterruptIfRunning`` - ``true``, если поток, выполняющий эту задачу, should be ``interrupted``; в противном случае выполняемые задачи могут быть завершены.

        V ``get()`` |br| |dl| throws ``InterruptedException, ExecutionException``
            При необходимости ожидает завершения вычисления, а затем извлекает его результат.

            **CancellationException** - Если поцесс в таске был отменен.

            **ExecutionException** - Если в результате выполнения таски возникло исключение.

            **InterruptedException** - Если текущий поток был прерван во время ожидания.


        V ``get​(long timeout, TimeUnit unit)`` |br| |dl| throws ``InterruptedException, ExecutionException, TimeoutException``
            При необходимости ожидает не более заданного времени для завершения вычисления, а затем извлекает его результат, если он доступен.

            **TimeoutException** - Если время ожидания истекло

        boolean ``isCancelled()``
            Возвращает ``true``, если эта задача была отменена до ее нормального завершения.

        boolean ``isDone()``
            Возвращает ``true``, если эта задача выполнена. 

            Завершение может быть связано с обычным завершением, исключением или отменой (``cancel()``) - во всех этих случаях этот метод вернет ``true``.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Interface Future[V] <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/Future.html>`_


ThreadPoolExecutor
============================
.. container:: left-col

    Используется для запуска асинхронных задач в пуле потоков. Тем самым практически полностью отсутствует оверхэд на поднятие и остановку потоков. 
    А за счет фиксируемого максимума потоков в пуле обеспечивается прогнозируемая производительность приложения.

    Создавать данный пул предпочтительно через один из методов фабрики :ref:`RefExecutors`.
    Если же стандартных конфигураций будет недостаточно, то через конструкторы или сеттеры можно задать все основые параметры пула.

    Внутри находится BlockingQueue добавленных тасок. При вызове ``submit`` или ``execute​`` происходит добавление в эту очередь. При вызове ``shutdown`` в очередь добавляется 
    `poison pill <https://medium.com/@iamtrk/poison-pill-pattern-in-java-concurrency-b63ce1860df2>`_ при этом сами выполняемые задачи ничего никак не уведомляются.
    При попытке изъятия задачи из очереди проверяется является ли она poison pill, и если да - потоки в пуле стопятся и дальнейшее добавление задач не происходит.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ThreadPoolExecutor <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/ThreadPoolExecutor.html#shutdown()>`_

    .. container:: remark-block

        .. toggle-header::
            :header: Конструктор

            .. container:: code-markup

                public ``ThreadPoolExecutor​(`` |br| |dl| int ``corePoolSize``, |br| |dl| int ``maximumPoolSize``, |br| |dl| long ``keepAliveTime``, |br| |dl| TimeUnit ``unit``, |br| |dl| BlockingQueue<Runnable> ``workQueue``, |br| |dl| ThreadFactory ``threadFactory``, |br| |dl| RejectedExecutionHandler ``handler``  |br|  ``)``
                    Creates a new ThreadPoolExecutor

                    .. container:: code-markup

                        corePoolSize 
                            The number of threads to keep in the pool, even if they are idle, unless allowCoreThreadTimeOut is set.

                        maximumPoolSize 
                            The maximum number of threads to allow in the pool

                        keepAliveTime 
                            When the number of threads is greater than the core, this is the maximum time that excess idle threads will wait for new tasks before terminating.

                        unit
                            The time unit for the keepAliveTime argument

                        workQueue 
                            The queue to use for holding tasks before they are executed. This queue will hold only the Runnable tasks submitted by the execute method.

                        threadFactory 
                            The factory to use when the executor creates a new thread. Опциональный

                        handler 
                            The handler to use when execution is blocked because the thread bounds and queue capacities are reached. Опциональный


.. _RefExecutors:

Executors
============================
.. container:: left-col

    .. container:: code-markup

        static ThreadFactory ``defaultThreadFactory()``
            Возвращает ThreadFactory используемую по умолчанию, для создания новых потоков.

            Данная ThreadFactory создает все новые потоки, используемые Executor в той же ThreadGroup

        static ExecutorService |br| |dl| ``newCachedThreadPool()`` |br| |dl| ``newCachedThreadPool​(ThreadFactory threadFactory)``
            Создает пул потоков, создающий новые потоки по мере необходимости, а так же будет повторно использовать ранее созданные потоки, когда они доступны. 

            .. toggle-header::
                :header: Определение

                .. code-block:: java

                    public static ExecutorService newCachedThreadPool(ThreadFactory threadFactory) {
                        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                                      60L, TimeUnit.SECONDS,
                                                      new SynchronousQueue<Runnable>(),
                                                      threadFactory);
                    }

        static ExecutorService |br| |dl| ``newFixedThreadPool​(int nThreads)`` |br| |dl| ``newFixedThreadPool​(int nThreads, ThreadFactory threadFactory)``
            Создает пул потоков, который повторно использует фиксированное число потоков, работающих в общей неограниченной очереди.

            .. toggle-header::
                :header: Определение

                .. code-block:: java

                    public static ExecutorService newFixedThreadPool(int nThreads, ThreadFactory threadFactory) {
                        return new ThreadPoolExecutor(nThreads, nThreads,
                                                      0L, TimeUnit.MILLISECONDS,
                                                      new LinkedBlockingQueue<Runnable>(),
                                                      threadFactory);
                    }



        static ScheduledExecutorService |br| |dl| ``newScheduledThreadPool​(int corePoolSize)`` |br| |dl| ``newScheduledThreadPool​(int corePoolSize, ThreadFactory threadFactory)``
            Создает пул потоков, который может запланировать выполнение команд после заданной задержки / периодическое выполнение.

            .. toggle-header::
                :header: Определение

                .. code-block:: java

                    public static ScheduledExecutorService newScheduledThreadPool(
                            int corePoolSize, ThreadFactory threadFactory) {
                        return new ScheduledThreadPoolExecutor(corePoolSize, threadFactory);
                    }


        static ExecutorService |br| |dl| ``newSingleThreadExecutor()`` |br| |dl| ``newSingleThreadExecutor​(ThreadFactory threadFactory)``
            Создает Executor, который использует один рабочий поток, работающий в неограниченной очереди.

            .. toggle-header::
                :header: Определение

                .. code-block:: java

                    public static ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory) {
                        return new FinalizableDelegatedExecutorService
                            (new ThreadPoolExecutor(1, 1,
                                                    0L, TimeUnit.MILLISECONDS,
                                                    new LinkedBlockingQueue<Runnable>(),
                                                    threadFactory));
                    }



        static ScheduledExecutorService |br| |dl| ``newSingleThreadScheduledExecutor()`` |br| |dl| ``newSingleThreadScheduledExecutor​(ThreadFactory threadFactory)``
            Создает однопоточный Executor, который может запланировать выполнение команд после заданной задержки / периодическое выполнение.

            .. toggle-header::
                :header: Определение

                .. code-block:: java

                    public static ScheduledExecutorService newSingleThreadScheduledExecutor(ThreadFactory threadFactory) {
                        return new DelegatedScheduledExecutorService
                            (new ScheduledThreadPoolExecutor(1, threadFactory));
                    }



        static ExecutorService |br| |dl| ``newWorkStealingPool()`` |br| |dl| ``newWorkStealingPool​(int parallelism)``
            Создает такой *work-stealing* пул, в котором достаточно потоков для поддержки заданного уровня параллелизма, и может использовать несколько очередей для уменьшения конкуренции.

            Уровень параллелизма соответствует максимальному количеству потоков, активно участвующих или доступных для обработки задач.

            (см. ForkJoinPool_)

            .. toggle-header::
                :header: Определение

                .. code-block:: java

                    public static ExecutorService newWorkStealingPool(int parallelism) {
                        return new ForkJoinPool
                            (parallelism,
                             ForkJoinPool.defaultForkJoinWorkerThreadFactory,
                             null, true);
                    }

                    public static ExecutorService newWorkStealingPool() {
                        return new ForkJoinPool
                            (Runtime.getRuntime().availableProcessors(),
                             ForkJoinPool.defaultForkJoinWorkerThreadFactory,
                             null, true);
                    }


        static ExecutorService |br| |dl| ``unconfigurableExecutorService​(ExecutorService executor)`` |br| static ScheduledExecutorService |br| |dl| ``unconfigurableScheduledExecutorService​(ScheduledExecutorService executor)``
            Возвращает объект, который делегирует все определенные методы ExecutorService данному Executor, 
            но не любые другие методы, которые в противном случае могли бы быть доступны с помощью приведений (например для переконфигурирования).

            .. toggle-header::
                :header: Определение

                .. code-block:: java

                    public static ExecutorService unconfigurableExecutorService(ExecutorService executor) {
                        if (executor == null)
                            throw new NullPointerException();
                        return new DelegatedExecutorService(executor);
                    }

                    public static ScheduledExecutorService unconfigurableScheduledExecutorService(ScheduledExecutorService executor) {
                        if (executor == null)
                            throw new NullPointerException();
                        return new DelegatedScheduledExecutorService(executor);
                    }


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Executors <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/Executors.html>`_

        `Java: ExecutorService <https://habr.com/ru/post/116363/>`_

    .. raw:: html 

        <iframe width="640" height="360" frameborder="0" src="https://mega.nz/embed/zYcFhQyT#twYjAr-Aw46xNVq9ovCnMwkn3RDn_FYiw6NhQiTDz8s!2581s" allowfullscreen ></iframe>




ForkJoinPool
============
.. container:: left-col
    
    Разделяй и властвуй

Балансировка задач между потоками
---------------------------------

.. container:: left-col

    .. raw:: html

        <div style="text-align:right; font-style: italic">
            В недрах тундры выдры в гетрах тырят в вёдра ядра кедра
        </div>

    |br|

    .. warning::
        Содержаться некоторые противроречия, указанные в блоках warning, во внутренней работе, так как многие источники говорят по-разному.
        Вот и хз кому верить Шипилеву или интернету.

        Вот тебе документация, читай: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html

    Work arbitrage 
        Внешний ресурс распределяет какой воркер будет брать ту или иную задачу

    Work dealing 
        При большой загруженности поток сам отдает задачу "соседу"

    Work stealing 
        При отсутствии задач поток забирает задачу у "соседа" (ForkJoinPool)


    Внутри ForkJoinPool есть N воркеров. Внутри каждого воркера есть (lock free) очередь задач которую он исполняет, из конца которой другие воркеры могут забрать задачу.

    Добавление задач в ForkJoinPool происходит в одну из (случайную) отдельных (входных) очередей для внешних задач (в хвост). Для входных очередей используется синхронизация.

    После добавления один из ожидающих воркеров (без задач и без задач у соседей) оповещается, что появилась новая задача.

    При появлении задач в очереди воркера, которые еще не исполняются - оповещаются соседние воркеры, которые берут эти самые задачи.

    При выполнении разделения задач в воркере (fork) они отправляются ему же в очередь задач (в голову).

    При оповещении воркера он сначала смотрит - можно ли у соседей украсть задачу (?), если нет - он идет в очереди входных задач (в случайной последовательности (так как он блокирует очередь), пока не найдет таску).

    .. container:: code-markup

        ``ForkJoinPool(`` |br| |dl| int ``parallelism``, ForkJoinPool.ForkJoinWorkerThreadFactory ``factory``, |br| |dl| Thread.UncaughtExceptionHandler ``handler``, boolean ``asyncMode`` |br| ``)``
            Создает экземпляр FJP.
            
            *asyncMode* - если true, устанавливает локальный FIFO scheduling mode для разветвленных задач, которые никогда не джойнятся. 
            This mode may be more appropriate than default locally stack-based mode in applications in which worker threads only process event-style asynchronous tasks. 
            
                For default value, use false.


        ForkJoinPool ``ForkJoinPool.commonPool()``
            Возвращает экземпляр common pool (*commonPool* полон демонов). Этот пул был построен статически; на его состояние не влияют вызовы ``shutdown()`` или ``shutdownNow()``.

            Any program that relies on asynchronous task processing to complete before program termination should invoke ``commonPool()``.

            Данный пул является Executor'ом по умолчанию (т.е. если вызов ``task.fork``/``task.invoke``... происходит вне контекста какого либо FJP - выбирается ``commonPool()``, 
            иначе тот FJP, в котором сейчас происходит вызов (см ``fork`` в ForkJoinTask_)). Например stream.parallel(), асинхронные действия с CompletableFuture, паралельные сортировки и тд.

            Размер (параллелизм) данного пула равен количеству ядер - 1 (вызов ``task.invoke``/``pool.invoke`` вне ForkJoinPool - вызвавший поток становится воркером для исполнения переданной задачи)

        T ``invoke(ForkJoinTask<T> task)``
            Performs the given task, returning its result upon completion.

            Вызванный из этого самого пула, в которм происходит действие - просто добавляет задачу текущему пулу.

            После добавления таски происходит ``join`` на ней из текущего потока. ~ Поэтому текущий поток становится одним из воркеров.

        void ``execute(ForkJoinTask<?> task)``
            Arranges for (asynchronous) execution of the given task.

        ForkJoinTask<T> ``submit(ForkJoinTask<T> task)``
            Submits a ForkJoinTask for execution (asynchronous).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Be Aware of ForkJoinPool#commonPool() <https://dzone.com/articles/be-aware-of-forkjoinpoolcommonpool>`_

    .. raw:: html

        <iframe width="560" height="315" src="https://www.youtube.com/embed/t0dGLFtRR9c" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

    .. toggle-header::
        :header: Презентация

        .. raw:: html

            <iframe src="../../_static/jeeconf-May2012-forkjoin.pdf" width="620" height="580"></iframe>

    |br|

    .. figure:: ../../_static/608570fc29847565_.png

ForkJoinTask
-------------------------------------------

.. container:: left-col

    Наледуется от **Future<V>** и имеет 2 предоставляемых субинтерфейса **RecursiveAction** (без результата выполнения), **RecursiveTask** (с результатом выполнения).

    .. container:: code-markup

        ``fork`` 
            Запуск на выполнение новой подзадачи, задачи отправляются в голову. Со стороны воркера формируемые задачи в голове организуют стек (текущая <- [третяя, вторая, первая, ..., то что могут забрать]).

            .. tip::
                Using the ``ForkJoinPool.commonPool()`` if not in ForkJoinPool. 

        ``join`` 
            Семантика приостановления текущей задачи, до завершения указанной.

            - Если в голове очереди текущего воркера есть требуемая задача - он начинает выполнять ее и возвращается по завершению к предыдущей задаче.

            - Если требуемая задача не в голове очереди, но все же есть в ней - выполнить ее и т.д. (чуть менее эффективно)

            - ~ Если требуемой задачи нет в очереди - пытаемся узнать кто ее забрал, и забираем ее обратно, либо забираем поражденные требуемой задачей задачи. 

              .. warning:: 
                    Скорее стырим просто другую задачу. Но нужно еще поразбираться!

            - ~ Или же разгребаем свои задачи или помогаем с задачами взявшему.

              .. warning::
                    ПРОТИВОРЕЧИТ: Охотимся только за зависимыми задачами (Иначе рискуем starvation'ом внутренних задач под напором внешних). Но Шипилев говорит по другому

            - Или блокируемся, даже если есть внешние задачи

              .. warning::
                  Что еще более не выгодно. Но куда мне.

            После завершения выполнения другой задачи воркер смотрит появился ли результат, если нет продолжает выполнение других задач.

            .. tip::
                ~ Если вызван не из потоков **ForkJoinPool** - текущий поток становится одним из воркеров до получения результата.

            .. note::
                Так как ForkJoinTask является Future - вызов ``get`` на нем на самом деле является вызовом ``join``.

        ``invokeAll`` 
            Ожидает выполнения всех переданных тасок. ``join`` на них не будет ожидать выполнения.

        ``invoke`` 
            ~ Вызванный не из потоков **ForkJoinPool** текущий поток становится одним из воркеров ``commonPool`` (так же как и ``fork`` но вызвавший поток становится одним из воркеров вследствии вызова ``join``).

            ~ Если же вызван из **ForkJoinPool** - семантика ``fork`` и затем ``join``.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ForkJoinTask <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/ForkJoinTask.html#fork-->`_

        `RecursiveAction <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/RecursiveAction.html>`_

        `RecursiveTask <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/RecursiveTask.html>`_

    .. code-block:: java 

        class Fibonacci extends RecursiveTask<Integer> {
          final int n;
          Fibonacci(int n) { this.n = n; }
          protected Integer compute() {
            if (n <= 1)
              return n;
            Fibonacci f1 = new Fibonacci(n - 1);
            f1.fork();
            Fibonacci f2 = new Fibonacci(n - 2);
            return f2.compute() + f1.join();
          }
        }

        var result1 = ForkJoinPool().invoke(Fibonacci(10))

        // commonPool if outside of FJP
        var result2 = Fibonacci(10).invoke()

    .. code-block:: java 

        static class SortTask extends RecursiveAction {
          final long[] array; final int lo, hi;

          SortTask(long[] array, int lo, int hi) {
            this.array = array; this.lo = lo; this.hi = hi;
          }

          SortTask(long[] array) { 
            this(array, 0, array.length); 
          }

          protected void compute() {
            if (hi - lo < THRESHOLD)
              sortSequentially(lo, hi);
            else {
              int mid = (lo + hi) >>> 1;
              invokeAll(new SortTask(array, lo, mid),
                        new SortTask(array, mid, hi));
              merge(lo, mid, hi);
            }
          }

          // implementation details follow:
          static final int THRESHOLD = 1000;

          void sortSequentially(int lo, int hi) {
            Arrays.sort(array, lo, hi);
          }

          void merge(int lo, int mid, int hi) {
            long[] buf = Arrays.copyOfRange(array, lo, mid);
            for (int i = 0, j = lo, k = mid; i < buf.length; j++)
              array[j] = (k == hi || buf[i] < array[k]) ?
                buf[i++] : array[k++];
          }
        }

Выбор ширины
--------------

.. container:: left-col

    .. math:: T = N / (C * L)

    T - число, означающее нужно ли дробить задачу на подзадачи (>T) |br|
    N - размер задачи |br|
    C - количество CPU |br|
    L - load factor [10..100]

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Закон Густавсона — Барсиса <https://ru.wikipedia.org/wiki/%D0%97%D0%B0%D0%BA%D0%BE%D0%BD_%D0%93%D1%83%D1%81%D1%82%D0%B0%D0%B2%D1%81%D0%BE%D0%BD%D0%B0_%E2%80%94_%D0%91%D0%B0%D1%80%D1%81%D0%B8%D1%81%D0%B0>`_

        `Закон Амдала <https://ru.wikipedia.org/wiki/%D0%97%D0%B0%D0%BA%D0%BE%D0%BD_%D0%90%D0%BC%D0%B4%D0%B0%D0%BB%D0%B0>`_

Блокировка
------------

.. container:: left-col

    A ManagedBlocker provides two methods:

    .. container:: code-markup

        boolean ``isReleasable()``
            Возвращает true, если блокировка не нужна.

        boolean ``block()``
            Possibly blocks the current thread, for example waiting for a lock or condition.

    Эти действия выполняются любым потоком, вызывающим ``ForkJoinPool.managedBlock(ManagedBlocker)``

    .. container:: code-markup

        ``ForkJoinPool.managedBlock(ForkJoinPool.ManagedBlocker blocker)``
            Runs the given possibly blocking task.

    .. code-block:: java 

         class ManagedLocker implements ManagedBlocker {
           final ReentrantLock lock;
           boolean hasLock = false;

           ManagedLocker(ReentrantLock lock) { this.lock = lock; }

           public boolean block() {
             if (!hasLock)
               lock.lock();
             return true;
           }

           public boolean isReleasable() {
             return hasLock || (hasLock = lock.tryLock());
           }
         }
    

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Interface ForkJoinPool.ManagedBlocker <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/ForkJoinPool.ManagedBlocker.html#block-->`_

        `Example of ManagedBlocker in Java <https://www.concretepage.com/java/jdk7/example-of-managedblocker-in-java>`_

    .. container:: remark-block

        ~ Блокировки через **ManagedBlocker** блокирют текущий поток (block) при этом создается новый поток в ForkJoinPool, на котором будут производится исполнения до пробуждения (isReleasable) первого.
