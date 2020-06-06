.. include:: ../../defs.rst

Spark
##################

.. container:: left-col

    * Кластер_

    * MapReduce_

    * `Data locality`_

    * RDD_

        * Трансформации_

        * Действия_

        * Dataset_

    * SparkContext_


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Spark tutorial <https://spark.apache.org/docs/latest/quick-start.html>`_

        `Мифы о Spark, или Может ли пользоваться Spark обычный Java-разработчик <https://habr.com/ru/company/jugru/blog/325070/>`_

    .. raw:: html

        <iframe width="560" height="315" src="https://www.youtube.com/embed/XLSQJQjmFFw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Кластер
*******
.. container:: left-col

    Группа компьютеров, объединённых высокоскоростными каналами связи, представляющая с точки зрения пользователя единый аппаратный ресурс. 

    Кластер - слабо связанная совокупность нескольких вычислительных систем, работающих совместно для выполнения общих приложений, и представляющихся пользователю единой системой
    

MapReduce
**********
.. container:: left-col

    MapReduce — это фреймворк для вычисления некоторых наборов распределенных задач с использованием большого количества компьютеров (называемых «нодами»), образующих кластер.

    Работа MapReduce состоит из двух шагов: Map и Reduce, названных так по аналогии с одноименными функциями высшего порядка, map и reduce.

    На Map-шаге происходит предварительная обработка входных данных. Для этого один из компьютеров (называемый главным узлом — master node) получает входные данные задачи, разделяет их на части и передает другим компьютерам (рабочим узлам — worker node) для предварительной обработки.

    На Reduce-шаге происходит свёртка предварительно обработанных данных. Главный узел получает ответы от рабочих узлов и на их основе формирует результат — решение задачи, которая изначально формулировалась.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Википедия <https://ru.wikipedia.org/wiki/MapReduce#:~:text=MapReduce%20%E2%80%94%20%D0%BC%D0%BE%D0%B4%D0%B5%D0%BB%D1%8C%20%D1%80%D0%B0%D1%81%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D1%91%D0%BD%D0%BD%D1%8B%D1%85%20%D0%B2%D1%8B%D1%87%D0%B8%D1%81%D0%BB%D0%B5%D0%BD%D0%B8%D0%B9%2C%20%D0%BF%D1%80%D0%B5%D0%B4%D1%81%D1%82%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%B0%D1%8F,%D0%BD%D0%B0%D0%B1%D0%BE%D1%80%D0%B0%D0%BC%D0%B8%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85%20%D0%B2%20%D0%BA%D0%BE%D0%BC%D0%BF%D1%8C%D1%8E%D1%82%D0%B5%D1%80%D0%BD%D1%8B%D1%85%20%D0%BA%D0%BB%D0%B0%D1%81%D1%82%D0%B5%D1%80%D0%B0%D1%85.>`_

        https://www.bigdataschool.ru/wiki/mapreduce

    .. toggle-header::
        :header: Изображения

        .. figure:: ../../_static/MapReduce0.png

        |br|

        .. figure:: ../../_static/MapReduce3.png

Data locality
*************
.. container:: left-col

    ~ Код распределяется по кластеру и выполняется на всех собранных данных. Паралельно обрабатывает данные и собирается в одно место (mapReduce).



RDD
*******
.. container:: left-col

    RDD - Resilient Distributed Dataset (Dataset, над которым можно делать преобразования двух типов)

    Dataset
        +-Stream. Представляют собой коллекции

    Distrubuted
        Распределенные коллекции

    Resilient
        Отвечает за востановление данных, при возникновении ошибки на ноде с частью данных

Трансформации 
=============
.. container:: left-col

    Результатом применения данной операции к RDD является новый RDD. Как правило, это операции, которые каким-либо образом преобразовывают элементы данного датасета

.. container:: right-col

    .. container:: remark-block

        ``map``, ``filter``, ``distinct``, ``union``, ``intersection``, ``cartesian``

Действия 
=========
.. container:: left-col

    Применяются тогда, когда необходимо материализовать результат — как правило, сохранить данные на диск, либо вывести часть данных в консоль

.. container:: right-col

    .. container:: remark-block

        ``saveAsTextFile``, ``collect``, ``take``, ``count``, ``reduce``


Dataset 
========
.. container:: left-col

    Strongly-typed like an RDD, with richer optimizations under the hood

    Dataset - распределенная коллекция айтемов, primary abstraction.

    Dataset могут быть созданы через Hadoop InputFormats (such as HDFS files) или через преобразование других Dataset.


SparkContext
*************
.. container:: left-col

    SparkContext - обьект, который отвечает за реализацию более низкоуровневых операций с кластером.

    ~ Обращение к ResourceManager (при сабмите приложения), смотрит какую конфигурацию нужно задействовать, сколько памяти было передано и тд.
    Выбирает ноду, являющуюся ApplicationMaster.
