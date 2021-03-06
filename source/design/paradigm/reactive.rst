.. include:: ../../defs.rst

Реактивное программирование
############################

.. container:: left-col

    Реактивное программирование — это программирование с асинхронными потоками(streams) данных (парадигма программирования, ориентированная на потоки данных и распространение изменений).

    *Реактивная архитектура* позволяет программистам создавать *событийно-ориентированные*, *масштабируемые, отказоустойчивые* и *отзывчивые* приложения — приложения, 
    работающие в реальном времени и обеспечивающие хорошее время реакции, основанные на масштабируемом и отказоустойчивом стеке и которые легко развернуть на многоядерных и 
    облачных архитектурах. Эти особенности критически важны для реактивности.


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Реактивное программирование <https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%B0%D0%BA%D1%82%D0%B8%D0%B2%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5>`_

        `Вступление в Реактивное Программирование, которое вы пропустили <https://habr.com/ru/post/279715/>`_

        `Akka, акторы и реактивное программирование <https://habr.com/ru/company/piter/blog/266103/>`_

    .. container:: defs-block

        Словарь Merriam Webster даёт определение реактивному как «готовому реагировать на внешние события», что означает что компоненты всё время активны и всегда готовы получать сообщения.

            * реагируют на события
            * реагируют на повышение нагрузки
            * реагируют на сбои
            * реагируют на пользователей

    .. container:: remark-block

        Шины событий или типичные события нажатий на кнопки — примеры асинхронных событийных потоков, которые можно слушать и выполнять некоторые побочные действия.

The Reactive Manifesto
**********************

.. container:: left-col, code-markup

    Message Driven (Ориентированность на события)
        В событийно-ориентированном приложении компоненты взаимодействуют друг с другом путём отправки и получения сообщений — дискретных частей информации, описывающих факты. 
        Эти сообщения отправляются и принимаются в асинхронном и неблокирующем режиме. 

        Приложения, использующие асинхронную модель, намного лучше обеспечивают слабую связанность, чем приложения, базирующиеся на чисто синхронных вызовах. Отправитель и получатель могут быть реализованы, не оглядываясь на детали того, как события распространяются в системе.
        
        Событийно-ориентированные системы более склонны к push-модели, нежели чем к pull или poll. Т.е. они проталкивают данные к своим потребителям, 
        когда данные становятся доступными, вместо того чтобы впустую тратить ресурсы, постоянно запрашивая или ожидая данные.

    Elastic (Масштабируемость)
        The system stays responsive under varying workload (при изменяющейся нагрузке).

        Событийно-ориентированная система, базирующаяся на асинхронной передаче сообщений, является основой масштабируемости. 
        Слабая связанность и локационная независимость компонентов и подсистем позволяют разворачивать систему на множестве узлов, 
        оставаясь в пределах той же самой программной модели с той же семантикой. 
        При добавлении новых узлов возрастает пропускная способность системы. 

    Resilient (Отказоустойчивость)
        The system stays responsive in the face of failure.

        Событийно-ориентированная модель, которая даёт масштабируемость, также предоставляет необходимые примитивы для решения проблемы отказоустойчивости. 
        Слабая связанность в событийно-ориентированной модели снабжает нас полностью изолированными компонентами, 
        в которых сбои инкапсулируются в сообщения вместе с необходимыми деталями и пересылаются другим компонентам, которые в свою очередь анализируют ошибки и решают, 
        как реагировать на них.

    Responsive (Отзывчивость)
        The system responds in a timely manner if at all possible (приложения реального времени).
        Отзывчивый определяется словарём Merriam-Webster как «отвечающий быстро или реагирующий надлежащим образом».

        Наблюдаемые модели позволяют другим системам получать события, когда их состояние изменяется. Это обеспечивает связь в реальном времени между пользователями и системами. 

        .. tip::
            Например, когда несколько пользователей работают одновременно над одной и той же моделью, изменения могут реактивно синхронизироваться между ними, 
            избавляя от необходимости блокировки модели.

        Потоки событий образуют базовую абстракцию, на которой строятся такие связи. 
        Сохраняя их реактивными, мы избегаем блокировок и позволяем преобразованиям и коммуникациям быть асинхронными и неблокирующими.

        .. note::
            Реактивные приложения должны иметь знание о порядках алгоритмов, чтобы быть уверенным, что время отклика на события не превышает O(1) или, как минимум, 
            O(log n) независимо от нагрузки. Может быть включён коэффициент масштабирования, но он не должен зависеть от количества клиентов, сессий, продуктов или сделок.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `The Reactive Manifesto <https://www.reactivemanifesto.org/>`_

        `Реактивный манифест <https://habr.com/ru/post/195562/>`_

    .. figure:: ../../_static/bb48f5ff51683dc6bae66eae41c714ca.png

    .. container:: remark-block

        Реактивные приложения представляют собой сбалансированный подход для решения современных проблем в разработке программных систем. 
        Они построены на каркасе, ориентированном на события и передаче сообщений, и обеспечивают инструменты для обеспечения масштабируемости и отказоустойчивости. 
        Поверх этого они поддерживают насыщенные и отзывчивые интерфейсы взаимодействия с пользователем. 


FRP (Функциональное реактивное программирование)
************************************************

.. container:: left-col

    Функциональное программирование является наиболее естественным базисом для реализации реактивной архитектуры, хорошо сочетаясь с параллелизмом.

    Functional reactive programming (FRP) is a programming paradigm for reactive programming (asynchronous dataflow programming) using the building blocks of functional programming (e.g. map, reduce, filter).

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Functional reactive programming <https://en.wikipedia.org/wiki/Functional_reactive_programming>`_

        `FRP explanation using reactive-banana (Haskell) <http://wiki.haskell.org/FRP_explanation_using_reactive-banana>`_