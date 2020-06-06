.. include:: ../../defs.rst

Примитивы синхронизации вне пакета java.util.concurrent
*******************************************************

.. container:: left-col

    .. figure:: ../../_static/base6482949728376bd328.png


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:
    
        `Пример использования <https://javadevblog.com/primer-ispol-zovaniya-wait-notify-i-notifyall-v-java.html>`_
    
        `Baeldung <https://www.baeldung.com/java-wait-notify>`_

Синхронизация с помощью ``wait() / notify()``
===============================================

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


.. container:: right-col

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

            waitCondition = false;
            monitor.notify();
        }
