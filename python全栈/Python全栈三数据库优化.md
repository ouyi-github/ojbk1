# Python全栈（三）数据库优化之1.数据库简介

## 一.数据库的介绍

### 1.传统数据记录的缺点：

- 不易保存；
- 备份困难；
- 查找不便。

### 2.现代化手段–文件：

对于数据容量较大的数据，不能够很好的满足，而且性能较差；
不易扩展。

### 3.数据库：

- 持久化存储（数据库是一种特殊的文件）；
- 读写速度**极高**；
- 保证数据的**有效性**；
- 对程序支持性非常好，容易扩展。
- 我们看到的数据库的呈现方式是这样的，
  ![数据库](https://img-blog.csdnimg.cn/20191125215614417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
  在网页上展现出来可能是这样的，
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191125215631150.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 二.数据库

### 1.相关名词

- 数据行（记录）
- 数据列（字段）
- 数据表（数据行的集合）
- 数据库（数据表的集合）
- 主键（能够**唯一**标识某个记录的字段）
  ![ju](https://img-blog.csdnimg.cn/20191125215722909.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.MySQL简介：

一个关系型数据库管理系统，目前属于Oracle旗下产品。
特点：

- 使用C和C++编写，并使用了多种编译器进行测试，保证源代码的可移植性；
- 支持多种操作系统，如Linux、Windows、AIX、FreeBSD、HP-UX、MacOS、NovellNetware、OpenBSD、OS/2 Wrap、Solaris等；
- 为多种编程语言提供了API，如C、C++、Python、Java、Perl、PHP、Eiffel、Ruby等；
- 支持多线程，充分利用CPU资源；
- 优化的SQL查询算法，有效地提高查询速度；
- 提供多语言支持，常见的编码如GB2312、BIG5、UTF8；
- 提供TCP/IP、ODBC和JDBC等多种数据库连接途径；
- 提供用于管理、检查、优化数据库操作的管理工具；
- 大型的数据库。可以处理拥有上千万条记录的大型数据库；
- 支持多种存储引擎；
- MySQL 软件采用了双授权政策，它分为社区版和商业版，由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发都选择MySQL作为网站数据库；
- MySQL使用标准的SQL数据语言形式；
- Mysql是可以定制的，采用了GPL协议，你可以修改源码来开发自己的Mysql系统；
- 在线DDL更改功能；
- 复制全局事务标识；
- 复制无崩溃从机；
- 复制多线程从机。

## 三、MySQL安装和配置

### 1.直接安装

下载地址：https://www.mysql.com/downloads
安装可参考：https://jingyan.baidu.com/article/0aa223751ed91188cc0d643f.html

### 2.集成安装

- phpstudy
- xampp

phpstudy：集成安装，可用于MySQL服务的**开启和停止**。
安装建议：
直接安装步骤繁琐，建议使用集成安装方式，使用也更方便。

### 3.图形化管理工具

- phpMyAdmin
- Navicat
- SQLyog

图形化管理工具：MySQL服务端开启服务后，是以命令行的方式操作数据库的，不方便，图形化工具可以更方便地操作数据库。
SQLyog的安装过程可参考：https://blog.csdn.net/lihua5419/article/details/73881837
使用建议：
图形化管理工具是为了辅助操作的，如果在现实工作中用图形化工具会导致**安全**问题，所以实际学习和工作中用的更多的是通过**语句**来操作。

# Python全栈（三）数据库优化之2.数据库的操作与数据表的操

## 一、SQL介绍&常见的数据类型

### 1.SQL定义

SQL是结构化查询语言，是一种用来操作**RDBMS**(关系型数据库管理系统)的数据库语言。
当前关系型数据库都支持使用SQL语言进行操作，也就是说可以通过SQL操作oracle，sql server,mysql等关系型数据库。

SQL语句分为：
DQL数据查询语言：
用于对数据进行查询，如select；
DML数据操作语言：
对数据进行增加、修改、删除，如insert、udpate、delete；
DDL数据定义语言：
进行数据库、表的管理等，如create、drop。
重点是数据的**CURD**（增删改查）。

### 2.数据的完整性：

在表中为了更加准确的存储数据，保证数据的正确有效，可以在创建表的时候，为表添加一些强制性的验证，包括数据字段的类型、约束。

#### 常见的数据类型：

整型int；
小数decimal:
decimal表示浮点数，如decimal(5,2)表示共存5位数，小数占2位.
字符串varchar、char：
varchar是可变长度的字符串，如varchar(3)，填充’ab’时会存储’ab’；
char是固定长度字符串，如char(3)，填充’ab’时会存储’ab '（ab后有一个空格）；
字符串text用于存储大文本，当字符数大于4000时推荐使用。
※对于图片、音频、文本等二进制文件，不存储在数据库中，而是上传到某个服务器上，然后在表中存储这个文件的保存地址。
日期时间date、time、datetime。
枚举类型enum：
把所有可能列举出来，如性别只有2种男、女，星期只有周一到周日7种。

#### 数值类型：

![数值类型](https://img-blog.csdnimg.cn/20191130001759126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### 字符串：

![字符串](https://img-blog.csdnimg.cn/20191130001859977.png)

#### 日期时间类型：

![日期时间](https://img-blog.csdnimg.cn/2019113000191575.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 二、数据库约束&数据库简单操作

### 1.约束

主键primary key：物理上存储的顺序，取值唯一,可以设置自增，并且只能是主键设置自增，其他字段不能设置自增，且主键不能设置默认值；
非空not null：此字段不允许填写空值；
无符号unsigned：没有负值；
自增auto_increment：主键自动增加；
唯一unique：此字段的值不允许重复；
默认default：当不填写此值时会使用默认值，如果填写时以填写为准；
外键foreign key：对关系字段进行约束，当为关系字段填写值时，会到关联的表中查询此值是否存在，如果存在则填写成功，如果不存在则填写失败并抛出异常。

### 2.数据库操作

操作都是基于命令行的,SQL语句后需要用**分号结尾**。
数据库的连接：

```mysql-sql
--连接数据库
mysql -u root -proot --不安全,密码直接显示，不推荐
12
```

结果：

```mysql-sql
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 94
Server version: 5.7.26 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
1234567891011121314
```

或者

```mysql-sql
mysql -uroot -p
1
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 97
Server version: 5.7.26 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
1234567891011121314
```

此时会提示输入密码，密码默认为root。
退出数据库

```mysql-sql
--退出数据库
exit --或者quit
12
```

打印

```mysql-sql
mysql> exit
Bye
12
```

查看所有数据库

```mysql-sql
--查看所有数据库
show databases;
12
```

打印

```mysql-sql
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.01 sec)
12345678910
```

显示数据库版本

```mysql-sql
--显示数据库版本
select version();
12
```

打印

```mysql-sql
+-----------+          
| version() |          
+-----------+          
| 5.7.26    |          
+-----------+          
1 row in set (0.00 sec)
                       
mysql>                 
12345678
```

显示时间

```mysql-sql
--查询时间
select now();
12
```

打印

```mysql-sql
+---------------------+
| now()               |
+---------------------+
| 2019-11-29 16:47:56 |
+---------------------+
1 row in set (0.00 sec)
123456
```

创建数据库

```mysql-sql
--创建数据库
create database pythontest;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
```

创建数据库时设置编码方式

```mysql-sql
--创建数据库（设置编码方式）
create database pythontest charset=utf8;
12
```

查看创建数据库的命令

```mysql-sql
--查看创建数据库的语句
show create database pythontest;
12
```

打印

```mysql-sql
+------------+---------------------------------------------------------------------------------------------+
| Database   | Create Database                                                                             |
+------------+---------------------------------------------------------------------------------------------+
| pythontest | CREATE DATABASE `pythontest` /*!40100 DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci */ |
+------------+---------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)                                                                                     
123456
```

使用数据库

```mysql-sql
--使用数据库
use test;
12
```

打印

```mysql-sql
Database changed
1
```

查看当前使用的数据库

```mysql-sql
--查看当前使用的数据库
select database();
12
```

打印

```mysql-sql
+------------+         
| database() |         
+------------+         
| test       |         
+------------+         
1 row in set (0.00 sec)
123456
```

删除数据库
这一语句需要慎用，

```mysql-sql
--删除数据库
drop database test;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)
1
```

## 三、数据表的操作和数据插入

### 1.数据表的操作

查看当前数据库中所有表

```mysql-sql
--查看当前数据库中所有的表
show tables;
12
```

打印

```mysql-sql
+----------------+     
| Tables_in_demo |     
+----------------+     
| test           |     
+----------------+     
1 row in set (0.00 sec)
123456
```

创建表

```mysql-sql
--create table tablename(字段 约束[,字段 约束 类型]);
create table demo1(
    id int,
    name varchar(30)
);
12345
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

查看表结构

```mysql-sql
--desc 数据表的名字；
desc demo1;
12
```

打印

```mysql-sql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(30) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
1234567
--创建students表（id、name、age、high、gender、cls_id）
create table students(
    id int not null primary key auto_increment,
    name varchar(30),
    age tinyint unsigned default 18,
    height decimal(5,2),
    gender enum('male','female','secret') default 'secret',     --存数据时，只能存male、female、secret三个值，不能存其他的值。
    cls_id int
);
123456789
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
1
```

查看表的创建语句

```mysql-sql
-- show create table 表名字;
show create table students;
12
```

打印

```mysql-sql
+----------+-------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------+                                          
| Table    | Create Table                                                                                          
                                                                                                                   
                                                                                                                   
                                                                        |                                          
+----------+-------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------+                                          
| students | CREATE TABLE `students` (                                                                             
  `id` int(11) NOT NULL AUTO_INCREMENT,                                                                            
  `NAME` varchar(30) COLLATE utf8_unicode_ci DEFAULT NULL,                                                         
  `age` tinyint(3) unsigned DEFAULT '18',                                                                          
  `height` decimal(5,2) DEFAULT NULL,                                                                              
  `gender` enum('male','female','secret') COLLATE utf8_unicode_ci DEFAULT 'secret',                                
  `cls_id` int(11) DEFAULT NULL,                                                                                   
  PRIMARY KEY (`id`)                                                                                               
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci |                                                     
+----------+-------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------+                                          
1 row in set (0.00 sec)                                                                                            
1234567891011121314151617181920212223242526
```

创建classes表（id、name）：

```mysql-sql
create table classes(
    id int primary key not null auto_increment,
    name varchar(30)
);
1234
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

修改表-添加字段

```mysql-sql
-- alter table 表名 add 列名 类型;
alter table students add birthday date;
12
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

修改表-修改字段（不重命名）

```mysql-sql
-- alter table 表名 modify 列名 类型及约束;
alter table students modify birthday date default '1990-1-1';
12
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

修改表-修改字段（重命名）

```mysql-sql
-- alter table 表名 change 原名 新名 类型及约束
alter table students change birthday birth date default '1990-1-1';
12
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

修改表-删除字段

```mysql-sql
-- alter table 表名 drop 列名;
alter table students drop height;
12
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

删除表或者数据库

```mysql-sql
drop table 表名;
drop database 数据库;
12
```

### 2.数据插入

#### 全列插入

```mysql-sql
--insert [into] 表名 values(...);
--向classes中插入一个班级
insert into classes values(1,'class1');
select * from classes;
1234
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)

+----+--------+
| id | name   |
+----+--------+
|  1 | class1 |
+----+--------+
1 row in set (0.00 sec)
12345678
```

主键字段是自增的，用0、null、default来占位。

```mysql-sql
--向students表插入一个学生信息
insert into students values(0,'Tom',18,1,1,'1999-9-9');
insert into students values(null,'Jerry',19,1,1,'1999-10-9');
insert into students values(default,'Nancy',17,1,2,'1999-8-9');
select * from students;
12345
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)                     
                                                        
Query OK, 1 row affected (0.00 sec)                     
                                                        
Query OK, 1 row affected (0.00 sec)                     
                                                        
+----+-------+------+--------+--------+------------+    
| id | NAME  | age  | gender | cls_id | birth      |    
+----+-------+------+--------+--------+------------+    
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |    
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |    
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |    
+----+-------+------+--------+--------+------------+    
3 rows in set (0.00 sec)                                
1234567891011121314
```

枚举类型插入,下标是从1开始。

```mysql-sql
insert into students values(default,'John',19,1,1,'1999-7-9');
select * from students;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)                  
                                                     
+----+-------+------+--------+--------+------------+ 
| id | NAME  | age  | gender | cls_id | birth      | 
+----+-------+------+--------+--------+------------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 | 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 | 
|  4 | John  |   19 | male   |      1 | 1999-07-09 | 
+----+-------+------+--------+--------+------------+ 
4 rows in set (0.00 sec)                             
1234567891011
```

#### 部分插入

非空字段必须要指定值，否则要指定默认值。

```mysql-sql
--insert into 表名(列1,...) values(值1,...)
insert into students(id,name,gender) values(default,'Tony',1);
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
```

#### 多行插入

```mysql-sql
insert into students values
(default,'John',19,1,2,'1999-7-9'),
(default,'Rose',19,2,2,'1999-7-9')
;
1234
```

打印

```mysql-sql
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0
```

# Python全栈（三）数据库优化之3.数据的修改和删除及数据的条件查询

## 一、修改&删除&简单查询

### 1.修改

update 表名 set 列1=值1,列2=值2,… where 条件;

```mysql-sql
--不加where，全部修改
update students set name = 'Jack';
12
--加where，修改符合条件的记录
update students set name = 'Jack' where name = 'John';
12
update students set name = 'John',gender = 'secret' where name = 'Jack';
1
```

### 2.删除

#### 物理删除

delete from 表名 where 条件;

```mysql-sql
--删除的是表中的全部数据
delete from demo1;
12
```

打印

```mysql-sql
Query OK, 2 rows affected (0.00 sec)
1
--删除的是表中符合条件的数据
delete from students where id = 5;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)
1
```

删除的是表中的数据，数据被真实地删除，删除操作后被删除的数据已不存在。

#### 逻辑删除

`is_delete`字段表示是否删除，相当于对原有数据增加一个标识，并未真正删除数据。
这样能保存原有的数据，因为在互联网时代数据有巨大的价值，不应该被任意删除。
查找：

```mysql-sql
select * from students where is_delete = 0;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | 
+----+-------+------+--------+--------+------------+-----------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 | 0         | 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 | 0         | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 | 0         | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 | 0         | 
|  6 | John  |   16 | secret |      1 | 1999-05-09 | 0         | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 | 0         | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
|  9 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 10 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 11 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 13 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 18 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 20 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 | 0         | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 | 0         | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 | 0         | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 | 0         | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
+----+-------+------+--------+--------+------------+-----------+ 
26 rows in set (0.00 sec)                                        
12345678910111213141516171819202122232425262728293031
```

此时删除即修改is_delete字段：

```mysql-sql
update students set is_delete = 1 where id = 7;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0
12
```

### 3.MySQL简单查询

（1）查询所有列：

```mysql-sql
select * from students;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
26 rows in set (0.01 sec)
12345678910111213141516171819202122232425262728293031
```

（2）去除重复字段的查询：
消除重复行：distinct字段

```mysql-sql
select distinct name from students;
1
```

打印

```mysql-sql
+-------+               
| name  |               
+-------+               
| Tom   |               
| Jerry |               
| Nancy |               
| John  |               
| Rose  |               
| Tony  |               
+-------+               
6 rows in set (0.00 sec)
1234567891011
```

distinct关注的是整个行是否重复。

```mysql-sql
select distinct name,age from students;
1
```

打印

```mysql-sql
+-------+------+        
| name  | age  |        
+-------+------+        
| Tom   |   18 |        
| Jerry |   19 |        
| Nancy |   17 |        
| John  |   19 |        
| John  |   16 |        
| Rose  |   19 |        
| Tony  |   18 |        
+-------+------+        
7 rows in set (0.00 sec)
123456789101112
```

（3）查询指定列：
select 列1,列2,… from 表名;

```mysql-sql
select name,age,gender from students;
1
```

打印

```mysql-sql
+-------+------+--------+ 
| name  | age  | gender | 
+-------+------+--------+ 
| Tom   |   18 | male   | 
| Jerry |   19 | male   | 
| Nancy |   17 | male   | 
| John  |   19 | secret | 
| John  |   16 | secret | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| Tony  |   18 | male   | 
| John  |   19 | secret | 
| John  |   16 | secret | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
+-------+------+--------+ 
26 rows in set (0.01 sec) 
12345678910111213141516171819202122232425262728293031
```

（4）可以为表、字段等重命名，

```mysql-sql
select name as n,gender from students as stu;
1
```

打印

```mysql-sql
+-------+--------+       
| n     | gender |       
+-------+--------+       
| Tom   | male   |       
| Jerry | male   |       
| Nancy | male   |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| John  | secret |       
| Tony  | male   |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| Rose  | female |       
+-------+--------+       
26 rows in set (0.00 sec)
12345678910111213141516171819202122232425262728293031
```

通常在对**多个表**进行联合查询时，会对表重命名。
as关键字表示在查询的结果中显示时，替换掉原来的名称，而不是将字段替换，只是显示时发生了变化，不能被引用，否则会报错。

```mysql-sql
select name as n,gender from students where n = 'Tom';
1
```

打印

```mysql-sql
ERROR 1054 (42S22): Unknown column 'n' in 'where clause'
1
```

即报错，字段别名不能在where条件中使用。

## 二、条件查询-比较&逻辑运算符

### 1.比较运算符：

（1）**>** 大于

```mysql-sql
select * from students where age > 18;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+  
| id | NAME  | age  | gender | cls_id | birth      | is_delete |  
+----+-------+------+--------+--------+------------+-----------+  
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |  
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
+----+-------+------+--------+--------+------------+-----------+  
21 rows in set (0.01 sec)                                         
1234567891011121314151617181920212223242526
```

（2）**<** 小于

```mysql-sql
select * from students where age < 18;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
3 rows in set (0.01 sec)                                        
12345678
```

（3）**>=** 大于等于

```mysql-sql
select * from students where age >= 18;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | 
+----+-------+------+--------+--------+------------+-----------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
+----+-------+------+--------+--------+------------+-----------+ 
23 rows in set (0.00 sec)                                        
12345678910111213141516171819202122232425262728
```

（4）**<=** 小于等于

```mysql-sql
select * from students where age <= 18;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
5 rows in set (0.00 sec)                                        
12345678910
```

（5）**=** 等于

```mysql-sql
select * from students where age = 18;
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 |
+----+------+------+--------+--------+------------+-----------+
2 rows in set (0.00 sec)
1234567
```

（6）**!=\**或\**<>** 不等于

```mysql-sql
select * from students where name != 'John';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
10 rows in set (0.00 sec)                                       
123456789101112131415
select * from students where name <> 'John';
1
```

结果与前面相同。

### 2.逻辑运算符

（1）**and**和

```mysql-sql
select * from students where age > 18 and age < 28;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
21 rows in set (0.00 sec)                                       
1234567891011121314151617181920212223242526
select * from students where age > 18 and gender = 'female';
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
| 11 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 13 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 15 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 18 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 20 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 27 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
6 rows in set (0.00 sec)                                       
1234567891011
```

（2）**or**或

```mysql-sql
select * from students where id < 4 or is_delete = 0;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
3 rows in set (0.00 sec)                                        
12345678
```

（3）**not**取反

```mysql-sql
select * from students where not (age = 18 and gender = 'female');
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
26 rows in set (0.00 sec)
12345678910111213141516171819202122232425262728293031
```

又如

```mysql-sql
select * from students where (not age <= 18) and gender = 2;
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
| 11 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 13 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 15 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 18 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 20 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 27 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
6 rows in set (0.00 sec)
1234567891011
```

等价于

```mysql-sql
select * from students where age > 18 and gender = 2;
1
```

结果与前者相同。
用**括号**解决优先级问题，会使代码可读性更高。

## 三、条件查询-模糊&范围查询&空判断

### 1.模糊查询：

（1）**like**关键字
**%**：替换0个或多个，即任意多个字符
**_**：替换1个字符

```mysql-sql
select * from students where name like 'J%';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | 
+----+-------+------+--------+--------+------------+-----------+ 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
+----+-------+------+--------+--------+------------+-----------+ 
17 rows in set (0.00 sec)                                        
12345678910111213141516171819202122
select * from students where name like '%o%';
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
|  4 | John |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John |   19 | secret |      1 | 1999-07-09 |         1 |
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 |
| 24 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
24 rows in set (0.00 sec)                                      
1234567891011121314151617181920212223242526272829
```

查询字段值长度为3的记录：

```mysql-sql
select * from students where name like '___';
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
1 row in set (0.00 sec)
123456
```

查询长度至少为4的数据值：

```mysql-sql
select * from students where name like '____%';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
25 rows in set (0.00 sec)
123456789101112131415161718192021222324252627282930
```

（2）**rlike**匹配正则表达式：

```mysql-sql
select * from students where name rlike '^J.*';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
17 rows in set (0.00 sec)
12345678910111213141516171819202122
select * from students where name rlike '^J.*y$';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
1 row in set (0.00 sec)
123456
```

### 2.范围查询：

（1）**in**表示在一个非连续的范围内

```mysql-sql
select * from students where age in (18,34);
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 |
+----+------+------+--------+--------+------------+-----------+
2 rows in set (0.00 sec)
1234567
select * from students where name in ('Tom','John');
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
|  4 | John |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 12 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 14 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 16 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 19 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 21 | John |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John |   19 | secret |      1 | 1999-07-09 |         1 |
| 24 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John |   19 | secret |      2 | 1999-07-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
17 rows in set (0.00 sec)                                      
12345678910111213141516171819202122
```

（2）**not in**表示不在一个非连续的范围内

```mysql-sql
select * from students where age not in (18,34);
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+  
| id | NAME  | age  | gender | cls_id | birth      | is_delete |  
+----+-------+------+--------+--------+------------+-----------+  
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |  
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |  
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |  
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |  
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
+----+-------+------+--------+--------+------------+-----------+  
24 rows in set (0.00 sec)                                         
1234567891011121314151617181920212223242526272829
```

（3）**between … and …** 表示在一个连续的范围内

```mysql-sql
select * from students where id between 3 and 8;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
5 rows in set (0.00 sec)
12345678910
select * from students where (id between 3 and 8) and gender = 1;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
1 row in set (0.00 sec)
123456
```

（4）**not between … and …** 表示不在一个连续的范围内

```mysql-sql
select * from students where age not between 18 and 34;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
3 rows in set (0.00 sec)                                        
12345678
```

等价于

```mysql-sql
select * from students where not age between 18 and 34;
1
```

### 3.空判断:

判空**is null**，不能用“=”。

```mysql-sql
select * from students where name is null;
1
```

打印

```mysql-sql
Empty set (0.00 sec)
1
select * from students where name is not null;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | 
+----+-------+------+--------+--------+------------+-----------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
+----+-------+------+--------+--------+------------+-----------+ 
26 rows in set (0.00 sec)                                        
12345678910111213141516171819202122232425262728293031
```

## 三、聚合函数

### 1.count()总数

```mysql-sql
select count(*) from students;
1
```

打印

```mysql-sql
+----------+
| count(*) |
+----------+
|       26 |
+----------+
1 row in set (0.00 sec)
123456
select count(*) as numofMale from students where gender = 1;
1
```

打印

```mysql-sql
+-----------+          
| numofMale |          
+-----------+          
|         4 |          
+-----------+          
1 row in set (0.00 sec)
123456
select count(*) as numofFemale from students where gender = 2;
1
```

打印

```mysql-sql
+-------------+
| numofFemale |
+-------------+
|           6 |
+-------------+
1 row in set (0.00 sec)
123456
```

### 2.max()最大值

```mysql-sql
select max(age) from students;
1
```

打印

```mysql-sql
+----------+
| max(age) |
+----------+
|       19 |
+----------+
1 row in set (0.00 sec)
123456
select max(id) from students where gender = 2;
1
```

打印

```mysql-sql
+---------+
| max(id) |
+---------+
|      27 |
+---------+
1 row in set (0.00 sec)
123456
```

### 3.min()最小值

```mysql-sql
select min(age) from students where is_delete = 1;
1
```

打印

```mysql-sql
+----------+
| min(age) |
+----------+
|       16 |
+----------+
1 row in set (0.00 sec)
123456
```

### 4.sum()求和

```mysql-sql
select sum(age) from students where gender = 1;
1
```

打印

```mysql-sql
+----------+           
| sum(age) |           
+----------+           
|       72 |           
+----------+           
1 row in set (0.00 sec)
123456
```

对于非数值型字段，如果全部数据均为字符型，求和为0，如果有部分为数字，会将所有数值进行相加。

```mysql-sql
select sum(name) from students;
1
```

打印

```mysql-sql
+-----------+
| sum(name) |
+-----------+
|         0 |
+-----------+
1 row in set, 26 warnings (0.01 sec)
123456
```

### 5.avg()平均值

默认保留4位小数，可以用`round(num,n)`可以使num保留n位小数。

```mysql-sql
select avg(age) from students where gender = 2 and is_delete = 1;
1
```

打印

```mysql-sql
+----------+           
| avg(age) |           
+----------+           
|  19.0000 |           
+----------+           
1 row in set (0.00 sec)
123456
select round(avg(age),2) from students where gender = 2 and is_delete = 1;
1
```

打印

```mysql-sql
+-------------------+  
| round(avg(age),2) |  
+-------------------+  
|             19.00 |  
+-------------------+  
1 row in set (0.00 sec)
```

# Python全栈（三）数据库优化之4.数据库查询（分组、排序、分页、连接查询、子查询）

## 一、分组与分组后的筛选

### 1.分组

关键字：group by
将查询结果按照1个或多个字段进行分组，字段值相同的为一组；
可用于单个字段分组，也可用于多个字段分组
语法：`select ... from 表 group by 组别;`

```mysql-sql
select name from students group by gender;
1
```

打印

```mysql-sql
ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'demo.students.NAME' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
1
```

报错，并且没有实际意义，即select后接的是能区分这个组的字段，一般是聚合函数。

```mysql-sql
select gender from students group by gender;
1
```

打印

```mysql-sql
+--------+              
| gender |              
+--------+              
| male   |              
| female |              
| secret |              
+--------+              
3 rows in set (0.01 sec)
12345678
```

统计每个组的数量

```mysql-sql
select gender,count(*) from students group by gender;
1
```

打印

```mysql-sql
+--------+----------+   
| gender | count(*) |   
+--------+----------+   
| male   |        4 |   
| female |        6 |   
| secret |       17 |   
+--------+----------+   
3 rows in set (0.00 sec)
12345678
```

换别名

```mysql-sql
select gender as sex,count(*) from students group by gender;
1
```

打印

```mysql-sql
+--------+----------+
| sex    | count(*) |
+--------+----------+
| male   |        4 |
| female |        6 |
| secret |       17 |
+--------+----------+
3 rows in set (0.00 sec)
12345678
```

查找各个组的最值

```mysql-sql
select gender as sex,max(age) from students group by gender;
1
```

打印

```mysql-sql
+--------+----------+   
| sex    | max(age) |   
+--------+----------+   
| male   |       19 |   
| female |       19 |   
| secret |       19 |   
+--------+----------+   
3 rows in set (0.00 sec)
12345678
```

查看组内信息：
`group_concat(...)`字段，可以作为一个输出字段来使用；
表示分组之后，根据分组结果，使用group_concat()来放置每一组的某字段的值的集合，相对于对字符串进行拼接。

```mysql-sql
select gender as sex,group_concat(name) from students group by gender;
1
```

打印

```mysql-sql
+--------+--------------------------------------------------------------------------------------+
| sex    | group_concat(name)                                                                   |
+--------+--------------------------------------------------------------------------------------+
| male   | Tom,Jerry,Nancy,Tony                                                                 |
| female | Rose,Rose,Rose,Rose,Rose,Rose                                                        |
| secret | John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,null |
+--------+--------------------------------------------------------------------------------------+
3 rows in set (0.00 sec)
12345678
```

查看多个字段

```mysql-sql
select gender as sex,group_concat(name,'-',age) from students group by gender;
1
```

打印

```mysql-sql
+--------+---------------------------------------------------------------------------------------------------------
--------------------------------+                                                                                  
| sex    | group_concat(name,'-',age)                                                                              
                                |                                                                                  
+--------+---------------------------------------------------------------------------------------------------------
--------------------------------+                                                                                  
| male   | Tom-18,Jerry-19,Nancy-17,Tony-18                                                                        
                                |                                                                                  
| female | Rose-19,Rose-19,Rose-19,Rose-19,Rose-19,Rose-19                                                         
                                |                                                                                  
| secret | John-19,John-16,John-19,John-19,John-19,John-19,John-19,John-19,John-19,John-19,John-19,John-19,John-19,
John-19,John-16,John-19,null-18 |                                                                                  
+--------+---------------------------------------------------------------------------------------------------------
--------------------------------+                                                                                  
3 rows in set (0.00 sec)                                                                                           
123456789101112131415
```

### 2.分组之后的筛选

分组之后根据条件进行筛选不能再用where语句，要用having语句；
having 条件表达式用来分组查询后指定一些条件来输出查询结果。

```mysql-sql
--查询性别分组中人数大于5的组别
select gender,count(*) from students group by gender having count(*) > 5;
12
```

打印

```mysql-sql
+--------+----------+   
| gender | count(*) |   
+--------+----------+   
| female |        6 |   
| secret |       17 |   
+--------+----------+   
2 rows in set (0.00 sec)
1234567
--查询男生女生人数大于2的姓名
select gender,count(*),group_concat(name) from students group by gender having count(*) > 2;
12
```

打印

```mysql-sql
+--------+----------+--------------------------------------------------------------------------------------+
| gender | count(*) | group_concat(name)                                                                   |
+--------+----------+--------------------------------------------------------------------------------------+
| male   |        4 | Tom,Jerry,Nancy,Tony                                                                 |
| female |        6 | Rose,Rose,Rose,Rose,Rose,Rose                                                        |
| secret |       17 | John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,null |
+--------+----------+--------------------------------------------------------------------------------------+
3 rows in set (0.01 sec)
12345678
--查询平均年龄超过18岁的姓名，以及姓名
select gender,avg(age),group_concat(name) from students group by gender having avg(age) > 18;
12
```

打印

```mysql-sql
+--------+----------+--------------------------------------------------------------------------------------+
| gender | avg(age) | group_concat(name)                                                                   |
+--------+----------+--------------------------------------------------------------------------------------+
| female |  19.0000 | Rose,Rose,Rose,Rose,Rose,Rose                                                        |
| secret |  18.5882 | John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,null |
+--------+----------+--------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)
1234567
```

having作用和where一样，但having只能用于group by。

## 二、排序

为了方便查看数据，可以对数据进行排序。
关键字：order by
语法：`select * from 表名 order by 列1 asc|desc [,列2 asc|desc,...];`
将行数据按照列1进行排序，如果某些行列1的值相同时，则按照列2排序，以此类推；
asc从小到大排列，即升序，是默认值；
desc从大到小排列，即降序。

```mysql-sql
--查询年龄在18-26岁之间的男同学，按照年龄从小到大排序
select * from students where (age between 18 and 26) and gender =1 order by age;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
3 rows in set (0.01 sec)
12345678
```

与

```mysql-sql
select * from students where (age between 18 and 26) and gender =1 order by age asc;
1
```

效果一样。

```mysql-sql
--查询年龄在18-26岁之间的女同学，按照身高从低到高排序
select * from students where (age between 18 and 26) and gender =1 order by height;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
3 rows in set (0.00 sec)                                                 
12345678
```

order by排序多个字段：
排序的字段相同时，再按照后边的字段进行排序。

```mysql-sql
--查询年龄在18-26岁之间的男同学，按照身高从高到低排序，身高相同时按照年龄从小到大排序
select * from students where (age between 18 and 26) and gender =1 order by height desc,age asc;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | 
+----+-------+------+--------+--------+------------+-----------+--------+ 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 | 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 | 
+----+-------+------+--------+--------+------------+-----------+--------+ 
3 rows in set (0.00 sec)                                                  
12345678
--按照年龄从大到小、身高从低到高排序
select * from students order by age desc,height asc;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)
1234567891011121314151617181920212223242526272829303132
```

## 三、分页与制作分页

### 1.分页简介

当数据量过大时，在一页中查看数据是一件非常麻烦的事情，我们可以限制查出来的数据个数，即限制显示条数，这就是分页。
语法：`select * from 表名 limit start,count;`
–start起始的位置，从0开始，count个数，表示从start开始，获取count条数据。

```mysql-sql
select * from students limit 2;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.01 sec)
1234567
```

显示了前2条数据。

```mysql-sql
select * from students limit 5,5;
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |
+----+------+------+--------+--------+------------+-----------+--------+
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
|  8 | John |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
|  9 | John |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
| 10 | John |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| 11 | Rose |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
+----+------+------+--------+--------+------------+-----------+--------+
5 rows in set (0.00 sec)                                                
12345678910
```

查询了第6-10条数据。

### 2.制作分页

```mysql-sql
--每页显示2个，第1页
select * from students limit 0,2;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.00 sec)                                                 
1234567
--每页显示2个，第2页
select * from students limit 2,2;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.00 sec)                                                 
1234567
--每页显示2个，第3页
select * from students limit 4,2;
12
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |
+----+------+------+--------+--------+------------+-----------+--------+
|  6 | John |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
+----+------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.00 sec)
1234567
--每页显示2个，第4页
select * from students limit 6,2;
12
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |
+----+------+------+--------+--------+------------+-----------+--------+
|  8 | John |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
|  9 | John |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
+----+------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.00 sec)
1234567
```

可总结出规律：
第n页：`limit (n-1)*每页显示个数`，但是不能在SQL语句中直接使用公式，要提前算出数值。
limit语句只能放在**SQL语句最后**，只有先查询到数据才能分页。

```mysql-sql
select gender from students group by gender limit 0,2;
1
```

打印

```mysql-sql
+--------+              
| gender |              
+--------+              
| male   |              
| female |              
+--------+              
2 rows in set (0.00 sec)
1234567
```

## 四、连接查询

当查询结果的列来源于多张表时，需要将多张表连接成一个大的数据集，再选择合适的列返回。
在MySQL中支持三种类型的连接查询：

### 1.内连接查询：

查询的结果为两个表匹配到的数据，如下图所示：
![内连接](https://img-blog.csdnimg.cn/20191203201129337.png#pic_center)
语法：`select ... from 表A inner join 表B on 表1.列 = 表2.列;`

```mysql-sql
select * from students inner join classes;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | id | name   | 
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |  1 | class1 | 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |  2 | class2 | 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |  3 | class3 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |  1 | class1 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |  2 | class2 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |  3 | class3 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |  1 | class1 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |  2 | class2 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |  3 | class3 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  1 | class1 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  2 | class2 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  3 | class3 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |  1 | class1 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |  2 | class2 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |  3 | class3 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |  1 | class1 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |  2 | class2 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |  3 | class3 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |  1 | class1 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |  2 | class2 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |  3 | class3 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |  1 | class1 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |  2 | class2 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |  3 | class3 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |  1 | class1 | 
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |  2 | class2 | 
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |  3 | class3 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |  1 | class1 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |  2 | class2 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |  3 | class3 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |  1 | class1 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |  2 | class2 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |  3 | class3 | 
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |  1 | class1 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |  2 | class2 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |  3 | class3 | 
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |  1 | class1 | 
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |  2 | class2 | 
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |  3 | class3 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |  1 | class1 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |  2 | class2 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |  3 | class3 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  1 | class1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  2 | class2 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  3 | class3 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  1 | class1 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  2 | class2 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  3 | class3 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |  1 | class1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |  2 | class2 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |  3 | class3 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |  1 | class1 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |  2 | class2 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |  3 | class3 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |  1 | class1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |  2 | class2 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |  3 | class3 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |  1 | class1 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |  2 | class2 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |  3 | class3 | 
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |  1 | class1 | 
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |  2 | class2 | 
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |  3 | class3 | 
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+ 
81 rows in set (0.00 sec)                                                               
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586
```

得到的是两个表的笛卡儿积，但是一般无意义。

```mysql-sql
--查询对应班级的学生及班级信息
select * from students inner join classes on students.cls_id = classes.id;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | id | name   |
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |  1 | class1 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |  3 | class3 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |  2 | class2 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  1 | class1 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |  3 | class3 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |  1 | class1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 |
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |  3 | class3 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |  3 | class3 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |  1 | class1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |  1 | class1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |  2 | class2 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |  2 | class2 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |  3 | class3 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |  2 | class2 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |  3 | class3 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |  1 | class1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  1 | class1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  1 | class1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |  2 | class2 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |  1 | class1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |  2 | class2 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |  2 | class2 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |  1 | class1 |
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+
27 rows in set (0.00 sec)                                                              
1234567891011121314151617181920212223242526272829303132
```

去除重复信息

```mysql-sql
--查询对应班级的学生及班级信息
select students.*,classes.name from students inner join classes on students.cls_id = classes.id;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+--------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | name   | 
+----+-------+------+--------+--------+------------+-----------+--------+--------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 | class1 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 | class3 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 | class2 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | class1 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 | class3 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 | class1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 | class3 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 | class3 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 | class1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 | 
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 | class1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 | class2 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 | class2 | 
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 | class3 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 | class2 | 
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 | class3 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 | class1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | class1 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 | class1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 | class2 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 | class1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 | class2 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 | class2 | 
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 | class1 | 
+----+-------+------+--------+--------+------------+-----------+--------+--------+ 
27 rows in set (0.00 sec)                                                          
1234567891011121314151617181920212223242526272829303132
```

换别名

```mysql-sql
--查询对应班级的学生及班级信息
select s.*,c.name from students as s inner join classes as c on s.cls_id = c.id;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | name   |
+----+-------+------+--------+--------+------------+-----------+--------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 | class1 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 | class3 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 | class2 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | class1 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 | class3 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 | class1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 |
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 | class3 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 | class3 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 | class1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 | class1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 | class2 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 | class2 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 | class3 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 | class2 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 | class3 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 | class1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | class1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 | class1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 | class2 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 | class1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 | class2 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 | class2 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 | class1 |
+----+-------+------+--------+--------+------------+-----------+--------+--------+
27 rows in set (0.00 sec)                                                         
1234567891011121314151617181920212223242526272829303132
--查询对应班级的学生及班级信息
select c.name,s.* from students as s inner join classes as c on s.cls_id = c.id;
12
```

打印

```mysql-sql
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| name   | id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| class1 |  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| class3 |  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
| class2 |  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
| class1 |  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class3 |  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
| class1 |  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| class2 |  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class3 |  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
| class3 | 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| class1 | 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| class2 | 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class1 | 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| class2 | 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| class2 | 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| class3 | 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
| class2 | 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| class3 | 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| class1 | 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| class1 | 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 | 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| class2 | 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| class1 | 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
| class2 | 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
| class2 | 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| class1 | 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)                                                         
1234567891011121314151617181920212223242526272829303132
```

加入排序

```mysql-sql
select c.name,s.* from students as s inner join classes as c on s.cls_id = c.id order by c.name;
1
```

打印

```mysql-sql
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| name   | id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| class1 | 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| class1 |  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| class1 |  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| class1 | 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| class1 | 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
| class1 | 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 | 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| class1 |  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 | 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| class1 | 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
| class2 | 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| class2 |  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| class2 | 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| class2 | 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| class2 | 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
| class2 | 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| class2 |  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
| class2 | 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class3 |  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
| class3 | 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
| class3 |  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
| class3 | 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| class3 | 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| class3 |  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)                                                         
1234567891011121314151617181920212223242526272829303132
select c.name,s.* from students as s inner join classes as c on s.cls_id = c.id order by c.name,s.id asc;
1
```

打印

```mysql-sql
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| name   | id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| class1 |  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| class1 |  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 |  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| class1 | 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| class1 | 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| class1 | 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| class1 | 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 | 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| class1 | 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
| class1 | 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
| class2 |  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
| class2 |  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| class2 | 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| class2 | 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| class2 | 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| class2 | 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
| class2 | 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| class3 |  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
| class3 |  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
| class3 |  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
| class3 | 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| class3 | 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
| class3 | 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)
1234567891011121314151617181920212223242526272829303132
```

### 2.左连接查询

查询的结果为两个表匹配到的数据，左表特有的数据，对于右表中不存在的数据使用null填充，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191203201107744.png#pic_center)
语法：`select ... from 表A left join 表B on 表1.列 = 表2.列;`
以第一个表为主，应用比右连接更多。

```mysql-sql
select * from students left join classes on students.cls_id = classes.id;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | id   | name   |
+----+-------+------+--------+--------+------------+-----------+--------+------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |    1 | class1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |    1 | class1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |    1 | class1 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |    1 | class1 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |    1 | class1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |    1 | class1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |    1 | class1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |    1 | class1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |    1 | class1 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |    1 | class1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |    2 | class2 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |    2 | class2 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |    2 | class2 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |    2 | class2 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |    2 | class2 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |    2 | class2 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |    2 | class2 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |    2 | class2 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |    2 | class2 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |    2 | class2 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |    2 | class2 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |    3 | class3 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |    3 | class3 |
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |    3 | class3 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |    3 | class3 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |    3 | class3 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |    3 | class3 |
+----+-------+------+--------+--------+------------+-----------+--------+------+--------+
27 rows in set (0.01 sec)
1234567891011121314151617181920212223242526272829303132
select * from students as s left join classes as c on s.cls_id = c.id having c.id is null;
1
```

打印

```mysql-sql
Empty set (0.00 sec)
1
```

与

```mysql-sql
select * from students as s left join classes as c on s.cls_id = c.id where c.id is null;
1
```

效果相同。
连接后筛选用的关键字是having，也可以用where，但是建议用having。

```mysql-sql
select c.name,s.* from students as s left join classes as c on s.cls_id = c.id having c.id is null and s.is_delete = 1;
1
```

打印

```mysql-sql
Empty set (0.00 sec)
1
```

### 3.右连接

查询的结果为两个表匹配到的数据，右表特有的数据，对于左表中不存在的数据使用null填充，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019120320104157.png#pic_center)
语法：`select ... from 表A right join 表B on 表1.列 = 表2.列;`
以第二个表为主，应用比左连接更少，一般可以转化成左连接。

```mysql-sql
select * from students right join classes on students.cls_id = classes.id;
1
```

打印

```mysql-sql
+------+-------+------+--------+--------+------------+-----------+--------+----+--------+
| id   | NAME  | age  | gender | cls_id | birth      | is_delete | height | id | name   |
+------+-------+------+--------+--------+------------+-----------+--------+----+--------+
|    1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |    165 |  1 | class1 |
|    2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 |    157 |  3 | class3 |
|    3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |    165 |  2 | class2 |
|    4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |    168 |  1 | class1 |
|    6 | John  |   16 | secret |      3 | 1999-05-09 |         1 |    165 |  3 | class3 |
|    7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |    176 |  1 | class1 |
|    8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    165 |  2 | class2 |
|    9 | John  |   19 | secret |      3 | 1999-07-09 |         1 |    153 |  3 | class3 |
|   10 | John  |   19 | secret |      3 | 1999-07-09 |         1 |    165 |  3 | class3 |
|   11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 |    169 |  1 | class1 |
|   12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    165 |  2 | class2 |
|   13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 |    158 |  1 | class1 |
|   14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    165 |  2 | class2 |
|   15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |    183 |  2 | class2 |
|   16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    165 |  2 | class2 |
|   17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    173 |  2 | class2 |
|   18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 |    165 |  3 | class3 |
|   19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    159 |  2 | class2 |
|   20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 |    176 |  3 | class3 |
|   21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |    157 |  1 | class1 |
|   22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |    168 |  1 | class1 |
|   23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |    173 |  1 | class1 |
|   24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    175 |  2 | class2 |
|   25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |    164 |  1 | class1 |
|   26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    160 |  2 | class2 |
|   27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |    171 |  2 | class2 |
|   28 | null  |   18 | secret |      1 | 1990-01-01 |         0 |    169 |  1 | class1 |
+------+-------+------+--------+--------+------------+-----------+--------+----+--------+
27 rows in set (0.02 sec)                                                                
1234567891011121314151617181920212223242526272829303132
```

## 五、子查询

即嵌套查询，在一个select语句中嵌套另一个select语句，被嵌入的select语句称之为子查询语句。

```mysql-sql
--查询最高的男生信息
select * from students where height = (
    select max(height) from students where gender = 1) and gender = 1;
123
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+  
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |  
+----+------+------+--------+--------+------------+-----------+--------+  
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  
+----+------+------+--------+--------+------------+-----------+--------+  
1 row in set (0.00 sec)                                                   
123456
--查询高于平均身高的信息
select * from students where height > (select avg(height) from students);
12
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |
+----+------+------+--------+--------+------------+-----------+--------+
|  4 | John |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| 11 | Rose |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| 15 | Rose |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| 17 | John |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| 20 | Rose |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| 22 | John |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| 24 | John |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| 27 | Rose |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| 28 | null |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
+----+------+------+--------+--------+------------+-----------+--------+
11 rows in set (0.01 sec)                                               
12345678910111213141516
```

### 列级子查询

与内连接效果一样。

```mysql-sql
--查询学生的班级号能够对应的学生信息
select * from students where cls_id in (select id from classes);
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)
1234567891011121314151617181920212223242526272829303132
--查询最大年龄的女性id
select id,name from students where age = (select max(age) from students where gender = 2) and gender = 2;
12
```

打印

```mysql-sql
+----+------+
| id | name |
+----+------+
| 11 | Rose |
| 13 | Rose |
| 15 | Rose |
| 18 | Rose |
| 20 | Rose |
| 27 | Rose |
+----+------+
6 rows in set (0.00 sec)
1234567891011
```

# Python全栈（三）数据库优化之5.MySQL自关联、外键与Python操作MySQL

## 一、自关联

引入：省市区三级联动数据
导入外部.sql文件的数据：
从SQLyog选择文件导入并执行SQL语句，导入了三张表（省、市、县区）的数据。
读者需要练习使用省市区数据执行脚本可以打开文末的网盘链接进行下载。

```mysql-sql
-查询湖南省
select * from provinces where province = '湖南省';
12
```

打印

```mysql-sql
+----+------------+-----------+
| id | provinceid | province  |
+----+------------+-----------+
| 18 |     430000 | 湖南省    |
+----+------------+-----------+
1 row in set (0.00 sec)
123456
-查询湖南省下面的市
select * from cities where provinceid = 430000;
12
```

打印

```mysql-sql
+-----+--------+--------------------------------+------------+
| id  | cityid | city                           | provinceid |
+-----+--------+--------------------------------+------------+
| 186 | 430100 | 长沙市                         | 430000     |   
| 187 | 430200 | 株洲市                         | 430000     |   
| 188 | 430300 | 湘潭市                         | 430000     |   
| 189 | 430400 | 衡阳市                         | 430000     |   
| 190 | 430500 | 邵阳市                         | 430000     |   
| 191 | 430600 | 岳阳市                         | 430000     |   
| 192 | 430700 | 常德市                         | 430000     |   
| 193 | 430800 | 张家界市                       | 430000     |    
| 194 | 430900 | 益阳市                         | 430000     |   
| 195 | 431000 | 郴州市                         | 430000     |   
...         
+-----+--------+--------------------------------+------------+
14 rows in set, 5 warnings (0.00 sec)                         
12345678910111213141516
```

嵌套子查询

```mysql-sql
select * from provinces as p inner join cities as c on p.provinceid = c.provinceid having p.province = '湖南省';
1
```

打印

```mysql-sql
+----+------------+-----------+-----+--------+--------------------------------+------------+   
| id | provinceid | province  | id  | cityid | city                           | provinceid |   
+----+------------+-----------+-----+--------+--------------------------------+------------+   
| 18 |     430000 | 湖南省    | 186 | 430100 | 长沙市                         | 430000     |         
| 18 |     430000 | 湖南省    | 187 | 430200 | 株洲市                         | 430000     |         
| 18 |     430000 | 湖南省    | 188 | 430300 | 湘潭市                         | 430000     |         
| 18 |     430000 | 湖南省    | 189 | 430400 | 衡阳市                         | 430000     |         
| 18 |     430000 | 湖南省    | 190 | 430500 | 邵阳市                         | 430000     |         
| 18 |     430000 | 湖南省    | 191 | 430600 | 岳阳市                         | 430000     |         
| 18 |     430000 | 湖南省    | 192 | 430700 | 常德市                         | 430000     |         
| 18 |     430000 | 湖南省    | 193 | 430800 | 张家界市                       | 430000     |          
| 18 |     430000 | 湖南省    | 194 | 430900 | 益阳市                         | 430000     |         
| 18 |     430000 | 湖南省    | 195 | 431000 | 郴州市                         | 430000     |         
...
+----+------------+-----------+-----+--------+--------------------------------+------------+   
14 rows in set, 170 warnings (0.00 sec)                                                        
12345678910111213141516
```

查询areas一张表

```mysql-sql
-查询湖南省
select * from areas where name = '湖南';
12
```

打印

```mysql-sql
+----+-----+--------+------+
| id | pid | name   | type |
+----+-----+--------+------+
| 14 |   1 | 湖南   |    1 |
+----+-----+--------+------+
1 row in set (0.00 sec)
123456
-查询湖南省下面的市
select * from areas where pid = 14;
12
```

打印

```mysql-sql
+-----+-----+-----------+------+
| id  | pid | name      | type |
+-----+-----+-----------+------+
| 197 |  14 | 长沙      |    2 |  
| 198 |  14 | 张家界    |    2 |   
| 199 |  14 | 常德      |    2 |  
| 200 |  14 | 郴州      |    2 |  
| 201 |  14 | 衡阳      |    2 |  
| 202 |  14 | 怀化      |    2 |  
| 203 |  14 | 娄底      |    2 |  
| 204 |  14 | 邵阳      |    2 |  
| 205 |  14 | 湘潭      |    2 |  
| 206 |  14 | 湘西      |    2 |  
...
+-----+-----+-----------+------+
14 rows in set (0.00 sec)       
12345678910111213141516
-查询长沙 市下面的区
select * from areas where pid = 197;
12
```

打印

```mysql-sql
+------+-----+-----------+------+
| id   | pid | name      | type |
+------+-----+-----------+------+
| 1647 | 197 | 岳麓区    |    3 |
| 1648 | 197 | 芙蓉区    |    3 |
| 1649 | 197 | 天心区    |    3 |
| 1650 | 197 | 开福区    |    3 |
| 1651 | 197 | 雨花区    |    3 |
| 1652 | 197 | 开发区    |    3 |
| 1653 | 197 | 浏阳市    |    3 |
| 1654 | 197 | 长沙县    |    3 |
| 1655 | 197 | 望城县    |    3 |
| 1656 | 197 | 宁乡县    |    3 |
+------+-----+-----------+------+
10 rows in set (0.00 sec)
123456789101112131415
```

内连接查询

```mysql-sql
-查询湖南省下面的市
select * from areas as p inner join areas as c on p.id = c.pid having p.name = '湖南';
12
```

打印

```mysql-sql
+----+-----+--------+------+-----+-----+-----------+------+  
| id | pid | name   | type | id  | pid | name      | type |  
+----+-----+--------+------+-----+-----+-----------+------+  
| 14 |   1 | 湖南   |    1 | 197 |  14 | 长沙      |    2 |      
| 14 |   1 | 湖南   |    1 | 198 |  14 | 张家界    |    2 |       
| 14 |   1 | 湖南   |    1 | 199 |  14 | 常德      |    2 |      
| 14 |   1 | 湖南   |    1 | 200 |  14 | 郴州      |    2 |      
| 14 |   1 | 湖南   |    1 | 201 |  14 | 衡阳      |    2 |      
| 14 |   1 | 湖南   |    1 | 202 |  14 | 怀化      |    2 |      
| 14 |   1 | 湖南   |    1 | 203 |  14 | 娄底      |    2 |      
| 14 |   1 | 湖南   |    1 | 204 |  14 | 邵阳      |    2 |      
| 14 |   1 | 湖南   |    1 | 205 |  14 | 湘潭      |    2 |      
| 14 |   1 | 湖南   |    1 | 206 |  14 | 湘西      |    2 |      
...    
+----+-----+--------+------+-----+-----+-----------+------+  
14 rows in set (0.00 sec)                                    
12345678910111213141516
```

此即自关联，同一个表关联查询。
自连结其实就是连结查询，需要两张表，只不过它的左表（主表）和右表（子表）都是自己。在做自连接查询的时候是自己链接自己，分别给主表和子表取别名，再附加条件执行。

```mysql-sql
-查询长沙市下面的区
select * from areas as c inner join areas as a on c.id = a.pid having c.name = '哈尔滨';
12
```

打印

```mysql-sql
+-----+-----+--------+------+------+-----+-----------+------+ 
| id  | pid | name   | type | id   | pid | name      | type | 
+-----+-----+--------+------+------+-----+-----------+------+ 
| 197 |  14 | 长沙   |    2 | 1647 | 197 | 岳麓区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1648 | 197 | 芙蓉区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1649 | 197 | 天心区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1650 | 197 | 开福区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1651 | 197 | 雨花区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1652 | 197 | 开发区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1653 | 197 | 浏阳市    |    3 |      
| 197 |  14 | 长沙   |    2 | 1654 | 197 | 长沙县    |    3 |      
| 197 |  14 | 长沙   |    2 | 1655 | 197 | 望城县    |    3 |      
| 197 |  14 | 长沙   |    2 | 1656 | 197 | 宁乡县    |    3 |      
+-----+-----+--------+------+------+-----+-----------+------+ 
10 rows in set (0.01 sec)                                     
123456789101112131415
```

## 二、外键

一个健壮的数据库一定有很好的参照完整性。为了保证数据的完整性，将两张表之间的数据建立关系，因此就需要在表中添加外键约束。
创建两张表：

```mysql-sql
create table classes(
    id int(4) not null primary key,
    name varchar(36)
);
create table student(
    sid int(4) not null primary key,
    sname varchar(36),
    gid int(4) not null
);
123456789
```

添加外键：
语法：`alert table 表名 add constraint 外键名字 foreign key(外键字段名) references 外表表名(主键字段名)`;
对student表添加外键

```mysql-sql
alter table student add constraint fk_gid foreign key(gid) references classes(id);
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

引入外键之后，外键列只能插入参照列存在的值，参照列被参照的值不能被删除，这就保证了数据的参照完整性。
验证外键的作用：

```mysql-sql
insert into student (sid, sname, gid) values(1, 'Tom', 1);
1
```

打印

```mysql-sql
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`demo1125`.`student`, CONSTRAINT `fk_gid` FOREIGN KEY (`gid`) REFERENCES `classes` (`id`))
1
```

很显然插入失败，是外键限制，应该先在classes中插入一个班级，才能在学生中插入记录。

```mysql-sql
insert into classes values(1,'class1');
1
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)
1
```

此时再执行之前插入失败的语句

```mysql-sql
insert into student (sid, sname, gid) values(1, 'Tom', 1);
1
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
```

即插入成功。
删除数据时，比如删除1班

```mysql-sql
delete from classes where id = 1;
1
```

打印

```mysql-sql
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`demo1125`.`student`, CONSTRAINT `fk_gid` FOREIGN KEY (`gid`) REFERENCES `classes` (`id`))
1
```

即删除失败
要想删除班级，这里应该先删除班级为1的学生

```mysql-sql
delete from student where sid = 1;
1
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)
1
```

此时再执行之前删除失败的语句

```mysql-sql
delete from classes where id = 1;
1
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
```

即删除成功。
在MySQL中，有4种针对存在外键的数据表进行操作时的设置：Cascade（级联）、No Action（不做操作）、Restrict（限制，默认）、Set null（设为空）。
删除外键约束：
`alter table 表名 drop foreign key 外键名;`
PS：有可能在加入外键约束后插入数据不会报错，这是因为MySQL安装时默认用的表引擎是MyISAM，而MyISAM是不支持外键的，如图，
![引擎](https://img-blog.csdnimg.cn/20191206192146654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
要想解决这个问题，可以在当前的表设置引擎为InnoDB、PBXT或SolidDB，但这只是修改了这一个数据库，下次建新的数据库默认引擎还是MyISAM，我们可以在MySQL的安装目录下的配置文件my.ini中的 [mysqld] 下面加入`default-storage-engine=INNODB`（其他支持外键的引擎也可），再重启Mysql服务器即可，小编在这块也遇到了问题，很久都没能解决，最后请教老师成功解决了。
可参考https://www.cnblogs.com/lqcdsns/p/7858279.html。

## 三、MySQL和Python交互

### 1.数据准备

创建数据库和数据表

```mysql-sql
--创建京东数据库
create database jingdong charset = utf8;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
--创建商品goods数据表
create table goods(
    id int unsigned primary key auto_increment not null,
    name varchar(150) not null,
    cate_name varchar(40) not null,
    brand_name varchar(40) not null,
    price decimal(10,3) not null default 0,
    is_show tinyint not null default 1,
    is_saleoff tinyint not null default 0
);
12345678910
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

插入数据

```mysql-sql
-- 向goods表中插入数据
insert into goods values(0,'r510vc 15.6英寸笔记本','笔记本','华硕','3399',default,default); 
insert into goods values(0,'y400n 14.0英寸笔记本电脑','笔记本','联想','4999',default,default);
insert into goods values(0,'g150th 15.6英寸游戏本','游戏本','雷神','8499',default,default); 
insert into goods values(0,'x550cc 15.6英寸笔记本','笔记本','华硕','2799',default,default); 
insert into goods values(0,'x240 超极本','超级本','联想','4880',default,default); 
insert into goods values(0,'u330p 13.3英寸超极本','超级本','联想','4299',default,default); 
insert into goods values(0,'svp13226scb 触控超极本','超级本','索尼','7999',default,default); 
insert into goods values(0,'ipad mini 7.9英寸平板电脑','平板电脑','苹果','1998',default,default);
insert into goods values(0,'ipad air 9.7英寸平板电脑','平板电脑','苹果','3388',default,default); 
insert into goods values(0,'ipad mini 配备 retina 显示屏','平板电脑','苹果','2788',default,default); 
insert into goods values(0,'ideacentre c340 20英寸一体电脑 ','台式机','联想','3499',default,default); 
insert into goods values(0,'vostro 3800-r1206 台式电脑','台式机','戴尔','2899',default,default); 
insert into goods values(0,'imac me086ch/a 21.5英寸一体电脑','台式机','苹果','9188',default,default); 
insert into goods values(0,'at7-7414lp 台式电脑 linux ）','台式机','宏碁','3699',default,default); 
insert into goods values(0,'z220sff f4f06pa工作站','服务器/工作站','惠普','4288',default,default); 
insert into goods values(0,'poweredge ii服务器','服务器/工作站','戴尔','5388',default,default); 
insert into goods values(0,'mac pro专业级台式电脑','服务器/工作站','苹果','28888',default,default); 
insert into goods values(0,'hmz-t3w 头戴显示设备','笔记本配件','索尼','6999',default,default); 
insert into goods values(0,'商务双肩背包','笔记本配件','索尼','99',default,default); 
insert into goods values(0,'x3250 m4机架式服务器','服务器/工作站','ibm','6888',default,default); 
insert into goods values(0,'商务双肩背包','笔记本配件','索尼','99',default,default);
12345678910111213141516171819202122
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
1234567891011121314151617181920212223242526272829303132333435363738394041
```

数据表大致如下

```mysql-sql
+----+---------------------------------------+---------------------+------------+-----------+---------+-----
+                                                                                                           
| id | name                                  | cate_name           | brand_name | price     | is_show | is_s
|                                                                                                           
+----+---------------------------------------+---------------------+------------+-----------+---------+-----
+                                                                                                           
|  1 | r510vc 15.6英寸笔记本                 | 笔记本              | 华硕       |  3399.000 |       1 |          0    
|                                                                                                           
|  2 | y400n 14.0英寸笔记本电脑              | 笔记本              | 联想       |  4999.000 |       1 |          0      
|                                                                                                           
|  3 | g150th 15.6英寸游戏本                 | 游戏本              | 雷神       |  8499.000 |       1 |          0    
|                                                                                                           
|  4 | x550cc 15.6英寸笔记本                 | 笔记本              | 华硕       |  2799.000 |       1 |          0    
|                                                                                                           
|  5 | x240 超极本                           | 超级本              | 联想       |  4880.000 |       1 |          0  
|                                                                                                           
|  6 | u330p 13.3英寸超极本                  | 超级本              | 联想       |  4299.000 |       1 |          0    
|                                                                                                           
|  7 | svp13226scb 触控超极本                | 超级本              | 索尼       |  7999.000 |       1 |          0    
|                                                                                                           
|  8 | ipad mini 7.9英寸平板电脑             | 平板电脑            | 苹果       |  1998.000 |       1 |          0      
|                                                                                                           
|  9 | ipad air 9.7英寸平板电脑              | 平板电脑            | 苹果       |  3388.000 |       1 |          0      
|                                                                                                           
| 10 | ipad mini 配备 retina 显示屏          | 平板电脑            | 苹果       |  2788.000 |       1 |          0     
...                                                                                                 
+----+---------------------------------------+---------------------+------------+-----------+---------+-----
+                                                                                                           
21 rows in set (0.00 sec)                                                                                   
                                                                                                            
123456789101112131415161718192021222324252627282930
```

### 2.数据表拆分

数据表拆分是一种思想，将大表拆分成很多小表，可以增加复用、提高效率。

创建 “商品分类” 表

```mysql-sql
--创建种类表
create table goods_cates(
    id int unsigned primary key auto_increment not null,
    name varchar(40) not null
);
12345
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
--将种类插入种类表
insert into goods_cates (name) select cate_name from goods group by cate_name;
12
```

打印

```mysql-sql
Query OK, 7 rows affected (0.01 sec)
Records: 7  Duplicates: 0  Warnings: 0
12
```

将查询的数据插入新表不需要values字段，否则会报错。

```mysql-sql
--修改goods表
update goods as g inner join goods_cates as c on g.cate_name = c.name set g.cate_name = c.id;
select * from goods;
123
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Rows matched: 21  Changed: 0  Warnings: 0
12
```

此时，goods表为

```mysql-sql
+----+---------------------------------------+-----------+------------+-----------+---------+------------+ 
| id | name                                  | cate_name | brand_name | price     | is_show | is_saleoff | 
+----+---------------------------------------+-----------+------------+-----------+---------+------------+ 
|  1 | r510vc 15.6英寸笔记本                 | 5         | 华硕       |  3399.000 |       1 |          0 |        
|  2 | y400n 14.0英寸笔记本电脑              | 5         | 联想       |  4999.000 |       1 |          0 |          
|  3 | g150th 15.6英寸游戏本                 | 4         | 雷神       |  8499.000 |       1 |          0 |        
|  4 | x550cc 15.6英寸笔记本                 | 5         | 华硕       |  2799.000 |       1 |          0 |        
|  5 | x240 超极本                           | 7         | 联想       |  4880.000 |       1 |          0 |      
|  6 | u330p 13.3英寸超极本                  | 7         | 联想       |  4299.000 |       1 |          0 |        
|  7 | svp13226scb 触控超极本                | 7         | 索尼       |  7999.000 |       1 |          0 |        
|  8 | ipad mini 7.9英寸平板电脑             | 2         | 苹果       |  1998.000 |       1 |          0 |         
|  9 | ipad air 9.7英寸平板电脑              | 2         | 苹果       |  3388.000 |       1 |          0 |         
| 10 | ipad mini 配备 retina 显示屏          | 2         | 苹果       |  2788.000 |       1 |          0 |        
...
+----+---------------------------------------+-----------+------------+-----------+---------+------------+ 
21 rows in set (0.00 sec)                                                                                  
12345678910111213141516
```

### 3.Python操作MySQL

#### Python-MySQL安装

在Windows操作系统上安装：
Python3:`pip install pymysql`
Python2:`pip install MySQLdb`
Linux（Ubuntu）安装：https://www.jianshu.com/p/d84cdb5e6273

#### 操作步骤

- 开始
- 创建connection
- 获取cursor
- 执行查询、执行命令、获取数据、处理数据
- 关闭cursor
- 关闭connection
- 结束

和文件操作类似。
如图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vZWQvMGMvZWQwYzExNTlhODJhZDNlNzg4YWM1MWM1N2M0NjI2ZjVfODc1eDU2Ny5wbmc?x-oss-process=image/format,png)
（1）Connection 对象
用于建立与数据库的连接
创建对象：调用**connect()** 方法。

```python
conn=connect(参数列表)
参数host：连接的mysql主机，如果本机是'localhost'
参数port：连接的mysql主机的端口，默认是3306
参数database：数据库的名称
参数user：连接的用户名
参数password：连接的密码
参数charset：通信采用的编码方式，推荐使用utf8
1234567
```

根据导入库的方式，具体可分为两种连接方式
方式一

```python
import pymysql
con = pymysql.connect(host = 'localhost',port=3306,database='jingdong',user='root',password = 'root',charset = 'utf8')
12
```

方式二

```python
from pymysql import *
conn = connect(host = 'localhost',port=3306,database='jingdong',user='root',password = 'root',charset = 'utf8')
12
```

对象的方法：

- commit()提交；
- cursor()返回Cursor对象,用于执行sql语句并获得结果；
- close()关闭连接。

（2）Cursor对象
用于执行sql语句,经常使用的语句为select、insert、update、delete
获取Cursor对象:调用Connection对象的cursor()方法。

```python
cur = conn.cursor()
1
```

对象的方法

- `execute(operation [, parameters ])`执行语句，返回受影响的行数，主要用于执行insert、update、delete语句，也可以执行create、alter、drop等语句；
- fetchone()执行查询语句时，获取查询结果集的第一个行数据，返回一个元组；
- fetchmany(n)执行查询语句时，获取查询结果集的n行数据，一行构成一个元组，再将这些元组装入一个元组返回；
- fetchall()执行查询时，获取结果集的所有行，一行构成一个元组，再将这些元组装入一个元组返回；
- close()关闭 先关闭游标,在关闭链接。

使用python连接数据库并代码实现查询数据库中的数据示例：

```python
from pymysql import *

#连接数据库
conn = connect(host='127.0.0.1',port=3306,db='jingdong',user='root',passwd='root',charset='utf8')

#获取游标对象
cur = conn.cursor()

#执行SQL语句，获取查询结果的记录数
r = cur.execute('select * from goods;')
print(r)

#得到数据
#获取一条数据
print(cur.fetchone())
print(cur.fetchone())
print(cur.fetchone())
#获取多条数据
print(cur.fetchmany(3))
#获取全部数据，是指游标之后的全部，而不一定是整个表的所有数据
print(cur.fetchall())
print(cur.fetchone())

#关闭游标对象
cur.close()

#关闭连接
conn.close()
12345678910111213141516171819202122232425262728
```

打印结果为

```python
21
(1, 'r510vc 15.6英寸笔记本', '5', '华硕', Decimal('3399.000'), 1, 0)
(2, 'y400n 14.0英寸笔记本电脑', '5', '联想', Decimal('4999.000'), 1, 0)
(3, 'g150th 15.6英寸游戏本', '4', '雷神', Decimal('8499.000'), 1, 0)
((4, 'x550cc 15.6英寸笔记本', '5', '华硕', Decimal('2799.000'), 1, 0), (5, 'x240 超极本', '7', '联想', Decimal('4880.000'), 1, 0), (6, 'u330p 13.3英寸超极本', '7', '联想', Decimal('4299.000'), 1, 0))
((7, 'svp13226scb 触控超极本', '7', '索尼', Decimal('7999.000'), 1, 0), (8, 'ipad mini 7.9英寸平板电脑', '2', '苹果', Decimal('1998.000'), 1, 0), (9, 'ipad air 9.7英寸平板电脑', '2', '苹果', Decimal('3388.000'), 1, 0), (10, 'ipad mini 配备 retina 显示屏', '2', '苹果', Decimal('2788.000'), 1, 0), (11, 'ideacentre c340 20英寸一体电脑 ', '1', '联想', Decimal('3499.000'), 1, 0), (12, 'vostro 3800-r1206 台式电脑', '1', '戴尔', Decimal('2899.000'), 1, 0), (13, 'imac me086ch/a 21.5英寸一体电脑', '1', '苹果', Decimal('9188.000'), 1, 0), (14, 'at7-7414lp 台式电脑 linux ）', '1', '宏碁', Decimal('3699.000'), 1, 0), (15, 'z220sff f4f06pa工作站', '3', '惠普', Decimal('4288.000'), 1, 0), (16, 'poweredge ii服务器', '3', '戴尔', Decimal('5388.000'), 1, 0), (17, 'mac pro专业级台式电脑', '3', '苹果', Decimal('28888.000'), 1, 0), (18, 'hmz-t3w 头戴显示设备', '6', '索尼', Decimal('6999.000'), 1, 0), (19, '商务双肩背包', '6', '索尼', Decimal('99.000'), 1, 0), (20, 'x3250 m4机架式服务器', '3', 'ibm', Decimal('6888.000'), 1, 0), (21, '商务双肩背包', '6', '索尼', Decimal('99.000'), 1, 0))
None
1234567
```

在执行`cur.fetchall()`后游标到数据最后位置，已查询完毕，此时再`cur.fetchone()`会返回None，要想再次查询再执行一次`r = cur.execute('select * from goods;')`即可。
在查询完毕后，要先关闭游标，再关闭连接。
在连接数据库时，一般要异常处理，如上边代码可修改为

```python
from pymysql import *

try:
    #连接数据库
    conn = connect(host='127.0.0.1',port=3306,db='jingdong',user='root',passwd='root',charset='utf8')

    #获取游标对象
    cur = conn.cursor()

    #执行SQL语句，获取查询结果的记录数
    r = cur.execute('select * from goods;')
    print(r)

    #得到数据
    #获取一条数据
    print(cur.fetchone())
    print(cur.fetchone())
    print(cur.fetchone())
    #获取多条数据
    print(cur.fetchmany(3))
    #获取全部数据，是指游标之后的全部，而不一定是整个表的所有数据
    print(cur.fetchall())
    print(cur.fetchone())

    #关闭游标对象
    cur.close()

    #关闭连接
    conn.close()

except Exception as e:
    print("Error %d:%s"%(e.args[0],e.args[1]))
1234567891011121314151617181920212223242526272829303132
```

查询结果与之前相同。
省市区数据脚本链接：
https://pan.baidu.com/s/1IkODeq4N_drQYWiysUuKjA
提取码: p39a。

# Python全栈（三）数据库优化之6.Python操作MySQL和视图

## 一、封装MySQL-DB类

用面向对象的思想，封装DB类。
实现初始化（连接）、查询、关闭等操作。

```python
from pymysql import *

class MyDB(object):
    def __init__(self):
        self.conn()

    def conn(self):
        '''连接'''
        try:
            self.conn = connect(
                host='127.0.0.1',
                port=3306,
                db='demo',
                user='root',
                passwd='root',
                charset='utf8'
            )
        except Exception as e:
            print(e)

    def get_one(self):
        '''单个记录查询'''
        sql = 'select * from students;'
        #获取游标
        cur = self.conn.cursor()
        #执行SQL语句
        cur.execute(sql)
        #获取执行结果
        result = cur.fetchone()
        cur.close()
        return result

    def get_many(self):
        '''多个记录查询'''
        sql = 'select * from students;'
        #获取游标
        cur = self.conn.cursor()
        #执行SQL语句
        cur.execute(sql)
        #获取执行结果
        result = cur.fetchmany()
        cur.close()
        return result

    def get_all(self):
        '''所有记录查询'''
        sql = 'select * from students;'
        #获取游标
        cur = self.conn.cursor()
        #执行SQL语句
        cur.execute(sql)
        #获取执行结果
        result = cur.fetchall()
        cur.close()
        return result

    def __del__(self):
        '''关闭连接，释放'''
        self.conn.close()

def main():
    obj = MyDB()
    res1 = obj.get_one()
    print(res1)
    res2 = obj.get_all()
    for r in res2:
        print(r)

if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970
```

进行实例化并测试得，

```python
(1, 'Tom', 18, 'male', 1, datetime.date(1999, 9, 9), 1, Decimal('165.00'))
(1, 'Tom', 18, 'male', 1, datetime.date(1999, 9, 9), 1, Decimal('165.00'))
(2, 'Jerry', 19, 'male', 3, datetime.date(1999, 10, 9), 1, Decimal('157.00'))
(3, 'Nancy', 17, 'male', 2, datetime.date(1999, 8, 9), 1, Decimal('165.00'))
(4, 'John', 19, 'secret', 1, datetime.date(1999, 7, 9), 1, Decimal('168.00'))
(6, 'John', 16, 'secret', 3, datetime.date(1999, 5, 9), 1, Decimal('165.00'))
(7, 'John', 19, 'secret', 1, datetime.date(1999, 7, 9), 1, Decimal('176.00'))
(8, 'John', 19, 'secret', 2, datetime.date(1999, 7, 9), 1, Decimal('165.00'))
(9, 'John', 19, 'secret', 3, datetime.date(1999, 7, 9), 1, Decimal('153.00'))
(10, 'John', 19, 'secret', 3, datetime.date(1999, 7, 9), 1, Decimal('165.00'))
...
1234567891011
```

## 二、练习-商品表的查询

需求：
输入1：查询所有商品
输入2：查询所有商品种类
输入3：查询所有品牌
输入4：添加商品种类
输入5：退出
代码如下，

```python
from pymysql import *
class Jd(object):
    def __init__(self):
        '''初始化'''
        #连接数据库
        self.conn = connect(
            host='127.0.0.1',
            port=3306,
            db='jingdong',
            user='root',
            passwd='root',
            charset='utf8'
        )
        #获取游标
        self.cur = self.conn.cursor()

    @staticmethod
    def print_menu():
        '''静态方法，不需要self，单独定义打印菜单函数，解耦，降低耦合性'''
        print('--jd shop--')
        print('1-查询所有商品')
        print('2-查询所有种类')
        print('3-查询所有品牌')
        print('4-添加商品种类')
        print('5-退出')
        num = input('请输入一个数字：')
        return num

    def execute_sql(self,sql):
        self.cur.execute(sql)
        result = self.cur.fetchall()
        for item in result:
            print(item)

    def show_all_goods(self):
        '''查询所有商品'''
        sql = 'select * from goods;'
        self.execute_sql(sql)

    def show_all_cates(self):
        '''查询所有种类'''
        sql = 'select * from goods_cates;'
        self.execute_sql(sql)

    def show_all_brands(self):
        sql = 'select distinct brand_name from goods;'
        self.execute_sql(sql)

    def add_cate(self):
        name = input('请输入新的商品分类名：')
        sql = 'insert into goods_cates(name) values("%s")' % name
        self.cur.execute(sql)
        print('Adding cates successfully!')

    def run(self):
        while True:
            num = self.print_menu()
            if num == '1':
                self.show_all_goods()
            elif num == '2':
                self.show_all_cates()
            elif num == '3':
                self.show_all_brands()
            elif num == '4':
                self.add_cate()
            elif num == '5':
                print('Bye-Bye')
                break

    def __del__(self):
        self.cur.close()
        self.conn.close()

def main():
    jd = Jd()
    jd.run()

if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879
```

进行实例化并测试得，

```python
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：1
(1, 'r510vc 15.6英寸笔记本', '5', '华硕', Decimal('3399.000'), 1, 0)
(2, 'y400n 14.0英寸笔记本电脑', '5', '联想', Decimal('4999.000'), 1, 0)
(3, 'g150th 15.6英寸游戏本', '4', '雷神', Decimal('8499.000'), 1, 0)
(4, 'x550cc 15.6英寸笔记本', '5', '华硕', Decimal('2799.000'), 1, 0)
(5, 'x240 超极本', '7', '联想', Decimal('4880.000'), 1, 0)
(6, 'u330p 13.3英寸超极本', '7', '联想', Decimal('4299.000'), 1, 0)
(7, 'svp13226scb 触控超极本', '7', '索尼', Decimal('7999.000'), 1, 0)
(8, 'ipad mini 7.9英寸平板电脑', '2', '苹果', Decimal('1998.000'), 1, 0)
(9, 'ipad air 9.7英寸平板电脑', '2', '苹果', Decimal('3388.000'), 1, 0)
(10, 'ipad mini 配备 retina 显示屏', '2', '苹果', Decimal('2788.000'), 1, 0)
(11, 'ideacentre c340 20英寸一体电脑 ', '1', '联想', Decimal('3499.000'), 1, 0)
(12, 'vostro 3800-r1206 台式电脑', '1', '戴尔', Decimal('2899.000'), 1, 0)
(13, 'imac me086ch/a 21.5英寸一体电脑', '1', '苹果', Decimal('9188.000'), 1, 0)
(14, 'at7-7414lp 台式电脑 linux ）', '1', '宏碁', Decimal('3699.000'), 1, 0)
(15, 'z220sff f4f06pa工作站', '3', '惠普', Decimal('4288.000'), 1, 0)
(16, 'poweredge ii服务器', '3', '戴尔', Decimal('5388.000'), 1, 0)
(17, 'mac pro专业级台式电脑', '3', '苹果', Decimal('28888.000'), 1, 0)
(18, 'hmz-t3w 头戴显示设备', '6', '索尼', Decimal('6999.000'), 1, 0)
(19, '商务双肩背包', '6', '索尼', Decimal('99.000'), 1, 0)
(20, 'x3250 m4机架式服务器', '3', 'ibm', Decimal('6888.000'), 1, 0)
(21, '商务双肩背包', '6', '索尼', Decimal('99.000'), 1, 0)
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：2
(1, '台式机')
(2, '平板电脑')
(3, '服务器/工作站')
(4, '游戏本')
(5, '笔记本')
(6, '笔记本配件')
(7, '超级本')
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：3
('华硕',)
('联想',)
('雷神',)
('索尼',)
('苹果',)
('戴尔',)
('宏碁',)
('惠普',)
('ibm',)
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：4
请输入新的商品分类名：minipads
Adding cates successfully!
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：5
Bye-Bye
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475
```

这个类种定义了一个**静态方法**，不需要传入self参数，因为这个print_menu()方法与类无实际联系，所以定义为静态方法；
同时运用了**解耦**的思想，降低耦合性，能独立出方法块的尽量抽离出独立的方法，减少依赖性，写程序时要求高内聚、低耦合。

## 三、封装类中数据库的新增

在封装类的数据库中进行数据的新增、更新、删除。
执行插入语句后，

```python
from pymysql import *

def operate_db():
    global conn
    #连接数据库
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo1125',
        user='root',
        passwd='root',
        charset='utf8'
    )
    #获取游标
    cur = conn.cursor()
    sql = 'insert into student(sname) values("Jack");'
    cur.execute(sql)
    cur.close()
    conn.close()

if __name__ == '__main__':
    operate_db()
12345678910111213141516171819202122
```

再查询数据库并没有看到新增的数据，说明数据并没有被成功插入，此时需要提交事务。
如下

```python
from pymysql import *

def operate_db():
    global conn
    #连接数据库
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo1125',
        user='root',
        passwd='root',
        charset='utf8'
    )
    #获取游标
    cur = conn.cursor()
    sql = 'insert into student(sname) values("Jack");'
    cur.execute(sql)
    # sql = 'insert into student(name) values("Jackson");'
    # cur.execute(sql)
    #提交事务
    conn.commit()
    cur.close()
    conn.close()

if __name__ == '__main__':
    operate_db()
1234567891011121314151617181920212223242526
```

此时，再去查询，发现已经有了一条刚刚插入的数据，并且id并不连续，即未成功提交事务的数据也会占用id，如下所示

```python
+-----+-------+-----+
| sid | sname | gid |
+-----+-------+-----+
|   3 | Jack  |   1 |
|   4 | Jack  |   1 |
|   5 | Jack  |   1 |
|   8 | Jack  |   1 |
+-----+-------+-----+
12345678
```

5过了就直接到8了，id自增是不可逆的。
提交事务：
只有提交之后，数据才会被真正地添加到数据库中，数据库才会发生相应的更改。
同时执行多条插入语句时

```python
from pymysql import *

def operate_db():
    global conn
    #连接数据库
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo1125',
        user='root',
        passwd='root',
        charset='utf8'
    )
    #获取游标
    cur = conn.cursor()
    sql = 'insert into student(sname) values("Jack");'
    cur.execute(sql)
    sql = 'insert into student(sname) values("Jackson");'
    cur.execute(sql)
    #提交事务
    conn.commit()
    cur.close()
    conn.close()

if __name__ == '__main__':
    operate_db()
1234567891011121314151617181920212223242526
```

查询到

```mysql-sql
+-----+---------+-----+
| sid | sname   | gid |
+-----+---------+-----+
|   3 | Jack    |   1 |
|   4 | Jack    |   1 |
|   5 | Jack    |   1 |
|   8 | Jack    |   1 |
|   9 | Jack    |   1 |
|  10 | Jackson |   1 |
+-----+---------+-----+
12345678910
```

当第2条SQL语句出错时，

```python
from pymysql import *

def operate_db():
    global conn
    #连接数据库
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo1125',
        user='root',
        passwd='root',
        charset='utf8'
    )
    #获取游标
    cur = conn.cursor()
    sql = 'insert into student(sname) values("Jack");'
    cur.execute(sql)
    sql = 'insert into student(name) values("Jackson");'
    cur.execute(sql)
    #提交事务
    conn.commit()
    cur.close()
    conn.close()

if __name__ == '__main__':
    operate_db()
1234567891011121314151617181920212223242526
```

此时，数据为

```mysql-sql
+-----+---------+-----+ 
| sid | sname   | gid | 
+-----+---------+-----+ 
|   3 | Jack    |   1 | 
|   4 | Jack    |   1 | 
|   5 | Jack    |   1 | 
|   8 | Jack    |   1 | 
|   9 | Jack    |   1 | 
|  10 | Jackson |   1 | 
+-----+---------+-----+ 
6 rows in set (0.00 sec)
1234567891011
```

可知，在第2条打起来语句有错的情况下，第1条语句也为成功插入。
可以加入异常处理

```python
from pymysql import *

def operate_db():
    global conn
    try:
        #连接数据库
        conn = connect(
            host='127.0.0.1',
            port=3306,
            db='demo1125',
            user='root',
            passwd='root',
            charset='utf8'
        )
        #获取游标
        cur = conn.cursor()
        sql = 'insert into student(sname) values("Jack");'
        cur.execute(sql)
        sql = 'insert into student(name) values("Jackson");'
        cur.execute(sql)
        #提交事务
        conn.commit()
        cur.close()
        conn.close()
    except Exception as e:
        print(e.args[0],e.args[1])
        conn.commit()

if __name__ == '__main__':
    operate_db()
123456789101112131415161718192021222324252627282930
```

打印`1054 Unknown column 'name' in 'field list'`。
此时再查询，

```mysql-sql
+-----+---------+-----+ 
| sid | sname   | gid | 
+-----+---------+-----+ 
|   3 | Jack    |   1 | 
|   4 | Jack    |   1 | 
|   5 | Jack    |   1 | 
|   8 | Jack    |   1 | 
|   9 | Jack    |   1 | 
|  10 | Jackson |   1 | 
|  22 | Jack    |   1 | 
+-----+---------+-----+ 
7 rows in set (0.01 sec)
123456789101112
```

显然，第1条语句被成功执行并插入，但是id已不再是从刚才的id自增1，而是间隔了很多个值，这样就能达到将前边正确的SQL语句插入的目的，即部分提交；
如果要使得如果在发生异常时，所有的操作都撤回，应该使用回滚，即**rollback**。

```python
from pymysql import *

def operate_db():
    global conn
    try:
        #连接数据库
        conn = connect(
            host='127.0.0.1',
            port=3306,
            db='demo1125',
            user='root',
            passwd='root',
            charset='utf8'
        )
        #获取游标
        cur = conn.cursor()
        sql = 'insert into student(sname) values("Jack");'
        cur.execute(sql)
        sql = 'insert into student(name) values("Jackson");'
        cur.execute(sql)
        #提交事务
        conn.commit()
        cur.close()
        conn.close()
    except Exception as e:
        print(e.args[0],e.args[1])
        #回滚
        conn.rollback()

if __name__ == '__main__':
    operate_db()
12345678910111213141516171819202122232425262728293031
```

此时查询数据库，可得

```mysql-sql
+-----+---------+-----+
| sid | sname   | gid |
+-----+---------+-----+
|   3 | Jack    |   1 |
|   4 | Jack    |   1 |
|   5 | Jack    |   1 |
|   8 | Jack    |   1 |
|   9 | Jack    |   1 |
|  10 | Jackson |   1 |
|  22 | Jack    |   1 |
+-----+---------+-----+
1234567891011
```

显然，这时即便第1条SQL语句即便未出错，但是因为第2条语句出错，导致回滚操作使所有的操作撤回，数据表未增加。

```python
from pymysql import *

def operate_db():
    global conn
    try:
        #连接数据库
        conn = connect(
            host='127.0.0.1',
            port=3306,
            db='demo1125',
            user='root',
            passwd='root',
            charset='utf8'
        )
        #获取游标
        cur = conn.cursor()
        sql = 'insert into student(sname) values("Tom");'
        cur.execute(sql)
        sql = 'insert into student(sname) values("Tommy");'
        cur.execute(sql)
        #提交事务
        conn.commit()
        cur.close()
        conn.close()
    except Exception as e:
        print(e.args[0],e.args[1])
        #回滚
        conn.rollback()


if __name__ == '__main__':
    operate_db()
1234567891011121314151617181920212223242526272829303132
```

此时插入两条正常数据

```mysql-sql
+-----+---------+-----+
| sid | sname   | gid |
+-----+---------+-----+
|   3 | Jack    |   1 |
|   4 | Jack    |   1 |
|   5 | Jack    |   1 |
|   8 | Jack    |   1 |
|   9 | Jack    |   1 |
|  10 | Jackson |   1 |
|  22 | Jack    |   1 |
|  26 | Tom     |   1 |
|  27 | Tommy   |   1 |
+-----+---------+-----+
12345678910111213
```

插入成功，显然id并未从22开始，说明rollback即使未插入数据，但是还是占用了id。
总结：
只要`cur.execute(sql)`被正常执行，即SQL语句正确，id就会自增，未被正确执行，就不会自增，而不管数据是否被插入和是否提交事务；
回滚时，正确执行的使id增加，未被正确执行的使id不变；
全部执行成功时，id增加；
全部都不提交：rollback，id部分增加
提交一部分：commit，id部分增加。
数据的更新、删除操作与新增类似。

## 四、视图

### 1.问题提出

对于复杂的查询，往往是有多个数据表进行关联查询而得到，如果数据库因为需求等原因发生了改变，为了保证查询出来的数据与之前相同，则需要在多个地方进行修改，维护起来非常麻烦。

### 2.视图定义

通俗地讲，视图就是一条select语句执行后返回的结果集，即虚拟的表。
我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。
视图是对若干张基本表的引用，一张虚表，查询语句执行的结果，不存储具体的数据（基本表数据发生了改变，视图也会跟着改变）。

### 3.视图操作

**创建视图**
语法：`create view 视图名称 as select语句;`
一般的查询语句：

```mysql-sql
select p.*,c.name as cname from areas as p inner join areas as c on p.id = c.pid having p.name = '湖南'
1
```

打印

```mysql-sql
+----+-----+--------+------+-----------+
| id | pid | name   | type | cname     |
+----+-----+--------+------+-----------+
| 14 |   1 | 湖南   |    1 | 长沙      |    
| 14 |   1 | 湖南   |    1 | 张家界    |     
| 14 |   1 | 湖南   |    1 | 常德      |    
| 14 |   1 | 湖南   |    1 | 郴州      |    
| 14 |   1 | 湖南   |    1 | 衡阳      |    
| 14 |   1 | 湖南   |    1 | 怀化      |    
| 14 |   1 | 湖南   |    1 | 娄底      |    
| 14 |   1 | 湖南   |    1 | 邵阳      |    
| 14 |   1 | 湖南   |    1 | 湘潭      |    
...  
+----+-----+--------+------+-----------+
14 rows in set (0.01 sec)               
123456789101112131415
```

运用查询到的结果创建视图

```mysql-sql
create view v_p_c as select p.*,c.name as cname from areas as p inner join areas as c on p.id = c.pid having p.name = '湖南';
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

**查看视图**
语法：`show tables;`

```mysql-sql
show tables;
1
```

打印

```mysql-sql
+--------------------+
| Tables_in_demo1125 |
+--------------------+
| areas              |
| cities             |
| classes            |
| provinces          |
| student            |
| v_p_c              |
+--------------------+
6 rows in set (0.00 sec)
1234567891011
```

很明显，此时已经增加了一个新表即视图v_p_c。
**使用视图**

```mysql-sql
select * from v_p_c;
1
```

打印

```mysql-sql
+----+-----+--------+------+-----------+
| id | pid | name   | type | cname     |
+----+-----+--------+------+-----------+
| 14 |   1 | 湖南   |    1 | 长沙      |
| 14 |   1 | 湖南   |    1 | 张家界    |
| 14 |   1 | 湖南   |    1 | 常德      |
| 14 |   1 | 湖南   |    1 | 郴州      |
| 14 |   1 | 湖南   |    1 | 衡阳      |
| 14 |   1 | 湖南   |    1 | 怀化      |
| 14 |   1 | 湖南   |    1 | 娄底      |
| 14 |   1 | 湖南   |    1 | 邵阳      |
| 14 |   1 | 湖南   |    1 | 湘潭      |
| 14 |   1 | 湖南   |    1 | 湘西      |
...
+----+-----+--------+------+-----------+
14 rows in set (0.01 sec)
12345678910111213141516
```

**删除视图**
语法：`drop view 视图名称;`
例如：

```mysql-sql
drop view v_p_c;
1
```

**视图修改**
有下列内容之一，视图不能做修改：

- select子句中包含distinct；
- select子句中包含组函数；
- select语句中包含group by子句；
- select语句中包含order by子句；
- where子句中包含相关子查询；
- from子句中包含多个表；
- 如果视图中有计算列，则不能更新；
- 如果基表中有某个具有非空约束的列未出现在视图定义中，则不能做insert操作。

可知，视图基本不允许修改。

```mysql-sql
update v_p_c set cname ='长沙市' where cname ='长沙;'
1
```

打印

```mysql-sql
ERROR 1288 (HY000): The target table v_p_c of the UPDATE is not updatable
1
```

报错，因为视图本来就是为了查询创建的，而不是为了修改而创建，所以视图修改是没有必要的，也不允许。
原表中的数据改变时：

- 如改变的数据未在select语句的条件中出现，视图中的数据也会相应发生改变；
- 如改变的数据在select语句的条件中出现，视图中的数据也会相应发生改变，因为查询的条件可能因为数据的修改而不再满足，使得查询到的数据量减少或增加。
  可以删除视图，但是不能删除视图中的数据，只能对视图进行查询操作。

**视图的作用**

- 方便操作，特别是查询操作；
- 减少复杂的SQL语句，就像一个函数，提高了重用性，增强代码可读性；
- 对数据库重构，却不影响程序的运行；
- 隐藏数据，提高了安全性能，可以对不同的用户提供不同的视图；
- 让数据更加清晰。

一般在**SQL语句较复杂**时使用视图来简化操作。

# Python全栈（三）数据库优化之7.MySQL高级-事务、索引、账户管理和存储引擎介绍

## 一、事务

### 1.事务引出

事务广泛用于订单系统、银行系统等多种场景。
例如：
A用户和B用户是银行的储户，现在A要给B转账500元，那么需要做以下几件事：
检查A的账户余额>500元；
A账户中扣除500元;
B账户中增加500元。
正常的流程走下来，A账户扣了500，B账户加了500；
那如果A账户扣了钱之后，系统出故障了，A会白白损失了500，而B也没有收到本该属于他的500。
以上的案例中，隐藏着一个前提条件：A扣钱和B加钱，要么同时成功，要么同时失败。事务的需求就在于此。

### 2.事务定义

事务是一个操作序列，这些操作要么都执行，要么都不执行，它是一个不可分割的工作单位。
例如，银行转帐工作：从一个帐号扣款并使另一个帐号增款，这两个操作要么都执行，要么都不执行。所以，应该把他们看成一个事务。
事务是数据库维护数据一致性的单位，在每个事务结束时，都能保持数据一致性。
那么至少需要三个步骤：
（1）检查账户1的余额高于或者等于100美元；
（2）从账户1余额中减去100美元；
（3）在帐户2余额中增加100美元。

```mysql-sql
begin;
update bank1 set money = money - 100 where id = 1;
12
```

此时没有commit，查询时数据不会发生变化，即体现了事物的隔离性。

```mysql-sql
update bank2 set money = money + 100 where id = 1;
commit;
12
```

此时两个表中的数据均发生变化，一个减少100，一个增加100。
以上两步操作组成一个完整的事务。
开启事务后每次执行SQL语句后不会自动提交，要提交才能操作成功。

### 3.事务的四大特性（ACID）

（1）**原子性**（Atomicity）：
一个事务被视为一个不可分割的最小工作单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说，不可能只执行其中的一部分操作，这就是事务的原子性。
（2）**一致性**（Consistency）：
数据库总是从一个一致性的状态转换到另一个一致性的状态。
在前面的例子中，一致性确保了，即使在执行两条update语句之间时系统崩溃，银行账户中也不会损失100美元，因为事务最终没有提交，所以事务中所做的修改也不会保存到数据库中。
（3）**隔离性**（Isolation）：
通常来说，一个事务所作的修改在最终提交以前，对其他事务是不可见的。
在前面的例子中，当执行完第一update语句、第四条语句还未开始时，此时有另外的一个账户汇总程序开始运行，则其看到帐户1的余额并没有被减去100美元。
在一个命令行中，开启一个事务

```mysql-sql
begin;
update goods set name = 'palyer' where id = 3;
12
```

未提交
在另一个命令行中执行另一个操作

```mysql-sql
update goods set name = 'Player' where id = 3;
1
```

此时会出现堵塞，因为前一个命令行未提交事务，即前一个在操作的时候，其他对该数据库的操作会被锁定，从而保证了数据的一致性。
（4）**持久性**（Durability）：
一旦事务提交，则其所做的修改会永久保存到数据库。
此时即使系统崩溃，修改的数据也不会丢失。

### 4.事务操作

表的引擎类型必须是innodb类型才可以使用事务，这是mysql表的默认引擎。
**开启事务**：
语法：`start transation;`或者`begin;`
开启事务后执行修改命令，变更会维护到本地缓存中，而不维护到物理表中。
**提交事务**：
语法：`commit;`
将缓存中的数据变更维护到物理表中。
**回滚事务**：
语法：`rollback;`
放弃缓存中变更的数据。
**练习**：
现在操作在一个事务中1给2转100，同时另一个事务中1给3转100.
数据如下

```mysql-sql
+----+-----+
| id | num |
+----+-----+
|  1 | 100 |
|  2 | 200 |
|  3 |   0 |
+----+-----+
1234567
```

操作过程如图
![事务操作](https://img-blog.csdnimg.cn/20191210221056134.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
事务1提交后，事务2对id1的更改会报错，因此需要回滚，从而保证了数据的一致性。
修改数据的命令会自动的触发事务，包括insert、update、delete；
如果对数据库的操作涉及多次数据的修改，并且成功一起成功，否则一起会滚到之前的数据，需要在SQL语句中有手动开启事务。

## 二、索引

### 1.引入

在图书馆中是如何找到一本书的？
一般的应用系统对比数据库的读写比例在**10:1**左右(即有10次查询操作时有1次写的操作)，而且插入操作和更新操作很少出现性能问题，遇到最多、最容易出问题还是一些复杂的查询操作，所以查询语句的优化显然是重中之重。
当数据库中数据量很大时，查找数据会变得很慢，所以引入索引进行优化。

### 2.定义

索引是一种**特殊的文件**(InnoDB数据表上的索引是表空间的一个组成部分)，它们包含着对数据表里所有记录的引用指针。
通俗地说，数据库索引就像一本书前面的目录，它们包含着对数据表里所有记录的引用指针。
**索引的目的**：
索引的目的在于提高查询效率，可以类比字典，如果要查“mysql”这个单词，我们肯定需要定位到m字母，然后从下往下找到y字母，再找到剩下的sql。
**索引原理**：
生活中随处可见索引的例子，如词典、火车站的车次表、图书的目录等。它们的原理都是一样的，通过不断的缩小想要获得数据的范围来筛选出最终想要的结果，同时把随机的事件变成顺序的事件，也就是我们总是通过同一种查找方式来锁定数据。
数据库也是一样，但显然要复杂许多，因为不仅面临着等值查询，还有范围查询(>、<、between、in)、模糊查询(like)、并集查询(or)等等。
如下图
![索引原理](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vN2UvZjgvN2VmOGViNWUxNjJlMTg4MDIzZjYwMmQyODY2NDBiOTVfNjI0eDMwMC5qcGc#pic_center?x-oss-process=image/format,png)
索引使用的数据结构一般是**B树**。
索引分为：

- 单值索引：单个字段；
- 复合索引：多个字段；
- 唯一索引：列的值必须唯一，应用于身份证号、电话号等。

### 3.索引的使用

查看索引：
语法：`show index from 表名;`

```mysql-sql
show index from money;
1
```

打印

```mysql-sql
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| money |          0 | PRIMARY  |            1 | id          | A         |           3 |     NULL | NULL   |      | BTREE      |         |               |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
1 row in set (0.01 sec)
123456
```

主键会自动创建索引；
创建索引：
语法：`create index 字段名称 on 表名(字段名称(长度));`

```mysql-sql
create index idx_num on money(num);
1
```

再查看索引

```mysql-sql
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| money |          0 | PRIMARY  |            1 | id          | A         |           3 |     NULL | NULL   |      | BTREE      |         |               |
| money |          1 | idx_num  |            1 | num         | A         |           2 |     NULL | NULL   |      | BTREE      |         |               |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
2 rows in set (0.00 sec)
1234567
```

如果指定字段是字符串，需要指定长度，建议长度与定义字段时的长度一致；
字段类型如果不是字符串，可以不填写长度部分。
创建唯一索引：

```mysql-sql
create unique index idx_num on money(num);
1
```

删除索引：
语法：`drop index 索引名称 on 表名;`

### 4.索引练习

创建测试表test：

```mysql-sql
create table test(title varchar(10));
1
```

使用python程序向表中加入十万条数据:

```python
from pymysql import *
def main():
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo',
        user='root',
        passwd='root',
        charset='utf8')
    cur = conn.cursor()
    for i in range(100000):
        cur.execute('insert into test values("ts-%d")' % i)
    conn.commit()

if __name__ == '__main__':
    main()
12345678910111213141516
```

开启运行时间监测：

```mysql-sql
set profiling=1;
1
```

查找第100000条数据：

```mysql-sql
select * from test where title='ts-99999';
1
```

打印

```mysql-sql
+----------+
| title    |
+----------+
| ts-99999 |
+----------+
1 row in set (0.04 sec)
123456
```

查看执行的时间：

```mysql-sql
show profiles;
1
```

打印

```mysql-sql
+----------+------------+-------------------------------------------+
| Query_ID | Duration   | Query                                     |
+----------+------------+-------------------------------------------+
|        1 | 0.03794450 | select * from test where title='ts-99999' |
+----------+------------+-------------------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

为表title_index的title列创建索引：

```mysql-sql
create index title_index on test(title(10));
1
```

再次执行查询语句：

```mysql-sql
select * from test where title='ts-99999';
1
```

打印

```mysql-sql
+----------+
| title    |
+----------+
| ts-99999 |
+----------+
1 row in set (0.00 sec)
123456
```

再次查看执行的时间:

```mysql-sql
show profiles;
1
```

打印

```mysql-sql
+----------+------------+---------------------------------------------+
| Query_ID | Duration   | Query                                       |
+----------+------------+---------------------------------------------+
|        1 | 0.03794450 | select * from test where title='ts-99999'   |
|        2 | 0.26895350 | create index title_index on test(title(10)) |
|        3 | 0.00031875 | select * from test where title='ts-99999'   |
+----------+------------+---------------------------------------------+
3 rows in set, 1 warning (0.00 sec)
12345678
```

显然，建立了索引后查询速度有了很大提升，即建立索引可以大幅提高查询效率。
适合建立索引的情况：

- 主键自动建立索引；
- 频繁作为查询条件的字段应该建立索引；
- 查询中与其他表关联的字段,外键关系建立索引；
- 在高并发的情况下创建复合索引；
- 查询中排序的字段,排序字段若通过索引去访问将大大提高排序速度 (建立索引的顺序跟排序的顺序保持一致)。

不适合建立索引的情况：

- 频繁更新的字段不适合建立索引；
- where条件里面用不到的字段不创建索引；
- 表记录太少,当表中数据量超过三百万条数据,可以考虑建立索引；
- 数据重复且平均的表字段,比如性别,国籍。

## 三、账户管理

在生产环境下操作数据库时，绝对不可以使用root账户连接，而是创建特定的账户，授予这个账户特定的操作权限，然后连接进行操作，主要的操作就是数据的**crud**。
MySQL账户体系：
根据账户所具有的权限的不同，MySQL的账户可以分为以下几种：

- 服务实例级账号：启动了一个mysqld，即为一个数据库实例；如果某用户如root,拥有服务实例级分配的权限，那么该账号就可以删除所有的数据库、连同这些库中的表；
- 数据库级别账号：对特定数据库执行增删改查的所有操作；
- 数据表级别账号：对特定表执行增删改查等所有操作；
- 字段级别的权限：对某些表的特定字段进行操作；
- 存储程序级别的账号：对存储程序进行增删改查的操作。
  账户的操作主要包括创建账户、删除账户、修改密码、授权权限等。

### 授予权限：

需要使用实例级账户登录后操作，以root为例
主要操作包括：

#### （1）查看所有用户：

所有用户及权限信息存储在mysql数据库的user表中；
查看user表的结构：

```mysql-sql
use mysql;
desc user;
12
```

打印

```mysql-sql
+------------------------+-----------------------------------+------+-----+-----------------------+-------+   
| Field                  | Type                              | Null | Key | Default               | Extra |   
+------------------------+-----------------------------------+------+-----+-----------------------+-------+   
| Host                   | char(60)                          | NO   | PRI |                       |       |   
| User                   | char(32)                          | NO   | PRI |                       |       |   
| Select_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Insert_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Update_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Delete_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Drop_priv              | enum('N','Y')                     | NO   |     | N                     |       |   
| Reload_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Shutdown_priv          | enum('N','Y')                     | NO   |     | N                     |       |   
| Process_priv           | enum('N','Y')                     | NO   |     | N                     |       |   
| File_priv              | enum('N','Y')                     | NO   |     | N                     |       |   
| Grant_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| References_priv        | enum('N','Y')                     | NO   |     | N                     |       |   
| Index_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| Alter_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| Show_db_priv           | enum('N','Y')                     | NO   |     | N                     |       |   
| Super_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_tmp_table_priv  | enum('N','Y')                     | NO   |     | N                     |       |   
| Lock_tables_priv       | enum('N','Y')                     | NO   |     | N                     |       |   
| Execute_priv           | enum('N','Y')                     | NO   |     | N                     |       |   
| Repl_slave_priv        | enum('N','Y')                     | NO   |     | N                     |       |   
| Repl_client_priv       | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_view_priv       | enum('N','Y')                     | NO   |     | N                     |       |   
| Show_view_priv         | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_routine_priv    | enum('N','Y')                     | NO   |     | N                     |       |   
| Alter_routine_priv     | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_user_priv       | enum('N','Y')                     | NO   |     | N                     |       |   
| Event_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| Trigger_priv           | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_tablespace_priv | enum('N','Y')                     | NO   |     | N                     |       |   
| ssl_type               | enum('','ANY','X509','SPECIFIED') | NO   |     |                       |       |   
| ssl_cipher             | blob                              | NO   |     | NULL                  |       |   
| x509_issuer            | blob                              | NO   |     | NULL                  |       |   
| x509_subject           | blob                              | NO   |     | NULL                  |       |   
| max_questions          | int(11) unsigned                  | NO   |     | 0                     |       |   
| max_updates            | int(11) unsigned                  | NO   |     | 0                     |       |   
| max_connections        | int(11) unsigned                  | NO   |     | 0                     |       |   
| max_user_connections   | int(11) unsigned                  | NO   |     | 0                     |       |   
| plugin                 | char(64)                          | NO   |     | mysql_native_password |       |   
| authentication_string  | text                              | YES  |     | NULL                  |       |   
| password_expired       | enum('N','Y')                     | NO   |     | N                     |       |   
| password_last_changed  | timestamp                         | YES  |     | NULL                  |       |   
| password_lifetime      | smallint(5) unsigned              | YES  |     | NULL                  |       |   
| account_locked         | enum('N','Y')                     | NO   |     | N                     |       |   
+------------------------+-----------------------------------+------+-----+-----------------------+-------+   
45 rows in set (0.00 sec)                                                                                     
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

主要字段说明：
Host表示允许访问的主机；
User表示用户名；
authentication_string表示密码，为加密后的值。
查看所有用户：

```mysql-sql
select host,user,authentication_string from user;
1
+-----------+---------------+-------------------------------------------+
| host      | user          | authentication_string                     |
+-----------+---------------+-------------------------------------------+
| localhost | root          | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |
| localhost | mysql.session | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| localhost | mysql.sys     | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| 127.0.0.1 | Tom           | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
| localhost | Tom2          | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
+-----------+---------------+-------------------------------------------+
5 rows in set (0.00 sec)
12345678910
```

#### （2）创建账户、授权

需要使用实例级账户登录后操作，以root为例
常用权限主要包括：create、alter、drop、insert、update、delete、select
如果分配所有权限，可以使用all privileges
语法：
`grant 权限列表 on 数据库 to '用户名'@'访问主机' identified by '密码';`

```mysql-sql
grant select on jingdong.* to 'Leo'@'localhost' identified by '123456';
--创建一个Mary的账号，密码为12345678，可以任意电脑进行链接访问, 并且对jd数据库中的所有表拥有所有权限
grant all privileges on jd.* to "Mary"@"%" identified by "12345678"
--刷新权限
flush privileges;
12345
```

说明：
可以操作数据库的所有表，方式为:jingdong.*；
访问主机通常使用 百分号% 表示此账户可以使用任何ip的主机登录访问此数据库；
访问主机可以设置成 localhost或具体的ip，表示只允许本机或特定主机访问。
查看用户有哪些权限：

```mysql-sql
show grants for Leo@localhost;
1
```

打印

```mysql-sql
+---------------------------------------------------+
| Grants for Leo@localhost                          |
+---------------------------------------------------+
| GRANT USAGE ON *.* TO 'Leo'@'localhost'           |
| GRANT SELECT ON `jingdong`.* TO 'Leo'@'localhost' |
+---------------------------------------------------+
2 rows in set (0.00 sec)                           
1234567
```

使用新创建的用户登录：

```mysql-sql
mysql -uLeo -p
1
```

查看数据库：

```mysql-sql
show databases;
1
```

打印

```mysql-sql
+--------------------+
| Database           |
+--------------------+
| information_schema |
| jingdong           |
+--------------------+
2 rows in set (0.00 sec)
1234567
```

进行删除操作

```mysql-sql
use jingdong;
drop table goods;
12
```

打印

```mysql-sql
ERROR 1142 (42000): DROP command denied to user 'Leo'@'localhost' for table 'goods'
1
```

报错，命令拒绝，因为只给Leo了查看权限，所以不能进行增加、修改、删除操作。

## 四、数据库存储引擎简介

### 1.MySQL体系结构：

- 连接层
- 服务层（核心）
- 存储引擎层
- 文件系统

如图：
![MySQL体系结构](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vNDAvMDIvNDAwMjgyZTUzMTU1ODQ0NTM2ZWIyYzAyNjc2MGZkZGZfMTEwMng3MTcucG5n?x-oss-process=image/format,png)
**服务层**：
服务层是MySQL的核心，MySQL的核心服务层都在这一层，查询解析、SQL执行计划分析、SQL执行计划优化、查询缓存、以及跨存储引擎的功能都在这一层实现：存储过程，触发器，视图等。
下图是服务层的内部结构：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vMmMvNzEvMmM3MWI1OTZkMDRkMjliOTlkNjYxMjg1NTNhNDNkNmNfMTA5Nng2MzUucG5n?x-oss-process=image/format,png)
**存储引擎层**：
负责MySQL中数据的存储与提取。
服务器中的查询执行引擎通过API与存储引擎进行通信，通过接口屏蔽了不同存储引擎之间的差异。
MySQL采用插件式的存储引擎。MySQL为我们提供了许多存储引擎，每种存储引擎有不同的特点。我们可以根据不同的业务特点，选择最适合的存储引擎。
如果对于存储引擎的性能不满意，可以通过修改源码来得到自己想要达到的性能。例如阿里巴巴的X-Engine，为了满足企业的需求facebook与google都对InnoDB存储引擎进行了扩充。

### 2.存储引擎操作

查看存储引擎：

```mysql-sql
show engines;
1
```

打印

```mysql-sql
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.01 sec)                                                                                            
1234567891011121314
```

查看默认的存储引擎：

```mysql-sql
show variables like '%storage_engines%';
1
```

打印

```mysql-sql
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| disabled_storage_engines |       |
+--------------------------+-------+
1 row in set, 1 warning (0.00 sec)
```

# Python全栈（三）数据库优化之8.MySQL高级-存储引擎和基准测试

## 一、MySQL引擎之MyISAM

面对不同的需求，选择不同的存储引擎。

```mysql-sql
create table enginetest(id int) engine='myisam';
1
```

### 1.定义

MyISAM是MySQL5.5之前版本的默认存储引擎。
MyISAM存储引擎表由MYD（数据文件）和MYI（索引文件）组成。

### 2.特性

锁的作用：
管理共享资源的并发访问；
实现事务的隔离性。
锁的类型：

- 共享锁（读锁）：
  针对同一份数据,多个读操作可以同时进行而不会互相影响。
- 独占锁（写锁）：
  当前写操作没有完成前,它会阻断其他写锁和读锁。

锁的粒度：

- 表级锁
- 行级锁

MyISAM存储引擎的特性：
（1）并发性与锁级别：
使用表级锁，并发性较低；
（2）表损坏修复：
check语句检查表的状态，repair语句来修复表的状态。

```mysql-sql
check table test;
1
```

打印

```mysql-sql
+-----------+-------+----------+----------+
| Table     | Op    | Msg_type | Msg_text |
+-----------+-------+----------+----------+
| demo.test | check | status   | OK       |
+-----------+-------+----------+----------+
1 row in set (0.20 sec)
123456
repair table test;
1
```

打印

```mysql-sql
+-----------+--------+----------+----------+
| Table     | Op     | Msg_type | Msg_text |
+-----------+--------+----------+----------+
| demo.test | repair | status   | OK       |
+-----------+--------+----------+----------+
1 row in set (0.31 sec)
123456
```

（3）MyISAM表支持数据压缩：
命令：`myisampack -b -f myIsam.MYI`
为Linux命令，Windows不支持。
存储引擎限制：
版本 < MySQL5.0时默认表大小为4G,如存储大表则要修改MAX_Rows和AVG_ROW_LENGTH;
版本 > MySQL5.0时默认支持256TB。

### 3.适用场景：

（1）非事务型应用：
不支持事务场景适合。
（2）只读类应用：
读取较快，适合于只读应用。

## 二、MySQL引擎之InnoDB

### 1.定义

InnoDB是MySQL5.5之后版本的默认存储引擎。
InnoDB支持事务的ACID特性。
InnoDB使用表空间进行数据存储：
`innodb_file_per_table`的值表示存储的表空间类型：
on：独立表空间，tablename.ibd，是默认的；
off：系统表空间，ibdaraX（X是一个数字）。
通过以下语句查看数据库的表空间类型：

```mysql-sql
show variables like 'innodb_file_per_table';
1
```

打印

```mysql-sql
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_file_per_table | ON    |
+-----------------------+-------+
1 row in set, 1 warning (0.01 sec)
123456
```

通过以下语句修改类型：

```mysql-sql
set global innodb_file_per_table=off;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
1
```

命令行修改，只是临时修改，不会永久性改变，重启数据库会恢复默认；
配置文件修改，会永久修改。
系统表空间和独立表空间的选择：
系统表空间会产生IO瓶颈，刷新数据的时候是顺序进行的，因此会产生文件的IO瓶颈；
独立表空间可以同时向多个文件刷新数据，一般情况下会选择独立表空间。

### 2.InnoDB存储引擎的特性：

（1）支持事务的ACID特性；
（2）支持行级锁，可以最大程度地支持并发。

MyISAM和InnoDB的对比：
如图
![对比](https://img-blog.csdnimg.cn/20191215201409511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 三、MySQL引擎之CSV

### 1.文件系统存储特点：

数据为以文本方式存储在文件中；
.csv文件存储表内容；
.csm文件存储表的元数据如表状态和数据量；
.frm文件存储数据表结构信息。

### 2.特点：

（1）以csv格式进行数据存储：
可以通过Excel或其他对csv文件编辑的方式对数据进行编辑，编辑后在命令行中通过`flush tables;`命令进行刷新数据再查看，但是一般不建议通过这种方式编辑数据，因为在其他方式（如命令行、可视化界面）编辑等同时存在时会产生同步的延迟和数据错误等问题。
（2）不支持自增。
（3）不支持索引。
（4）不支持空字段。

### 3.使用场景：

适合作为数据交换的中间表。

## 四、MySQL引擎之Memory

### 1.定义

也称HEAP存储引擎，数据保存在内存中，如果MySQL服务重启数据会丢失，数据表结构会保存下来。

### 2.功能特点

（1）支持HASH索引和BTREE索引：
hash索引一般用于等值查找，btree一般用于范围查找。
Memory支持两种索引，其他引擎只支持btree，所以可为Memory数据表指定索引类型。
查看表索引：

```mysql-sql
show index from test_memory;
1
```

打印

```mysql-sql
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table       | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| test_memory |          0 | PRIMARY  |            1 | id          | NULL      |           0 |     NULL | NULL   |      | HASH       |         |               |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
1 row in set (0.01 sec)
123456
```

指定新的索引类型：

```mysql-sql
create index idx_name using btree on test_memory(name(31));
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

再次查看索引

```mysql-sql
show index from test_memory;
1
```

打印

```mysql-sql
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table       | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| test_memory |          0 | PRIMARY  |            1 | id          | NULL      |           0 |     NULL | NULL   |      | HASH       |         |               |
| test_memory |          1 | idx_name |            1 | name        | A         |        NULL |     NULL | NULL   | YES  | BTREE      |         |               |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
2 rows in set (0.00 sec)
1234567
```

（2）所有字段都为固定长度varchar(10)=char(10)；
（3）不支持BLOB和TEXT等大字段；
（4）Memory存储引擎使用表级锁。

### 3.选择存储引擎

除非需要使用到某些InnoDB不具备的特性，并且没有其他办法可以代替，否则都应该优先选择InnoDB引擎，并且一般情况下**不要混用**。
应用举例：

- 事务：
  如果应用需要事务支持，那么InnoDB(或者XtraDB)是目前最稳定并且经过验证的选择；
- 备份：
  如果可以定期地关闭服务器来执行备份，那么备份的因素可以忽略；反之，如果需要在线热备份，那么选择InnoDB就是基本的要求。
- 崩溃恢复：
  MyISAM崩溃后发生损坏的概率比InnoDB要高很多，而且恢复速度也要慢。

### 4.应用举例：

日志型应用：
MyISAM，因为大部分只读和插入，速度较快。
只读或大部分情况只读的表：
MyISAM，读取较快。
订单处理：
InnoDB，涉及到事务和资金处理。

## 五、基准测试

### 1.定义

基准测试是一种测量和评估软件性能指标的活动，用于建立某个时刻的性能基准,以便当系统发生软硬件变化时重新进行基准测试以评估变化对性能的影响。
基准测试是针对系统设置的一种压力测试。

### 2.基准测试特点

基准测试特点：
直接、简单、易于比较,用于评估服务器的处理能力；
可能不关心业务逻辑,所使用的查询和业务的真实性可以和业务环境没关系。
压力测试特点：
对真实的业务数据进行测试,获得真实系统所能承受的压力；
需要针对不同主题,所使用的数据和查询也是真实用到的。
基准测试是**简化的压力测试**。

### 3.基准测试的目的

建立MySQL服务器的性能基准线,确定当前MySQL服务器运行情况,确定优化之后的效果；
模拟比当前系统更高的负载,已找出系统的扩展瓶颈,可以增加数据库并发,观察QPS(每秒处理的查询数),TPS(每秒处理的事务数)变化,确定并发量与性能最优的关系；
测试不同的硬件、软件和操作系统配置；
证明新的硬件设备是否配置正确。

### 4.如何进行基准测试

对整个系统进行基准测试：
优点：

- 能够测试整个系统的性能,包括web服务器缓存、数据库等；
- MySQL并不总是出现性能问题的瓶颈,如果只关注MySQL可能忽略其他问题,能反映出系统中各个组件接口间的性能问题，体现真实性能状况。

缺点：

- 测试设计复杂,消耗时间长。

对MySQL进行基准测试：
优点：

- 测试设计简单,所耗费时间短；

缺点：

- 无法全面了解整个系统的性能基线。

### 5.MySQL基准测试的常见指标

（1）TPS：
单位时间内处理的事务数
MySQL中查询语句为

```mysql-sql
--提交事务速率查询
show global status like 'Com_commit';
12
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Com_commit    | 0     |
+---------------+-------+
1 row in set (0.01 sec)
123456
--回滚事务速率查询
show global status like 'Com_rollback';
12
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Com_rollback  | 0     |
+---------------+-------+
1 row in set (0.00 sec)
123456
```

（2）QPS：
单位时间内处理的查询数
MySQL中查询语句为

```mysql-sql
show global status like 'Question%';
1
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Questions     | 436   |
+---------------+-------+
1 row in set (0.00 sec)  
123456
```

### 6.MySQL基准测试工具

#### mysqlslap：

MySQL5.5以后自带，可以模拟服务器负载,并输出相关统计信息。
常用参数说明：

- auto-generate-sql 由系统自动生成SQL脚本进行测试
- auto-generate-sql-add-autoincrement 在生成的表中增加自增ID
- auto-generate-sql-load-type 指定测试中使用的查询类型 读写或者混合,默认是混合
- auto-generate-sql-write-number 指定初始化数据时生成的数据量
- concurrency 指定并发线程的数量 1,10,50,200
- engine 指定要测试表的存储引擎,可以用逗号分割多个存储引擎
- no-drop 指定不清理测试数据
- iterations 指定测试运行的次数 指定了这个不能指定no-drop
- number-of-queries 指定每一个线程执行的查询数量
- debug-info 指定输出额外的内存及CPU统计信息
- number-int-cols 指定测试表中包含的INT类型列的数量
- number-char-cols 指定测试表中包含的varchar类型的数量
- create-schema 指定了用于执行测试的数据库的名字
- query 用于指定自定义SQL的脚本
- only-print 并不运行测试脚本,而是把生成的脚本打印出来
  测试举例：
  `mysqlslap --concurrency=1,50,100,200 --iterations=3 --number-int-cols=5 --number-char-cols=5 --auto-generate-sql --aut o-generate-sql-add-autoincrement --engine=myisam,innodb --number-of-queries=10 --create-schema=test –uroot -proot | more`
  结果为

```bash
Benchmark
        Running for engine myisam
        Average number of seconds to run all queries: 0.000 seconds
        Minimum number of seconds to run all queries: 0.000 seconds
        Maximum number of seconds to run all queries: 0.000 seconds
        Number of clients running queries: 1
        Average number of queries per client: 10

Benchmark
        Running for engine myisam
        Average number of seconds to run all queries: 0.036 seconds
        Minimum number of seconds to run all queries: 0.031 seconds
        Maximum number of seconds to run all queries: 0.047 seconds
        Number of clients running queries: 50
        Average number of queries per client: 0

Benchmark
        Running for engine myisam
        Average number of seconds to run all queries: 0.056 seconds
        Minimum number of seconds to run all queries: 0.046 seconds
        Maximum number of seconds to run all queries: 0.062 seconds
        Number of clients running queries: 100
        Average number of queries per client: 0

Benchmark
        Running for engine myisam
        Average number of seconds to run all queries: 0.140 seconds
        Minimum number of seconds to run all queries: 0.109 seconds
        Maximum number of seconds to run all queries: 0.187 seconds
        Number of clients running queries: 200
        Average number of queries per client: 0

Benchmark
        Running for engine innodb
        Average number of seconds to run all queries: 0.010 seconds
        Minimum number of seconds to run all queries: 0.000 seconds
        Maximum number of seconds to run all queries: 0.016 seconds
        Number of clients running queries: 1
        Average number of queries per client: 10

Benchmark
        Running for engine innodb
        Average number of seconds to run all queries: 0.401 seconds
        Minimum number of seconds to run all queries: 0.063 seconds
        Maximum number of seconds to run all queries: 1.063 seconds
        Number of clients running queries: 50
        Average number of queries per client: 0

Benchmark
        Running for engine innodb
        Average number of seconds to run all queries: 0.458 seconds
        Minimum number of seconds to run all queries: 0.125 seconds
        Maximum number of seconds to run all queries: 1.125 seconds
        Number of clients running queries: 100
        Average number of queries per client: 0

Benchmark
        Running for engine innodb
        Average number of seconds to run all queries: 0.313 seconds
        Minimum number of seconds to run all queries: 0.313 seconds
        Maximum number of seconds to run all queries: 0.313 seconds
        Number of clients running queries: 200
        Average number of queries per client: 0
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263
```

#### sysbench:

可参考[https://www.cnblogs.com/kismetv/archive/2017/09/30/7615738.htm](https://www.cnblogs.com/kismetv/archive/2017/09/30/7615738.html)

# Python全栈（三）数据库优化之9.MySQL高级-explain分析SQL语句

## 一、影响服务器性能的几个方面

### 1.硬件条件原因

- 服务器硬件：
  内存大小等。
- 服务器的操作系统：
  不同系统（Linux、Windows）。
- 数据库存储引擎的选择：
  根据不同的需求选择不同的存储引擎。
- 数据库参数配置：
  不同的参数配置会影响到性能。
- 数据库结构设计和SQL语句：
  拆表后，小表的查询效率更快。

### 2.SQL本身性能下降原因

- 查询语句写的不好：
  语句太长，有太多连接。
- 索引失效：
  建立的索引未用上。
- 关联查询太多join：
  有太多连接，降低性能。
- 服务器调优及各个参数设置。

### 3.SQL语句加载顺序

手写SQL代码的顺序：

```mysql-sql
select distinct 
    <select _list>
from 
    <left_table>
join  <right_table> on <join_codition>
where
    <where_condition>
group by
    <group_by_list>
having
    <having_condition>
order by
    <order_by_condition>
limit <limit number>
1234567891011121314
```

机器读取执行代码的顺序：
![机器执行顺序](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vY2IvOWUvY2I5ZTgxM2I1ZTRmYjUyMzNlYzUwMWJiNDJjNTdmMmRfNDAweDMzMC5wbmc#pic_center?x-oss-process=image/format,png)
如图
![SQL解析](https://img-blog.csdnimg.cn/20191216164812562.png)

### 4.MySQL常见瓶颈

- CPU:
  CPU在饱和的时候一般发生在数据装入内存或从磁盘读取数据的时候；
- IO:
  磁盘I/O瓶颈发生在装入数据远大于内存容量的时候；
- 服务器硬件的性能瓶颈：
  内存、硬盘大小等。

## 二、explain分析SQL

### 1.基本定义

使用explain关键字可以**模拟优化器**执行SQL查询语句,从而知道MySQL是如何处理SQL语句的。

### 2.explain的作用：

- 表的读取顺序；
- 数据读取操作的操作类型；
- 哪些索引可以使用；
- 哪些索引被实际使用；
- 表之间的引用；
- 每张表有多少行被优化器查询。

### 3.explain的使用

语法：

```mysql-sql
explain + SQL语句;
1
```

例如

```mysql-sql
explain select * from students;
1
```

打印

```mysql-sql
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table    | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | students | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   27 |   100.00 | NULL  |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
123456
```

### 4.explain字段解释

#### id：

表的读取顺序：
select查询的顺序号,包含一组数字,表示查询中执行select子句或操作表的顺序。
两种情况:
（1）id相同,执行顺序由上至下：
例如：

```mysql-sql
explain select test2.* from test1,test2,test3 where test1.id = test2.id and test1.id = test3.id;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
| id | select_type | table | partitions | type   | possible_keys | key     | key_len | ref           | rows | filtered | Extra       |
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
|  1 | SIMPLE      | test1 | NULL       | index  | PRIMARY       | PRIMARY | 4       | NULL          |    1 |   100.00 | Using index |
|  1 | SIMPLE      | test2 | NULL       | eq_ref | PRIMARY       | PRIMARY | 4       | demo.test1.id |    1 |   100.00 | Using index |
|  1 | SIMPLE      | test3 | NULL       | eq_ref | PRIMARY       | PRIMARY | 4       | demo.test1.id |    1 |   100.00 | Using index |
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
3 rows in set, 1 warning (0.00 sec)                                                                                                   
12345678
```

（2）id不同,如果是子查询,id的序号会递增,id值越大优先级越高,越先被执行：
例如：

```mysql-sql
 explain select test2.* from test2 where id = (select id from test1 where id = (select test3.id from test3 where test3.other_columns = ''));
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
|  1 | PRIMARY     | test2 | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | Using index |
|  2 | SUBQUERY    | test1 | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | Using index |
|  3 | SUBQUERY    | test3 | NULL       | ALL   | NULL          | NULL    | NULL    | NULL  |    1 |   100.00 | Using where |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
3 rows in set, 1 warning (0.00 sec)                                                                                                  
12345678
```

显然，根据id的大小，表读取的顺序依次是test3、test1、test2，先执行子查询，再执行父查询，从底层到上层依次执行。
（3）id相同不同：
id时同时执行，不同时id越大越先被执行。

#### select_type：

数据读取操作的操作类型：
（1）simple：
简单的select查询,查询中不包含子查询或者union；
（2）primary：
查询中若包含任何复杂的子部分,最外层查询则被标记，就是最后加载的那一层查询；
（3）subquery：
在select或where列表中包含了子查询；
（4）union：
union是把两个表的查询结果连接起来。
若第二个select出现在union之后,则被标记为union，

```mysql-sql
explain select * from test2 union select * from test4;
1
```

打印

```mysql-sql
+----+--------------+------------+------------+-------+---------------+---------+---------+------+------+----------+-----------------+ 
| id | select_type  | table      | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra           | 
+----+--------------+------------+------------+-------+---------------+---------+---------+------+------+----------+-----------------+ 
|  1 | PRIMARY      | test2      | NULL       | index | NULL          | PRIMARY | 4       | NULL |    1 |   100.00 | Using index     | 
|  2 | UNION        | test4      | NULL       | index | NULL          | PRIMARY | 4       | NULL |    1 |   100.00 | Using index     | 
| NULL | UNION RESULT | <union1,2> | NULL       | ALL   | NULL          | NULL    | NULL    | NULL | NULL |     NULL | Using temporary 
+----+--------------+------------+------------+-------+---------------+---------+---------+------+------+----------+-----------------+ 
3 rows in set, 1 warning (0.01 sec)                                                                                                                                                                                                   
12345678
```

只有两张表的**字段个数相同**才能使用。
（5）union result：
从union表获取结果的select。

#### table：

显示这一行的数据是关于哪张表的。

#### partitions：

查询访问的分区。

#### type：

显示SQL的执行效率，包括ALL，index，range，ref，eq_ref，const、system，NULL等。
从最好到最差依次是：
system > const > eq_ref > ref > range > index > ALL。
（1）system：
表只有一行记录(等于系统表),这是const类型的特例，意义不大。
例如

```mysql-sql
explain select * from mysql.db;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+---------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type    | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+---------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | db    | NULL       | system  | NULL          | NULL | NULL    | NULL |    6 |   100.00 | NULL  |
+----+-------------+-------+------------+---------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                                 
123456
```

（2）const：
表示通过索引一次就找到了,const用于比较primary key，使用较少。
例如

```mysql-sql
explain select * from test1 where id = 1;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | test1 | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                
123456
```

（3）eq_ref：
唯一索引扫描,对于每个索引键,表中只有一条记录与之匹配
例如

```mysql-sql
explain select * from test1,test2 where test1.id = test2.id;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
| id | select_type | table | partitions | type   | possible_keys | key     | key_len | ref           | rows | filtered | Extra       |
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
|  1 | SIMPLE      | test1 | NULL       | ALL    | PRIMARY       | NULL    | NULL    | NULL          |    1 |   100.00 | NULL        |
|  1 | SIMPLE      | test2 | NULL       | eq_ref | PRIMARY       | PRIMARY | 4       | demo.test1.id |    1 |   100.00 | Using index |
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
2 rows in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                              
1234567
```

（4）ref：
非唯一性索引扫描,返回匹配某个单独值的所有行,本质上也是一种索引访问,它返回所有匹配某个单独值的行
例如

```mysql-sql
create index idx_col1_col2 on test3(col1,col2);
explain select * from test3 where col1 = 'test';
12
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key           | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | test3 | NULL       | ref  | idx_col1_col2 | idx_col1_col2 | 93      | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                             
123456
```

（5）range：
只检索给定范围的行,使用一个索引来选择行，where语句中包含between…and…、in…条件时即为range，一般where语句中的字段是主键时才可能出现range。
例如

```mysql-sql
explain select * from test1 where id between 1 and 10;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | test1 | NULL       | range | PRIMARY       | PRIMARY | 4       | NULL |    1 |   100.00 | Using where |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                            
123456
```

（6）index：
Full Index Scan,index与ALL区别为index类型只遍历索引树。
例如

```mysql-sql
explain select id from test1;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+ 
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       | 
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+ 
|  1 | SIMPLE      | test1 | NULL       | index | NULL          | PRIMARY | 4       | NULL |    1 |   100.00 | Using index | 
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+ 
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                                                                                                    
123456
```

（7）ALL：
遍历全表找到匹配的行。
例如

```mysql-sql
explain select * from test1;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | test1 | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                                                                                                                  
123456
```

#### possible_keys：

显示可能应用在这张表中的索引,一个或多个。

#### key：

实际使用的索引，如果为null,则没有使用索引。
例如

```mysql-sql
explain select col1,col2 from test3;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key           | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | test3 | NULL       | index | NULL          | idx_col1_col2 | 186     | NULL |    1 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
123456
```

覆盖索引，查询列要被所建的索引覆盖，上例中查询的列col1、col2即被创建的索引idx_col1_col2覆盖。
查询中若使用了覆盖索引,则该索引仅出现在key列表中。

#### key_len：

表示索引中使用的字节数,可通过该列计算查询中使用的索引长度。
在不损失精确性的情况下,长度越短越好。
key_len显示的值为索引字段的最大可能长度,**并非实际使用长度**,即key_len是根据表定义计算而得,不是通过表内检索出的。
索引长度计算：

```bash
varchr(n)变长字段且允许NULL：
n*(Character Set：utf8=3,gbk=2,latin1=1)+1(NULL)+2(变长字段)；
varchr(n)变长字段且不允许NULL：
n*(Character Set：utf8=3,gbk=2,latin1=1)+2(变长字段)；
char(n)固定字段且允许NULL：
n*(Character Set：utf8=3,gbk=2,latin1=1)+1(NULL)；
char(n)固定字段且不允许NULL：
n*(Character Set：utf8=3,gbk=2,latin1=1)。
12345678
```

上例中，字符集为utf8为3，允许非空1，可变长2，并且有2个字段，所以有
key_len=(30×3+1+2)×2=186。

#### ref：

显示索引哪一列被使用到了。
在id表的读取顺序部分示例的结果中，ref列的第2、3个值为demo.test1.id，表示demo数据库中的test1表中的id字段被用到。

#### rows：

根据表统计信息及索引选用情况,大致估算出找到所需的记录所需要读取的行数。
如图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vYmMvYTMvYmNhM2RlNWZkZTU1ZGYwMGJhOTY3ZDU4Y2M4MThlNDJfOTcyeDMwMS5wbmc#pic_center?x-oss-process=image/format,png)

#### Extra：

包含不适合在其他列中显示但十分重要的额外信息。
（1）Using filesort：
说明mysql会对数据使用一个外部的索引排序,而不是按照表内的索引顺序进行读取,MySQL中无法利用索引完成的排序操作称为"文件排序"，说明需要进行优化。
例如

```mysql-sql
drop index idx_col1_col2 from test3;
create index idx_col1_col2_col3 on test3(col1,col2,col3);
explain select * from test3 where col1 = 'test' order by col3;
123
```

打印

```mysql-sql
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+---------------------------------------+
| id | select_type | table | partitions | type | possible_keys      | key                | key_len | ref   | rows | filtered | Extra                                 |
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+---------------------------------------+
|  1 | SIMPLE      | test3 | NULL       | ref  | idx_col1_col2_col3 | idx_col1_col2_col3 | 93      | const |    1 |   100.00 | Using index condition; Using filesort |
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+---------------------------------------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
123456
```

显然，此时出现了filesort，需要进行优化。
例如

```mysql-sql
explain select * from test3 where col1 = 'test' order by col2,col3;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+-----------------------+
| id | select_type | table | partitions | type | possible_keys      | key                | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | test3 | NULL       | ref  | idx_col1_col2_col3 | idx_col1_col2_col3 | 93      | const |    1 |   100.00 | Using index condition |
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
123456
```

此时不再有filesort。
（2）Using temporary：
使用临时表保存中间结果,MySQL在对查询结果排序时使用临时表，一般出现在使用了order by、group by的时候，性能极差。
例如

```mysql-sql
explain select * from test3 where col1 in ('te','st') group by col2;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+--------------------+------+---------+------+------+----------+----------------------------------------------+
| id | select_type | table | partitions | type | possible_keys      | key  | key_len | ref  | rows | filtered | Extra                                        |
+----+-------------+-------+------------+------+--------------------+------+---------+------+------+----------+----------------------------------------------+
|  1 | SIMPLE      | test3 | NULL       | ALL  | idx_col1_col2_col3 | NULL | NULL    | NULL |    1 |   100.00 | Using where; Using temporary; Using filesort |
+----+-------------+-------+------------+------+--------------------+------+---------+------+------+----------+----------------------------------------------+                                                                                                                                                                                                                                                                                                                                                                                                                                                                
1 row in set, 1 warning (0.01 sec)
123456
```

显然，同时出现了filesort、temporary，需要优化。
例如

```mysql-sql
explain select * from test3 where col1 in ('te','st') group by col1,col2;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+--------------------+--------------------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys      | key                | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+--------------------+--------------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | test3 | NULL       | index | idx_col1_col2_col3 | idx_col1_col2_col3 | 279     | NULL |    1 |   100.00 | Using where |
+----+-------------+-------+------------+-------+--------------------+--------------------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                                                                                                                                                           
123456
```

此时filesort、temporary均消失，我们应该尽量按照**建立索引的顺序**进行分组、排序。
（3）Using index：
使用了索引,避免了全表扫描。
（4）Using where：
使用了where过滤。
（5）Using join buffer：
使用了连接缓存。
（6）impossible where：
不可能的条件,where子句的值总是false。
例如

```mysql-sql
explain select * from test3 where id = 1 and id = 2;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra            |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | Impossible WHERE |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
1 row in set, 1 warning (0.01 sec)                                                                                    
```

# Python全栈（三）数据库优化之10.MySQL高级-表优化和索引优化

## 一、单表优化

建表：

```mysql-sql
create table article(
    id int unsigned not null primary key auto_increment,
    author_id int unsigned not null,
    category_id int unsigned not null,
    views int unsigned not null,
    comments int unsigned not null,
    title varchar(255) not null,
    content text not null
);
123456789
```

打印

```mysql-sql
Query OK, 0 rows affected (0.05 sec)
1
```

插入数据：

```mysql-sql
insert into article(`author_id`,`category_id`,`views`,`comments`,`title`,`content`) values 
(1,1,1,1,'1','1'),
(2,2,2,2,'2','2'),
(1,1,3,3,'3','3');
1234
```

打印

```mysql-sql
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0
12
```

查询category_id为1且comments大于1的情况下,views最多的article_id：

```mysql-sql
select * from article where category_id = 1 and comments > 1 order by views desc limit 1;
1
```

打印

```mysql-sql
+----+-----------+-------------+-------+----------+-------+---------+
| id | author_id | category_id | views | comments | title | content |
+----+-----------+-------------+-------+----------+-------+---------+
|  3 |         1 |           1 |     3 |        3 | 3     | 3       |
+----+-----------+-------------+-------+----------+-------+---------+
1 row in set (0.00 sec)
123456
```

explain语句分析

```mysql-sql
explain select * from article where category_id = 1 and comments > 1 order by views desc limit 1;
1
```

打印

```mysql-sql
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | article | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where; Using filesort |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
1 row in set, 1 warning (0.01 sec)                                                                                                        
123456
```

type为all，全表扫描，且Using filesort，需要进行优化。
优化尝试：

```mysql-sql
--建立复合索引
create index idx_article_ccv on article(category_id,comments,views);
explain select * from article where category_id = 1 and comments > 1 order by views desc limit 1;
123
```

打印

```mysql-sql
+----+-------------+---------+------------+-------+-----------------+-----------------+---------+------+------+----------+---------------------------------------+
| id | select_type | table   | partitions | type  | possible_keys   | key             | key_len | ref  | rows | filtered | Extra                                 |
+----+-------------+---------+------------+-------+-----------------+-----------------+---------+------+------+----------+---------------------------------------+
|  1 | SIMPLE      | article | NULL       | range | idx_article_ccv | idx_article_ccv | 8       | NULL |    1 |   100.00 | Using index condition; Using filesort |
+----+-------------+---------+------------+-------+-----------------+-----------------+---------+------+------+----------+---------------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

显然，type变成了range并且出现了Using index condition，但是Using filesort仍然存在，未解决问题，改变条件语句进行尝试可知，views字段索引是多余的，所以在索引中去掉views，再次尝试。

```mysql-sql
drop index idx_article_ccv on article;
--重新建索引
create index idx_article_cv on article(category_id,views);
explain select * from article where category_id = 1 and comments > 1 order by views desc limit 1;
1234
```

打印

```mysql-sql
+----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys  | key            | key_len | ref   | rows | filtered | Extra       |
+----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+-------------+
|  1 | SIMPLE      | article | NULL       | ref  | idx_article_cv | idx_article_cv | 4       | const |    2 |    33.33 | Using where |
+----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                                    
123456
```

此时type为ref，且Using filesort未出现，达到优化效果。

## 二、双表优化

建表：

```mysql-sql
--商品类别
create table class(
    id int unsigned not null primary key auto_increment,
    card int unsigned not null
);

--图书表
create table book(
    bookid int unsigned not null auto_increment primary key,
    card int unsigned not null
);
1234567891011
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.01 sec)
123
```

插入随机数据：
MySQL中，rand()函数返回随机小数（0-1），floor()函数返回小数向下取整的结果。

```mysql-sql
--向class表插入数据
insert into class(card) values(floor(rand()*100));
--向book表插入数据
insert into book(card) values(floor(rand()*100));
1234
```

反复执行多次可以插入多条数据。
联合查询：

```mysql-sql
select * from class left join book on class.card = book.card;
1
```

打印

```mysql-sql
+----+------+--------+------+
| id | card | bookid | card |
+----+------+--------+------+
| 10 |    6 |      9 |    6 |
| 12 |   13 |     10 |   13 |
|  1 |   12 |   NULL | NULL |
|  2 |   43 |   NULL | NULL |
|  3 |   78 |   NULL | NULL |
|  4 |   63 |   NULL | NULL |
|  5 |   81 |   NULL | NULL |
|  6 |   15 |   NULL | NULL |
|  7 |   32 |   NULL | NULL |
|  8 |   18 |   NULL | NULL |
|  9 |   92 |   NULL | NULL |
| 11 |   51 |   NULL | NULL |
| 13 |   16 |   NULL | NULL |
| 14 |   39 |   NULL | NULL |
| 15 |   47 |   NULL | NULL |
| 16 |   21 |   NULL | NULL |
| 17 |   63 |   NULL | NULL |
| 18 |   53 |   NULL | NULL |
| 19 |   78 |   NULL | NULL |
| 20 |   31 |   NULL | NULL |
+----+------+--------+------+
20 rows in set (0.00 sec)    
12345678910111213141516171819202122232425
```

性能测试

```mysql-sql
explain select * from class left join book on class.card = book.card;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   20 |   100.00 | NULL                                               |
|  1 | SIMPLE      | book  | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   20 |   100.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)
1234567
```

type为all，全表扫描，且出现Using join buffer连接缓存，需要优化。
先向book表加索引

```mysql-sql
--向右表加索引
create index idx_book_card on book(card);
explain select * from class left join book on class.card = book.card;
123
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+---------------+---------+-----------------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key           | key_len | ref             | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+---------------+---------+-----------------+------+----------+-------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL          | NULL    | NULL            |   20 |   100.00 | NULL        |
|  1 | SIMPLE      | book  | NULL       | ref  | idx_book_card | idx_book_card | 4       | demo.class.card |    1 |   100.00 | Using index |
+----+-------------+-------+------------+------+---------------+---------------+---------+-----------------+------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)
1234567
```

此时，book表的type变为ref，且Using join buffer消失。
向class表加索引

```mysql-sql
drop index idx_book_card on book;
--向左表加索引
create index idx_class_card on class(card);
explain select * from class left join book on class.card = book.card;
1234
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+----------------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys | key            | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+-------+---------------+----------------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | class | NULL       | index | NULL          | idx_class_card | 4       | NULL |   20 |   100.00 | Using index                                        |
|  1 | SIMPLE      | book  | NULL       | ALL   | NULL          | NULL           | NULL    | NULL |   20 |   100.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+-------+---------------+----------------+---------+------+------+----------+----------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)                                                                                                                                       
1234567
```

此时只是class的type变为index，其他未变化。
我们可得出，左连接往右表加索引优化效果更好，因为左连接时左表全部包含在内，而右表只是包含部分数据；
同理，右连接往左表建立索引。
驱动表：
mysql中指定了连接条件时，满足查询条件的记录行数少的表为驱动表；如未指定查询条件，则扫描行数少的为驱动表。mysql优化器就是以小表驱动大表的方式来决定执行顺序的。

```mysql-sql
--指定class表先载入
explain select * from class straight_join book on class.card = book.card;
12
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  100 |   100.00 | NULL                                               |
|  1 | SIMPLE      | book  | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 1000 |    10.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                           
1234567
--指定book表先载入
explain select * from book straight_join class on class.card = book.card;
12
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | book  | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 1000 |   100.00 | NULL                                               |
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  100 |    10.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                           
1234567
```

`straight_join`实现强制多表的载入顺序，选择其左边的表先载入；而join语法是根据“哪个表的结果集小，就以哪个表为驱动表”来决定谁先载入的
使用STRAIGHT_JOIN一定要慎重，因为啊部分情况下认为指定的执行顺序并不一定会比优化引擎要靠谱。

## 三、三表优化

建立新表并插入多条数据

```mysql-sql
--手机
create table phone(
    phoneid int unsigned not null primary key auto_increment,
    card int unsigned not null
);
12345
insert into phone(card) values(floor(rand()*20));
1
```

进行测试

```mysql-sql
create index idx_book_card on book(card);
create index idx_phone_card on phone(card);
explain select * from class left join book on class.card = book.card left join phone on book.id = phone.id;
123
```

打印

```mysql-sql
+----+-------------+-------+------------+------+----------------+----------------+---------+-----------------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys  | key            | key_len | ref             | rows | filtered | Extra       |
+----+-------------+-------+------------+------+----------------+----------------+---------+-----------------+------+----------+-------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL           | NULL           | NULL    | NULL            |  100 |   100.00 | NULL        |
|  1 | SIMPLE      | book  | NULL       | ref  | idx_book_card  | idx_book_card  | 4       | demo.class.card |    1 |   100.00 | Using index |
|  1 | SIMPLE      | phone | NULL       | ref  | idx_phone_card | idx_phone_card | 4       | demo.book.card  |    1 |   100.00 | Using index |
+----+-------------+-------+------------+------+----------------+----------------+---------+-----------------+------+----------+-------------+
3 rows in set, 1 warning (0.00 sec)
12345678
```

## 四、索引优化

### 1.建表：

```mysql-sql
create table staffs(
    id int primary key auto_increment,
    name varchar(24) not null default "",
    age int not null default 0,
    pos varchar(20) not null default "",
    add_time timestamp not null default CURRENT_TIMESTAMP 
)charset utf8;
create table user(
    id int not null auto_increment primary key,
    name varchar(20) default null,
    age int default null,
    email varchar(20) default null
) engine=innodb default charset=utf8;
12345678910111213
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.01 sec)
123
```

### 2.插入数据：

```mysql-sql
insert into staffs(`name`,`age`,`pos`,`add_time`) values('z3',22,'manager',now());
insert into staffs(`name`,`age`,`pos`,`add_time`) values('July',23,'dev',now());
insert into staffs(`name`,`age`,`pos`,`add_time`) values('2000',23,'dev',now());
insert into user(name,age,email) values('1aa1',21,'b@163.com');
insert into user(name,age,email) values('2aa2',22,'a@163.com');
insert into user(name,age,email) values('3aa3',23,'c@163.com');
insert into user(name,age,email) values('4aa4',25,'d@163.com');
1234567
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.01 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)
1234567891011
```

### 3.建立复合索引：

```mysql-sql
create index idx_staffs_nameAgePos on staffs(name,age,pos);
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

### 4.口诀：

> 全值匹配我最爱,最左前缀要遵守；
> 带头大哥不能死,中间兄弟不能断；
> 索引列上少计算,范围之后全失效；
> like百分写最右,覆盖索引不写星；
> 不等空值还有or,索引失效要少用；
> varchar引号不可丢,SQL高级也不难。

解释：

#### 全值匹配我最爱：

查询使用的字段和建立的索引对应的字段完全匹配。

```mysql-sql
explain select * from staffs where name = 'July';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref   | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.01 sec)
123456
```

此时key_len为74，只用到了name。
增加一个字段age：

```mysql-sql
explain select * from staffs where name = 'July' and age = 23;
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref         | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------+------+----------+-------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 78      | const,const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
123456
```

此时key_len变为78，索引多了一个age。
再增加字段pos：

```mysql-sql
explain select * from staffs where name = 'July' and age = 23 and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref               | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 140     | const,const,const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
123456
```

此时key_len变为140，索引多了一个pos。
索引的个数变化，个数越多索引越长，查询范围越小，越快，第3种情况即为全值匹配，建立索引的字段和where条件句中的字段完全匹配。

#### 最左前缀要遵守：

查询从索引的最左前列开始，并且不跳过索引中的列：

##### 带头大哥不能死：

建立索引的第一个字段不能缺失，即查询从索引的最左前列开始。

```mysql-sql
explain select * from staffs where age = 23 and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
123456
```

显然没有Using index condition，即没有使用到索引。

##### 中间兄弟不能断：

建立索引的中间字段不能缺失，即不能跳过索引中的列。

```mysql-sql
explain select * from staffs where name = 'July' and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | const |    1 |    33.33 | Using index condition |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

key_len为74，说明用到了1个索引，即只用到了name，索引中的字段在where条件句中中间的字段不能缺失，否则缺失后面的字段索引将失效，如例中的pos字段即失效。

#### 索引列上少计算：

不能**在where条件语句的索引列**上直接进行数值计算、调用函数。

```mysql-sql
explain select * from staffs where lower(name) = 'july';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |   100.00 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)
123456
```

type为all，全表扫描，索引失效。

```mysql-sql
explain select * from staffs where name = 'July' and age - 1 = 22;
1
```

打印

```mysql-sql
 id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | const |    1 |   100.00 | Using index condition |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)
12345
```

key_len为74，只用到了1个索引，说明age索引失效，要想使用索引，可以在等号右边进行计算。

```mysql-sql
create index idx_add_yime on staffs(add_time);
explain select * from staffs where date(add_time) = '2019-12-16';
12
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |   100.00 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                       
123456
```

即使用函数会使索引失效。

#### 范围之后全失效：

```mysql-sql
explain select * from staffs where name = 'july' and age > 25 and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
| id | select_type | table  | partitions | type  | possible_keys         | key                   | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | staffs | NULL       | range | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 78      | NULL |    1 |    33.33 | Using index condition |
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

有2个索引字段有效，即name、age有效，age为范围，所以之后的pos失效。

#### like百分写最右：

%、_写到字符串最右边，type为range。

```mysql-sql
explain select * from staffs where name like '%july%';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+ 
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       | 
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+ 
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where | 
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+ 
1 row in set, 1 warning (0.01 sec)                                                                                        
123456
```

模糊查询（字段值首位都有%），为全表查询，未使用索引。

```mysql-sql
explain select * from staffs where name like 'july%';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
| id | select_type | table  | partitions | type  | possible_keys         | key                   | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | staffs | NULL       | range | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | NULL |    1 |   100.00 | Using index condition |
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

字段值尾有%，能用到索引。

```mysql-sql
explain select * from staffs where name like '%july';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)
123456
```

显然，%位于字段值首时，又变为全表扫描，索引失效。
_与%的效果相同。

```mysql-sql
create index idx_user_nameage on user(name,age);
explain select name from user where name like '%aa%';
12
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+------------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys | key              | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+---------------+------------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | user  | NULL       | index | NULL          | idx_user_nameage | 68      | NULL |    4 |    25.00 | Using where; Using index |
+----+-------------+-------+------------+-------+---------------+------------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

select后的字段都必须为含有索引的字段的组合，才能使用索引，用全局索引或搜索引擎解决问题。

```mysql-sql
explain select email from user where name like '%aa%';
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | user  | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    4 |    25.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)
123456
```

email字段不含有索引，所以为全表扫描，不能使用索引。

#### 覆盖索引不写星：

select不能用*，一般根据需要用什么字段select就接什么字段。

```mysql-sql
explain select name from staffs where name = 'july' and age = 23 and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------------+ 
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref               | rows | filtered | Extra       | 
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------------+ 
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 140     | const,const,const |    1 |   100.00 | Using index | 
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------------+ 
1 row in set, 1 warning (0.01 sec)                                                                                                                              
123456
```

#### 不等空值还有or,索引失效要少用：

```mysql-sql
explain select * from staffs where name != 'july';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys         | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | idx_staffs_nameAgePos | NULL | NULL    | NULL |    3 |    66.67 | Using where |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                            
123456
```

即为全表扫描，且未使用索引。
可以用不等号（>、<）解决这个问题。

```mysql-sql
explain select * from staffs where name is null;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra            |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | Impossible WHERE |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                        
123456
```

因为建表时标记name为非空，所以这里为Impossible WHERE。

```mysql-sql
explain select * from staffs where name is not null;
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys         | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | idx_staffs_nameAgePos | NULL | NULL    | NULL |    3 |    66.67 | Using where |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                                                       
123456
```

也为全表扫描。

```mysql-sql
explain select * from staffs where name = 'july' or name = 'tom';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys         | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | idx_staffs_nameAgePos | NULL | NULL    | NULL |    2 |   100.00 | Using where |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                         
123456
```

#### varchar引号不可丢,SQL高级也不难：

```mysql-sql
explain select * from staffs where name = '2000';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref   | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.01 sec)                                                                                                                       
123456
```

没有引号时

```mysql-sql
explain select * from staffs where name = 2000;
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys         | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | idx_staffs_nameAgePos | NULL | NULL    | NULL |    2 |    50.00 | Using where |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
1 row in set, 3 warnings (0.00 sec)                                                                                                                          
123456
```

即没有引号时MySQL也能隐式转换为char型，可以正常运行实现功能、查询到数据，但是索引会失效，成为全表扫描，降低查询效率。
可得：虽然对varchar可以进行隐式类型转化，但是会导致索引失效。

### 5.练习：

假设index(a,b,c)，如下图
![索引练习](https://img-blog.csdnimg.cn/20191218142606589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 五、排序优化

### 1.分析

- 观察,至少跑一天,看看生产的慢SQL情况，问题复现；
- 开启慢查询日志,设置阙值,比如超过5秒钟的就是慢SQL,并抓取出来；
- explain + 慢SQL分析；
- show profile查询SQL在MySQL服务器里面的执行细节；
- 进行SQL数据库服务器的参数调优(运维 or DBA来做)。

### 2.永远小表驱动大表

小表驱动大表，即小的数据集驱动大的数据集。
类比循环

```python
for i in range(5):
    for j in range(1000):
        pass

for i in range(1000):
    for j in range(5):
        pass
1234567
```

如果小的循环在外层，对于数据库连接来说就只连接5次，进行5000次操作，如果1000在外，则需要进行1000次数据库连接，从而浪费资源，增加消耗。这就是为什么要小表驱动大表。
可参考https://blog.csdn.net/akunshouyoudou/article/details/90518955。

# Python全栈（三）数据库优化之11.MySQL高级-排序优化、慢查询日志、批量插入数据和Show Profile

## 一.排序优化

### 1.定义

即order by优化。
order by子句,尽量使用**index方式**排序,避免使用filesort方式排序。

### 2.建表，插入测试数据：

```mysql-sql
create table tbla(
    age int,
    birth timestamp not null
);

insert into tbla(age,birth) values(22,now());
insert into tbla(age,birth) values(23,now());
insert into tbla(age,birth) values(24,now());
12345678
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)

Query OK, 1 row affected (0.01 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)
1234567
```

### 3.建立索引：

```mysql-sql
create index idx_tbla_agebrith on tbla(age,birth);
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

### 4.分析：

```mysql-sql
explain select * from tbla where age > 30 order by age;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

未出现Using filesort。

```mysql-sql
explain select * from tbla where age > 30 order by age,birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

仍然未出现Using filesort。

```mysql-sql
explain select * from tbla where age > 30 order by birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

此时出现了Using filesort。

```mysql-sql
explain select * from tbla where age > 30 order by birth,age;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

也出现了Using filesort。
可以得出，order by也遵循最佳左前缀法则。
改变查询条件：

```mysql-sql
explain select * from tbla where birth > '2019-01-01 00:00:00' order by birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | index | NULL          | idx_tbla_agebrith | 9       | NULL |    3 |    33.33 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

也出现了文件内排序，即是否出现文件内排序与where条件的字段无关，与order by后边的字段有关，order by关心的是后边接的用于排序的字段，而不是where条件句中的字段。

```mysql-sql
explain select * from tbla where birth > '2019-01-01 00:00:00' order by age desc,birth asc;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | index | NULL          | idx_tbla_agebrith | 9       | NULL |    3 |    33.33 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

可得出，order by后边有多个字段时，如果排序方式不一致时也会出现文件内排序。

```mysql-sql
explain select * from tbla where birth > '2019-01-01 00:00:00' order by age desc,birth desc;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys | key               | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | tbla  | NULL       | index | NULL          | idx_tbla_agebrith | 9       | NULL |    3 |    33.33 | Using where; Using index |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

显然，order by后边有多个字段时，如果排序方式一致时不会出现文件内排序。

#### 分析总结：

MySQL支持两种方式的排序,filesort和index。
index效率高,MySQL扫描索引本身完成排序；filesort方式效率较低。
order by在两种情况下,会使用index方式排序：

- order by语句使用索引最左前列；
- 使用where子句与order by子句条件组合满足索引最左前列。
  如

```mysql-sql
explain select * from tbla where age > 30 order by birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

会出现文件内排序。

```mysql-sql
explain select * from tbla where age = 30 order by birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+-------------------+-------------------+---------+-------+------+----------+--------------------------+
| id | select_type | table | partitions | type | possible_keys     | key               | key_len | ref   | rows | filtered | Extra                    |
+----+-------------+-------+------------+------+-------------------+-------------------+---------+-------+------+----------+--------------------------+
|  1 | SIMPLE      | tbla  | NULL       | ref  | idx_tbla_agebrith | idx_tbla_agebrith | 5       | const |    1 |   100.00 | Using where; Using index |
+----+-------------+-------+------------+------+-------------------+-------------------+---------+-------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

去掉范围后，age和birth可以组成最佳左前缀，无文件内排序。

### 5.filesort算法：

filesort有两种算法：双路排序和单路排序。

#### 双路排序：

MySQL4.1之前是使用双路排序,字面意思就是**两次扫描磁盘**,最终得到数据,读取行指针和order by列,对他们进行排序,然后扫描已经排序好的列表,按照列表中的值重新从列表中读取对应的数据输出。

#### 单路排序：

单路排序,从磁盘读取查询需要的所有列,按照order by列在buffer对他们进行排序,然后扫描排序后的列表进行输出,它的效率更快一些,避免了第二次读取数据,并且把**随机IO变成了顺序IO**,但是它会使用更多的空间。

在数据量超过缓存区大小sort_buffer_size的时候，可能出现单路排序性能低于双路排序的情况，需要进行优化：

#### 优化策略：

调整MySQL参数：

- 增加sort_buffer_size参数设置；
- 增大max_lenght_for_sort_data参数的设置。

### 6.提高order by的速度：

- order by时select * 是一个大忌,只写需要的字段：
  当查询的字段大小总和小于max_length_for_sort_data而且排序字段不是text|blob类型时,会用改进后的算法–单路排序；
  两种算法的数据都有可能超出sort_buffer的容量,超出之后,会创建tmp文件进行合并排序,导致多次I/O。
- 尝试提高sort_buffer_size。
- 尝试提高max_length_for_sort_data。

group by的用法、分析、优化与order by类似。

### 7.练习

索引`a_b_c(a,b,c)`：

| SQL语句                                    | 是否会出现filesort |
| ------------------------------------------ | ------------------ |
| order by a,b                               | 不会               |
| order by a,b,c                             | 不会               |
| order by a desc,b desc,c desc              | 不会               |
| where a = const order by b,c               | 不会               |
| where a = const and b = const order by c   | 不会               |
| where a = const and b > const order by b,c | 不会               |
| order by a asc,b desc,c desc               | 会                 |
| where g = const order by b,c               | 会                 |
| where a = const order by c                 | 会                 |
| where a = const order by a,d               | 会                 |

## 二、慢查询日志

### 1.定义

MySQL的慢查询日志MySQL提供的一种日志记录,它用来记录在MySQL中**响应时间超过阙值**的语句。
具体指运行时间超过long_query_time值的SQL,则会被记录到慢查询日志中。
如long_query_time的默认值为10,意思是运行10秒以上（>10秒，即不包括10秒）的语句。
由它来查看哪些SQL超出了我们的最大忍耐时间值,比如一条SQL执行超过5秒钟,我们就算是慢SQL,希望能收集超过5秒的SQL,结合之前explain进行全面分析。

### 2.使用

默认情况下,MySQL数据库没有开启慢查询日志,需要我们手动来设置这个参数；
如果不是调优需要的话,一般不建议启动该参数,因为开启慢查询日志会或多或少带来一定的**性能影响**，影响效率。
慢查询日志支持将日志记录写入文件。
查看慢查询日志状态：

```mysql-sql
show variables like '%slow_query_log%';
1
```

打印

```mysql-sql
+---------------------+----------------------------------------------------------------------------+
| Variable_name       | Value                                                                      |
+---------------------+----------------------------------------------------------------------------+
| slow_query_log      | OFF                                                                        |
| slow_query_log_file | E:\MySQL\phpstudy_pro\Extensions\MySQL5.7.26\data\xxx.log |
+---------------------+----------------------------------------------------------------------------+
2 rows in set, 1 warning (0.01 sec)
1234567
```

slow_query_log_file是slow_query_log的保存路径，可以在配置文件中修改。
slow_query_log默认关闭，可通过下面命令开启：

```mysql-sql
set global slow_query_log = 1;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

再次查询

```mysql-sql
show variables like '%slow_query_log%';
1
```

打印

```mysql-sql
+---------------------+----------------------------------------------------------------------------+
| Variable_name       | Value                                                                      |
+---------------------+----------------------------------------------------------------------------+
| slow_query_log      | ON                                                                         |
| slow_query_log_file | E:\MySQL\phpstudy_pro\Extensions\MySQL5.7.26\data\xxx.log |
+---------------------+----------------------------------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)
1234567
```

以上在命令行中的修改都是临时的，重启MySQL会恢复默认，要想永久修改，要在配置文件my.ini中修改，并重启服务才能生效。
查看响应时间阙值：

```mysql-sql
show variables like '%long_query_time%';
1
```

打印

```mysql-sql
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
1 row in set, 1 warning (0.01 sec)
123456
```

可知，时间阙值默认为10秒。
重新设置阙值

```mysql-sql
set global long_query_time = 3;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
1
```

此时再次查看阙值

```mysql-sql
show variables like '%long_query_time%';
1
```

打印

```mysql-sql
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 3.000000 |
+-----------------+----------+
1 row in set, 1 warning (0.01 sec)
123456
```

如果未改变，可以新打开一个命令行，连接数据库即可看到变为3秒。
此时进行测试

```mysql-sql
select sleep(4);
1
```

打印

```mysql-sql
+----------+
| sleep(4) |
+----------+
|        0 |
+----------+
1 row in set (4.00 sec)
123456
```

sleep()函数可以实现阻塞功能，阻塞给定的时间，可以实现用于测试慢查询的时间。
此时可查看慢查询日志文件，可看到之前执行的慢SQL以及被记录到日志：

```mysql-sql
E:\MySQL\phpstudy_pro\COM\..\Extensions\MySQL5.7.26\\bin\mysqld.exe, Version: 5.7.26 (MySQL Community Server (GPL)). started with:
TCP Port: 3306, Named Pipe: MySQL
Time                 Id Command    Argument
# Time: 2019-12-19T11:22:04.811013Z
# User@Host: root[root] @ localhost [::1]  Id:    13
# Query_time: 4.000478  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 0
use demo;
SET timestamp=1576754524;
select sleep(4);
123456789
```

一般情况下不应该开启慢查询日志，在测试环境下可以使用，在生产环境下可以使用。

### 3.慢查询日志工具

mysqldumpslow，一般是在Linux系统下使用。

| 符号 | 意义                     |
| ---- | ------------------------ |
| s    | 表示按照何种方式排序     |
| c    | 访问次数                 |
| l    | 锁定时间                 |
| r    | 返回记录                 |
| t    | 查询时间                 |
| al   | 平均锁定时间             |
| ar   | 平均返回记录数           |
| t    | 即为返回前面多少条的数据 |

例如，
得到返回记录集最多的10个SQL：

```mysql-sql
mysqldumpslow -s r -t 10 D:/phpStudy/PHPTutorial/MySQL/slow_log.txt;
1
```

得到访问次数最多的10个SQL：

```mysql-sql
mysqldumpslow -s c -t 10 D:/phpStudy/PHPTutorial/MySQL/slow_log.txt;
1
```

查询有多少条慢SQL语句：

```mysql-sql
show global status like '%slow_queries%';
1
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Slow_queries  | 3     |
+---------------+-------+
1 row in set (0.01 sec)
123456
```

易知，此时有3条慢SQL语句。

## 三、批量插入数据

### 1.创建表：

```mysql-sql
--部门表
create table dept(
            id int primary key auto_increment,
            deptno mediumint not null,
            dname varchar(20) not null,
            loc varchar(13) not null
)engine=innodb default charset=gbk;
--员工表
create table emp(
            id int primary key auto_increment,
            empno mediumint not null,   
            ename varchar(20) not null, 
            job varchar(9) not null, 
            mgr mediumint not null,  
            hiredate DATE not null,  
            sal decimal(7,2) not null, 
            comm decimal(7,2) not  null, 
            deptno mediumint not null   
)engine=innodb default charset=gbk;
12345678910111213141516171819
```

打印

```mysql-sql
Query OK, 0 rows affected (0.03 sec)

Query OK, 0 rows affected (0.02 sec)
123
```

### 2.创建函数



——定界符、分隔符。DELIMITER——定界符、分隔符。DELIMITER

：开始；
END $$：结束。
随机产生字符串：



```mysql-sql
--开始符
DELIMITER $$
--创建函数
CREATE FUNCTION rand_string(n INT) RETURNS VARCHAR(255)
BEGIN
--声明变量
DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
DECLARE return_str VARCHAR(255) DEFAULT '';
DECLARE i INT DEFAULT 0;
--开始循环
WHILE i < n DO
--contact连接字符串
SET return_str = CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
SET i = i + 1;
--结束循环
END WHILE;
RETURN return_str;
--结束符
END $$
12345678910111213141516171819
```

随机产生部门编号：

```mysql-sql
DELIMITER $$
CREATE FUNCTION rand_num() RETURNS INT(5)
BEGIN
DECLARE i INT DEFAULT 0;
SET i = FLOOR(100+RAND()*10);
RETURN i;
END $$
1234567
```

创建函数，如果报错：`this function has none of DETERMINISTIC...` 时设置参数

```mysql-sql
set global log_bin_trust_function_creators=1;
1
```

### 3.创建存储过程

存储过程用于批量处理数据。
为emp创建存储过程、插入数据：

```mysql-sql
DELIMITER $$
CREATE PROCEDURE insert_emp(IN START INT(10),IN max_num INT(10))
BEGIN
DECLARE i INT DEFAULT 0;
--关闭自动提交，最后一起提交
SET autocommit = 0;
REPEAT
SET i = i + 1;
--调用函数插入
INSERT INTO emp(empno,ename,job,mgr,hiredate,sal,comm,deptno) VALUES ((START+i),rand_string(6),'SALESMAN',0001,CURDATE(),2000,400,rand_num());
UNTIL i = max_num
END REPEAT;
--一起提交
COMMIT;
END $$
123456789101112131415
```

为dept创建存储过程、插入数据：

```mysql-sql
DELIMITER $$
CREATE PROCEDURE insert_dept(IN START INT(10),IN max_num INT(10))
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit = 0;
REPEAT
SET i = i + 1;
INSERT INTO dept(deptno,dname,loc) VALUES ((START + i),rand_string(10),rand_string(8));
UNTIL i = max_num
END REPEAT;
COMMIT;
END $$
123456789101112
```

建议在可视化界面里执行函数和存储过程，因为可以多行同时输入并同时执行，不会产生语法错误；
如果在命令行里执行，要逐行输入，不可一次性复制粘贴，会出现语法错误，并且在`DELIMITER $$`语句后MYSQL的结束符变为"$$"，要再次执行`DELIMITER ;`才会使结束符变为";"，执行一般的SQL语句。

### 4.调用存储过程

调用存储过程实现批量插入数据：

```mysql-sql
delimiter ; 
call insert_dept(100,10);
    
call insert_emp(100001,500000);
1234
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (55.29 sec)
123
```

## 四、Show Profile进行SQL分析

### 1.定义

Show Profile是MySQL提供可以用来分析当前会话中语句执行的资源消耗情况,可以用于SQL的调优的测量。
默认情况下,参数处于关闭状态,并保存最近15次的运行结果。

### 2.Show Profile分析步骤：

- 是否支持,看看当前MySQL版本是否支持；
- 开启功能,默认是关闭,使用前需要开启。

版本 > 5.5均支持Show Profile。
查看状态：

```mysql-sql
show variables like 'profiling';
1
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| profiling     | OFF   |
+---------------+-------+
1 row in set, 1 warning (0.01 sec)
123456
```

改变状态

```mysql-sql
--set profiling = on;
set profiling = 1;
12
```

打印

```mysql-sql
Query OK, 0 rows affected, 1 warning (0.00 sec)
1
```

再次查看状态：

```mysql-sql
show variables like 'profiling';
1
```

打印

```mysql-sql
+---------------+-------+         
| Variable_name | Value |         
+---------------+-------+         
| profiling     | ON    |         
+---------------+-------+         
1 row in set, 1 warning (0.00 sec)
123456
```

此时已经打开。
进行几次查询：

```mysql-sql
select * from students group by age limit 3; 
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
3 rows in set (0.01 sec)
12345678
select * from students order by age limit 3; 
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
3 rows in set (0.00 sec)
12345678
select * from students; 
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | 
+----+-------+------+--------+--------+------------+-----------+--------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 | 
... 
+----+-------+------+--------+--------+------------+-----------+--------+ 
27 rows in set (0.00 sec)                                                 
12345678910111213141516
select * from emp group by id%10 limit 15000; 
1
```

打印

```mysql-sql
+----+--------+--------+----------+-----+------------+---------+--------+--------+
| id | empno  | ename  | job      | mgr | hiredate   | sal     | comm   | deptno |
+----+--------+--------+----------+-----+------------+---------+--------+--------+
| 10 | 100011 | fIaaad | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    102 |
|  1 | 100002 | APwPJb | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    100 |
|  2 | 100003 | uKqxof | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    106 |
|  3 | 100004 | fxYAIL | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    106 |
|  4 | 100005 | jUWYeC | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    104 |
|  5 | 100006 | wUkqYU | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    105 |
|  6 | 100007 | hZDRyS | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    108 |
|  7 | 100008 | SJBNmU | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    107 |
|  8 | 100009 | ccemXd | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    104 |
|  9 | 100010 | nQrmha | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    106 |
+----+--------+--------+----------+-----+------------+---------+--------+--------+
10 rows in set (1.64 sec)                                                         
123456789101112131415
```

查看各个SQL语句的执行时间：

```mysql-sql
show profiles;
1
```

打印

```mysql-sql
+----------+------------+----------------------------------------------+
| Query_ID | Duration   | Query                                        |
+----------+------------+----------------------------------------------+
|        1 | 0.00027575 | select * from students group by age limit 3  |
|        2 | 0.00029700 | select * from students order by age limit 3  |
|        3 | 0.00021350 | select * from students                       |
|        4 | 1.65845325 | select * from emp group by id%10 limit 15000 |
+----------+------------+----------------------------------------------+
4 rows in set, 1 warning (0.00 sec)
123456789
```

查看单个SQL语句的执行情况

```mysql-sql
show profile cpu,block io for query 1;
1
```

打印

```mysql-sql
+----------------------+----------+----------+------------+--------------+---------------+
| Status               | Duration | CPU_user | CPU_system | Block_ops_in | Block_ops_out |
+----------------------+----------+----------+------------+--------------+---------------+
| starting             | 0.000057 | 0.000000 |   0.000000 |         NULL |          NULL |
| checking permissions | 0.000009 | 0.000000 |   0.000000 |         NULL |          NULL |
| Opening tables       | 0.000017 | 0.000000 |   0.000000 |         NULL |          NULL |
| init                 | 0.000048 | 0.000000 |   0.000000 |         NULL |          NULL |
| System lock          | 0.000005 | 0.000000 |   0.000000 |         NULL |          NULL |
| optimizing           | 0.000002 | 0.000000 |   0.000000 |         NULL |          NULL |
| optimizing           | 0.000002 | 0.000000 |   0.000000 |         NULL |          NULL |
| statistics           | 0.000013 | 0.000000 |   0.000000 |         NULL |          NULL |
| preparing            | 0.000010 | 0.000000 |   0.000000 |         NULL |          NULL |
| statistics           | 0.000020 | 0.000000 |   0.000000 |         NULL |          NULL |
| preparing            | 0.000009 | 0.000000 |   0.000000 |         NULL |          NULL |
| executing            | 0.000012 | 0.000000 |   0.000000 |         NULL |          NULL |
| Sending data         | 0.000005 | 0.000000 |   0.000000 |         NULL |          NULL |
| executing            | 0.000002 | 0.000000 |   0.000000 |         NULL |          NULL |
| Sending data         | 0.001247 | 0.000000 |   0.000000 |         NULL |          NULL |
| end                  | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| query end            | 0.000006 | 0.000000 |   0.000000 |         NULL |          NULL |
| closing tables       | 0.000001 | 0.000000 |   0.000000 |         NULL |          NULL |
| removing tmp table   | 0.000156 | 0.000000 |   0.000000 |         NULL |          NULL |
| closing tables       | 0.000006 | 0.000000 |   0.000000 |         NULL |          NULL |
| freeing items        | 0.000038 | 0.000000 |   0.000000 |         NULL |          NULL |
| cleaning up          | 0.000015 | 0.000000 |   0.000000 |         NULL |          NULL |
+----------------------+----------+----------+------------+--------------+---------------+
22 rows in set, 1 warning (0.00 sec)
123456789101112131415161718192021222324252627
```

可以看出，在主要操作之外，删除临时表removing tmp table用时相对较长。
进行验证：

```mysql-sql
explain select * from students group by age limit 3;
1
```

打印

```mysql-sql
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+---------------------------------+
| id | select_type | table    | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                           |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+---------------------------------+
|  1 | SIMPLE      | students | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   27 |   100.00 | Using temporary; Using filesort |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+---------------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

出现Using temporary，确实用到了临时表，先创建临时表，将查询到的数据存到临时表，再从临时表查询数据，再删除临时表，所以会耗费很多时间。
再查看一个SQL语句

```mysql-sql
show profile cpu,block io for query 4;
1
```

打印

```mysql-sql
+----------------------+----------+----------+------------+--------------+---------------+
| Status               | Duration | CPU_user | CPU_system | Block_ops_in | Block_ops_out |
+----------------------+----------+----------+------------+--------------+---------------+
| starting             | 0.000067 | 0.000000 |   0.000000 |         NULL |          NULL |
| checking permissions | 0.000005 | 0.000000 |   0.000000 |         NULL |          NULL |
| Opening tables       | 0.002077 | 0.000000 |   0.000000 |         NULL |          NULL |
| init                 | 0.000030 | 0.000000 |   0.000000 |         NULL |          NULL |
| System lock          | 0.000011 | 0.000000 |   0.000000 |         NULL |          NULL |
| optimizing           | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| statistics           | 0.000022 | 0.000000 |   0.000000 |         NULL |          NULL |
| preparing            | 0.000009 | 0.000000 |   0.000000 |         NULL |          NULL |
| Creating tmp table   | 0.000032 | 0.000000 |   0.000000 |         NULL |          NULL |
| Sorting result       | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| executing            | 0.000001 | 0.000000 |   0.000000 |         NULL |          NULL |
| Sending data         | 1.659538 | 1.515625 |   0.062500 |         NULL |          NULL |
| Creating sort index  | 0.000060 | 0.000000 |   0.000000 |         NULL |          NULL |
| end                  | 0.000004 | 0.000000 |   0.000000 |         NULL |          NULL |
| query end            | 0.000007 | 0.000000 |   0.000000 |         NULL |          NULL |
| removing tmp table   | 0.000008 | 0.000000 |   0.000000 |         NULL |          NULL |
| query end            | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| closing tables       | 0.000077 | 0.000000 |   0.000000 |         NULL |          NULL |
| freeing items        | 0.000061 | 0.000000 |   0.000000 |         NULL |          NULL |
| cleaning up          | 0.000015 | 0.000000 |   0.000000 |         NULL |          NULL |
+----------------------+----------+----------+------------+--------------+---------------+
20 rows in set, 1 warning (0.00 sec)                                                      
12345678910111213141516171819202122232425
```

出现了Sorting result，说明存在文件内排序。
进行验证

```mysql-sql
explain select * from emp group by id%10 limit 15000;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+---------+----------+---------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows    | filtered | Extra                           |
+----+-------------+-------+------------+------+---------------+------+---------+------+---------+----------+---------------------------------+
|  1 | SIMPLE      | emp   | NULL       | ALL  | PRIMARY       | NULL | NULL    | NULL | 1395176 |   100.00 | Using temporary; Using filesort |
+----+-------------+-------+------------+------+---------------+------+---------+------+---------+----------+---------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

出现了Using filesort，说明确实存在文件内排序，需要进行优化。
**type参数：**

| 参数        | 意义                       |
| ----------- | -------------------------- |
| all         | 显示所有的开销信息         |
| block io    | 显示块IO相关开销           |
| cpu         | 显示CPU相关开销信息        |
| ipc         | 显示发送和接收相关开销信息 |
| memory      | 显示内存相关开销信息       |
| page faults | 显示页面错误相关开销信息   |

**参数注意**：

| 参数                         | 意义                                  |
| ---------------------------- | ------------------------------------- |
| converting HEAP to MyISAM    | 查询结果太大,内存都不够用了往磁盘上搬 |
| Creating tmp table           | 创建临时表                            |
| Copying to tmp table on disk | 把内存中临时表复制到磁盘,危险         |
| locked                       |                                       |

### 3.全局查询日志

把每一条SQL语句记录下来，量大，无必要。
开启命令：

```mysql-sql
set global general_log = 1;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

将SQL语句写到表中：

```mysql-sql
set global log_output = 'TABLE';
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
1
```

所编写的SQL语句,会记录到MySQL库里的genral_log表。
执行一条SQL语句，如

```mysql-sql
select * from students;
1
```

再查看全局查询日志：

```mysql-sql
select * from mysql.general_log;
1
```

打印

```mysql-sql
+----------------------------+------------------------------+-----------+-----------+--------------+---------------------------------------------------------------+
| event_time                 | user_host                    | thread_id | server_id | command_type | argument                                                      |
+----------------------------+------------------------------+-----------+-----------+--------------+---------------------------------------------------------------+
| 2019-12-18 21:47:51.376592 | root[root] @ localhost [::1] |        10 |         1 | Query        | use `mysql`                                                   |
| 2019-12-18 21:47:52.914806 | root[root] @ localhost [::1] |        10 |         1 | Query        | show full tables from `mysql` where table_type = 'BASE TABLE' |
| 2019-12-18 21:48:02.936032 | root[root] @ localhost [::1] |        10 |         1 | Query        | describe `mysql`.`general_log`                                |
| 2019-12-18 21:48:02.937085 | root[root] @ localhost [::1] |        10 |         1 | Query        | show index from `mysql`.`general_log`                         |
| 2019-12-18 21:48:05.235971 | root[root] @ localhost [::1] |        10 |         1 | Query        | select * from `mysql`.`general_log` limit 0, 1000             |
...
| 2019-12-18 22:07:08.593162 | root[root] @ localhost [::1] |        19 |         1 | Query        | show profile                                                  |
| 2019-12-19 20:38:20.901277 | root[root] @ localhost [::1] |        24 |         1 | Query        | select * from demo                                            |
| 2019-12-19 20:38:37.884983 | root[root] @ localhost [::1] |        24 |         1 | Query        | select * from studens                                         |
| 2019-12-19 20:38:43.057866 | root[root] @ localhost [::1] |        24 |         1 | Query        | select * from students                                        |
| 2019-12-19 20:39:55.765410 | root[root] @ localhost [::1] |        24 |         1 | Query        | select * from mysql.general_log                               |
+----------------------------+------------------------------+-----------+-----------+--------------+---------------------------------------------------------------+
28 rows in set (0.00 sec)
```

# Python全栈（三）数据库优化之12.MySQL高级-数据库锁和数据库分区

## 一、数据库锁

锁是计算机协调多个进程或线程并发访问某一资源的机制。
根据对数据操作的粒度，分为表锁、行锁、间隙锁。

### 1.表锁

更偏读。
偏向MyISAM存储引擎,开销小,加锁快;
无死锁,因为整个表都被锁了，别人不能访问；
锁定粒度大,发送锁冲突的概率最高,并发度低。
表锁举例：
创建表并插入数据：

```sql
create table mylock(
    id int not null primary key auto_increment,
    name varchar(20)
)engine myisam;

insert into mylock(name) values('a');
insert into mylock(name) values('b');
insert into mylock(name) values('c');
insert into mylock(name) values('d');
insert into mylock(name) values('e');
12345678910
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)

Query OK, 1 row affected (0.01 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)
1234567891011
```

手动增加表锁：
语法：
`lock table 表名字 read(write),表名字2 read(write);`

```sql
lock table mylock read,book write;
1
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)
1
```

查看所有数据库的所有表，同时可以查看创建的锁：

```sql
show open tables;
1
```

打印

```sql
+--------------------+------------------------------------------------------+--------+-------------+
| Database           | Table                                                | In_use | Name_locked |
+--------------------+------------------------------------------------------+--------+-------------+
| performance_schema | events_waits_summary_by_user_by_event_name           |      0 |           0 |
| performance_schema | events_waits_summary_global_by_event_name            |      0 |           0 |
| performance_schema | events_transactions_summary_global_by_event_name     |      0 |           0 |
| performance_schema | replication_connection_status                        |      0 |           0 |
| mysql              | time_zone_leap_second                                |      0 |           0 |
| mysql              | columns_priv                                         |      0 |           0 |
| performance_schema | metadata_locks                                       |      0 |           0 |
| performance_schema | status_by_user                                       |      0 |           0 |
| demo               | mylock                                               |      1 |           0 |
| mysql              | time_zone_transition_type                            |      0 |           0 |
| performance_schema | events_statements_current                            |      0 |           0 |
| performance_schema | prepared_statements_instances                        |      0 |           0 |
| performance_schema | events_statements_history_long                       |      0 |           0 |
| mysql              | db                                                   |      0 |           0 |
| performance_schema | file_instances                                       |      0 |           0 |
| performance_schema | events_stages_summary_by_user_by_event_name          |      0 |           0 |
| performance_schema | memory_summary_by_thread_by_event_name               |      0 |           0 |
...
| performance_schema | events_stages_history                                |      0 |           0 |
| mysql              | time_zone                                            |      0 |           0 |
| mysql              | slave_master_info                                    |      0 |           0 |
| mysql              | proxies_priv                                         |      0 |           0 |
| performance_schema | events_statements_summary_by_host_by_event_name      |      0 |           0 |
| demo               | book                                                 |      1 |           0 |
| performance_schema | socket_instances                                     |      0 |           0 |
| performance_schema | host_cache                                           |      0 |           0 |
| performance_schema | status_by_account                                    |      0 |           0 |
| performance_schema | events_statements_summary_by_account_by_event_name   |      0 |           0 |
| performance_schema | memory_summary_by_account_by_event_name              |      0 |           0 |
...
+--------------------+------------------------------------------------------+--------+-------------+
110 rows in set (0.00 sec)
1234567891011121314151617181920212223242526272829303132333435
```

显然，mylock表和book表的In_use值为1.
此时在同一命令行中读取

```sql
select * from mylock;
1
```

打印

```sql
+----+------+           
| id | name |           
+----+------+           
|  1 | a    |           
|  2 | b    |           
|  3 | c    |           
|  4 | d    |           
|  5 | e    |           
+----+------+           
5 rows in set (0.00 sec)
12345678910
```

此时再打开一个命令行，执行相同的命令：

```sql
select * from mylock;
1
```

打印

```sql
+----+------+           
| id | name |           
+----+------+           
|  1 | a    |           
|  2 | b    |           
|  3 | c    |           
|  4 | d    |           
|  5 | e    |           
+----+------+           
5 rows in set (0.00 sec)
12345678910
```

在命令行一中更新数据

```sql
update mylock set name = 'a2' where id = 1;
1
```

打印

```sql
ERROR 1099 (HY000): Table 'mylock' was locked with a READ lock and can't be updated
1
```

即读锁限制了对表的更新。
在命令行二中执行相同命令

```sql
update mylock set name = 'a2' where id = 1;
1
```

会发生堵塞，直到命令行一中解锁

```sql
unlock tables;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)
1
```

命令行二中打印

```sql
Query OK, 0 rows affected (57.79 sec)
Rows matched: 1  Changed: 0  Warnings: 0
12
```

即堵塞了57.79秒。
在命令行一中锁定mylock表之后，再在命令行二中查询其他表能正常查询，但是在命令行一中不能再查询其他表，否则会报错，要想读其他表，必须先对mylock解锁。
并且在命令行一关闭后，对mylock的锁自动关闭，其他命令行可对mylock表进行操作。

```sql
select * from book;
1
```

打印

```sql
ERROR 1100 (HY000): Table 'book' was not locked with LOCK TABLES
1
```

在命令行一中加写锁

```sql
lock table mylock write;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)
1
```

查询数据

```sql
select * from mylock;
1
```

打印

```sql
+----+------+           
| id | name |           
+----+------+           
|  1 | a2   |           
|  2 | b    |           
|  3 | c    |           
|  4 | d    |           
|  5 | e    |           
+----+------+           
5 rows in set (0.00 sec)
12345678910
```

能正常查询数据。
在命令行二中执行相同命令：

```sql
select * from mylock;
1
```

发生堵塞。
在命令行二查询其他表能正常查询到。
在命令行一中更新数据

```sql
update mylock set name = 'a4' where id = 1;
1
```

打印

```sql
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
12
```

在命令行二中执行相同操作

```sql
update mylock set name = 'a4' where id = 1;
1
```

也会发生堵塞，直到命令行一释放写锁。
释放表锁：
语法：
`unlock tables;`
表锁总结：
MyISAM在执行查询语句(select)前,会自动给涉及的所有表加读锁,在执行增删改操作前,会自动给涉及的表加写锁：

- 对MyISAM表的读操作(加读锁),不会阻塞其他进程对同一表的读请求,但会阻塞对同一表的写请求。只有当读锁释放后,才会执行其他进行的写操作。即读锁会堵塞写，但是不会堵塞读。
- 对MyISAM表的写操作(加写锁),会阻塞其他进程对同一表的读和写操作,只有当写锁释放后,才会执行其他进程的读写操作。即写锁会堵塞写和读。

```sql
show status like 'table%';
1
```

打印

```sql
+----------------------------+-------+ 
| Variable_name              | Value | 
+----------------------------+-------+ 
| Table_locks_immediate      | 112   | 
| Table_locks_waited         | 0     | 
| Table_open_cache_hits      | 0     | 
| Table_open_cache_misses    | 0     | 
| Table_open_cache_overflows | 0     | 
+----------------------------+-------+ 
5 rows in set (0.01 sec)               
12345678910
```

Table_locks_immediate表示产生表级锁的次数，Table_locks_waited表示表级锁发生冲突时等待的次数，这两个值较高，存在较高的表级锁竞争。

### 2.行锁

更偏写。
偏向InnoDB存储引擎,开销大,加锁慢,会出现死锁；
锁定粒度最小,发生锁冲突的概率最低,并发度也最高。
InnoDB与MyISAM的最大不同点,是支持事务,采用了行级锁。
行锁举例：
创建表并插入数据

```sql
create table test_innodb_lock(a int(11),b varchar(16))engine=innodb;

insert into test_innodb_lock values(1,'b2');
insert into test_innodb_lock values(3,'3');
insert into test_innodb_lock values(4,'4000');
insert into test_innodb_lock values(5,'5000');

create index idx_test_innodb_a on test_innodb_lock(a);

create index idx_test_innodb_b on test_innodb_lock(b);
12345678910
```

打印

```sql
Query OK, 0 rows affected (0.02 sec)  
                                      
Query OK, 1 row affected (0.01 sec)   
                                      
Query OK, 1 row affected (0.00 sec)   
                                      
Query OK, 1 row affected (0.00 sec)   
                                      
Query OK, 1 row affected (0.00 sec)   
                                      
Query OK, 0 rows affected (0.02 sec)  
Records: 0  Duplicates: 0  Warnings: 0
                                      
Query OK, 0 rows affected (0.02 sec)  
Records: 0  Duplicates: 0  Warnings: 0              
123456789101112131415
```

两个命令行关闭自动提交：

```sql
set autocommit = 0;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)            
1
```

在命令行一中更新数据：

```sql
update test_innodb_lock set b = '4001' where a = 4;
1
```

打印

```sql
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0          
12
```

在命令行一中查询，

```sql
select * from test_innodb_lock;
1
```

打印

```sql
+------+------+
| a    | b    |
+------+------+
|    1 | b2   |
|    3 | 1024 |
|    4 | 4001 |
|    5 | 1024 |
+------+------+
4 rows in set (0.00 sec)
123456789
```

数据已经改变
此时在命令行二中查询，

```sql
select * from test_innodb_lock;
1
```

打印

```sql
+------+------+         
| a    | b    |         
+------+------+         
|    1 | b2   |         
|    3 | 1024 |         
|    4 | 1024 |         
|    5 | 1024 |         
+------+------+         
4 rows in set (0.01 sec) 
123456789
```

会发现未改变，需要在两个命令行中commit，然后再在命令行二中查询，

```sql
select * from test_innodb_lock;
1
```

打印

```sql
+------+------+         
| a    | b    |         
+------+------+         
|    1 | b2   |         
|    3 | 1024 |         
|    4 | 4001 |         
|    5 | 1024 |         
+------+------+         
4 rows in set (0.00 sec)
123456789
```

在命令行一中执行

```sql
update test_innodb_lock set b = '4002' where a = 4;
1
```

打印

```sql
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0          
12
```

此时在命令行二中执行

```sql
update test_innodb_lock set b = '4003' where a = 4;
1
```

会发生堵塞
在命令行一中提交后，命令行二打印

```sql
Query OK, 1 row affected (1 min 17.33 sec)
Rows matched: 1  Changed: 1  Warnings: 0
12
```

此时两个命令行分别查询，值分别为4002、4003，出现不一致的情况，再在两个命令行同时提交后，再次查询，数据一致，均为4003。
两个命令行中修改不同行的数据，不会发生冲突。
varchar不加引号：

- 索引失效，全表扫描；
- 由行锁变为表锁，可能阻塞。

在命令行一中执行

```sql
update test_innodb_lock set a = 5 where b = 4000;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0        
12
```

再在命令行二中执行

```sql
update test_innodb_lock set b = '4003' where a = 4;
1
```

会发生堵塞。

#### 如何分析行锁定：

通过检查innodb_row_lock状态变量来分析系统上的行锁争夺情况。

```sql
show status like 'innodb_row_lock%';
1
```

打印

```sql
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| Innodb_row_lock_current_waits | 0     |
| Innodb_row_lock_time          | 77325 |
| Innodb_row_lock_time_avg      | 77325 |
| Innodb_row_lock_time_max      | 77325 |
| Innodb_row_lock_waits         | 1     |
+-------------------------------+-------+
5 rows in set (0.00 sec)      
12345678910
```

### 各个状态量的说明

| 状态量                        | 说明                                                   |
| ----------------------------- | ------------------------------------------------------ |
| Innodb_row_lock_current_waits | 当前正在等待锁定的数量                                 |
| Innodb_row_lock_time          | 从系统启动到现在锁定的总时间长度，值越大，锁定时间越长 |
| Innodb_row_lock_time_avg      | 每次等待所花费平均时间                                 |
| Innodb_row_lock_time_max      | 从系统启动到现在等待最长的一次所花费的时间             |
| Innodb_row_lock_waits         | 系统启动后到现在总共等待的次数                         |

### 3.间隙锁

在命令行一中：

```sql
update test_innodb_lock set b = '1024' where a > 1 and a < 6;
1
```

打印

```sql
Query OK, 1 row affected (0.00 sec)
Rows matched: 3  Changed: 1  Warnings: 0      
12
```

在命令行二中

```sql
insert into test_innodb_lock values(2,'2000');
1
```

发生堵塞，在命令行一中commit才能插入成功。
此即间隙锁。
当我们用范围条件而不是相等条件检索数据,并请求共享或排他锁时,innodb会给符合条件的已有数据记录的索引项加锁, 对于键值在条件范围内但并不存在的记录,叫做"间隙"；
innodb也会对这个"间隙"加锁,这种锁机制就是所谓的间隙锁。
间隙锁的危害：

- 因为SQL执行过程中通过范围查找的话,他会锁定整个范围内所有的索引值,即使这个键值并不存在；
- 间隙锁有一个比较致命的弱点,就是当锁定以为范围键值之后,即使某些不存在的键值也会被无辜的锁定,而造成在锁定的时候无法插入锁定键值范围内的任何数据。在某些场景下这可能会对性能造成很大的危害。

**如何锁定一行**：
`select * from test_innodb_lock where a = 8 for update;`

## 二、MySQL分区表

### 1.定义

分区表的特点：
在逻辑上为一个表,在物理上存储在多个文件中。
查看MySQL是否支持分区：

```sql
show plugins;
1
```

打印

```sql
+----------------------------+----------+--------------------+---------+---------+
| Name                       | Status   | Type               | Library | License |
+----------------------------+----------+--------------------+---------+---------+
| binlog                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| mysql_native_password      | ACTIVE   | AUTHENTICATION     | NULL    | GPL     |
| sha256_password            | ACTIVE   | AUTHENTICATION     | NULL    | GPL     |
| CSV                        | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| MEMORY                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| InnoDB                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| INNODB_TRX                 | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_LOCKS               | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_LOCK_WAITS          | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
...
| MyISAM                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| MRG_MYISAM                 | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| PERFORMANCE_SCHEMA         | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| ARCHIVE                    | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| BLACKHOLE                  | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| FEDERATED                  | DISABLED | STORAGE ENGINE     | NULL    | GPL     |
| partition                  | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| ngram                      | ACTIVE   | FTPARSER           | NULL    | GPL     |
+----------------------------+----------+--------------------+---------+---------+
44 rows in set (0.01 sec)                                                               
1234567891011121314151617181920212223
```

创建表

```sql
create table `login_log`(
    login_id int(10) unsigned not null comment '登录用户id',
    login_time timestamp not null default current_timestamp,
    login_ip int(10) unsigned not null comment '登录类型'
)engine=innodb default charset=utf8 partition by hash(login_id) partitions 4;
12345
```

打印

```sql
Query OK, 0 rows affected (0.06 sec)      
1
```

分为4个区，此时在数据文件中有4个文件login_log#p#p0.ibd、login_log#p#p1.ibd、login_log#p#p2.ibd、login_log#p#p3.ibd。
分区键：
分区引入了分区键的概念,分区键用于根据某个区间值、特定值、或者HASH函数值执行数据的聚集,让数据根据规则分布在不同的分区中。
分区类型：

- RANGE分区
- LIST分区
- HASH分区

无论哪种分区类型,要么分区表上没有主键/唯一键,要么分区表的主键/唯一键都必须包括分区键,也就是说不能使用主键/唯一字段之外的其他字段分区。

#### 分区表的原理

分区的主要目的是将数据按照一个较粗的粒度分在不同的表中， 这样可以将相关的数据存放在一起，而且如果想一次性删除整个分区的数据也很方便；
对用户而言,分区表是一个独立的逻辑表，但是底层MySQL将其分成多个物理子表，这对用户来说是透明的，每一个 分区表都会使用一个独立的表文件；
创建表时使用partition by子句定义每个分区存放的数据，执行查询时,优化器会根据分区定义过滤那些没有我们需要数据的分区,这样查询只需要查询所需要数据在的分区即可。

### 2.RANGE分区：

RANGE分区特点：

- 根据分区键值的范围把数据行存储到表的不同分区中；
- 多个分区的范围要连续,但是不能重叠；
- 分区不包括上限,取不到上限值。
  建立RANGE分区：

```sql
create table `login_log_range`(
    login_id int(10) unsigned not null comment '登录用户ID',
    login_time timestamp not null default CURRENT_TIMESTAMP,
    login_ip int(10) unsigned not null comment '登录ip'
)engine=innodb 
partition by range(login_id)(
partition p0 values less than(10000),   #实际范围0-9999
partition p1 values less than(20000),   #实际范围10000-19999
partition p2 values less than(30000),   #实际范围20000-29999
partition p3 values less than maxvalue  #存储大于30000的数据
);
1234567891011
```

打印

```sql
Query OK, 0 rows affected (0.04 sec)     
1
```

此时插入一条login_id大于30000的数据：

```sql
insert into login_log_range values(30001,now(),1)
1
```

打印

```sql
Query OK, 1 row affected (0.01 sec)      
1
```

分析SQL语句，查看分区

```sql
explain select * from login_log_range where login_id = 30001;
1
```

打印

```sql
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table           | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_range | p3         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)     
123456
```

显示位于p3分区。
查看id为500的记录对应的分区：

```sql
explain select * from login_log_range where login_id = 500;
1
```

打印

```sql
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table           | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_range | p0         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)     
123456
```

显示位于p0分区。
查看所有分区：

```sql
explain select * from login_log_range;
1
```

打印

```sql
+----+-------------+-----------------+-------------+------+---------------+------+---------+------+------+----------+-------+ 
| id | select_type | table           | partitions  | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra | 
+----+-------------+-----------------+-------------+------+---------------+------+---------+------+------+----------+-------+ 
|  1 | SIMPLE      | login_log_range | p0,p1,p2,p3 | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | NULL  | 
+----+-------------+-----------------+-------------+------+---------------+------+---------+------+------+----------+-------+ 
1 row in set, 1 warning (0.01 sec)                                                                                               
123456
```

RANGE分区使用场景：

- 分区键为日期或是时间类型；
- 经常运行包含分区键的查询,MySQL可以很快的确定只有某一个或某些分区需要扫描,例如检索商品login_id小于10000的记录数,MySQL只需要扫描p0分区即可；
- 定期按分区范围清理历史数据。

删除分区：

```sql
alter table login_log_range drop partition p0;
1
```

打印

```sql
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0  
12
```

### 3.HASH分区

HASH分区的特点：

- 根据MOD(分区键,分区值)的值把数据行存储到表的不同分区内；
- 数据可以平均地分布在各个分区中；
- HASH分区的键值必须是一个INT类型的值,或是通过函数可以转为INT类型。

建立HASH分区表：

```sql
create table `login_log_hash`(
    login_id int(10) unsigned not null comment '登录用户ID',
    login_time timestamp not null default CURRENT_TIMESTAMP,
    login_ip int(10) unsigned not null comment '登录ip'
)engine=innodb default charset=utf8 partition by hash(login_id) partitions 4;
12345
```

或者

```sql
create table `login_log_hash`(
    login_id int(10) unsigned not null comment '登录用户ID',
    login_time timestamp not null default CURRENT_TIMESTAMP,
    login_ip int(10) unsigned not null comment '登录ip'
)engine=innodb default charset=utf8 partition by hash(UNIX_TIMESTAMP(login_time)) partitions 4;
12345
```

打印

```sql
Query OK, 0 rows affected (0.04 sec)
1
```

插入数据

```sql
insert into login_log_hash values(1,now(),1);
insert into login_log_hash values(2,now(),2);
insert into login_log_hash values(3,now(),3);
insert into login_log_hash values(4,now(),4);
insert into login_log_hash values(5,now(),5);
12345
```

打印

```sql
Query OK, 1 row affected (0.00 sec)
                                   
Query OK, 1 row affected (0.00 sec)
                                   
Query OK, 1 row affected (0.00 sec)
                                   
Query OK, 1 row affected (0.00 sec)
                                   
Query OK, 1 row affected (0.00 sec)
123456789
```

查看分区

```sql
explain select * from login_log_hash where login_id = 1;
explain select * from login_log_hash where login_id = 2;
explain select * from login_log_hash where login_id = 3;
explain select * from login_log_hash where login_id = 4;
explain select * from login_log_hash where login_id = 5;
12345
```

打印

```sql
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p1         | ALL  | NULL          | NULL | NULL    | NULL |    2 |    50.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                               
                                                                                                                                 
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p2         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                               
                                                                                                                                 
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p3         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                               
                                                                                                                                 
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p0         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                               
                                                                                                                                 
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p1         | ALL  | NULL          | NULL | NULL    | NULL |    2 |    50.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                      
12345678910111213141516171819202122232425262728293031323334
```

显然，每条数据位于对应的分区。
查看所有分区：

```sql
explain select * from login_log_hash;
1
```

打印

```sql
+----+-------------+----------------+-------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table          | partitions  | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+----------------+-------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | login_log_hash | p0,p1,p2,p3 | ALL  | NULL          | NULL | NULL    | NULL |    4 |   100.00 | NULL  |
+----+-------------+----------------+-------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
123456
```

hash分区为全表扫描。

### 4.LIST分区

LIST分区特点：

- 按分区键取值的列表进行分区；
- 同范围分区一样,各分区的列表值不能重复；
- 每一行数据必须能找到对应的分区列表,否则数据插入失败。
  建立LIST分区

```sql
create table `login_log_list`(
    login_id int(10) unsigned not null comment '登录用户ID',
    login_time timestamp not null default CURRENT_TIMESTAMP,
    login_ip int(10) unsigned not null comment '登录ip',
    login_type int(10) not null
)engine=innodb 
partition by list(login_type)(
partition p0 values in(1,3,5,7,9),    
partition p1 values in(2,4,6,8)   
);
12345678910
```

打印

```sql
Query OK, 0 rows affected (0.03 sec)
1
```

插入值

```sql
insert into login_log_list values(1,now(),2,1);
1
```

打印

```sql
Query OK, 1 row affected (0.00 sec)
1
```

可以正常插入。
插入一条login_type列表中没有的数据

```sql
insert into login_log_list values(1,now(),2,10);
1
```

打印

```sql
ERROR 1526 (HY000): Table has no partition for value 10
1
```

会报错，即没有10对应的分区。

## 三、练习–如何选择合适的分区方式

业务场景：

- 用户每次登录都会记录到日志表中；
- 用户登录日志保存一年,一年后可以删除。

```sql
create table login_lg_log(
    login_id int(10) not null,
    login_time datetime not null default current_timestamp,
    login_ip int(10) not null
)engine = INNODB
partition by range(year(login_time))(
    partition p0 values less than(2015),
    partition p1 values less than(2016),
    partition p2 values less than(2017),
    partition p3 values less than(2018)
);
1234567891011
```

打印

```sql
Query OK, 0 rows affected (0.03 sec)
1
```

插入数据

```sql
insert into login_lg_log values
(1,'2015-01-01',1),
(2,'2015-01-02',1),
(3,'2016-01-01',1),
(4,'2016-01-01',1),
(5,'2017-01-01',1),
(6,'2017-01-01',1),
(7,'2017-01-01',1)
;
123456789
```

打印

```sql
Query OK, 7 rows affected (0.00 sec)
Records: 7  Duplicates: 0  Warnings: 0
12
```

查看分区

```sql
select table_name,partition_name,partition_description,table_rows from information_schema.partitions where table_name = 'login_lg_log';
1
```

打印

```sql
+--------------+----------------+-----------------------+------------+
| table_name   | partition_name | partition_description | table_rows |
+--------------+----------------+-----------------------+------------+
| login_lg_log | p0             | 2015                  |          0 |
| login_lg_log | p1             | 2016                  |          2 |
| login_lg_log | p2             | 2017                  |          2 |
| login_lg_log | p3             | 2018                  |          3 |
+--------------+----------------+-----------------------+------------+
4 rows in set (0.00 sec)
123456789
```

添加分区

```sql
alter table login_lg_log add partition(partition p4 values less than(2019));
1
```

打印

```sql
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

删除分区

```sql
alter table login_lg_log drop partition p0;
1
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

### 数据归档：

新建一个和要归档的表的结构一样的表：

```sql
create table login_lg_log_test(
    login_id int(10) not null,
    login_time datetime not null default current_timestamp,
    login_ip int(10) not null
)engine = INNODB;
12345
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)
1
```

分区迁移：

```sql
alter table login_lg_log exchange partition p1 with table login_lg_log_test;
1
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)
1
```

查看两个表：

```sql
select * from login_lg_log;
1
```

打印

```sql
+----------+---------------------+----------+
| login_id | login_time          | login_ip |
+----------+---------------------+----------+
|        3 | 2016-01-01 00:00:00 |        1 |
|        4 | 2016-01-01 00:00:00 |        1 |
|        5 | 2017-01-01 00:00:00 |        1 |
|        6 | 2017-01-01 00:00:00 |        1 |
|        7 | 2017-01-01 00:00:00 |        1 |
+----------+---------------------+----------+
5 rows in set (0.00 sec)                     
12345678910
```

即原表中p1分区的数据被移除。

```sql
select * from login_lg_log_test;
1
```

打印

```sql
+----------+---------------------+----------+
| login_id | login_time          | login_ip |
+----------+---------------------+----------+
|        1 | 2015-01-01 00:00:00 |        1 |
|        2 | 2015-01-02 00:00:00 |        1 |
+----------+---------------------+----------+
2 rows in set (0.00 sec)
1234567
```

新表中即为原表p1分区的数据。
使用分区表的注意事项：

- 结合业务场景选择分区键,避免跨分区查询；
- 对分区表进行查询最好在where从句中包含分区键；
- 具有主键或唯一索引的表,主键或唯一索引必须是分区键的一部分。

### 分区适用场景

- 表非常大，无法全部存在内存,或者只在表的最后有热点数据,其他都是历史数据；
- 分区表的数据更容易维护，可以对独立的分区进行独立的操作；
- 分区表的数据可以分布在不同的机器上,从而高效使用资源；
- 可以使用分区表来避免某些特殊的瓶颈；
- 可以备份和恢复独立的分区。

# Python全栈（三）数据库优化之13.MySQL高级-主从复制和数据库总结

## 一、主从复制

### 1.目的和解决的问题：

目的：读写分离，一个数据库只负责读，一个只负责写。
解决的问题：

- 数据分布:随意停止或开始复制，并在不同地理位置分布数据备份；
- 负载均衡:降低单个服务器的压力；
- 故障切换:帮助应用程序避免单点失败；
- 升级测试:可以使用更高版本的MySQL作为从库。

### 2.基本原理：

如图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vN2IvMjEvN2IyMWJhMzBmODdlZWFlYTE1ZGI1OTZhZmEzMDhkNjNfNTgzeDQyMS5wbmc#pic_center?x-oss-process=image/format,png)

### 3.复制的三步骤：

（1）master将改变记录到二进制日志，这些记录过程叫做二进制日志事件binary log events；
（2）slave将master的binary log events拷贝到它的中继日志；
（3）slave重做中继日志中的事件,将改变应用到自己的数据库中。
MySQL复制是**异步**的且**串行**的。

### 4.复制的基本原则：

- 每个slave只有一个master；
- 每个slave只能有一个唯一的服务器ID；
- 每个master可以有多个salve。

### 5.一主一从常见配置：

- MySQL版本一致且后台服务可以运行；
- 主从主机可以相互通信；
- 主从配置都在[mysqld]结点下,都是小写。

主机（Windows系统）配置my.ini：

```
server_id=2 #[必须]主服务器唯一ID，不能与从机重复
log-bin=自己本地的路径/mysqlbin（例如mysql-bin） #[必须]启用二进制日志
log-err = 自己本地的路径/mysqlerr #[可选]启用错误日志
123
```

从机（Linux Ubuntu系统）配置mysqld.cnf(/etc/mysql/mysql.conf.d/mysqld.cnf)：

```
bind-address=0.0.0.0 #让任何IP地址连接
server_id=1 #[必须]主服务器唯一ID
log-bin=自己本地的路径/mysqlbin（例如/var/log/mysql/mysql-bin.log） #[可选]启用二进制日志
123
```

修改过配置文件之后，要重启MySQL服务：

```shell
service mysql restart
1
```

关闭Linux防火墙：

```shell
service iptables stop
1
```

关闭Windows防火墙：
关闭**专用网络**防火墙。
在Windows主机上建立账户并授权slave：

```sql
create user 'slave'@'从机数据库IP' identified with mysql_native_password by 'password';
#例如create user 'zhangsan'@'192.168.1.1' identified with mysql_native_password by '123456';
12
```

授权并查看状态：

```sql
grant replication slave on *.* to 'slave'@'从机数据库IP' identified by 'password';
#例如grant replication slave on *.* to 'zhangsan'@'192.168.1.1' identified by '123456';

show master status;
1234
```

记录下File和position的值。

### 6.配置Linux从机

```
change master to master_host = '192.168.0.161',
master_user = 'jerry',
master_password = '123456',
master_log_file = 'binlog.000004',
master_log_pos= 908;
12345
```

### 7.测试是否配置成功

```
start slave;  #启动从服务器复制功能

show slave status;
123
```

下面两个参数都是yes,则说明主从配置成功：
`slave_io_running:yes`；
`slave_sql_running:yes`。

主从复制总结：
当读数据和写数据都是对同一个数据库进行操作时，可能会遇到性能瓶颈，所以新增一个与原数据库相同的数据库作为从数据库，即备份，实现**读写分离**（主数据库写，从数据库读），既能提高读写效率，又能提高安全性，但是会产生一定延迟。

## 二、MySQL操作规范

### 1.命名规范

- 表名建议使用有业务意义的英文词汇，必要时可加数字和下划线，并以英文字母开头。
- 库、表、字段全部采用小写：
  MySQL在Linux下默认是区分大小写的，而在Windows下不区分大小写。因此，防止出现问题，建议都设置为小写。
- 避免用 MySQL 的保留字。
- 命名（包括表名、列名）禁止超过 30 个字符。
- 临时库、表名必须以tmp为前缀，并以日期为后缀，如：tmp_shop_info_20200120。
- 备份库、表必须以bak为前缀，并以日期为后缀，如：bak_shop_info_20200120。
- 索引命名：
  非唯一索引必须按照"**idx_字段名称**"或"**idx_表名_字段名称**"进行命名；
  唯一索引必须按照"**uniq_字段名称**"或"**uniq_表名_字段名称**"进行命名。

### 2.设计规范

- 主键：
  表必须有主键；
  不使用更新频繁的列做主键；
  尽量不选择字符串列做主键；
  不使用 UUID（不重复的字符串）、MD5 HASH 做主键；
  默认使用非空的唯一键。
- 如无特殊要求，建议都使用 InnoDB 引擎。
- 默认使用 utf8mb4 字符集，数据排序规则使用 utf8mb4_general_ci：
  utf8mb4 为万国码，无乱码风险；与 utf8 编码相比，utf8mb4 能支持 Emoji 表情。
- 所有表、字段都需要增加 comment 来描述此表、字段所表示的含义：
  如`data_status TINYINT NOT NULL DEFAULT '1' COMMENT '1代表记录有效，0代表记录无效'`。
- 尽可能不使用 TEXT、BLOB 类型：
  原因：会浪费更多的磁盘和内存空间，非必要的大量大字段查询会淘汰掉热数据，导致内存命中率急剧降低，影响数据库性能。如果实在有某个字段过长需要使用TEXT、BLOB类型，则建议独立出来一张表，用主键来对应，避免影响原表的查询效率。字段用多大就取多大，避免资源浪费和性能下降。
- 单表列数目建议小于30。

### 3.SQL语句规范

- 避免隐式转换：
  varchar要加引号。
- 尽量不使用`select *`,只select需要的字段：
  读取不需要的列会增加CPU、IO、NET消耗，并且不能有效的利用覆盖索引。使用`SELECT *`容易在增加或者删除字段后导致程序报错。
- 建议将子查询转换为关联查询。
- 建议应用程序捕获SQL异常，并有相应处理：
  可以避免数据库攻击。

### 4.行为规范

- 批量导入、导出数据必须提前通知DBA协助观察；
- 不在业务高峰期批量更新、查询数据库；
- 删除表或者库要求尽量先重命名rename、备份，观察几天，确定对业务没影响，再drop

## 三、数据库基础总结

### 1.数据类型

- 整数：tinyint、smallint、mediumint、int、bigint；
- 实数：float、double；
- 字符串：varchar、char、text、blob；
- 日期时间：timestamp、datetime。

### 2.列属性

- 自增auto_increment；
- 默认值default；
- 非空not null；
- 零填充zerofill；
- 无符号unsigned。

### 3.数据库操作

连接数据库：

```shell
mysql -uroot -p #更安全
或者
mysql -u root -proot
123
```

退出数据库：

```sql
exit
--或者quit
12
```

查看所有数据库：

```sql
show databases;
1
```

创建数据库：

```sql
#创建数据库
create database 数据库名字 charset = utf8;
#查看创建数据库的命令
show create database 数据库名字;
#使用数据库
use mydatabase;
123456
```

### 4.数据库表操作

查看当前数据库中所有表：

```sql
show tables;
1
```

创建表：

```sql
create table 数据表名字(字段 类型 约束[,字段 类型 约束]);
1
```

查看表：

```sql
desc demo1;
1
```

查看表的创建语句：

```sql
show create table salary;
1
```

### 5.DML数据库管理语言

新增：

```sql
#全列插入：
insert [into] 表名 values(...);
#部分插入：
insert into 表名(列1 ,...) values(值1 ,...);
1234
```

修改：

```sql
update 表名 SET 字段1=新值,字段2=新值[where 条件];
update 表名 SET 字段1=新值,字段2=新值;
12
```

删除：

```sql
delete from 表名 [where条件];
delete from 表名; #清空表中的数据
12
```

查询：
where子句：=、>、<
逻辑运算符：and、or、not
模糊查询：%、_
范围查询：in、between…and…
空判断：is null、is not null
聚合函数：count、max、min、sum、avg
分组：group by
排序：asc、desc
分页：limit

## 四、MySQL-视图、索引、事务、引擎总结

### 1.视图

（1）作用：
视图是**虚拟的表**，只包含动态检索数据的查询，不包含数据；
简化操作,隐藏细节，保护数据；
对视图的更新会作用于基表，一般不更新。

（2）视图操作
定义视图：

```sql
create view 视图名称 as select语句;
1
```

查看视图：

```sql
show tables;
1
```

使用视图：

```sql
select * from v_stu_score;
1
```

删除视图：

```sql
drop view 视图名称;
1
```

### 2.索引

（1）索引类型：
唯一索引：具有唯一性约束，不允许为空；
主键索引：特殊的唯一索引，不允许有空值；
单值索引：单列；
复合索引：将多个列组合在一起创建索引，可以覆盖多个列。
（2）索引操作：
创建索引：

```sql
create [unique] index 索引名称 on 表名(字段名称(长度));
1
```

查看索引：

```sql
show index from 表名;
1
```

删除索引：

```sql
drop index 索引名称 on 表名;
1
```

（3）索引对性能的影响：

- 大大减少服务器需要扫描的数据量；
- 帮助服务器避免排序和临时表；
- 将随机I/O变顺序I/O；
- 大大提高查询速度，降低写的速度，占用磁盘。

（4）索引的使用场景：

- 对于非常小的表，大部分情况下全表扫描效率更高；
- 中到大型表，索引非常有效；
- 特大型的表，建立和使用索引的代价将随之增长,可以使用分区技术来解决。

（5）唯一索引与主键索引的区别：

- 一个表只能有一个主键索引，可以有很多个唯一索引；
- 主键索引一定是唯一索引，唯-索引不是主键索引；
- 主键可以与外键构成参照完整性约束，防止数据不一致。

（6）不建立索引的情况：

- 频繁更新的字段不适合建立素引；
- where条件里面用不到的字段不创建索引；
- 表记录太少，当表中数据量超过三百万条数据，可以考虑建立素引；
- 数据重复且平均的表字段，比如性别、国籍。

### 3.事务

（1）ACID特性：

- A原子性
- C一致性
- I隔离性
- D持久性

（2）事务操作
开启事务：

```sql
begin;
start transaction;
12
```

提交事务：

```sql
commit;
1
```

回滚事务：

```sql
rollback;
1
```

### 4.存储引擎

（1）InnoDB：
支持事务，支持行级锁，支持外键。
（2）MyISAM：
不支持事务和表级锁，不支持外键。
（3）CSV：
以文本形式存储，不支持索引和自增。
（４）Memory：
存储在内存中，服务重启后数据丢失。
（５）选择：
根据事务、崩溃恢复、备份三个特性选择。

## 五、MySQL优化总结

### 1.分析慢SQL方法

（1）记录慢查询日志

- 开启慢查询日志`set global slow_ query_ log = 1;`
- 设置阙值，默认是10秒`set global long_ query_ time = 3;`
- 查看多少条慢SQL`show global status like "%slow_queries%';`

（2）使用explain
用法：`explain + sql`
（3）使用show profile

- `set profiling = 1;`
- `show profiles;`
- `show profile cpu,block io for query 临时表ID;`

### 2.SQL语句优化

- 选择正确的存储引擎；
- 优化字段的数据类型；
- 为搜索字段添加索引；
- 避免使用`select *`，从数据库里读出越多的数据，查询就会变得越慢；
- 尽可能使用**NOT NULL**。

### 3.SQL注释

| 符号  | 举例                                                    |
| ----- | ------------------------------------------------------- |
| #     | SELECT * FROM USER WHERE NAME = ‘1aa1’ #and age = 22    |
| –空格 | SELECT * FROM USER WHERE NAME = ‘1aa1’ – and age = 22   |
| /**/  | SELECT * FROM USER WHERE NAME = ‘1aa1’ /*and age = 22*/ |

### 4.分区

（1）原理
分区的主要目的是将数据按照一个较粗的粒度分在不同的表中, 这样可以将相关的数据存放在一起，而且如果想一次性删除整个分区的数据也很方便。
对用户而言,分区表是-一个独立的逻辑表，但是底层MySQL将其分成多个物理子表，这对用户来说是透明的，每一个 分区表都会使用一个独立的表文件。
（2）方式

- RANGE分区
- LIST分区
- HASH分区

（3）适用场景

- 表非常大，无法全部存在内存,或者只在表的最后有热点数据,其他都是历史数据；
- 分区表的数据更容易维护，可以对独立的分区进行独立的操作；
- 分区表的数据可以分布在不同的机器上,从而高效使用资源；
- 可以使用分区表来避免某些特殊的瓶颈；
- 可以备份和恢复独立的分区。

### 5.主从复制

（1）原理

- 在主库上把数据更改记录到二进制日志；
- 从库将主库的日志复制到自己的中继日志；
- 从库读取中继日志中的事件,将其重放到从库数据中。

（2）解决的问题

- 数据分布；
- 负载均衡；
- 故障切换；
- 升级测试。

### 6*.SQL查询的安全方案

- 使用预处理语句防止SQL注入；
- 写入数据库的数据要进行特殊字符的转义；
- 查询错误信息不要返回给用户，将错误记录到日志