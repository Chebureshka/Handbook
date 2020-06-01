.. include:: ../../defs.rst

JUnit
*********
Фреймворк автоматического тестирования

Лежит в основе TDD (Test-Driven Development) и входит в семейство фрейморков для тестирования xUnit.

Тестовый случай (Test Case) в юнит тестировании – это часть кода, которая проверяет, что другая часть кода  (в частности – метод) работает в соответствии с определёнными требованиями.
Тесты могут быть организованы  в связки тестов (test suites).

Аннотации
=========

@Before 
    Обозначает методы, которые будут вызваны до исполнения теста, методы должны быть ``public void``

@BeforeClass (@BeforeAll JUnit5)
    Обозначает методы, которые будут вызваны до создания экземпляра тест-класса, методы должны быть ``public static void``

@After 
    Обозначает методы, которые будут вызваны после выполнения теста, методы должны быть ``public void``

@AfterClass (@AfterAll JUnit5)
    Связана по смыслу с ``@BeforeClass``, но выполняет методы после теста, как и в случае с ``@BeforeClass``, методы должны быть ``public static void``.

@Test 
    Обозначает тестовые методы. Как и ранее, эти методы должны быть ``public void``. Здесь размещаются сами проверки. 
    Кроме того, у данной аннотации есть два параметра, ``expected`` — задает ожидаемое исключение и ``timeout`` — задает время, по истечению которого тест считается провалившимся.

@Ignore (@Disabled JUnit5)
    Если какой-либо тест по какой-либо серьезной причине нужно отключить(например, этот тест постоянно валится, но его исправление отложено до светлого будущего)
    Также, если поместить эту аннотацию на класс, то все тесты в этом классе будут отключены.


JUnit4
-------

@Rule
    Правила это некое подобие утилит для тестов, которые добавляют функционал до и после выполнения теста.

@RunWith(Suite.class)
    Класс, указанный в аннотации должен наследоваться от Runner.

    JUnit4 — запускалка по умолчанию, предназначена для запуска JUnit 4 тестов.

    Suite 
        Передается класс со статическим методом ``suite`` возвращающим тест(последовательность всех тестов).

        Для настройки запускаемых тестов используется аннотация ``@SuiteClasses``.

        .. code-block:: java

            @Suite.SuiteClasses( { OtherJUnit4Test.class, StringUtilsJUnit4Test.class })
            @RunWith(Suite.class)
            public class JUnit4TestSuite {}

    Parameterized 
        Запускалка, позволяет писать параметризированные тесты.

        Для этого в тест-классе объявляется статический метод возвращающий список данных, которые затем будут использованы в качестве аргументов конструктора класса через ``@Parameterized.Parameters``.
            
            
    Theories 
        Чем-то схожа с предыдущей, но параметризирует тестовый метод, а не конструктор.

        Данные помечаются с помощью ``@DataPoints`` и ``@DataPoint``, тестовый метод — с помощью ``@Theory``.

        .. code-block:: java

            @RunWith(Theories.class)
            public class StringUtilsJUnit4TheoryTest extends Assert {
        
                @DataPoints
                public static Object[][] isEmptyData = new Object[][] { ... }
            
                @DataPoint
                public static Object[] nullData = new Object[] { null, true }
            
                @Theory
                public void testEmpty(final Object... testData) { ... }
            }

    Category
        Попытка организовать тесты в категории(группы). Настраиваются запускаемые категории тестов в сюите.

        .. code-block:: java
    
            public class StringUtilsJUnit4CategoriesTest extends Assert {
                @Category(Unit.class)
                @Test
                public void testIsEmpty() {}
            }
    
            @RunWith(Categories.class)
            @Categories.IncludeCategory(Unit.class)
            @Suite.SuiteClasses( { OtherJUnit4Test.class, StringUtilsJUnit4CategoriesTest.class })
            public class JUnit4TestSuite {}


JUnit5
------

.. todo:: TODO  https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations

Links
=======

https://habr.com/ru/post/120101/

https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations