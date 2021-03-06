.. include:: ../../defs.rst

Serialization 
********************************
.. container:: left-col

    Сериализация представляет процесс записи состояния объекта в поток, соответственно процесс извлечения или восстановления состояния объекта из потока называется десириализацией.



Serializable
==============
.. container:: left-col

    **Serializable** - интерфейс маркер, отвечающий за возможность сериализации объекта, и его полей (поля должны быть так же **Serializable**)

    Для **Serializable** не нужен конструктор для создания восстановления объекта. Он просто полностью восстановится из байтов, в отличии от Externalizable_.

    .. warning::
        ~ Однако если класс предок был несереализуемым, то этот суперкласс должен содержать доступный (*public*, *protected*) конструктор без параметров для инициализации полей.

    .. note::
        Сериализация через **Serializable** использует рефлексию.

    .. container:: code-markup

        ``transient``
            Ключевое слово для обозначения не сериализуемого поля. 
            При десириализации такие поля == ``null``.

Externalizable
================
.. container:: left-col

    **Externalizable** - интерфейс с двумя методами:

    .. code-block:: java

        void writeExternal(ObjectOutput out) throws IOException;
        void readExternal(ObjectInput in) throws IOException, ClassNotFoundException;

    При этом **ObjectOutput**/**ObjectInput** - интерфейсы, предоставляющие способ последовательной записи информации об объекте и способ аналогичного последовательного чтения соответственно.

    .. note::
        Externalizable работает **не** через рефлексию

    .. warning::
        При использовании данного способа сериализации необходимо иметь публичный определенный конструктор по умолчанию.

    В сравнении с Serializable_ объект не востанавливается из байтов, 
    при восстановлении сначала будет создан объект с помощью конструктора в точке объявления, а затем в него будут записаны значения его полей из байтов, полученных при сериализации


    Как **Externalizable** можно определить Serializable_, добавив (не переопределив) в него методы ``writeObject()`` и ``readObject()``. 
    Но для того, чтобы они «работали» нужно точно соблюсти их сигнатуру:

    .. code-block:: java

        void writeObject(ObjectOutputStream stream) throws IOException;
        void readObject(ObjectInputStream stream) throws IOException, ClassNotFoundException;

.. container:: right-col

    |br|