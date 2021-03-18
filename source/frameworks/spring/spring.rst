.. include:: ../../defs.rst

Spring
#################

.. container:: left-col

    Spring Framework — один из самых популярных фреймворков для создания веб-приложений на Java.
    Spring управляет инфраструктурой приложения, и позволяет отделить конфигурацию и спецификацию зависимостей от реальной программной логики (dependecy injection).

    Spring позволяет создавать приложения из POJO объектов и неинвазивно применять *enterprise* сервисы к ним.    

    .. figure:: ../../_static/spring-overview.png

        The Spring Framework consists of features organized into about 20 modules. 
        These modules are grouped into Core Container, Data Access/Integration, Web, AOP (Aspect Oriented Programming), Instrumentation, and Test




.. container:: right-col


    .. container:: links-block

        .. rubric:: Документация:

        `Spring Framework Documentation <https://docs.spring.io/spring/docs/current/spring-framework-reference/index.html>`_

        `Introduction to Spring Framework (4.0). Modules <https://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/overview.html>`_

    .. container:: links-block

        .. rubric:: Ссылки:

        `Spring Tutorial <https://www.tutorialspoint.com/spring/index.htm>`_

        `Spring Framework Guide <https://ru.wikibooks.org/wiki/Spring_Framework_Guide>`_

        `Руководство по Spring. Введение <https://proselyte.net/tutorials/spring-tutorial-full-version/introduction/>`_

        `Spring Framework Reference Documentation <https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/index.html>`_

        `Spring. Учебные материалы <https://spring-projects.ru/guides/>`_

        `spring.io/guides <https://spring.io/guides>`_


IoC Container
***************
.. container:: left-col

    Модули **Core** и **Beans** обеспечивают основные части платформы, включая функции IoC и Dependency Injection, а так же предоставляет реализации *BeanFactory*.

    .. tip::
        **Spring Core** - Ядро платформы, предоставляет базовые средства для создания приложений — управление компонентами (*бинами, beans*), 
        внедрение зависимостей, MVC фреймворк, транзакции, базовый доступ к БД. 

        В основном это низкоуровневые компоненты и абстракции. По сути, неявно используется всеми другими компонентами.

    Модуль **Context** основан на базе модулей **Core** и **Beans**.
    The :ref:`AppContextRef` interface is the focal point of the Context module.

    The **Expression Language** module provides an expression language for querying and manipulating an object graph at runtime.

    .. toctree::
        :maxdepth: 1

        ioc.rst
        beans.rst

.. container:: right-col

    .. code-block:: xml

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-expression</artifactId>
        </dependency>




Data Access
*************************
.. container:: left-col

    **Spring Data** - Предоставляет доступ к данным: реляционные и нереляционные БД, KV хранилища и т.п. 
    Обеспечивает взаимодействие между уровнем доступа к данным и уровнем бизнес или сервис логики.

    Модуль **Transaction** поддерживает программное и декларативное управление транзакциями (**transaction management**) для классов, 
    *реализующих специальные интерфейсы*, и для всех *POJO* объектов.

    Поддержка Data Access Object (**DAO Support**) в Spring нацелена на то, чтобы упростить согласованную работу с технологиями доступа к данным (такими как JDBC, Hibernate или JPA),
    и позволяет писать код переключаясь между упомянутыми технологиями, не беспокоясь о перехвате исключений, специфичных для каждой из них.

        .. note::
            The best way to guarantee that your Data Access Objects (*DAOs*) or repositories provide exception translation is to use the ``@Repository`` annotation.

    Модуль **JDBC** предоставляет уровень абстракции JDBC, который устраняет необходимость выполнять излишнее кодирование и анализ кодов ошибок, специфичных для database-vendor.

    Модуль **ORM** предоставляет уровни интеграции для популярных API объектно-реляционных сопоставлений (ORM), включая :ref:`JPARef`, :ref:`JDORef` и :ref:`HibernateRef`.
    Пакет **ORM**, позволяет использовать все вышеупомянутые фреймворки *O/R-mapping* в сочетании со всеми другими функциями Spring, 
    такими как функция декларативного управления транзакциями (:ref:`TransactionManagementRef`).

    The **OXM** module provides an abstraction layer that supports Object/XML mapping implementations for JAXB, Castor, XMLBeans, JiBX and XStream.

    .. toctree::
        :maxdepth: 1

        transaction.rst
        jdbc.rst
        orm.rst

.. container:: right-col

    .. container:: links-block

        .. rubric:: Документация:

        `Data Access <https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html>`_

    .. container:: links-block

        .. rubric:: Ссылки:

        `Spring Data JPA (пример использования) <https://habr.com/ru/post/435114/>`_

Integration
****************************
.. container:: left-col

    Обработка данных из разных источников. Если надо раз в час брать файл с ФТП, разбивать его на строки, которые потом фильтровать, а дальше отправлять в какую-то очередь — это к Spring Integration.

    The Java Messaging Service (**JMS**) module contains features for producing and consuming messages.

Spring Web
****************************


Spring Security
*******************
.. container:: left-col

    Предотавляет механизмы для авторизации и аутентификации, доступ к данным, методам и т.п. OAuth, LDAP, и разные провайдеры.

Spring Cloud
***************
.. container:: left-col

    Включает в себя много полезного для микросервисной архитектуры — service discovery, трасировка и диагностика, балансировщики запросов, circuit breaker-ы, роутеры и т.п.


Spring Test
****************
.. container:: left-col

    The Test module supports the testing of Spring components with JUnit or TestNG. It provides consistent loading of Spring ApplicationContexts and caching of those contexts. 
    It also provides mock objects that you can use to test your code in isolation.


Logging
**********

Spring Boot
***************

Доп. материалы
*****************
.. container:: left-col

    .. toctree::
        :maxdepth: 1

        lectures.rst
        difvers.rst