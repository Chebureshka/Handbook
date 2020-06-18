.. include:: ../../defs.rst

Proxy
************************************
.. container:: left-col

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Динамическое генерирование прокси-классов в Java <https://habr.com/ru/post/348906/>`_

java.lang.reflect.Proxy
========================
.. container:: left-col

    Позволяет сконструировать объект во время исполнения программы (динамически), не создавая для него отдельного класса.

    .. note::
        proxy-объект можно создать только используя интерфейсы.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Динамический прокси Java: что это и как им пользоваться? <https://habr.com/ru/company/otus/blog/434214/>`_

        `Proxy (docs.oracle.com) <https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Proxy.html>`_

InvocationHandler
------------------
.. container:: left-col

    Функциональный интерфейс, предоставляющий метод ``invoke``, в который направляются все вызовы, обращенные к proxy-объекту.
    Позволяет перехватывать все вызовы методов, обращенные к proxy-объекту.

    .. code-block:: java

        invoke(Object proxy, Method method, Object[] args)

    .. code-block:: java
        :caption: Создание Proxy объекта

        MyInterface proxyObject = (MyInterface)Proxy.newProxyInstance(new CustomInvocationHandler());
            // Создает объект интерфейса MyInterface, адресующий все вызовы к 
            //   CustomInvocationHandler.invoke(proxyObject, method, args)
            // Для получения имени вызванного метода нужно использовать method.getName()
            // method.invoke(original, args) - вызов метода у оригинального объекта

    .. code-block:: java
        :caption: Более полный способ конструирования прокси объекта

        Proxy.newProxyInstance(ClassLoader classLoader, Class<?>[] interfaces, InvocationHandler invocationHandler);
            // classLoader - the class loader to define the proxy class
            // interfaces - the list of interfaces for the proxy class to implement

Cglib
========================
.. container:: left-col

    Code Generation Library - библиотека позволяющая изменять и исполнять байткод инструкции, в процессе исполнения программы.
    The bytecode instrumentation allows manipulating or creating classes after the compilation phase of a program.



.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Cglib (baeldung) <https://www.baeldung.com/cglib>`_