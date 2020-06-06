.. include:: ../../defs.rst

Proxy
************************************
.. container:: left-col

    Позволяет сконструировать объект во время исполнения программы (динамически), не создавая для него отдельного класса.

    .. note::
        proxy-объект можно создать только используя интерфейсы.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Динамический прокси Java: что это и как им пользоваться? <https://habr.com/ru/company/otus/blog/434214/>`_

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
            // Создает объект интерфейса MyInterface, адресующий все вызовы к CustomInvocationHandler.invoke(proxyObject, method, args)
            // Для получения имени вызванного метода нужно использовать method.getName()
            // method.invoke(original, args) - вызов метода у оригинального объекта

    .. code-block:: java
        :caption: Более полный способ конструирования прокси объекта

        Proxy.newProxyInstance(ClassLoader classLoader, Class<?>[] interfaces, InvocationHandler invocationHandler);
            // classLoader - the class loader to define the proxy class
            // interfaces - the list of interfaces for the proxy class to implement