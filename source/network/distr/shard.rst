.. include:: ../../defs.rst

Шардирование
*********************

.. container:: left-col

    Принцип проектирования базы данных, при котором логически независимые строки таблицы базы данных хранятся раздельно, 
    заранее сгруппированные в секции, которые, в свою очередь, размещаются на разных, физически и логически независимых серверах базы данных, при этом один физический узел кластера может содержать несколько серверов баз данных.


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Шардирование баз данных <https://ru.bmstu.wiki/%D0%A8%D0%B0%D1%80%D0%B4%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%B1%D0%B0%D0%B7_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85>`_

        `How Sharding Works <https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6>`_

        `Troubles With Sharding - What Can We Learn From The Foursquare Incident? <http://highscalability.com/blog/2010/10/15/troubles-with-sharding-what-can-we-learn-from-the-foursquare.html>`_

    .. container:: remark-block

        Данные горизонтально партиционируются при помощи некоего хеша, присвоенного партиции.