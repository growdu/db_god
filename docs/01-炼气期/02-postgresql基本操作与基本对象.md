# postgresql基本操作与基本对象

- postgresql是一个C/S架构的大型软件，提供数据存储索引和查询的功能。postgresql分为客户端和服务端，客户端叫作psql，或者其他语言基于odbc，jdbc，libpq开发的客户端程序，服务端叫作postgres，需要监听ip和端口，通过tcp网络协议与客户端进行通信。

- 在网络通信之上，采用sql作为数据通信协议，支持增删改查。sql针对的是结构化数据，因而要先有数据结构，这里其实就是表。为了对表进行隔离，由将表划分到不同的数据库里。

- 客户端和服务端通信需要验证，验证采用用户名和密码的方式。
- 因而客户端想要访问数据需要服务器ip、服务器端口、用户名、密码、数据库名称，而要取出一条数据，则还需要表名称、sql语句

## postgresql外部对象

对于客户端来说，通过库-->表-->sql的方式操作数据。

- 数据库

  一个postgres可以有多个数据库，数据库是表的集合，数据库名称是客户端访问的标识之一。

- 表

  结构相同的数据放在一起就是一张表，表有多行多列。（一般表的列数在创建表时确定，行数则会不断增加或减少。）表的数据最终会写入到文件里。

- 视图

  视图是表的一部分在内存中的缓存，并不会写入文件。

数据最终以表的形式写入到文件里，每一个数据库有一个目录，每一张表是一个文件或多个文件（表的数据小于1gb时是一个文件，大于1gb会按照1gb文件拆分）。

而对于服务端来说则根据oid-->文件的方式来操作数据。

- oid

  每个数据库都有自己的oid，不同数据库的oid不同。

  每张表都有自己的oid，相同数据库的表的oid不同。

- 文件

  每个数据库以oid为名创建目录，每张表在数据库oid目录下以oid创建文件。

### 基操

- 如何定位student表存储在什么位置

  先用psql连接到数据库。

  ```sql
  test=#
  test=# SELECT pg_relation_filepath('student');
   pg_relation_filepath
  ----------------------
   base/16384/16387
  (1 row)
   
  test=#
  ```

- 如何查找数据库对应的oid

  ```shell
  --注意大小写敏感，要用小写
  test=# select oid,datname from pg_database where datname='test';
    oid  | datname
  -------+---------
   16384 | test
  (1 row)
  ```

- 如何查找数据库有多少张表

  ```sql
  test=# \d
  test=# \d+
  ```

  