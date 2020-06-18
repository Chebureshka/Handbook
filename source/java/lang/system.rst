.. include:: ../../defs.rst

System
*******************************************************
.. container:: left-col

    Класс **java.lang.System** является ``final``, все поля и методы являются статическими (static). Он располагает множеством полезных методов для работы.
    Класс Java System не предоставляет каких-либо публичных конструкторов, поэтому нельзя создать экземпляр этого класса.


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `System (docs.oracle) <https://docs.oracle.com/javase/9/docs/api/java/lang/System.html>`_

        `Работа с классом Java System <https://javadevblog.com/rabota-s-klassom-java-system-java-lang-system.html>`_

.. contents::
    :local:


Работа с I/O
================================================

.. container:: left-col

    .. rubric:: Поля:

    .. container:: code-markup

        ``out``
            The "*standard*" output stream (**PrintStream**).

            Typically this stream corresponds to display output or another output destination specified by the host environment or user.

        ``in``
            The "*standard*" input stream (**InputStream**).

            Typically this stream corresponds to keyboard input or another input source specified by the host environment or user.

        ``err``
            The "*standard*" error output stream (**PrintStream**).

            По соглашению этот выходной поток используется для отображения сообщений об ошибках или другой информации, 
            которая должна быть незамедлительно предоставлена пользователю, даже если основной выходной поток (``out``) был перенаправлен в файл или другое место назначения, 
            которое обычно не контролируется непрерывно.


    .. rubric:: Методы:

    .. container:: code-markup

        ``console​()``
            Returns the unique `Console <https://docs.oracle.com/javase/9/docs/api/java/io/Console.html>`_ (methods to access the character-based console device) object associated with the current Java virtual machine, if any.

        ``inheritedChannel​()``
            Returns the `Channel <https://docs.oracle.com/javase/9/docs/api/java/nio/channels/Channel.html>`_ (nio) inherited from the entity that created this Java virtual machine.

        ``lineSeparator​()``
            Returns the system-dependent line separator string.

        ``setErr​(PrintStream err)``
            Reassigns the "*standard*" error output stream.

        ``setIn​(InputStream in)``
            Reassigns the "*standard*" input stream.

        ``setOut​(PrintStream out)``
            Reassigns the "*standard*" output stream.


Работа с Property
================================================
.. container:: left-col

    .. container:: code-markup

        ``clearProperty​(String key)``
            Removes the system property indicated by the specified key.

        ``getProperties​()``
            Determines the current system `Properties <https://docs.oracle.com/javase/9/docs/api/java/util/Properties.html>`_ (``Properties extends Hashtable<Object,Object>`` + additional methods for work with properties).

            `Default properties <https://docs.oracle.com/javase/9/docs/api/java/lang/System.html#getProperties-->`_

        ``getProperty​(String key)``
            Gets the system property (**String**) indicated by the specified key.

        ``getProperty​(String key, String def)``
            Gets the system property (**String**) indicated by the specified key. ``def`` - a default value.

        ``setProperties​(Properties props)``
            Sets the system properties to the `Properties <https://docs.oracle.com/javase/9/docs/api/java/util/Properties.html>`_ argument.

        ``setProperty​(String key, String value)``
            Sets the system property indicated by the specified key. Returns the previous value of the system property, or null if it did not have one.


    .. rubric:: Environment (переменные среды):

    System properties and environment variables are both conceptually mappings between names and values. 
    Both mechanisms can be used to pass user-defined information to a Java process.

    Переменные среды имеют более глобальный эффект нежели properties, потому что они видны всем потомкам процесса, который их определяет, а не только непосредственному подпроцессу Java.

    Переменные среды следует использовать, когда требуется глобальный эффект или когда внешнему системному интерфейсу требуется переменная среды (например, PATH).

    .. container:: code-markup

        ``getenv​()``
            Returns an unmodifiable string map (**Map<String,String>**) view of the current system environment.

        ``getenv​(String name)``
            Gets the value of the specified environment variable.


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Class Properties <https://docs.oracle.com/javase/9/docs/api/java/util/Properties.html>`_

        `Default properties table <https://docs.oracle.com/javase/9/docs/api/java/lang/System.html#getProperties-->`_


Получение текущего времени
================================================
.. container:: left-col

    .. container:: code-markup

        ``currentTimeMillis​()``
            Returns the current time in milliseconds (**long**).

            Effectively equivalent to the call ``Date().getTime()``

        ``nanoTime​()``
            Returns the current value of the running Java Virtual Machine's high-resolution time source, in nanoseconds (**long**).
            Возвращаемое значение представляет наносекунды с некоторого фиксированного, но произвольного времени начала (возможно, в будущем, поэтому значения могут быть отрицательными).

            This method can only be used to measure elapsed time and is not related to any other notion of system or wall-clock time

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:
        
        `Class Date (docs.oracle) <https://docs.oracle.com/javase/9/docs/api/java/util/Date.html>`_

Управление состоянием JVM
================================================
.. container:: left-col

    .. container:: code-markup

        ``gc​()``
            Runs the garbage collector.

            Когда управление возвращается из вызова метода, виртуальная машина Java сделала все возможное, чтобы освободить пространство от всех недоступных объектов.

            Effectively equivalent to the call ``Runtime.getRuntime().gc()``

        ``runFinalization​()``
            Runs the finalization methods of any objects pending finalization.

            Effectively equivalent to the call ``Runtime.getRuntime().runFinalization()``

        ``exit​(int status)``
            Terminates the currently running Java Virtual Machine.

            Effectively equivalent to the call ``Runtime.getRuntime().exit(n)``



Работа с объектами
========================
.. container:: left-col

    .. container:: code-markup

        ``arraycopy​(Object src, int srcPos,`` |br| |dl| ``Object dest, int destPos, int length)``
            Copies an array from the specified source array, beginning at the specified position, to the specified position of the destination array.

        ``identityHashCode​(Object x)``
            Returns the same hash code for the given object as would be returned by the default method ``hashCode()``, whether or not the given object's class overrides ``hashCode()``.



System.Logger
======================
.. container:: left-col

    `System.LoggerFinder <https://docs.oracle.com/javase/9/docs/api/java/lang/System.LoggerFinder.html>`_ - The LoggerFinder service is responsible for creating, managing, and configuring loggers to the underlying framework it uses.
    
    `System.Logger <https://docs.oracle.com/javase/9/docs/api/java/lang/System.Logger.html>`_ - Экземпляры System.Logger регистрируют сообщения, которые будут направлены в underlying logging framework, который использует **LoggerFinder**.

    .. container:: code-markup

        ``getLogger​(String name)``
            Returns an instance of **System.Logger** for the caller's use.
        
        ``getLogger​(String name, ResourceBundle bundle)``
            Returns a localizable instance of **System.Logger** for the caller's use.




SecurityManager
======================
.. container:: left-col

    The security manager is a class that allows applications to implement a security policy. 
    It allows an application to determine, before performing a possibly unsafe or sensitive operation, what the operation is and whether it is being attempted in a security context that allows the operation to be performed. 
    The application can allow or disallow the operation.

    .. container:: code-markup

        ``getSecurityManager​()``
            Gets the system security (**SecurityManager**) interface.

        ``setSecurityManager​(SecurityManager s)``
            Sets the System security.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `SecurityManager (docs.oracle) <https://docs.oracle.com/javase/9/docs/api/java/lang/SecurityManager.html>`_

        `SecurityManager (baeldung) <https://www.baeldung.com/java-security-manager>`_

    .. toggle-header::
        :header: Пример

        .. code-block:: java 
    
            public class CustomPermission extends BasicPermission {
                public CustomPermission(String name) {
                    super(name);
                }
    
                public CustomPermission(String name, String actions) {
                    super(name, actions);
                }
            }
    
            public class Service {
    
                public static final String OPERATION = "my-operation";
    
                public void operation() {
                    SecurityManager securityManager = System.getSecurityManager();
                    if (securityManager != null) {
                        securityManager.checkPermission(new CustomPermission(OPERATION));
                    }
                    System.out.println("Operation is executed");
                }
            }

.. _LibRef:

Загрузка динамических библиотек
=================================
.. container:: left-col

    .. container:: code-markup

        ``load​(String filename)``
            Loads the *native library* specified by the filename argument.

            ``Runtime.getRuntime().load(name)``

        ``loadLibrary​(String libname)``
            Loads the *native library* specified by the libname argument.

            ``Runtime.getRuntime().loadLibrary(name)``

        ``mapLibraryName​(String libname)``
            Maps a library name into a *platform-specific* string representing a native library.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `JNI Specification <https://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/jniTOC.html>`_