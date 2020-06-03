.. include:: ../../defs.rst

Redis
##################


.. contents:: 



Основы
******
**Redis** - Удаленная база данных в оперативной памяти, которая предлагает высокую производительность, репликацию и уникальную модель данных для создания платформы для решения проблем.
    
**Redis** - in-memory data structure, используемая как база данных, кеш и брокер сообщений. Он классифицируется как хранилище значений ключей NoSQL. 

**Redis** имеет встроенную репликацию, Lua-скриптинг, выключение LRU, транзакции и различные уровни сохранения на диске (sync / async).

**Redis** holds its database entirely in the memory, using the disk only for persistence.


From version 3, Redis works & recommends multi-master mode where failover, sharding/paritioning, resharding features are in-built.
In order for the redis cluster to operate a minimum of 3 master nodes/processes are required.
Additional features are replication, persistence, and client-side sharding.
Redis can replicate data to any number of slaves.


Redis Advantages
================
Exceptionally fast - perform about 110000 SETs per second, about 81000 GETs per second

Supports rich data types

Operations are atomic − All Redis operations are atomic, which ensures that if two clients concurrently access, Redis server will receive the updated value.

Multi-utility tool:

    Redis is a multi-utility tool and can be used in a number of use cases such as caching, messaging-queues (Redis natively supports Publish/Subscribe), 
    any short-lived data in your application, such as web application sessions, web page hit counts, etc.




Поддерживаемые типы данных
**************************
STRING (Operate on the whole string, parts, integers and floats)
    Redis string is a sequence of bytes. Strings in Redis are binary safe, meaning they have a known length not determined by any special terminating characters. 
    Thus, you can store anything up to 512 megabytes in one string.

СПИСОК (Push или pop елементов с обоих концов)
    Redis Lists are simply lists of strings, sorted by insertion order. You can add elements to a Redis List on the head or on the tail

SET (добавление, выборка, удаление, проверка, пересечение, объединение, разность и т. Д.)
    Redis Sets are an unordered collection of strings. In Redis, you can add, remove, and test for the existence of members in O(1) time complexity.

HASH (сохранение, удаление, удаление в хэше)
    A Redis hash is a collection of key value pairs. Redis Hashes are maps between string fields and string values. 
    Hence, they are used to represent objects.

ZSET (тот же, что и SET, но упорядоченный)
    Redis Sorted Sets are similar to Redis Sets, non-repeating collections of Strings. 

    The difference is, every member of a Sorted Set is associated with a score, that is used in order to take the sorted set ordered, from the smallest to the greatest score. 
    While members are unique, the scores may be repeated.

GEO (добавление, обновление, удаление широты и долготы, попадание в заданный redius)
    ...


Transactions
************
Redis transactions allow the execution of a group of commands in a single step

All commands in a transaction are sequentially executed as a single isolated operation. 
It is not possible that a request issued by another client is served in the middle of the execution of a Redis transaction.

Redis transaction is also atomic. Atomic means either all of the commands or none are processed.

Redis transaction is initiated by command MULTI and then you need to pass a list of commands that should be executed in the transaction, after which the entire transaction is executed by EXEC command.




Backup
********
Redis SAVE command is used to create a backup of the current Redis database.
This command will create a dump.rdb file in your Redis directory.


Security
********
Redis database can be secured, such that any client making a connection needs to authenticate before executing a command. To secure Redis, you need to set the password in the config file.


Pipelining
**********
Redis is a TCP server and supports request/response protocol.
The client sends a query to the server, and reads from the socket, usually in a blocking way, for the server response.
The server processes the command and sends the response back to the client.

The basic meaning of pipelining is, the client can send multiple requests to the server without waiting for the replies at all, and finally reads the replies in a single step


Partitioning
************
Partitioning is the process of splitting your data into multiple Redis instances, so that every instance will only contain a subset of your keys.

* It allows for much larger databases, using the sum of the memory of many computers. Without partitioning you are limited to the amount of memory that a single computer can support

* It allows to scale the computational power to multiple cores and multiple computers, and the network bandwidth to multiple computers and network adapters.

Disadvantages
=============
Operations involving multiple keys are usually not supported. For instance, you can't perform the intersection between two sets if they are stored in the keys that are mapped to different Redis instances.
Redis transactions involving multiple keys cannot be used.

The partitioning granuliary is the key, so it is not possible to shard a dataset with a single huge key like a very big sorted set.

When partitioning is used, data handling is more complex. 
For instance, you have to handle multiple RDB/AOF files, and to get a backup of your data you need to aggregate the persistence files from multiple instances and hosts

Adding and removing the capacity can be complex. For instance, Redis Cluster supports mostly transparent rebalancing of data with the ability to add and remove nodes at runtime. 
However, other systems like client-side partitioning and proxies don't support this feature. A technique called Presharding helps in this regard


Types of Partitioning
=====================
Range Partitioning
    Range partitioning is accomplished by mapping ranges of objects into specific Redis instances. 
    Suppose in our example, the users from ID 0 to ID 10000 will go into instance R0, while the users from ID 10001 to ID 20000 will go into instance R1 and so forth.

Hash Partitioning
    In this type of partitioning, a hash function (eg. modulus function) is used to convert the key into a number and then the data is stored in different-different Redis instances.


Java API
********

.. code-block:: java

    //Connecting to Redis server on localhost
    Jedis jedis = new Jedis("localhost");

    //check whether server is running or not 
    jedis.ping()

    //set the data in redis string 
    jedis.set("key", "value"); 

    //Get the stored data
    jedis.get("key")




Links
*******

`Redis Tutorial <https://www.tutorialspoint.com/redis/index.htm>`_