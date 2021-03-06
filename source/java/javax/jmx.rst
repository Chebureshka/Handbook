.. include:: ../../defs.rst

JMX
************************************
.. container:: left-col

    Java Management Extensions. JMX предназначен для контроля и управления приложениями, системными объектами, устройствами (например, принтерами) и компьютерными сетями.

    Она позволяет управлять внутренним состоянием так называемых MBean-ов, которые по сути являются классами Java, предоставляющими доступ к части своих полей и методов извне.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `JMX <https://www.oracle.com/technical-resources/articles/javase/jmx.html>`_

MBean
=======
.. container:: left-col

    Стандартный MBean определяется с помощью интерфейса с именем ``<имя>MBean`` и его реализацией ``<имя>`` соответственно.

    Интерфейс определяет все экспортируемые наружу методы и атрибуты MBean-а. Атрибуты должны следовать правилам именования getter-ов и setter-ов.

    Далее нужно добавить созданный MBean в MBeanServer, который является частью так называемого JMX agent:

    .. code-block:: java

        MBeanServer mbs = ManagementFactory.getPlatformMBeanServer();

        // ObjectName, с помощью которого задаётся имя MBean-а (домен:ключ=значение, ключ=значение…)
        // Это имя будет видно JConsole
        ObjectName name = new ObjectName("javaexamples.exampl:type=My");

        // Теперь после запуска приложения и JConsole можно из-вне вызывать методы переданного MBean
        My mbean = new My(); 
        mbs.registerMBean(mbean, name);


MXBean 
-------
.. container:: left-col

    Это специальный тип MBean, который использует только предопределённое множество типов данных. 
    Это нужно для того, чтобы наш MBean мог использовать любой клиент, даже тот, у которого нет доступа к классам модели нашего MBean-а.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        https://urvanov.ru/2018/05/06/mxbeans-%D0%B2-java/