.. include:: ../../defs.rst

ThreadLocal
************
.. container:: left-col

    Этот класс предоставляет локальные для потока переменные.
    Эти переменные отличаются от своих обычных аналогов тем, что каждый поток, который обращается к объекту ThreadLocal (через метод ``get`` или ``set``), имеет свою собственную, независимо инициализированную копию переменной.

    ``remove`` - Removes the current thread's value for this thread-local variable.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Class ThreadLocal<T> <https://docs.oracle.com/javase/7/docs/api/java/lang/ThreadLocal.html>`_

    .. toggle-header::
        :header: Пример

        .. code-block:: java 

            import java.util.concurrent.atomic.AtomicInteger;

            public class ThreadId {
                // Atomic integer containing the next thread ID to be assigned
                private static final AtomicInteger nextId = new AtomicInteger(0);

                // Thread local variable containing each thread's ID
                private static final ThreadLocal<Integer> threadId =
                    new ThreadLocal<Integer>() {
                        @Override protected Integer initialValue() {
                            return nextId.getAndIncrement();
                    }
                };

                // Returns the current thread's unique ID, assigning it if necessary
                public static int get() {
                    return threadId.get();
                }
            }


InheritableThreadLocal
======================
.. container:: left-col

    Этот класс расширяет ThreadLocal для обеспечения наследования значений из родительского потока в дочерний поток:
    когда создается дочерний поток - он получает начальные значения для всех inheritable thread-local переменных, для которых родительский имеет значения.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Class InheritableThreadLocal<T> <https://docs.oracle.com/javase/7/docs/api/java/lang/InheritableThreadLocal.html>`_