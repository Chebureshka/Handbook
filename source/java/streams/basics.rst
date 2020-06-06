.. include:: ../../defs.rst


Stream. Основы
==============
.. container:: left-col

    Stream API это новый способ работать со структурами данных в функциональном стиле.


Устройство работы
-----------------
.. container:: left-col

    Внутри stream содержит Spliterator, который так же как и Iterator позволяет итерироваться по объектам, и к тому же содержит методы для возможности разбиения одного Spliterator на несколько для возможности паралельной обработки лежащих в них сущностей. 

    Конвеерные операции создают новый стрим, в котором Spliterator оборачивает Spliterator предыдущего стрима, определенным образом, при этом новый Spliterator будет иметь (в зависимости от операции) новые характеристики (`Изменение свойств стримов (Сплитераторов)`_).

.. container:: left-col

    Однопроходность стримов обуславливается наличием такого Spliterator'а, паралельным подходом к обработке и возможностью хранения бесконечных последовательностей.
    Так же если был создан новый стрим после выполнения конвеерной операции, он так же будет не валиден, после терминализации нового, так как Spliterator декарирует другой Spliterator, а не копирует его.

.. container:: right-col

    .. toggle-header::
        :header: Пример однопроходности

        .. code-block:: java

            Stream<?> stream = MyStream();

            Stream<?> stream2 = stream.map(Element::getField1);

            // Ругнеться!
            Stream<?> stream3 = stream.map(Element::getField2);

            stream2.collect(...)

            // Ругнеться!
            stream2.collect(...)

Конвеерные методы работы со стримами
------------------------------------
.. container:: left-col

    .. container:: code-markup

        ``filter``	
            Отфильтровывает записи, возвращает только записи, соответствующие условию

            .. code-block:: java

                collection.stream()
                          .filter("a1"::equals)
                          .count();


        ``skip``
            Позволяет пропустить N первых элементов

            .. code-block:: java

                collection.stream()
                          .skip(collection.size() - 1)
                          .findFirst().orElse("1");


        ``distinct``
            Возвращает стрим без дубликатов (для метода equals)	

            .. code-block:: java

                collection.stream()
                          .distinct()
                          .collect(Collectors.toList());


        ``map``	
            Преобразует каждый элемент стрима

            .. code-block:: java

                collection.stream()
                          .map((s) -> s + "_1")
                          .collect(Collectors.toList());


        ``peek``
            Возвращает тот же стрим, но применяет функцию к каждому элементу стрима

            .. code-block:: java

                collection.stream()
                          .map(String::toUpperCase)
                          .peek((e) -> System.out.print("," + e))
                          .collect(Collectors.toList());


        ``limit``
            Позволяет ограничить выборку определенным количеством первых элементов

            .. code-block:: java

                collection.stream()
                          .limit(2)
                          .collect(Collectors.toList());


        ``sorted``
            Позволяет сортировать значения либо в натуральном порядке, либо задавая Comparator

            .. code-block:: java

                collection.stream()
                          .sorted()
                          .collect(Collectors.toList());


        ``mapToInt``, ``mapToDouble``, ``mapToLong``
            Аналог map, но возвращает числовой стрим (то есть стрим из числовых примитивов)	

            .. code-block:: java

                collection.stream()
                          .mapToInt((s) -> Integer.parseInt(s))
                          .toArray();


        ``flatMap``, ``flatMapToInt``, ``flatMapToDouble``, ``flatMapToLong``
            Похоже на map, но может создавать из одного элемента несколько

            .. code-block:: java

                collection.stream()
                          .flatMap((p) -> Arrays.asList(p.split(","))
                                                .stream())
                          .toArray(String[]::new);


Изменение свойств стримов (Сплитераторов)
-----------------------------------------
.. container:: left-col

    Immutable
        DataSource не меняет свое состояние в процессе работы со стримом. |br| т.е. никакой другой поток в тот-же момент когда происходит обработка dataSource в стриме не внесет никаких изменений.

    Concurrent
        DataSource может изменяться в процессе работы стрима извне. Противоположно Immutable.

    Distinct
        На текущей стадии стрима (после выполнения одной из промеэуточных операций или при создании стрима) все элементы различные.

        .. container:: remark-block

            ``map``, ``flatMap`` - снимает характеристику |br|
            ``distinct`` - проставляет характеристику

    NonNull
        Гарантирует, что внутри стрима нет null елементов. Оптимизирует работу в отноении null-checking.

        .. container:: remark-block

            ``map``, ``flatMap`` - снимает характеристику 

    Ordered
        Гарантирует упорядоченный набор (Не отсортированный, а лишь важность порядка, т.е. последовательность элементов). Например dataSource:List - гарантирует порядок, а dataSource:Set - не гарантирует.

        .. container:: remark-block

                ``unordered`` - снимает характеристику |br|
                ``sorted`` - проставляет характеристику

    Sorted
        Гарантирует отсортированность по какому либо признаку.

        .. container:: remark-block

                ``map``, ``flatMap`` - снимает характеристику 

    Sized
        Показывает знает ли стрим (Spliterator) свой размер.

        .. container:: remark-block

                ``filter``, ``flatMap``, ``distinct`` - снимает характеристику |br|
                ``limit`` - проставляет характеристику

    Subsized
        В паралельных стримах кусочки будут знать свой размер. Т.е. Spliterator делит на определенные в отношении размера последовательности. Несбалансированное дерево например будет обладать характеристикой Sized, но не будет обладать характеристикой Subsized.


Терминальные методы работы со стримами
----------------------------------------
.. container:: left-col

    .. container:: code-markup

        ``findFirst``
            Возвращает первый элемент из стрима (возвращает Optional)	

            .. code-block:: java

                collection.stream()
                          .findFirst().orElse("1");


        ``findAny``
            Возвращает любой подходящий элемент из стрима (возвращает Optional)	

            .. code-block:: java

                collection.stream()
                          .findAny().orElse("1");


        ``collect``
            Представление результатов в виде коллекций и других структур данных	(+ много чего другого)

            .. code-block:: java

                collection.stream()
                          .filter((s) -> s.contains("1"))
                          .collect(Collectors.toList());

        ``count``
            Возвращает количество элементов в стриме

            .. code-block:: java

                collection.stream()
                          .filter("a1"::equals).count();

        ``anyMatch``
            Возвращает true, если условие выполняется хотя бы для одного элемента

            .. code-block:: java

                collection.stream()
                          .anyMatch("a1"::equals);

        ``noneMatch``
            Возвращает true, если условие не выполняется ни для одного элемента

            .. code-block:: java

                collection.stream()
                          .noneMatch("a8"::equals);

        ``allMatch``
            Возвращает true, если условие выполняется для всех элементов

            .. code-block:: java

                collection.stream()
                          .allMatch((s) -> s.contains("1"));

        ``min``
            Возвращает минимальный элемент, в качестве условия использует компаратор

            .. code-block:: java

                collection.stream()
                          .min(String::compareTo).get();

        ``max``
            Возвращает максимальный элемент, в качестве условия использует компаратор

            .. code-block:: java

                collection.stream()
                          .max(String::compareTo).get();

        ``forEach``
            Применяет функцию к каждому объекту стрима, порядок при параллельном выполнении не гарантируется

            .. code-block:: java

                set.stream()
                   .forEach((p) -> p.append("_1"));

        ``forEachOrdered``
            Применяет функцию к каждому объекту стрима, сохранение порядка элементов гарантирует

            .. code-block:: java

                list.stream()
                    .forEachOrdered((p) -> p.append("_new"));

        ``toArray``
            Возвращает массив значений стрима

            .. code-block:: java

                collection.stream()
                          .map(String::toUpperCase)
                          .toArray(String[]::new);

        ``reduce``
            Позволяет выполнять агрегатные функции на всей коллекцией и возвращать один результат

            .. code-block:: java

                collection.stream()
                          .reduce((s1, s2) -> s1 + s2).orElse(0);


Методы создания стримов
------------------------------------
.. container:: left-col

    Классический: Создание стрима из коллекции ``collection.stream()``
    
        .. code-block:: java
    
            Collection<String> collection = Arrays.asList("a1", "a2", "a3");
                    Stream<String> streamFromCollection = collection.stream();
    
    
    Создание стрима из значений ``Stream.of(значение1,… значениеN)``
    
        .. code-block:: java
    
            Stream<String> streamFromValues = Stream.of("a1", "a2", "a3");
    
    
    Создание стрима из массива ``Arrays.stream(массив)``
    
        .. code-block:: java
    
            String[] array = {"a1","a2","a3"};                     
            Stream<String> streamFromArrays = Arrays.stream(array);
    
    Создание стрима из файла (каждая строка в файле будет отдельным элементом в стриме)
    ``Files.lines(путь_к_файлу)``
    
        .. code-block:: java
    
            Stream<String> streamFromFiles = Files.lines(Paths.get("file.txt"))
    
    Создание стрима из строки
    ``"строка".chars()``
    
        .. code-block:: java
    
            IntStream streamFromString = "123".chars()
    
    С помощью Stream.builder ``Stream.builder().add(...)....build()``
    
        .. code-block:: java
    
            Stream.builder().add("a1").add("a2").add("a3").build()
    
    Создание параллельного стрима ``collection.parallelStream()``
    
        .. code-block:: java
    
            Stream<String> stream = collection.parallelStream();
    
    Создание бесконечных стрима с помощью ``Stream.iterate``
    
        .. code-block:: java
    
            Stream.iterate(начальное_условие, выражение_генерации)	
            Stream<Integer> streamFromIterate = Stream.iterate(1, n -> n + 1)
    
    Создание бесконечных стрима с помощью Stream.generate
    ``Stream.generate(выражение_генерации)``
    
        .. code-block:: java
    
            Stream<String> streamFromGenerate = Stream.generate(() -> "a1")