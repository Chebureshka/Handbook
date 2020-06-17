.. include:: ../../defs.rst

Synchronizers
*******************************************************

.. contents:: 
    :local:

Thread
==================

.. container:: left-col

    **Получение Thread объекта**:

        .. container:: code-markup
    
            static Thread ``currentThread()``
                Returns a reference to the currently executing thread object.
    
            ``Thread​(`` |br| |dl| ThreadGroup ``group``, Runnable ``target``, String ``name``, |br| |dl| long ``stackSize``, boolean ``inheritThreadLocals`` |br| ``)``
                Allocates a new Thread object so that it has target as its run object, has the specified name as its name, belongs to the thread group referred to by group, has the specified stackSize, and inherits initial values for inheritable thread-local variables if inheritThreadLocals is true.
    
                Все параметры опциональны.
    
                **ThreadGroup**:
    
                    A thread group represents a set of threads. In addition, a thread group can also include other thread groups (thread groups form a tree).
    
            void ``run()``
                Представляет иной способ определения поведения потока.
    
                .. toggle-header::
                    :header: Пример
    
                    .. code-block:: java 
    
                        class PrimeThread extends Thread {
                            long minPrime;
                            PrimeThread(long minPrime) {
                                this.minPrime = minPrime;
                            }
    
                            public void run() {
                                // compute primes larger than minPrime
                                 . . .
                            }
                        }
    
                        PrimeThread p = new PrimeThread(143);
                        p.start();
    
                Если поток был создан с использованием объекта **Runnable**, то вызывается метод ``run`` из Runnable; otherwise, this method does nothing and returns.
    



    **Контроль жизненного цикла потока**:

        .. container:: code-markup

            Thread.State ``getState()``
                Returns the state of this thread.

                .. toggle-header::
                    :header: Thread.State

                    .. container:: code-markup

                        NEW
                            A thread that has not yet started is in this state.

                        RUNNABLE
                            A thread executing in the Java virtual machine is in this state.

                        BLOCKED
                            A thread that is blocked waiting for a monitor lock is in this state.

                        WAITING
                            A thread that is waiting indefinitely for another thread to perform a particular action is in this state.

                        TIMED_WAITING
                            A thread that is waiting for another thread to perform an action for up to a specified waiting time is in this state.

                        TERMINATED
                            A thread that has exited is in this state.

            void ``start()``
                Causes this thread to begin execution; the Java Virtual Machine calls the ``run`` method of this thread (or Runnable).

            void ``interrupt()``
                Interrupts this thread.

            static void ``sleep​(long millis)`` |br| static void ``sleep​(long millis, int nanos)``
                Causes the currently executing thread to sleep (temporarily cease execution) for the specified number of milliseconds [plus the specified number of nanoseconds, subject to the precision and accuracy of system timers and schedulers].

                .. toggle-header::
                    :header: TimeUnit.sleep

                    Вызов метода ``sleep(long timeout)`` на одном из инстансов `TimeUnit <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/TimeUnit.html>`_ выполняет ``Threa.sleep`` используя указанную еденицу времени.
                

            void ``join()`` |br| void ``join​(long millis)`` |br| void ``join​(long millis, int nanos)``
                Waits for this thread to die.

            static void ``yield()``
                Подсказка к планировщику, что текущий поток готов уступить текущее использование процессорного времени.



    **Конфигурация потоков**:

        .. container:: code-markup

            void ``setName​(String name)``
                Changes the name of this thread to be equal to the argument name.

            void ``setPriority​(int newPriority)``
                Changes the priority of this thread.

            void ``setDaemon​(boolean on)``
                Marks this thread as either a daemon thread or a user thread.

        .. toggle-header::
            :header: Расширенный список

            |br|

            .. container:: code-markup

                static void ``setDefaultUncaughtExceptionHandler​(`` |br| |dl| Thread.UncaughtExceptionHandler ``eh`` |br| ``)``
                    Set the default handler invoked when a thread abruptly terminates due to an uncaught exception, and no other handler has been defined for that thread.

                void ``setUncaughtExceptionHandler​(`` |br| |dl| Thread.UncaughtExceptionHandler ``eh`` |br| ``)``
                    Set the handler invoked when this thread abruptly terminates due to an uncaught exception.

                void ``setContextClassLoader​(ClassLoader cl)``
                    Sets the context ClassLoader for this Thread.


    **Получение информации о потоке**:

        .. container:: code-markup

            static boolean ``interrupted()``
                Tests whether the current thread has been interrupted.

            boolean ``isInterrupted()``
                Tests whether this thread has been interrupted.

            boolean ``isAlive()``
                Tests if this thread is alive.

            boolean ``isDaemon()``
                Tests if this thread is a daemon thread.

            long ``getId()``
                Returns the identifier of this Thread.

            String ``getName()``
                Returns this thread's name.

            int ``getPriority()``
                Returns this thread's priority.

            StackTraceElement[] ``getStackTrace()``
                Returns an array of stack trace elements representing the stack dump of this thread.

            static void ``dumpStack()``
                Prints a stack trace of the current thread to the standard error stream.


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Thread scheduling and wait/notify <https://www.javaworld.com/article/2071214/java-101--understanding-java-threads--part-3--thread-scheduling-and-wait-notify.html>`_

        `Memory Management in Java <https://www.javatpoint.com/memory-management-in-java>`_

        `How does Java (JVM) allocate stack for each thread (stackoverflow) <https://stackoverflow.com/questions/36898701/how-does-java-jvm-allocate-stack-for-each-thread>`_

    .. container:: links-block

        .. rubric:: Документация:

        `Class Thread <https://docs.oracle.com/javase/10/docs/api/java/lang/Thread.html>`_

        `Class ThreadGroup <https://docs.oracle.com/javase/10/docs/api/java/lang/ThreadGroup.html>`_

        `Enum Thread.State <https://docs.oracle.com/javase/10/docs/api/java/lang/Thread.State.html>`_

        `Interface ThreadFactory <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/ThreadFactory.html>`_
    
    .. container:: remark-block

        A thread is a thread of execution in a program. The Java Virtual Machine allows an application to have multiple threads of execution running concurrently.

        Наследуется от **Runnable**.

    .. container:: defs-block

        .. rubric:: Thread scheduling

        Either the JVM or the underlying platform's operating system deciphers how to share the processor resource among threads—a task known as *thread scheduling*. That portion of the JVM or operating system that performs thread scheduling is a *thread scheduler*.

        Every thread has a priority. Threads with higher priority are executed in preference to threads with lower priority.

        .. figure:: ../../_static/base6482949728376bd328_.png

    .. container:: defs-block

        .. rubric:: Memory managment

        Each Java Virtual Machine thread has a private Java Virtual Machine stack, created at the same time as the thread.

        Java thread stack in HotSpot is software managed, it is not an OS native thread stack.

        The minimum stack size in HotSpot for a thread seems to be fixed. This is what the aforementioned **-Xss** option is for.

        .. figure:: ../../_static/memory-management-in-java_.png
        
        JVM может использовать метод, называемый escape-анализом, с помощью которого она может сказать, 
        что определенные объекты остаются ограниченными одним потоком в течение всего их времени жизни, и что время жизни ограничено временем жизни данного стекфрейма.
        Такие объекты можно безопасно размещать в стеке, а не в куче.

        .. thumbnail:: ../../_static/1ai0j_.png

        `More about this in the specification <https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.5.6>`_



Примитивы синхронизации. wait() / notify()
==============================================

.. container:: left-col

    .. rubric:: Определения:
    
    Класс Object в Java содержит три ``final`` метода для взаимодействия потоков. Это методы ``wait()``, ``notify()`` и ``notifyAll()``


    .. container:: code-markup

        ``wait()``
            Метод ``wait()`` бесконечно ждет другой поток, пока не будет вызван метод ``notify()`` или ``notifyAll()`` на объекте. Другие две вариации метода ``wait()`` ставят текущий поток в ожидание на определенное время. По истечении этого времени поток просыпается и продолжает работу.

        ``notify()``
            Пробуждает только один поток, после чего этот поток начинает выполнение. Если объект ожидают несколько потоков, то метод ``notify()`` разбудит только один из них. Выбор потока зависит от системной реализации управления потоками.

        ``notifyAll()``
            Пробуждает все потоки, хотя в какой последовательности они будут пробуждаться зависит от реализации ОС.

    Для всех вышеприведенных методов требуется получение блокировки (synchronized) на мониторе:

        **IllegalMonitorStateException** - if the current thread is not the owner of this object's monitor.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Object <https://docs.oracle.com/javase/10/docs/api/java/lang/Object.html>`_

        `Пример использования <https://javadevblog.com/primer-ispol-zovaniya-wait-notify-i-notifyall-v-java.html>`_
    
        `Baeldung <https://www.baeldung.com/java-wait-notify>`_

    .. rubric:: Пример:

    .. code-block:: java

        void waiting() {

            synchronized (monitor) {
                while(waitCondition) {
                    try {
                        monitor.wait();
                    } catch(InterruptedException e) {
                        ...
                    }
                }
            }
        }

        void notifier() {

            synchronized (monitor) {
                waitCondition = false;
                monitor.notify();
            }
        }



java.util.concurrent.Synchronizers
==============================================
.. container:: left-col

    Синхронизаторы – вспомогательные утилиты для синхронизации потоков, которые дают возможность разработчику регулировать и/или ограничивать работу потоков и предоставляют 
    более высокий уровень абстракции, чем основные примитивы языка (мониторы).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Справочник по синхронизаторам java.util.concurrent.* <https://habr.com/ru/post/277669/>`_


Semaphore
-----------------------------

.. container:: left-col

    Синхронизатор Semaphore реализует шаблон синхронизации Семафор. 
    Чаще всего, семафоры необходимы, когда нужно ограничить доступ к некоторому общему ресурсу.

    .. figure:: ../../_static/9da48f85b5874362bc2279f181613c0e.gif

CountDownLatch
-----------------------------

.. container:: left-col

    Предоставляет возможность любому количеству потоков в блоке кода ожидать до тех пор, пока не завершится определенное количество операций, выполняющихся в других потоках, перед тем как они будут «отпущены»

    .. figure:: ../../_static/46b3aeb417cf4fb4ba271b4c66b52436.gif

CyclicBarrier
-----------------------------

.. container:: left-col

    Циклический барьер является точкой синхронизации, в которой указанное количество параллельных потоков встречается и блокируется. Как только все потоки прибыли, выполняется опционное действие (или не выполняется, если барьер был инициализирован без него), и, после того, как оно выполнено, барьер ломается и ожидающие потоки «освобождаются».

    .. figure:: ../../_static/89af0cb71aad4465bb9c934b8be91a67.gif

Exchanger<V>
-----------------------------

.. container:: left-col

    Exchanger (обменник) может понадобиться, для того, чтобы обменяться данными между двумя потоками в определенной точки работы обоих потоков.

    .. figure:: ../../_static/947ef3f47ff843a099059006b30ea54d.gif

Phaser
-----------------------------

.. container:: left-col

    Phaser (фазер), как и CyclicBarrier, является реализацией шаблона синхронизации Барьер, но, в отличии от CyclicBarrier, предоставляет больше гибкости. 
    Этот класс позволяет синхронизировать потоки, представляющие отдельную фазу или стадию выполнения общего действия. 
    Как и CyclicBarrier, Phaser является точкой синхронизации, в которой встречаются потоки-участники. 
    Когда все стороны прибыли, Phaser переходит к следующей фазе и снова ожидает ее завершения.

    .. figure:: ../../_static/0866a4b7acdf416384d4e7372b49a34b.gif

Locks
======================================
.. container:: left-col

    Представляет собой альтернативные и более гибкие механизмы синхронизации потоков по сравнению с базовыми ``synchronized``, ``wait``, ``notify``, ``notifyAll``.

    .. figure:: ../../_static/4f1edf0f8640f54475bf37ff72d04895.png


.. container:: right-col

    .. container:: links-block
    
        .. rubric:: Ссылки:
    
        `java.util.concurrent.* <https://habr.com/ru/company/luxoft/blog/157273/>`_


Lock
-----
.. container:: left-col

    Lock — Базовый интерфейс из lock framework, предоставляющий более гибкий подход по ограничению доступа к ресурсам/блокам нежели при использовании synchronized.

ReentrantLock
--------------
.. container:: left-col

    ReentrantLock — Лок на вхождение. Только один поток может зайти в защищенный блок. Класс поддерживает «честную» (fair) и «нечестную» (non-fair) разблокировку потоков. 

ReadWriteLock
-------------
.. container:: left-col
    
    ReadWriteLock — Дополнительный интерфейс для создания read/write локов. Такие локи необычайно полезны, когда в системе много операций чтения и мало операций записи.

    The read lock may be held simultaneously by multiple reader threads, so long as there are no writers. The write lock is exclusive.

.. container:: right-col

    .. container:: links-block
    
        .. rubric:: Ссылки:

        `ReadWriteLock <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/locks/ReadWriteLock.html>`_

StampedLock
---------------------
.. container:: left-col

    StampedLock поддерживает разбиение на ``readLock()`` и ``writeLock()`` и различные их модификации. 
    Однако, в отличие от **ReadWriteLock**, метод блокировки **StampedLock**  возвращает “штамп” — значение типа *long*.
    Этот штамп может использоваться в дальнейшем как для высвобождения ресурсов, так и для проверки состояния блокировки.

    .. warning::
        В StampedLock не реализована реентерантность.

    .. toggle-header::
        :header: Пример **ReadWriteLock** и **StampedLock**

        |br|

        .. list-table::
            :header-rows: 1

            * - **ReadWriteLock**

              - **StampedLock**

            * - .. code-block:: java

                    var executor = Executors.newFixedThreadPool(2);
                    var map = new HashMap<>();
                    var lock = new ReentrantReadWriteLock();

                    executor.submit(() -> {
                        lock.writeLock().lock();
                        try {
                            sleep(1);
                            map.put("foo", "bar");
                        } finally {
                            lock.writeLock().unlock();
                        }
                    });

                    Runnable readTask = () -> {
                        lock.readLock().lock();
                        try {
                            System.out.println(map.get("foo"));
                            sleep(1);
                        } finally {
                            lock.readLock().unlock();
                        }
                    };

                    executor.submit(readTask);
                    executor.submit(readTask);

                    stop(executor);

              - .. code-block:: java

                    var executor = Executors.newFixedThreadPool(2);
                    var map = new HashMap<>();
                    var lock = new StampedLock();

                    executor.submit(() -> {
                        long stamp = lock.writeLock();
                        try {
                            sleep(1);
                            map.put("foo", "bar");
                        } finally {
                            lock.unlockWrite(stamp);
                        }
                    });

                    Runnable readTask = () -> {
                        long stamp = lock.readLock();
                        try {
                            System.out.println(map.get("foo"));
                            sleep(1);
                        } finally {
                            lock.unlockRead(stamp);
                        }
                    };

                    executor.submit(readTask);
                    executor.submit(readTask);

                    stop(executor);

    |br|


.. container:: right-col

    .. container:: links-block
    
        .. rubric:: Ссылки:

        `StampedLock (docs.oracle) <https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/StampedLock.html>`_

        `StampedLock (>рабочие заметки) <https://dev.cheremin.info/2012/10/stampedlock.html>`_

        `Синхронизация доступа к изменяемым объектам <https://tproger.ru/translations/java8-concurrency-tutorial-2/>`_

    .. container:: remark-block

        Появился в Java 8

    .. toggle-header::
        :header: Методы

        .. container:: leftside, grid-markup

            Неэксклюзивно захватывает лок на чтение:
                .. code-block:: java 

                    long readLockInterruptibly();

                    long readLock();

                    long tryReadLock(
                        long time, TimeUnit unit
                    );

                    long tryReadLock();


            Освобождение лока:
                .. code-block:: java 

                    void    unlock(long stamp);

                    void    unlockWrite(long stamp);
                    boolean tryUnlockWrite();

                    void    unlockRead(long stamp);
                    boolean tryUnlockRead();

        .. container:: rightside, grid-markup

            Эксклюзивно захватывает лок на запись:
                .. code-block:: java 

                    long writeLockInterruptibly();

                    long writeLock();

                    long tryWriteLock(
                        long time, TimeUnit unit
                    );

                    long tryWriteLock();


            Проверка состояния лока:
                .. code-block:: java 

                    boolean isWriteLocked();
                    boolean isReadLocked();

                    int     getReadLockCount();


            Получение ReadWriteLock:
                .. code-block:: java 

                    Lock asReadLock();
                    Lock asWriteLock();
                    ReadWriteLock asReadWriteLock();

    |br|


.. container:: left-col

    .. rubric:: Оптимистичная блокировка:

    Оптимистичная блокировка для чтения, вызываемая с помощью метода ``tryOptimisticRead()``, отличается тем, что она всегда будет возвращать “штамп” не блокируя текущий поток, 
    вне зависимости от того, занят ли ресурс, к которому она обратилась. В случае, если ресурс был заблокирован блокировкой для записи, возвращённый штамп будет равняться нулю. 
    В любой момент можно проверить, является ли блокировка валидной с помощью ``lock.validate(stamp)``

    .. toggle-header::
        :header: Пример

        .. code-block:: java 

            ExecutorService executor = Executors.newFixedThreadPool(2);
            StampedLock lock = new StampedLock();

            executor.submit(() -> {

                // В отличии от обычных блокировок для чтения, оптимистичная не запрещает 
                //   другим потокам блокировать ресурс для записи
                long stamp = lock.tryOptimisticRead();

                try {
                    System.out.println("Optimistic Lock Valid: " + lock.validate(stamp));
                    // После захвата ресурса блокировка является валидной и оптимистичный поток отправляется спать
                    sleep(1);
                    // В это время другой поток блокирует ресурсы для записи, не дожидаясь окончания работы чтения

                    // Начиная с этого момента, оптимистичная блокировка перестаёт быть валидной 
                    //   (даже после окончания записи)
                    System.out.println("Optimistic Lock Valid: " + lock.validate(stamp));
                    sleep(2);

                    System.out.println("Optimistic Lock Valid: " + lock.validate(stamp));

                } finally {
                    lock.unlock(stamp);
                }
            });

            executor.submit(() -> {
                long stamp = lock.writeLock();
                try {
                    System.out.println("Write Lock acquired");
                    sleep(2);
                } finally {
                    lock.unlock(stamp);
                    System.out.println("Write done");
                }
            });

            stop(executor);

        **Вывод**::

            Optimistic Lock Valid: true
            Write Lock acquired
            Optimistic Lock Valid: false
            Write done
            Optimistic Lock Valid: false

    |br|

    .. note::
        Таким образом, при использовании оптимистичных блокировок нужно постоянно следить за их валидностью (проверять её нужно уже после того, как выполнены все необходимые операции).

.. container:: right-col

    .. toggle-header::
        :header: Методы

        .. container:: leftside, grid-markup

            Оптимистичная блокировка:
                .. code-block:: java 

                    long tryOptimisticRead();

                    boolean validate(long stamp);

    |br|



.. container:: left-col

    .. rubric:: Преобразование:

    Иногда может быть полезным преобразовать блокировку для чтения в блокировку для записи не высвобождая ресурсы. 
    В **StampedLock** это можно сделать с помощью метода ``tryConvertToWriteLock()``.

    .. toggle-header::
        :header: Пример

        .. code-block:: java 

            ExecutorService executor = Executors.newFixedThreadPool(2);
            StampedLock lock = new StampedLock();

            // В этом примере нужно прочитать значение переменной count и вывести его в консоль
            // Однако, если значение равно нулю, нужно изменить его на 23
            executor.submit(() -> {
                long stamp = lock.readLock();
                try {
                    if (count == 0) {
                        stamp = lock.tryConvertToWriteLock(stamp);

                        // В случае, если tryConvertToWriteLock() вызывается в тот момент, 
                        //   когда ресурс занят для записи другим потоком, 
                        //   текущий поток остановлен не будет, однако метод вернёт нулевое значение.
                        if (stamp == 0L) {
                            System.out.println("Could not convert to write lock");
                            // В таком случае можно вызвать writeLock() вручную.
                            stamp = lock.writeLock();
                        }

                        count = 23;
                    }
                    System.out.println(count);
                } finally {
                    lock.unlock(stamp);
                }
            });

            stop(executor);

    |br|

.. container:: right-col

    .. toggle-header::
        :header: Методы

        .. container:: leftside, grid-markup

            Преобразования:
                .. code-block:: java 

                    long tryConvertToWriteLock(
                        long stamp
                    );

                    long tryConvertToReadLock(
                        long stamp
                    );

                    long tryConvertToOptimisticRead(
                        long stamp
                    );

    |br|

Condition
----------
.. container:: left-col

    Condition — Интерфейс, который описывает альтернативные методы стандарным ``wait/notify/notifyAll``. 
    Condition чаще всего получается из локов через метод ``lock.newCondition()``. Тем самым можно получить несколько комплектов ``wait/notify`` для одного объекта.

    .. container:: code-markup

        void ``await()`` |br| boolean ``await​(long time, TimeUnit unit)`` |br| long ``awaitNanos​(long nanosTimeout)`` |br| void ``awaitUninterruptibly()`` |br| boolean ``awaitUntil​(Date deadline)``
            Causes the current thread to wait until it is signalled or interrupted (if no ``awaitUninterruptibly``).

            **Lock**, связанный с этим **Condition**, снимается атомарно, 
            и текущий поток становится неактивным и остается таким, пока не произойдет одно из четырех:

                * Какой-то другой поток вызывает метод ``signal()`` для этого **Condition**, и текущий поток выберется для пробуждения.
                * Какой-то другой поток вызывает метод ``signalAll()`` для этого **Condition**.
                * Какой-то другой поток прерывает (*interrupt*) текущий поток, при этом прерывание потока должно поддерживаться.
                * A "spurious wakeup" occurs. Поэтому нужно оборачивать в while блоки вызовы ``await`` с проверкой условия выхода из ``await``.

            Во всех случаях, прежде поток выйдет из ожидания, он должен вернуть **Lock** связанный с этим **Condition**. (When the thread returns it is *guaranteed* to hold this lock)

        void ``signal()`` |br| void ``signalAll()``
            Wakes up one/all waiting thread.

            Реализация может (обычно) требовать, чтобы текущий поток удерживал **Lock**, связанный с этим **Condition**, когда вызывается этот метод.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Condition <https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/locks/Condition.html>`_

    .. code-block:: java 

        class BoundedBuffer<E> {
          final Lock lock = new ReentrantLock();
          final Condition notFull  = lock.newCondition(); 
          final Condition notEmpty = lock.newCondition(); 

          final Object[] items = new Object[100];
          int putptr, takeptr, count;

          public void put(E x) throws InterruptedException {
            lock.lock();

            try {
              while (count == items.length) // while защищает от spurious wakeup
                // Если количество элементов больше максимального - ожидаем, пока элемент не будет изъят
                notFull.await();

              items[putptr] = x;
              if (++putptr == items.length) putptr = 0;
              ++count;

              // Сигнализируем ожидающий поток на получение элемента, когда он был пустой
              notEmpty.signal(); 

            } finally {
              lock.unlock();
            }
          }

          public E take() throws InterruptedException {
            lock.lock();

            try {
              while (count == 0) // while защищает от spurious wakeup
                // Если количество элементов 0, то ожидаем пока элемент не будет добавлен и вызван signal
                notEmpty.await();

              E x = (E) items[takeptr];
              if (++takeptr == items.length) takeptr = 0;
              --count;

              // Сигнализируем, что элементов не максимальное количество, и теперь можно их добавить
              notFull.signal();

              return x;

            } finally {
              lock.unlock();
            }
          }
        }