.. include:: ../../defs.rst

DCL
********************************

.. raw:: html

    <iframe width="640" height="360" frameborder="0" src="https://mega.nz/embed/zYcFhQyT#twYjAr-Aw46xNVq9ovCnMwkn3RDn_FYiw6NhQiTDz8s!715s" allowfullscreen ></iframe>


|br|

Не ленивая инициализация
========================

.. code-block:: java

    class Singleton {
        private final static Singleton INSTANCE = new Singleton();

        private Singleton() {}

        public static Singleton getInstance() {
            return INSTANCE;
        } 
    }


Не Thread-safe Синглтон
============================

.. code-block:: java

    class Singleton {

        private static Singleton INSTANCE;

        private Singleton() {}

        public static Singleton getInstance() {
            if (INSTANCE == null) {
                INSTANCE = new Singleton();
            }

            return INSTANCE;
        }
    }


Синхронизированный Синглтон
============================

.. code-block:: java

    class Singleton {

        private static Singleton INSTANCE;

        private Singleton() {}

        public synchronized static Singleton getInstance() {
            if (INSTANCE == null) {
                INSTANCE = new Singleton();
            }

            return INSTANCE;
        }
    }


DCL (double check locking) Синглтон
===================================

.. code-block:: java

    class Singleton {

        private volatile static Singleton INSTANCE;

        private Singleton() {}

        public static Singleton getInstance() {
            if (INSTANCE == null) {                 // Приходят 3 потока
                synchronized (Singleton.class) {    // Первый заходит, остальные ждут и заходят по очереди
                    if (INSTANCE == null) {         // Первый проходит. Остальные видят изменившееся значение, так как volatile
                        INSTANCE = new Singleton(); // Инициализирует только первый
                    }
                }
            }
            return INSTANCE;
        }
    }


DCL с оптимизацией (+ 20-25%)
===================================

.. code-block:: java

    class Singleton {

        private volatile static Singleton INSTANCE;

        private Singleton() {}

        public static Singleton getInstance() {
            Singleton localRef = INSTANCE;

            if (localRef == null) {
                synchronized (Singleton.class) {
                    localRef = INSTANCE;

                    if (localRef == null) {
                        localRef = new Singleton();
                        INSTANCE = localRef;
                    }
                }
            }

            return localRef;
        }
    }


Enum Singleton
===================================

.. code-block:: java

    enum Singleton {
        INSTANCE
    }


Lazy static load Singleton
===================================

.. code-block:: java

    class Singleton {

        public static Singleton getInstance() {
            return SingletonHolder.INSTANCE;
        }

        private static class SingletonHolder {
            private final static Singleton INSTANCE = new Singleton();
        }
    }