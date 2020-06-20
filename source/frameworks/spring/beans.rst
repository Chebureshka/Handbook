.. include:: ../../defs.rst

.. _SpringBeans:

Spring Beans
===============
.. container:: left-col

    Одно из ключевых понятий в спринге — это **Bean**. По сути, это просто объект какого-то класса.
    Эти объекты создаются с помощью метаданных конфигурации, которые предоставляются контейнеру.

    .. code-block:: java 

        @Bean
        public EventLogger consoleLogger() {
            return new ConsoleEventLogger();
        }

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Bean Overview (docs.spring.io) <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-definition>`_

    .. container:: defs-block

        A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container.

        Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.



BeanDefinition
--------------
.. container:: left-col

    В самом контейнере определения bean-компонентов представлены в виде объектов **BeanDefinition**, которые содержат некоторые метаданные.
    Эти метаданные преобразуются в набор свойств, которые составляют каждый **BeanDefinition**:
    
    .. container:: code-markup

        Class 
            Имя класса с указанием пакета: обычно это фактический класс реализации определяемого компонента.

        Name
            Каждый Bean имеет один или несколько идентификаторов. These identifiers must be unique within the container that hosts the bean. 
            
            A bean usually has only one identifier. However, if it requires more than one, the extra ones can be considered *aliases*.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Spring - Bean Definition <https://www.tutorialspoint.com/spring/spring_bean_definition.htm>`_

    .. container:: remark-block

        When you create a bean definition, you create a recipe for creating actual instances of the class defined by that bean definition.

.. container:: left-col

    .. container:: code-markup

        Scope
            Определяет отношение между **BeanDefinition** и конкретными инстансами Bean-ов (например при указании аргументов конструктора или пропертей (см. следующий пункт)).

            The Spring Framework supports six scopes, four of which are available only if you use a web-aware ApplicationContext:

                .. container:: code-markup

                    singleton
                        (Default) Scopes a single bean definition to a single object instance for each Spring IoC container.

                    prototype
                        Scopes a single bean definition to any number of object instances.

                    request
                        Scopes a single bean definition to the lifecycle of a *single HTTP request*.

                    session
                        Scopes a single bean definition to the lifecycle of an *HTTP Session*.

                    application
                        Scopes a single bean definition to the lifecycle of a *ServletContext*.

                    websocket
                        Scopes a single bean definition to the lifecycle of a *WebSocket*.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Spring - Bean Scopes <https://www.tutorialspoint.com/spring/spring_bean_scopes.htm>`_

.. container:: left-col

    .. container:: code-markup

        Constructor arguments & Properties
            Ссылки на другие bean-компоненты, коллекции, примитивные значения, которые необходимы для работы. These references are also called collaborators or dependencies.


.. container:: left-col

    .. container:: code-markup

        Autowiring mode
            The Spring container can autowire relationships between collaborating beans.

            Можно указать autowiring для bean-компонента и, таким образом, можно выбирать, какой именно bean использовать. Описываются следующие autowiring modes:

                .. container:: code-markup

                    no
                        (Default) No autowiring

                    byName
                        Autowiring by property name. Spring looks for a bean with the same name as the property that needs to be autowired.

                    byType
                        Lets a property be autowired if exactly one bean of the property type exists in the container.

                    constructor
                        Аналогичен byType, но применяется к аргументам конструктора.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Spring Beans Auto-wiring Example using XML Configuration <http://websystique.com/spring/spring-beans-auto-wiring-example-using-xml-configuration/>`_
        
        `Spring - Beans Auto-Wiring <https://www.tutorialspoint.com/spring/spring_beans_autowiring.htm>`_

    .. code-block:: java 

        public class Application {

            private ApplicationUser applicationUser;

            public void setApplicationUser(ApplicationUser applicationUser) {
                this.applicationUser = applicationUser;
            }
        }

    .. code-block:: xml

        <!-- byName example -->
        <bean id="application" class="com.websystique.spring.domain.Application" autowire="byName"/>

        <bean id="applicationUser" class="com.websystique.spring.domain.ApplicationUser" >
            <property name="name" value="superUser"/>
        </bean>

.. container:: left-col

    .. container:: code-markup

        Lazy initialization mode
            By default, ApplicationContext implementations eagerly create and configure all singleton beans as part of the initialization process.
            A lazy-initialized bean tells the IoC container to create a bean instance when it is first requested, rather than at startup.

            .. note::
                When a lazy-initialized bean is a dependency of a singleton bean that is not lazy-initialized.

.. container:: left-col

    .. container:: code-markup


        Lifecycle Callbacks
            The container calls ``afterPropertiesSet()`` (Initialization method) *for the former* 
            and ``destroy()`` (Destruction method) *for the latter* to let the bean perform certain actions upon initialization and destruction of your beans.

            .. tip::
                The Lifecycle interface defines the essential methods for any object that has its own lifecycle requirements (such as starting and stopping some background process)

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Spring - Bean Life Cycle <https://www.tutorialspoint.com/spring/spring_bean_life_cycle.htm>`_

.. _AnnBaseBean:

Annotation-based Bean configuration
------------------------------------
.. container:: left-col

    Аннотации поддерживаются характерными ним BeanPostProcessor. CommonAnnotationBeanPostProcessor позволяет использовать конфигурацию с помощью аннотаций, независимо от выбранного BeanDefinitionReader (например через xml)

    .. container:: code-markup

        ``@Bean``
            Помечает метод в конфигурации или в компоненте (~), как определение бина. Метод впредь будет возвращать конкретное запроксированное представление бина в контексте.

            Для данной аннотации присутствуют следующие поля:

            .. container:: code-markup

                name
                    Указывает, какое имя будет иметь бин.

                autowire
                    Указывает, какой Autowiring mode будет у данного бина (см BeanDefinition_).

                initMethod
                    Указывает, какой метод будет обрабатывать событие инициализации этого бина в контексте (см ``@PostConstruct``).

                destroyMethod
                    Указывает, какой метод будет обрабатывать событие "уничтожения" этого объекта в контексте (см ``@PreDestroy``).

        ``@Component``
            Аннотированный класс описывает внутренние свойства BeanDefinition.
            Имя бина может быть задано параметром в ``@Component("name")``.

            Сама же декларация бина происходит с помощью аннотации ``@Bean`` или с помощью ``@ComponentScan``.

            Данная аннотация является базовой для многих других аннотаций в Spring:

            .. container:: code-markup

                ``@Service``
                    (Сервис-слой приложения) Аннотация, объявляющая, что этот класс представляет собой сервис – компонент сервис-слоя.

                    Сервис является подтипом класса ``@Component``. Использование данной аннотации позволит искать бины-сервисы автоматически.
                
                ``@Controller``
                    (Слой представления) Аннотация для маркировки java класса, как класса контроллера. Данный класс представляет собой компонент, похожий на обычный сервлет (HttpServlet) 
                    (работающий с объектами HttpServletRequest и HttpServletResponse), но с расширенными возможностями от Spring Framework. 
                
                ``@Repository``
                    (Доменный слой) Аннотация показывает, что класс функционирует как репозиторий и требует наличия прозрачной трансляции исключений. 
                    Преимуществом трансляции исключений является то, что слой сервиса будет иметь дело с общей иерархией исключений от Spring (DataAccessException) вне зависимости от используемых 
                    технологий доступа к данным в слое данных.

                ``@RestController``
                    Аннотация аккумулирует поведение двух аннотаций ``@Controller`` и ``@ResponseBody`` (показывает что данный метод может возвращать кастомный объект в виде xml, json...).
                    

        ``@PostConstruct``
            Помечает метод в компоненте, что он является ``post-construct``, и вызовется в соответствии с *Bean Lifecycle*: (``afterPropertiesSet()``).

        ``@PreDestroy``
            Помечает метод в компоненте, что он является ``pre-destroy``, и вызовется в соответствии с *Bean Lifecycle*: (``destroy()``).

        ``@DependsOn``
            Beans on which the current bean depends. Any beans specified are guaranteed to be created by the container before this bean. 

            Указывается как с ``@Bean``, так и с ``@Component``.

        ``@Scope``
            Специфицирует **Scope** бина.

            Указывается как с ``@Bean``, так и с ``@Component``.

        ``@Autowired``
            Аннотированное поле автоматически связывается (позднее связывание при инициализации бина) с конкретным бином в контейнере (по имени/по типу).

            Так же можно аннотировать сетер или конструктор - тогда связываться будут параметры сетера или конструктора, при их вызове при инициализации бина.

            .. container:: code-markup

                ``@Primary``
                    Indicates that a particular bean should be given preference when multiple beans are candidates to be autowired to a single-valued dependency.

                    Указывается как с ``@Bean``, так и с ``@Component``.

                ``@Qualifier``
                    Специфицирует имя бина для связывания. Может быть также указан с конкретными аргументами конструктора или сеттера.

                    Дефолтный ``Qualifier`` (т.е. ``Autowired`` без ``Qualifier``) - это имя поля.

            .. note::
                Инжекшн в спринге вначале отбирает по типам все подходящие бины, далее проверяет по ``Qualifier`` и под конец ``Primary``.

            .. note::
                Инжекшн нужно указывать для интерфесов, а не для классов, так как создаются динамические прокси (Proxy) (до Spring 5, после используется только cglib, создающий прокси как объект-наследник) 
                для искомых классов, а сохраняют они при этом интерфейсы а не классы (динамические прокси), по которым будут искаться бины.

            

        ``@Lazy``
            Помечает бин, что он инициализируется, только при первом обращении (*Lazy initialization mode*). 
            
            Аннотируется с ``@Bean`` или с ``@Component``, как и на конфигурации, при этом если помечен компонент - это значит, что сам компонент является Lazy, 
            в противоположность конфигурации - все бины в ней становятся Lazy (см. :ref:`AnnBasedConf`).

            .. tip::
                Со Spring 4.3 можно поставить с анотацией ``@Autowired``, для пометки того, что инжекшн будет ленивым, т.е. например, это позволит создавать в синглтонах ленивые (по первому вызову) зависимости.

        ``@Value``
            Вычисляет указанное spring-expression и присваивает в аннотированное поле / параметр (функции / конструктора)

            См. :ref:`AnnBasedConf`

        ``@Profile``
            Аннотация для создания профилей конфигурации проекта. Может применяться как к бинам так и к конфигурационным классам.

        ``@Aspect``
            Помечает класс, использующий aspectj.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Annotation-based Container Configuration <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-annotation-config>`__

        `Аннотации - Spring Framework Guide <https://ru.wikibooks.org/wiki/Spring_Framework_Guide>`_


    .. code-block:: java

        @Component
        @DependsOn(" ... ")
        public class ComponentExample extends SuperComponentExample {

            @Autowired
            ComponentExample (
                @Value("string example") String param,
                @Qualifier("dependentBean") MyBean bean
            ) {
                super(fileName);

                ...
            }

            @Bean
            @Lazy
            @Primary
            public MyBean bean1() {
                return new MyBean();
            }

            @Bean
            @Scope("singleton")
            public MyBean bean2() {
                return new MyBean();
            }

            @Bean
            @Scope("prototype")
            public MyBean bean3() {
                assert(bean2() == bean2()) // ибо singleton

                return new MyBean();
            }

            @PreDestroy
            public void destroy() {
                ...
            }
        }

EventListener
-----------------------------------------