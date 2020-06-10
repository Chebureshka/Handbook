.. include:: ../../defs.rst

Spring IoC Container
========================================
.. container:: left-col

    Главной из особенностей Spring Framework является **IoC Container**, сопровождающаяся полным охватом технологий Spring Apect-Oriented Programming (AOP).

    Container создаёт объекты, связывает их вместе, настраивает и управляет ими от создания до момента уничтожения.
    Для управления компонентами, из которых состоит приложение, Spring Container использует Внедрение Зависимостей (DI). Эти объекты называются :ref:`SpringBeans`.
    The container then injects those dependencies when it creates the bean.

    Spring Container получает инструкции какие объекты инстанциировать и как их конфигурировать через метаданные.

    .. figure:: ../../_static/container-magic.png

    The ``org.springframework.beans`` and ``org.springframework.context`` (ApplicationContext_) packages are the basis for Spring Framework’s IoC container.


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Container Overview (docs.spring.io) <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-basics>`_

        `Руководство по Spring. Контейнеры IoC <https://proselyte.net/tutorials/spring-tutorial-full-version/ioc-containers/>`_

    .. container:: remark-block

        Фреймворк будет вызывать в нужный ему момент тот код, который вы написали. 
        То есть, тут уже не вы управляете процессом выполнения кода/программы, а фреймворк это делает за вас.
        Вы передали ему управление (инверсия управления).

    .. container:: remark-block

        Под **DI** понимают то **Dependency Inversion** (инверсию зависимостей, то есть попытки не делать жестких связей между вашими модулями/классами, где один класс напрямую завязан на другой), 
        то **Dependency Injection** (внедрение зависимостей, 
        это когда объекты создаете не вы в main-е и потом передаете их в свои методы, а за вас их создает спринг, а вы ему просто говорите что-то типа "Желаю Здѣсь ѻбъѥктъ" и он вам его передает в ваш метод). 


ApplicationContext
------------------
.. container:: left-col

    ``org.springframework.context.ApplicationContext`` представляет IoC Container и отвечает за создание, настройку и сборку bean-компонентов.

    .. note::
        The configuration metadata is represented in XML, Java annotations, or Java code.

    Ваши классы приложений объединяются с метаданными конфигурации, так что после того, как ApplicationContext создан и инициализирован, у вас есть полностью сконфигурированная и исполняемая система или приложение.
    (см. рисунок выше)

    .. tip::
        Контекст — это набор бинов (объектов). Обращаясь к контексту — мы можем получить нужный нам бин (объект) по его имени например, или по его типу, или еще как-то.

    .. code-block:: java

        // Создание ApplicationContext, на основе класса конфигурации ApplicationConfig
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(ApplicationConfig.class);

        // Получение бина из контейнера
        final var myBean = ctx.getBean("myBean", MyBean.class);

        ctx.close();

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ApplicationContext (documentation) <https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html>`_

        `Spring для ленивых <https://javarush.ru/groups/posts/477-spring-dlja-lenivihkh-osnovih-bazovihe-koncepcii-i-primerih-s-kodom-chastjh-2>`_

    .. container:: remark-block

        **ApplicationContext** - sub-interface of BeanFactory_ . Adds more enterprise-specific functionality:

            * Более простая интеграция с функциями Spring AOP
            * Message resource handling (for use in internationalization)
            * Event publication
            * Application-layer specific contexts such as the **WebApplicationContext** for use in web applications.

    .. container:: remark-block

        Чаще всего используются следующие реализации AppicationContext:

            * **ClassPathXmlApplicationContext**
            * **GenericApplicationContext**
            * **FileSystemXmlApplicationContext**
            * **AnnotationConfigApplicationContext**

            * *.....*


BeanFactory
------------
.. container:: left-col

    Это самый простой контейнер, который обеспечивает базовую поддержку **DI** и который основан на интерфейсе ``org.springframework.beans.factory.BeanFactory``.

    .. note::
        BeanFactory обычно используется тогда, когда ресурсы ограничены (мобильные устройства). 

        Поэтому, если ресурсы не сильно ограничены, то лучше использовать ApplicationContext_.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `The BeanFactory (docs.spring.io) <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-beanfactory>`_

.. _AnnBasedConf:

Annotation-based Configuration
--------------------------------
.. container:: left-col

    .. container:: code-markup

        ``@Configuration``
            Помечает, что класс является конфигурацией, и в нем могут быть декларированы ``@Bean`` методы (см :ref:`SpringBeans`).

            Конфигурация процессится Spring контейнером для генерации **BeanDefinition**

            .. note::
                ``@Configuration`` класс является ``@Component``, а то есть может быть представлен в контексте как бин.

                ``@Autowired``, ``@Bean`` (+ все аннотации над бинами), и т.д. ведут себя так же, как и в определении Компонента (см :ref:`AnnBaseBean`).

        ``@Import``
            Позволяет импортировать другие конфигурации.

        ``@Required``
            This annotation indicates that the affected bean property must be populated at configuration time, through an explicit property value in a bean definition or through autowiring. 
            
            The container throws an exception if the affected bean property has not been populated.

        ``@ComponentScan``
            Используется с аннотацией ``@Configuration``. Производит поиск ``@Component`` бинов в специфицированном пакете и обрабатывает их в контексте.

        ``@EnableAspectJAutoProxy``
            Enables support for handling components marked with AspectJ's annotation. 
            
            ~ Компонент (бин) с желаемой аннотацией (``@Aspect``) должен определяться внутри данной конфигурации 

        ``@PropertySource``
            Указывает, что в данной конфигурации используется указанный файл с пропетрями для аннотации ``@Value``

            .. container:: code-markup

                ``@Value``
                    Специфицирует значение параметра/поля в конфигурации или бине.

                    Может как ссылаться на имя проперти в проперти-файле (т.е. быть Spring-expression) или являться конечным строковым значением.

        ``@Lazy``
            When we put ``@Lazy`` annotation over the ``@Configuration`` class, it indicates that all the methods with ``@Bean`` annotation should be loaded lazily.



.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Annotation-based Container Configuration <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-annotation-config>`_

    .. code-block:: java

        @Configuration
        @EnableAspectJAutoProxy
        @Import({LoggerConfig.class, EventConfig.class})
        @PropertySource("classpath:application.properties")
        public class ApplicationConfig {

            @Bean
            public LoggingAspect aspLogger() {
                return new LoggingAspect();
            }

            @Bean
            @Scope("prototype")
            @Primary
            public Event fstEvent() {
                return new Event(new Date(), DateFormat.getDateTimeInstance());
            }

            @Bean
            public Event sndEvent (
                @Qualifier("fstEvent") Event event, 
                @Value("${event.name}") String name
            ) {
                ...
            }

            @Autowired
            @Qualifier("exConsoleLogger")
            private EventLogger consoleLogger;

            @Autowired
            private void configure (
                EventLogger fstLogger, 
                @Qualifier("sndLogger") EventLogger consoleLogger
            ) {
                ...
            }
        }



Container Extension Points
---------------------------
.. container:: left-col

    The BeanPostProcessor interface defines callback methods, которые можно реализовать, чтобы обеспечить собственную логику создания экземпляров, логику разрешения зависимостей и т. д.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Container Extension Points (docs.spring.io) <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-extension>`_
    