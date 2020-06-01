.. include:: ../../defs.rst

JDBC
****
JDBC (англ. Java DataBase Connectivity — соединение с базами данных на Java) — платформенно независимый промышленный стандарт взаимодействия Java-приложений с различными СУБД, 
реализованный в виде пакета java.sql, входящего в состав Java SE.

.. code-block::

    Class.forName(driverClass);
    Connection connection = DriverManager
            .getConnection(url, user, password) ;

    // driverClass - строка с полным именем класса JDBC драйвера
    // Class.forName загружает класс и этим гарантирует выполнение статического блока инициализации, а значит и регистрацию драйвера в DriverManager


    Statement statement = connection.createStatement();
    statement.execute("create table user(" +
            "id integer primary key auto_increment, " +
            "name varchar(100));");
    ResultSet rs = statement.executeQuery("select * from user");

    // Statement можно использовать для выполнения любых запросов, будь то DDL, DML, либо обычные запросы на выборку данных.
    // ResultSet - это результат выполнения запроса

    // ...
    statement.close();
    connection.close();

.. code-block::

    PreparedStatement statement = connection
        .prepareStatement("insert into user(id,name) values(?,?)");

    // PreparedStatement представляет собой скомпилированную версию SQL-выражения, выполнение которого будет быстрее и эффективнее

.. code-block::

    connection.setAutoCommit(false);
    st.execute("insert into user(name) values('kesha')");
    connection.commit(); / connection.rollback();

    // По умолчанию каждое SQL-выражение автоматически коммитится при выполнении statement.execute и подобных методов

Links
=====

https://habr.com/ru/post/326614/

https://proselyte.net/tutorials/jdbc/simple-application-example/