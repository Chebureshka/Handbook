.. include:: ../../defs.rst

Compiler
*******************************************************
.. container:: left-col

    The Compiler class is provided to support Java-to-native-code compilers and related services. 

    By design, the Compiler class does nothing; it serves as a placeholder for a JIT compiler implementation.

    Когда JVM запускается - определяется, существует ли системное свойство ``java.compiler``. (System properties are accessible through ``System.getProperty(String)`` and ``System.getProperty(String, String)``)
    Данное свойство определяет имя библиотеки, ``System.loadLibrary(java.lang.String)`` вызывается для загрузки этой библиотеки. 
    Если эта загрузка завершается успешно, вызывается функция с именем ``java_lang_Compiler_start()`` в этой библиотеке.


    .. container:: code-markup

        ``static Object command(Object any)``
            Проверяет тип аргумента и его поля и выполняет некоторые задокументированные операции.

        ``static boolean compileClass(Class<?> clazz)``
            Компилирует указанный класс.

        ``static boolean compileClasses(String string)``
            Компилирует все классы, чье имя соответствует указанной строке.

        ``static void disable()``
            Заставить компилятор прекратить работу.

        ``static void enable()``
            Заставить компилятор возобновить работу.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Compiler (docs.oracle) <https://docs.oracle.com/javase/7/docs/api/java/lang/Compiler.html#command(java.lang.Object)>`_

        `Java.lang.Compiler Class (tutorialspoint) <https://www.tutorialspoint.com/java/lang/java_lang_compiler.htm>`_
