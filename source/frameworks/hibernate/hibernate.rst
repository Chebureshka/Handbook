.. include:: ../../defs.rst

Hibernate
##############
.. container:: left-col

    **Hibernate** — самая популярная реализация спецификации `JPA`_, предназначенная для решения задач объектно-реляционного отображения (**ORM**).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Hibernate <https://ru.wikipedia.org/wiki/Hibernate_(%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA%D0%B0)>`_

        `Шпаргалка Java программиста 1: JPA и Hibernate в вопросах и ответах <https://habr.com/ru/post/265061/>`_

        `JPA : Знакомство с технологией. Пример использования Hibernate <https://javarush.ru/groups/posts/2259-jpa--znakomstvo-s-tekhnologiey>`_

Базовые определения
************************
.. container:: left-col

    **Object-Relational Mapping (ORM)** позволяет проектировать работу с данными в терминах классов, а не таблиц данных 
    и напротив, преобразовать термины и данные классов в данные, пригодные для хранения в СУБД. Так же обеспечивает интерфейс для **CRUD** операций.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `ORM <https://ru.bmstu.wiki/ORM_(Object-Relational_Mapping)#:~:text=ORM%20(Object%2DRelational%20Mapping)%20%E2%80%93%20%D1%82%D0%B5%D1%85%D0%BD%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F%20%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F%2C%20%D0%BA%D0%BE%D1%82%D0%BE%D1%80%D0%B0%D1%8F,%C2%AB%D0%B2%D0%B8%D1%80%D1%82%D1%83%D0%B0%D0%BB%D1%8C%D0%BD%D1%83%D1%8E%20%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D1%83%D1%8E%20%D0%B1%D0%B0%D0%B7%D1%83%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85%C2%BB.>`_



.. container:: left-col

    **Data Access Object (DAO)** — широко распространенный паттерн для сохранения объектов бизнес-области в базе данных. 
    В самом широком смысле, **DAO** — это класс, содержащий **CRUD** методы для конкретной сущности.

    .. code-block:: java

        public interface AccountDAO {

            Account get(String userName);
            void create(Account account);
            void update(Account account);
            void delete(String userName);

        }

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `DAO <https://ru.wikipedia.org/wiki/Data_Access_Object>`_
    
        `Забудьте о DAO, используйте Repository <https://habr.com/ru/post/263033/>`_



.. container:: left-col

    **CRUD** — акроним, обозначающий четыре базовые функции, используемые при работе с базами данных: *create, read, update, delete*. 



.. container:: left-col

    **Data Transfer Object (DTO)** - один из шаблонов проектирования, используется для передачи данных между подсистемами приложения. **DTO** не содержит какого либо поведения.

        В **Enterprise JavaBeans (EJB)**: **Entity Beans** представляют объекты, находящиеся в постоянном хранилище, например, в базе данных. Минус такого подхода: так как **DTO** используется лишь как объект 
        инкапсулирующий логику передачи/обновления/... состояния, то каждое изменение в *entity bean* может вызывать методы удалённого доступа, 
        что увеличивает нагрузку на сеть и снижает скорость работы программы.



Persistence
=============
.. container:: left-col

    Persistence относится к характеристике состояния, пережившего процесс, создавший его.
    Например, программы редактирования изображений или текстовые редакторы достигают персистентности, сохраняя свои документы в файлы.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Persistence <https://en.wikipedia.org/wiki/Persistence_(computer_science)>`_



Persistence techniques
-------------------------
.. container:: left-col

    **System images**
        Образ системы представляет собой сериализованную копию всего состояния компьютерной системы, сохраненную в некоторой энергонезависимой форме, такой как файл

        **Orthogonal**: Режим гибернации - не требует никаких действий от других программ для сохранения состояния и записывает всю используемую RAM память в постоянное хранилище. 

        **Non-orthogonal**: Программа редактирования текста, выполняющая конкретные инструкции для сохранения всего документа в файл.

    **Journals**
        Ведение журнала - процесс сохранения событий в логе перед каждым изменением в системе. Такие логи называются журналами (*Journals*). Example: Undo/Redo.

    **Dirty writes**
        Метод записи в хранилище только тех частей состояния системы, которые были изменены (загрязнены) с момента последней записи.

        *Например, сложные приложения для редактирования документов будут использовать грязные записи для сохранения только тех частей документа, 
        которые были фактически изменены с момента последнего сохранения.*

.. container:: right-col

    .. container:: remark-block

        .. rubric:: Orthogonal or transparent persistence

        - Среда с **ортогональной персистентностью** не требует каких-либо специальных действий от программ, работающих в ней, для извлечения или сохранения своего состояния.
          *Orthogonal persistence is widely adopted in operating systems for hibernation and in platform virtualization systems such as VMware and VirtualBox for state saving*.

        - **Неортогональная персистентнось** требует, чтобы данные записывались и читались в хранилище и из него с использованием специальных инструкций в программе 
          (*по завершении программа сохраняет данные*).


.. _JPARef:

JPA
======
.. container:: left-col

    **Java Persistence API** (JPA) — спецификация API Java EE, предоставляет возможность сохранять в удобном виде Java-объекты в базе данных.
    Это не единственный способ сохранения java объектов в базы данных (ORM систем), но один из самых популярных в Java мире.

    Области, определяемые спецификацией JPA и обеспеивающие сохранность данных:

        - **API**, заданный в пакете ``javax.persistence``
        - **Java Persistence Query Language**
        - *Метаинформация*, описывающая связи между объектами
        - Генерация **DDL** для сущностей

    **Entity** (Сущность, *persistent domain object*) — *POJO*-класс (но может так же содержать методы и реализовывать интерфейсы), связанный с БД с помощью аннотации ``@Entity`` или через **XML**. 
    К такому классу предъявляются требования, связанные с внутренними полями и определениями:

        - Должен иметь пустой конструктор (``public`` или ``protected``). Так же может содержать непустые конструкторы.
        - Не может быть вложенным, интерфейсом или ``enum``
        - Не может быть ``final`` и не может содержать ``final``-полей/свойств
        - Должен содержать хотя бы одно ``@Id``-поле

    Entities могут быть связаны друг с другом (один-к-одному, один-ко-многим, многие-к-одному и многие-ко-многим).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Java Persistence API <https://ru.wikipedia.org/wiki/Java_Persistence_API>`_

        `JSR 338: JavaTM Persistence API <http://download.oracle.com/otn-pub/jcp/persistence-2_2-mrel-spec/JavaPersistence.pdf>`_

        `JPA : Знакомство с технологией <https://javarush.ru/groups/posts/2259-jpa--znakomstvo-s-tekhnologiey>`_

    .. container:: remark-block

        **Спецификация** — это описание **API**, которое выражает данную концепцию. 
        **Спецификация** рассказывает, какими средствами должны быть обеспечены (т.е. через какие интерфейсы можно работать), чтобы работать по концепции **ORM**. 
        И как использовать эти средства. Реализацию средств спецификация не описывает.

        Т.е. Воплощением **ORM** является спецификация **JPA**. А реализацией спецификации **JPA** является **Hibernate**.

        **JPA** только описывает правила и **API**, а **Hibernate** реализует эти описания, впрочем у **Hibernate** (как и у многих других реализаций **JPA**) есть дополнительные возможности, 
        не описанные в **JPA** (и не переносимые на другие реализации **JPA**).

.. _JDORef:

JDO
-------------------------
.. container:: left-col

    JDO - одна из стандартных технологий (спецификаций) ORM на платформе JavaEE.

    **Java Data Objects** is a specification of Java object persistence. One of its features is a transparency of the persistence services to the domain model.

    **JPA** (**Java Persistence API**) и **Java Data Objects** (**JDO**) две спецификации сохранения java объектов в базах данных. 
    Если *JPA* сконцентрирована только на реляционных базах, то **JDO** более общая спецификация которая описывает ORM для любых возможных баз и хранилищ. 
    В принципе можно рассматривать **JPA** как специализированную на релятивистских баз часть спецификации **JDO**, даже при том что API этих двух спецификаций не полностью совпадает. 
    Также отличаются «разработчики» спецификаций — если **JPA** разрабатывается как JSR, то **JDO** сначала разрабатывался как JSR, теперь разрабатывается как проект Apache JDO.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `JDO <https://en.wikipedia.org/wiki/Java_Data_Objects>`_

        `JDO: долговременное хранение Java-объектов <https://www.osp.ru/os/2001/09/180408>`_

.. _HibernateRef:

Hibernate
**********************************


