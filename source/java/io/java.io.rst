.. include:: ../../defs.rst

java.io 
********************************
.. container:: left-col

    Потокоориентированный, Блокирующий (синхронный) ввод/вывод

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Package java.io <https://docs.oracle.com/javase/10/docs/api/java/io/package-summary.html>`_

.. contents:: Иерархия:


InputStream
=============
.. container:: left-col

    Байтовый поток ввода используется для считывания данных (байт) с источника

    .. toggle-header::
        :header: Методы

        .. code-block:: java

            read(); read(byte[]); read(byte[], int, int); readAllBytes(); skip(long); ... close();

            // получить приблизительное число байт, которое можно прочитать из данного потока ввода
            available;

            // чтение всех байт из потока ввода и передача их в указанный поток вывода
            transferTo(OutputStream out);


        .. code-block:: java

            // метка позиции потока ввода
            mark;

            // сброс до помеченной позиции
            reset;

            // доступна ли возможность помечания позиции
            markSupported;

    |br|



FileInputStream
-----------------
.. container:: left-col

    Чтение из файла

.. container:: right-col

    .. code-block:: java

        new FileInputStream(fileName/File/FileDescriptor);


ByteArrayInputStream
------------------------
.. container:: left-col

    Чтение из указанного буфера

.. container:: right-col

    .. code-block:: java

        new ByteArrayInputStream(byte buf[]);

SequenceInputStream
-----------------------
.. container:: left-col

    Объединяет потоки ввода последовательно, т.е. после прочтения s1 последует чтение из s2.

    Внутренние потоки закрываются по достижению их конца, а также при явном вызове close() 

.. container:: right-col

    .. code-block:: java

        new SequenceInputStream(InputStream s1, InputStream s2);

ObjectInputStream
--------------------
.. container:: left-col

    Чтение ранее сериализованных данных из потока. В конструкторе он принимает ссылку на поток ввода.

    Вложенный поток закрывается при закрытии данного.

.. container:: right-col

    .. code-block:: java

        new ObjectInputStream(InputStream in);

PipedInputStream
-----------------
.. container:: left-col

    Анологично PipedOutputStream_


FilterInputStream
--------------------
.. container:: left-col



    Оборачивает базовый стрим (делегирование), добавляя некоторые полезные методы

.. container:: right-col

    .. code-block:: java

        new FilterInputStream(InputStream in);

DataInputStream
^^^^^^^^^^^^^^^
.. container:: left-col

    Входной поток, включающий методы для чтения стандартных типов данных Java


BufferedInputStream
^^^^^^^^^^^^^^^^^^^
.. container:: left-col

    Буферезированный входной поток 


PushbackInputStream
^^^^^^^^^^^^^^^^^^^^

.. container:: right-col

    .. code-block:: java

        new PushbackInputStream(InputStream in);



OutputStream
==============
.. container:: left-col

    Байтовый поток вывода используется для записи данных (байт) по месту назначения

    .. toggle-header::
        :header: Методы

        .. code-block:: java

            write(); write(byte[]); write(byte[], int, int); ... close();

            // очищает буфер вывода, записывая все его содержимое
            flush;

    |br|

FileOutputStream
-----------------
.. container:: left-col

    Запись в файл

.. container:: right-col

    .. code-block:: java

        new FileOutputStream(fileName/File/FileDescriptor);


ByteArrayOutputStream
------------------------
.. container:: left-col

    Создает буфер в памяти. Все данные, посылаемые в этот поток, размещаются в созданном буфере.


ObjectOutputStream
----------------------------
.. container:: left-col

    Оборачивает поток вывода для отправки в него сериализованных объектов.

    Вложенный поток закрывается при закрытии данного.

.. container:: right-col

    .. code-block:: java

        new ObjectOutputStream(OutputStream out);

PipedOutputStream
-----------------
.. container:: left-col

    Предоставляет канал для передачи байт в указанный PipedInputStream_ ``(connect(PipedInputStream snk))``


FilterOutputStream
-------------------
.. container:: left-col



    Оборачивает базовый стрим (делегирование), добавляя некоторые полезные методы.

    Вложенный поток закрывается при закрытии данного.

.. container:: right-col

    .. code-block:: java

        new FilterOutputStream(OutputStream out);

DataOutputStream
^^^^^^^^^^^^^^^^
.. container:: left-col

    Выходной поток, включающий методы для записи стандартных типов данных Java


BufferedOutputStream
^^^^^^^^^^^^^^^^^^^^
.. container:: left-col

    Буферизированный выходной поток


PrintStream
^^^^^^^^^^^
.. container:: left-col

    Используется для вывода на консоль, файл и тд.






Reader
========
.. container:: left-col

    Символьный поток ввода используется для считывания данных (символов) с источника

    .. toggle-header::
        :header: Методы

        .. code-block:: java

            read(); read(char[]); read(char[], int, int); 
            available(); skip(long); transferTo(..);
            mark(long); ... close();

    |br|

StringReader
---------------------------------
.. container:: left-col



    Входной поток, читающий из строки

.. container:: right-col

    .. code-block:: java

        new StringReader(String s);

CharArrayReader
---------------------------------
.. container:: left-col



    Входной поток, который читает из символьного массива

.. container:: right-col

    .. code-block:: java

        new CharArrayReader(char buf[]);

BufferedReader
---------------------------------
.. container:: left-col



    Буферизированный входной символьный поток

.. container:: right-col

    .. code-block:: java

        new BufferedReader(Reader in);

FilterReader
---------------------------------
.. container:: left-col




    Фильтрующий читатель (абстрактный класс)

.. container:: right-col

    .. code-block:: java

        new FilterReader(Reader in);

PipedReader
---------------------------------
.. container:: left-col


    Входной канал


PushbackReader
---------------------------------
.. container:: left-col

    Входной поток, позволяющий возвращать символы обратно в поток

.. container:: right-col

    .. code-block:: java

        new PushbackReader(Reader in);

InputStreamReader
---------------------------------
.. container:: left-col

    An InputStreamReader is a bridge from byte streams to character streams


FileReader
^^^^^^^^^^^^
.. container:: left-col



    Входной поток, читающий файл

.. container:: right-col

    .. code-block:: java

        new FileReader(fileName/File/FileDescriptor);



Writer
===========
.. container:: left-col

    Символьный поток вывода используется для записи данных (символы) по месту назначения

    .. toggle-header::
        :header: Методы

        .. code-block:: java

            write(); write(byte[]); write(byte[], int, int); 
            flush; ... close();

    |br|

StringWriter
---------------------------------
.. container:: left-col

    Выходной поток, пишущий в строку


BufferedWriter
---------------------------------
.. container:: left-col




    Буферизированный выходной символьный поток

.. container:: right-col

    .. code-block:: java

        new BufferedWriter(Writer out);

CharArrayWriter
---------------------------------
.. container:: left-col

    Выходной поток, который пишет в символьный массив


FilterWriter
---------------------------------
.. container:: left-col



    Фильтрующий писатель (абстрактный класс)

.. container:: right-col

    .. code-block:: java

        new FilterWriter(Writer out);

PipedWriter
---------------------------------
.. container:: left-col

    Выходной канал


PrintWriter
---------------------------------
.. container:: left-col

    Выходной поток в консоль, включающий методы ``print()`` и ``println()``


InputStreamWriter
---------------------------------
.. container:: left-col

    Выходной поток, транслирующий байты в символы


FileWriter
^^^^^^^^^^^
.. container:: left-col

    Выходной поток, пишущий в файл


File
========
.. container:: left-col


    Представляет дескриптор файла или каталога. В том числе не существующего.

    Позволяет получить информацию о файле: права доступа, время и дата создания, путь к каталогу. А также осуществлять навигацию по иерархиям подкаталогов.


RandomAccessFile
==================
.. container:: left-col


    Дескриптор файла, со случайным доступом. Реализуют интерфейсы DataInput и DataOutput.

    .. code-block:: java

        // позволяет переместиться к определенной позиции и изменить хранящееся там значение
        seek();
