.. include:: ../../defs.rst

SQL
##################

.. contents:: 
    :local:



SQL engine
************
.. container:: left-col

    When you are executing an SQL command for any RDBMS, the system determines the best way to carry out your request and SQL engine figures out how to interpret the task.

    There are various components included in this process
        Query Dispatcher

        Optimization Engines

        Classic Query Engine

        SQL Query Engine 

        ...


    .. figure:: ../../_static/sql-architecture.jpg

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Oracle Sql <http://citforum.ru/database/articles/oracle_ansi.shtml>`_

        `SQL Tutorial <https://www.tutorialspoint.com/sql/sql-overview.htm>`_


Database Normalization
**********************
.. container:: left-col

    Database normalization is the process of efficiently organizing data in a database. 

    There are two reasons of this normalization process
        Eliminating redundant data, for example, storing the same data in more than one table

        Ensuring data dependencies make sense


First Normal Form (1NF)
=======================
.. container:: left-col


    * Определение необходимых элементов данных, потому что они становятся столбцами в таблице

    * Помещение связанных элементов данных в таблицу

    * Ensure that there are no repeating groups of data

    * Ensure that there is a primary key


    .. csv-table:: Не нормализованная таблица
       :header-rows: 1

       Сотрудник, Номер телефона
       Иванов И. И. , 283-56-82 |br| 390-57-34 
       Петров П. П. , 708-62-34


    .. csv-table:: Нормализованная таблица
       :header-rows: 1

        Сотрудник, Номер телефона
        Иванов И. И., 283-56-82
        Иванов И. И., 390-57-34
        Петров П. П., 708-62-34

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `First Normal Form (1NF) <https://ru.wikipedia.org/wiki/%D0%9F%D0%B5%D1%80%D0%B2%D0%B0%D1%8F_%D0%BD%D0%BE%D1%80%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D0%B0%D1%8F_%D1%84%D0%BE%D1%80%D0%BC%D0%B0>`_


Second Normal Form (2NF)
========================
.. container:: left-col

    Каждый неключевой атрибут неприводимо зависит от (каждого) её потенциального ключа

    .. csv-table:: Не нормализованная таблица
       :header-rows: 1

       Филиал компании, Должность,Зарплата,Наличие компьютера
       Филиал в Томске,Уборщик,20000,Нет
       Филиал в Москве,Программист,40000,Есть
       Филиал в Томске,Программист,25000,Есть


    .. csv-table:: Нормализованная таблица R1
       :header-rows: 1

       Филиал компании,Должность,Зарплата
       Филиал в Томске,Уборщик,20000
       Филиал в Томске,Программист,25000
       Филиал в Москве,Программист,40000



    .. csv-table:: Нормализованная таблица R2
       :header-rows: 1

       Должность,Наличие компьютера
       Уборщик,Нет
       Программист,Есть

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Second Normal Form (2NF) <https://ru.wikipedia.org/wiki/%D0%92%D1%82%D0%BE%D1%80%D0%B0%D1%8F_%D0%BD%D0%BE%D1%80%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D0%B0%D1%8F_%D1%84%D0%BE%D1%80%D0%BC%D0%B0>`_


Third Normal Form (3NF)
==========================
.. container:: left-col


    Ни один неключевой атрибут R не находится в транзитивной функциональной зависимости от потенциального ключа R.

    .. csv-table:: Не нормализованная таблица
       :header-rows: 1

       Сотрудник,Отдел,Телефон
       Гришин,Бухгалтерия,11-22-33
       Васильев,Бухгалтерия,11-22-33
       Петров,Снабжение,44-55-66


    .. csv-table:: Нормализованная таблица R1
       :header-rows: 1

       Отдел,Телефон
       Бухгалтерия,11-22-33
       Снабжение,44-55-66


    .. csv-table:: Нормализованная таблица R2
       :header-rows: 1

       Сотрудник,Отдел
       Гришин,Бухгалтерия
       Васильев,Бухгалтерия
       Петров,Снабжение

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Third Normal Form (3NF) <https://ru.wikipedia.org/wiki/%D0%A2%D1%80%D0%B5%D1%82%D1%8C%D1%8F_%D0%BD%D0%BE%D1%80%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D0%B0%D1%8F_%D1%84%D0%BE%D1%80%D0%BC%D0%B0>`_


Data types
*********************
.. container:: left-col

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Data types <https://www.tutorialspoint.com/sql/sql-data-types.htm>`__

SQL Commands
*************
.. container:: left-col

    .. container:: code-markup

        DDL - Data Definition Language
            .. container:: code-markup

                ``CREATE``
                    Creates a new table, a view of a table, or other object in the database.

                    .. code-block:: sql

                        CREATE TABLE table_name(
                            column1 datatype,
                            column2 datatype,
                            column3 datatype,
                            .....
                            columnN datatype,
                          PRIMARY KEY( one or more columns )
                        );

                        CREATE TABLE new_table_name AS
                            SELECT column1, column2,...
                            FROM existing_table_name
                            WHERE ....;

                        CREATE TABLE TestTable AS
                            SELECT customername, contactname
                            FROM customers;

                ``ALTER``
                    Modifies an existing database object, such as a table

                    .. code-block:: sql

                        ALTER TABLE table_name <ADD|DROP|MODIFY> column_name data_type;

                ``DROP``
                    Deletes an entire table, a view of a table or other objects in the database

                    .. code-block:: sql

                        DROP TABLE table_name;
    
        DML - Data Manipulation Language
            .. container:: code-markup

                ``SELECT``
                    Retrieves certain records from one or more tables.	

                    `SELECT syntax`_

                ``INSERT``
                    Creates a record.

                    .. code-block:: sql

                        INSERT INTO table_name( column1, column2....columnN)
                        VALUES ( value1, value2....valueN);

                ``UPDATE``
                    Modifies records.

                    .. code-block:: sql

                        UPDATE table_name
                        SET column1 = value1, column2 = value2, ... columnN=valueN
                        [WHERE  CONDITION];

                ``DELETE``
                    Deletes records.

                    .. code-block:: sql

                        DELETE FROM table_name
                        WHERE  condition;

        DCL - Data Control Language
            .. container:: code-markup

                ``GRANT``
                    Gives a privilege to user.

                ``REVOKE``
                    Takes back privileges granted from user

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `SQL syntax <https://www.tutorialspoint.com/sql/sql-syntax.htm>`_

SELECT syntax
==============
.. container:: left-col

    .. code-block:: java

        SELECT
          [DISTINCT | DISTINCTROW | ALL]
          select_expression [AS column_alias],...

        FROM table_reference [AS table_alias_name] [, ...] // Декартово произведение всех переданных таблиц
          [JOIN other_table ON (table_reference.column = other_table.column)]

        [WHERE 
            CONDITION1 
          <AND|OR>
            CONDITION2
          <AND|OR>
            column_name IN (<val-1, val-2,...val-N | SELECT ...>)
          <AND|OR>
            column_name BETWEEN val-1 AND val-2
          <AND|OR>
            ...
        ]

        // Группировка строк по результатам агрегатных функций (MAX, SUM, AVG, …)
        // Необходимо, чтобы в SELECT были заданы только требуемые в выходном потоке столбцы, 
        //   перечисленные в GROUP BY и/или агрегированные значения
        [GROUP BY <unsigned_integer | col_name | formula>]  

        // Указания условия на результат агрегатных функций (MAX, SUM, AVG, …)
        //   в группах, выбранных через GROUP BY
        // HAVING <условия> аналогичен WHERE <условия> за исключением того, что строки отбираются не по значениям столбцов, 
        //   а строятся из значений столбцов, указанных в GROUP BY
        [HAVING where_definition]

        [ORDER BY <unsigned_integer | col_name | formula> [ASC | DESC], ...]

        [UNION | UNION ALL]
        SELECT ...


Joins
--------------
.. container:: left-col

    .. container:: code-markup

        ``INNER JOIN`` 
            Returns rows when there is a match in both tables.

        ``LEFT JOIN`` 
            Returns all rows from the left table, even if there are no matches in the right table.

        ``RIGHT JOIN`` 
            Returns all rows from the right table, even if there are no matches in the left table.

        ``FULL JOIN`` 
            Returns rows when there is a match in one of the tables.

        ``SELF JOIN`` 
            Is used to join a table to itself as if the table were two tables, temporarily renaming at least one table in the SQL statement.

        ``CARTESIAN JOIN`` 
            Returns the Cartesian product of the sets of records from the two or more joined tables.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        https://shra.ru/2017/09/sql-join-v-primerakh-s-opisaniem/