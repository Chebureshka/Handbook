.. include:: ../../defs.rst

Mockito
*********
.. container:: left-col

    Фреймворк для работы с заглушками mock

    .. container:: code-markup

        ``Mockito.mock`` 
            Оборачивает весь создаваемый объект, при этом вызов непереопределенных методов будет возвращать 0/null/пустые коллекции.

            .. note::
                ~ Может обернуть объект класса или интерфейса. При этом по умолчанию нельзя обернуть final класс, enum и переопределить final методы.

            .. code-block:: java

                SomeInterface mocked = mock(SomeInterface.class);

                instanceof DataService; // вернёт true, 
                dataServiceMock.getClass(); // — именно SomeInterface.class

        ``Mockito.mockingDetails`` 
            Возвращает **MockingDetails**, содержащий информацию о замоканном объекте с точки зрения фреймворка Mockito

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Mockito и как его готовить <https://habr.com/ru/post/444982/>`_

        `Шесть простых примеров по Mockito (перевод) <https://habr.com/ru/post/243155/>`_


Mockito.mock
=======================
.. container:: left-col

    .. code-block:: java

        when(mocked.method()).thenReturn("A").thenReturn("B");
        // будут возвращены методом последовательно, пока не вернётся последнее, 
        //   после этого при последующих вызовах будет возвращаться только последнее

        when(mocked.methodWithParam1("Param")).thenReturn(1);

        when(mocked.methodWithParam2(anyInt())).thenReturn(-1);
        // заглушка возвращает -1 вне зависимости от переданного аргумента

        when(mocked.methodWithParam3(argThat(arg -> arg == null || arg.length() > 5)));

        when(mocked.method()).thenAnswer(invocation -> {...});

        when(mocked.method()).thenThrow(new IllegalArgumentException());

        doReturn(-1).when(mocked).method();

        doThrow(new IOException()).when(mocked).method();

        verify(mock).method();
        // Определяет то, что method будет вызван (Должен быть вызван)


Mockito.spy
===============================
.. container:: left-col

    Если нужно использовать в качестве заглушки объект реального класса с имеющимся функционалом, переопределив работу только части его методов:

    .. code-block:: java

        DataService dataServiceSpy = Mockito.spy(DataService.class);
        // Или
        DataService dataService = new DataService();
        dataServiceSpy = Mockito.spy(dataService);

    При создании spy на основе класса (.class), если его тип — интерфейс, будет создан обычный mock-объект, а если тип — класс, то Mockito попытается создать экземпляр при помощи конструктора по умолчанию (без параметров).
    И только если такого конструктора нет, произойдёт ошибка и тест не сработает.


Аннотации
=============================
.. container:: left-col

    .. container:: code-markup

        ``@Mock`` 
            Ставится перед полем и мокает указанного типа объект. 

            .. code-block:: java

                @Mock DataService dataService;

        ``@BeforeMethod``, ``@AfterMethod`` 
            Вызывается перед/после метода

        ``@Test`` 
            Тестовый метод
