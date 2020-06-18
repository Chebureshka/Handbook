.. include:: ../../defs.rst

.. _ProcessRef:

Process
*******************************************************

.. container:: left-col

    Process provides control of native processes started by ``ProcessBuilder.start`` and ``Runtime.exec`` (create a native process and return an instance of a subclass of Process that can be used to control the process and obtain information about it).



.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ProcessBuilder (docs.oracle) <https://docs.oracle.com/javase/10/docs/api/java/lang/ProcessBuilder.html#start()>`_

        `Process (docs.oracle) <https://docs.oracle.com/javase/10/docs/api/java/lang/Process.html>`_

.. contents::
    :local:





Получение информации о процесссе
=========================================================
.. container:: left-col

    .. container:: code-markup

        Stream<ProcessHandle> ``children()``
            Returns a snapshot of the direct children of the process.

            The parent of a direct child process is the process. Typically, a process that is not alive has no children.

        Stream<ProcessHandle> ``descendants()``
            Returns a snapshot of the descendants of the process.

            The descendants of a process are the children of the process plus the descendants of those children, recursively.

        ProcessHandle.Info ``info()``
            Returns a snapshot of information about the process.

            .. rubric:: ProcessHandle.Info:

            .. container:: code-markup

                Optional<String[]> ``arguments()``
                    Returns an array of Strings of the arguments of the process.

                Optional<String> ``command()``
                    Returns the executable pathname of the process.

                Optional<String> ``commandLine()``
                    Returns the command line of the process.

                Optional<Instant> ``startInstant()``
                    Returns the start time of the process.

                Optional<Duration> ``totalCpuDuration()``
                    Returns the total cputime accumulated of the process.

                Optional<String> ``user()``
                    Return the user of the process.

        boolean ``isAlive()``
            Tests whether the process represented by this **Process** is alive.

        long ``pid()``
            Returns the native process ID of the process.

        int ``exitValue()``
            Returns the exit value for the process.

            Throws **IllegalThreadStateException** - if the process represented by this **Process** object has not yet terminated.

            .. note::
                By convention, the value 0 indicates normal termination.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ProcessHandle.Info (docs.oracle) <https://docs.oracle.com/javase/10/docs/api/java/lang/ProcessHandle.Info.html>`_


Управление завершением процесса
======================================================
.. container:: left-col

    .. container:: code-markup

        boolean ``supportsNormalTermination()``
            Returns true if the implementation of ``destroy()`` is to normally terminate the process, Returns false if the implementation of destroy forcibly and immediately terminates the process.

        void ``destroy()``
            Kills the process.

        Process ``destroyForcibly()``
            Kills the process forcibly.

        CompletableFuture<Process> ``onExit()``
            Returns a **CompletableFuture<Process>** for the termination of the Process.

        int ``waitFor()``
            Causes the current thread to wait, if necessary, until the process represented by this Process object has terminated.

        boolean ``waitFor​(long timeout, TimeUnit unit)``
            Causes the current thread to wait, if necessary, until the process represented by this Process object has terminated, or the specified waiting time elapses.




Работа с потоками ввода/вывода процесса
=============================================================
.. container:: left-col

    .. container:: code-markup

        InputStream ``getErrorStream()``
            Returns the input stream connected to the error output of the process.

        InputStream ``getInputStream()``
            Returns the input stream connected to the normal output of the process.

        OutputStream ``getOutputStream()``
            Returns the output stream connected to the normal input of the process.



ProcessHandle
==============
.. container:: left-col

    ProcessHandle identifies and provides control of native processes. 
    Each individual process can be monitored for liveness, list its children, get information about the process or destroy it. 
    
    By comparison, **Process** instances were started by the current process and additionally provide access to the process input, output, and error streams.

    .. container:: code-markup

        ProcessHandle ``toHandle()``
            Returns a `ProcessHandle <https://docs.oracle.com/javase/10/docs/api/java/lang/ProcessHandle.html>`_ for the **Process**.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ProcessHandle (docs.oracle) <https://docs.oracle.com/javase/10/docs/api/java/lang/ProcessHandle.html>`_















