.. include:: ../../defs.rst

Порождающие
*************

.. contents:: Порождающие патерны
    :local:

.. container:: left-col

    Отвечают за удобное и безопасное создание новых объектов или даже целых семейств объектов.

Фабричный метод
===============

.. container:: left-col

    Определяет общий интерфейс для создания объектов в суперклассе, позволяя подклассам изменять тип создаваемых объектов.
    Данный шаблон делегирует создание объектов наследникам родительского класса.

    Паттерн Фабричный метод предлагает создавать объекты не напрямую, используя оператор new, а через вызов особого фабричного метода.

    .. toggle-header::
        :header: Аналогия

        .. figure:: ../../_static/patterns/11_.png

.. container:: right-col

    .. figure:: ../../_static/patterns/11.png


Абстрактная фабрика
====================

.. container:: left-col

    Позволяет создавать семейства связанных объектов, не привязываясь к конкретным классам создаваемых объектов.

    Паттерн Абстрактная фабрика предлагает выделить общие интерфейсы для отдельных продуктов, составляющих семейства.
    Далее создается общий интерфейс, который содержит методы создания всех продуктов семейства.

    .. tip::

        Классы Абстрактной фабрики чаще всего реализуются с помощью Фабричного метода, хотя они могут быть построены и на основе Прототипа.

    .. toggle-header::
        :header: Аналогия

        .. figure:: ../../_static/patterns/12_.png

.. container:: right-col

    .. figure:: ../../_static/patterns/12.png

Строитель (Builder)
===================

.. container:: left-col

    Позволяет создавать сложные объекты пошагово. Строитель даёт возможность использовать один и тот же код строительства для получения разных представлений объектов.

    Паттерн Строитель предлагает вынести конструирование объекта за пределы его собственного класса, поручив это дело отдельным объектам, называемым строителями (Builder).

    .. toggle-header::
        :header: Аналогия

        .. figure:: ../../_static/patterns/13_.png

.. container:: right-col

    .. figure:: ../../_static/patterns/13.png

Прототип 
========

.. container:: left-col

    Позволяет копировать объекты, не вдаваясь в подробности их реализации.

    Паттерн Прототип поручает создание копий самим копируемым объектам. Он вводит общий интерфейс для всех объектов, поддерживающих клонирование. 
    Это позволяет копировать объекты, не привязываясь к их конкретным классам

    .. toggle-header::
        :header: Аналогия

        .. figure:: ../../_static/patterns/14_.png

.. container:: right-col

    .. figure:: ../../_static/patterns/14.png

Одиночка (Singleton)
=====================

.. container:: left-col

    Гарантирует, что у класса есть только один экземпляр, и предоставляет к нему глобальную точку доступа.

    Все реализации одиночки сводятся к тому, чтобы скрыть конструктор по умолчанию и создать публичный статический метод, который и будет контролировать жизненный цикл объекта-одиночки.

    .. toggle-header::
        :header: Аналогия

        .. figure:: ../../_static/patterns/15_.png

.. container:: right-col

    .. figure:: ../../_static/patterns/15.png