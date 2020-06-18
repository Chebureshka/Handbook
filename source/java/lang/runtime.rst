.. include:: ../../defs.rst

Runtime
*******************************************************

.. container:: left-col

    Каждое Java-приложение имеет один экземпляр класса Runtime, который позволяет приложению взаимодействовать со средой, в которой выполняется приложение.

    .. container:: code-markup

        ``getRuntime()``
            Returns the **Runtime** object associated with the current Java application.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Runtime (docs.oracle) <https://docs.oracle.com/javase/9/docs/api/java/lang/Runtime.html>`_

.. contents::
    :local:


Получение текущих характеристик
================================
.. container:: left-col

    .. container:: code-markup

        ``availableProcessors()``
            Returns (int) the number of processors available to the Java virtual machine.

        ``freeMemory()``
            Returns (long) the amount of free memory in the Java Virtual Machine.

        ``maxMemory()``
            Returns (long) the maximum amount of memory that the Java virtual machine will attempt to use.

        ``totalMemory()``
            Returns (long) the total amount of memory in the Java virtual machine.



Управление состоянием JVM
===========================================
.. container:: left-col

    .. rubric:: Прерывание процесса исполнения:

    .. container:: code-markup

        ``exit(int status)``
            Terminates the currently running Java virtual machine by initiating its shutdown sequence.

        ``halt(int status)``
            Forcibly terminates the currently running Java virtual machine. This method never returns normally.

            В отличие от метода выхода, этот метод не приводит к запуску *Shutdown hook* и не запускает непроверенные *finalizators*, если включена финализация при выходе.

    .. rubric:: Shutdown hook:

    A shutdown hook is simply an initialized but unstarted thread. 
    When the virtual machine begins its shutdown sequence it will start all registered shutdown hooks in some unspecified order and let them run concurrently.

    When all the hooks have finished it will then run all uninvoked finalizers if finalization-on-exit has been enabled.

    Finally, the virtual machine will halt.

    .. note::
        Daemon потоки будут продолжать работать во время shutdown последовательности. 
        
        Так же как и non-daemon потоки, если завершение работы было инициировано путем вызова ``exit()`` метода.

    .. container:: code-markup

        ``addShutdownHook(Thread hook)``
            Registers a new virtual-machine shutdown hook.

            ``hook`` - An initialized but unstarted Thread object.

        ``removeShutdownHook(Thread hook)``
            De-registers a previously-registered virtual-machine shutdown hook. Returns boolean.

    .. rubric:: Управление GC:

    .. container:: code-markup

        ``gc()``
            Runs the garbage collector.

        ``runFinalization()``
            Runs the finalization methods of any objects pending finalization.



Запуск сторонних процессов
===========================================
.. container:: left-col

    .. container:: code-markup

        ``exec(String command): Process``
            Executes the specified string command in a separate process.

        ``exec(String[] cmdarray): Process``
            Executes the specified command and arguments in a separate process.

        ``exec(String[] cmdarray, String[] envp): Process``
            Executes the specified command and arguments in a separate process with the specified environment.

        ``exec(String[] cmdarray, String[] envp, File dir): Process``
            Executes the specified command and arguments in a separate process with the specified environment and working directory.

        ``exec(String command, String[] envp): Process``
            Executes the specified string command in a separate process with the specified environment.

        ``exec(String command, String[] envp, File dir): Process``
            Executes the specified string command in a separate process with the specified environment and working directory.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        :ref:`ProcessRef`

        `Process (docs.oracle) <https://docs.oracle.com/javase/7/docs/api/java/lang/Process.html>`_



Загрузка динамических библиотек
===========================================
.. container:: left-col

    .. container:: code-markup

        ``load(String filename)``
            Loads the specified filename as a dynamic library.
        
        ``loadLibrary(String libname)``
            Loads the dynamic library with the specified library name.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        :ref:`LibRef`

        `JNI Specification <https://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/jniTOC.html>`_