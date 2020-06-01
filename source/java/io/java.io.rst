.. include:: ../../defs.rst

java.io 
********************************

Потокоориентированный, Блокирующий (синхронный) ввод/вывод

.. contents:: Иерархия:



InputStream
=============

Байтовый поток ввода используется для считывания данных (байт) с источника

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
        


FileInputStream
-----------------
.. code-block:: java

    constructor(fileName/File/FileDescriptor);

Чтение из файла


ByteArrayInputStream
------------------------
.. code-block:: java

    constructor(byte buf[]);

Чтение из указанного буфера


SequenceInputStream
-----------------------
.. code-block:: java

    constructor(InputStream s1, InputStream s2);

Объединяет потоки ввода последовательно, т.е. после прочтения s1 последует чтение из s2.

Внутренние потоки закрываются по достижению их конца, а также при явном вызове close() 


ObjectInputStream
--------------------
.. code-block:: java

    constructor(InputStream in);

Чтение ранее сериализованных данных из потока. В конструкторе он принимает ссылку на поток ввода.

Вложенный поток закрывается при закрытии данного.


PipedInputStream
-----------------
Анологично PipedOutputStream_


FilterInputStream
--------------------
.. code-block:: java

    constructor(InputStream in);

Оборачивает базовый стрим (делегирование), добавляя некоторые полезные методы


DataInputStream
^^^^^^^^^^^^^^^
Входной поток, включающий методы для чтения стандартных типов данных Java


BufferedInputStream
^^^^^^^^^^^^^^^^^^^
Буферезированный входной поток 


PushbackInputStream
^^^^^^^^^^^^^^^^^^^^
.. code-block:: java

    constructor(InputStream in);





OutputStream
==============
Байтовый поток вывода используется для записи данных (байт) по месту назначения

.. code-block:: java

    write(); write(byte[]); write(byte[], int, int); ... close();

    // очищает буфер вывода, записывая все его содержимое
    flush;
        


FileOutputStream
-----------------
.. code-block:: java

    constructor(fileName/File/FileDescriptor);

Запись в файл


ByteArrayOutputStream
------------------------
Создает буфер в памяти. Все данные, посылаемые в этот поток, размещаются в созданном буфере.


ObjectOutputStream
----------------------------
.. code-block:: java

    constructor(OutputStream out);

Оборачивает поток вывода для отправки в него сериализованных объектов.

Вложенный поток закрывается при закрытии данного.


PipedOutputStream
-----------------
Предоставляет канал для передачи байт в указанный PipedInputStream (connect(PipedInputStream_ snk))


FilterOutputStream
-------------------
.. code-block:: java

    constructor(OutputStream out);

Оборачивает базовый стрим (делегирование), добавляя некоторые полезные методы.

Вложенный поток закрывается при закрытии данного.


DataOutputStream
^^^^^^^^^^^^^^^^
Выходной поток, включающий методы для записи стандартных типов данных Java


BufferedOutputStream
^^^^^^^^^^^^^^^^^^^^
Буферизированный выходной поток


PrintStream
^^^^^^^^^^^
Используется для вывода на консоль






Reader
========
Символьный поток ввода используется для считывания данных (символов) с источника

.. code-block:: java

    read(); read(char[]); read(char[], int, int); 
    available(); skip(long); transferTo(..);
    mark(long); ... close();


StringReader
---------------------------------
.. code-block:: java

    constructor(String s);

Входной поток, читающий из строки


CharArrayReader
---------------------------------
.. code-block:: java

    constructor(char buf[]);

Входной поток, который читает из символьного массива


BufferedReader
---------------------------------
.. code-block:: java

    constructor(Reader in);

Буферизированный входной символьный поток


FilterReader
---------------------------------
.. code-block:: java

    constructor(Reader in);

Фильтрующий читатель (абстрактный класс)


PipedReader
---------------------------------
Входной канал


PushbackReader
---------------------------------
.. code-block:: java

    constructor(Reader in);

Входной поток, позволяющий возвращать символы обратно в поток


InputStreamReader
---------------------------------
An InputStreamReader is a bridge from byte streams to character streams


FileReader
^^^^^^^^^^^^
.. code-block:: java

    constructor(fileName/File/FileDescriptor);

Входной поток, читающий файл




Writer
===========
.. code-block:: java

    write(); write(byte[]); write(byte[], int, int); 
    flush; ... close();

Символьный поток вывода используется для записи данных (символы) по месту назначения


StringWriter
---------------------------------
Выходной поток, пишущий в строку


BufferedWriter
---------------------------------
.. code-block:: java

    constructor(Writer out);

Буферизированный выходной символьный поток


CharArrayWriter
---------------------------------
Выходной поток, который пишет в символьный массив


FilterWriter
---------------------------------
.. code-block:: java

    constructor(Writer out);

Фильтрующий писатель (абстрактный класс)


PipedWriter
---------------------------------
Выходной канал


PrintWriter
---------------------------------
Выходной поток в консоль, включающий методы print() и println()


InputStreamWriter
---------------------------------
Выходной поток, транслирующий байты в символы


FileWriter
^^^^^^^^^^^
Выходной поток, пишущий в файл


File
========
Представляет дескриптор файла или каталога. В том числе не существующего.

Позволяет получить информацию о файле: права доступа, время и дата создания, путь к каталогу. А также осуществлять навигацию по иерархиям подкаталогов.


RandomAccessFile
==================
Дескриптор файла, со случайным доступом. Реализуют интерфейсы DataInput и DataOutput.

.. code-block:: java

    // позволяет переместиться к определенной позиции и изменить хранящееся там значение
    seek();
