## sql语句的在mysql基本操作

>创建数据库和判断

数据库中column和field的区别
Field: smallest unit of application data recognized by system software指数据字段，数据记录中已有定义的部分，例如数据库表中的一列，但一般特指某列某行（数据库里的特定一格）当我说field的时候，不仅要说特定格里的值还要说column name a column is a set of data values of a particular simple type, one value for each row of the database.指列，一列所有数据

drop database if exists testdb;//不存在就创建

create database testdb;
-- 使用数据库
use testdb;

```mysql
--  create table test （字段名 类型 长度 primary key主键 约束<约束默认值，comment注释>）;
drop database if exists testdb1;
create database testdb1;
use testdb1;
drop table if exists tb_test1;
create table tb_test1
(
test_id integer(10) primary key comment'测试id',
test_name varchar(30) not null comment'测试名称'
);
```

创建两个

```sql
drop database if exists testdb1;
create database testdb1;
use testdb1;
drop table if exists tb_test1;
create table tb_test1
(
test_id integer(10) AUTO_INCREMENT primary key comment'测试id',
test_name varchar(30) not null comment'测试名称',
test_pwd varchar(20) not null default '88888' comment'默认密码'
);
create table tb_test2
(use_id integer(10)auto_increment primary key comment '用户编号',
use_name varchar(20) not null comment'用户名',
use_birthday date,
use_gener char(3),
use_state tinyint(1) not null,
use_height decimal(4,2) not null,
user_decribe text
)ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4;##支持中文
```

> MYSQL支持所有SQL数据类型

| 类型         | 大小                                     | 范围（无符号）                                               | 范围（有符号）                                               | 用途            |
| :----------- | :--------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------- |
| TINYINT      | 1 Bytes                                  | (0，255)                                                     | (-128，127)                                                  | 小整数值        |
| SMALLINT     | 2 Bytes                                  | (0，65 535)                                                  | (-32 768，32 767)                                            | 大整数值        |
| MEDIUMINT    | 3 Bytes                                  | (0，16 777 215)                                              | (-8 388 608，8 388 607)                                      | 大整数值        |
| INT或INTEGER | 4 Bytes                                  | (0，4 294 967 295)                                           | (-2 147 483 648，2 147 483 647)                              | 大整数值        |
| BIGINT       | 8 Bytes                                  | (0，18 446 744 073 709 551 615)                              | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)      | 极大整数值      |
| FLOAT        | 4 Bytes                                  | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 单精度 浮点数值 |
| DOUBLE       | 8 Bytes                                  | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

------

## 日期和时间类型

表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。

每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。

TIMESTAMP类型有专有的自动更新特性，将在后面描述。

| 类型      | 大小 ( bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4             | 1970-01-01 00:00:00/2038结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

------

## 字符串类型

字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

tinyint 一个字节 TINYINT

default 默认为8888

integer int长度限制为10

date 日历

varchar(20)可字符串最大长度为255

char定长字符串最大255字节没有给定长度用空格补充

单精度decimal(4,1) 表示4位数小数保留2 位-最大（6，3）

int不能后面设置其他数据类型只能填充int类型

auto_increment 自增长

>Navicat Premium常用快捷键

1.ctrl+r 运行当前查询窗口的所有sql语句

**2.ctrl+shift+r 只运行选中的sql语句**

**3.ct m rl+/ 注释sql语句**

**4.ctrl+shift +/ 解除注释**

5.ctrl+q 打开查询窗口

6.ctrl+n 打开一个新的查询窗口

7.ctrl+w 关闭当前查询窗口

8.ctrl+l 删除一行

9.Shift+Home 鼠标在当前一行末尾，按快捷选中当前一行

10. F6: 打开一个mysql命令行窗口

11. ctrl + l: 删除一行

12. F7: 运行从光标当前位置开始的一条完整sql语句

## alter和DROP

 删除和修改列名或数据库名

```sql
alter table tb_test2 add use_phone varchar(20) not null comment'添加';
-- 修改类型和约束条件
alter table tb_test2 modify use_phone int not null comment'修改类型';
-- 修改列名
alter table tb_test2 change use_phone change_use2 int not null comment'修改列名';
```

```sql
--删除索引
ALTER TABLE table_name DROP INDEX index_name
```

查询

```sql
desc tb_test2 
```



删除表的列行

```sql
alter table tb_test2 drop use_phone;##删除一个不存在
```

> 1091 - Can't DROP 'use_phone'; check that column/key exists
>
> 

```sql
alter table 表名 drop 列行名
alter table tb_test2 drop change_use2;##删除一个存在
--不提醒已删除，重复删除会报错
```

添加列add，最好有约束条件

```sql
alter table user_info add use_phone varchar(20) not null comment'添加';
```

修改表名

```sql
##修改表名
alter table tb_test2 rename to user_info;
##修改引擎
alter table user_info engine=myisam;##默认为InnoDB
```

## insert into 插入表数据

```sql
insert into user_info(use_name,use_birthday,use_gener,use_state,use_height,user_decribe)values("dada","2022-07-08","2",1.78,4,"golang")
insert into 表名（字段名称，。。。）values（插入的值）
```

插入类型要匹配

在插入指定的列的值的时候要看其他列是否有not null的约束不然会报错

NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

## update set字段修改

```sql
update user_info set use_birthday="2021-04-06" where use_id=1; 
update(表名)set修改字段=值，。。。where（主键不变值或者其他）
```

## where筛选查询和全部查询

```sql
 select *from 表名；
 -- where可以通过比较（>,<,<=,>=,!=,=）来查询结果
 select *from user_info where use_gener="男";
  select *from user_info where use_height>=1;
```

```sql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
 -- WHERE BINARY 可以区分大小写
 -- SELECT * from 表一，表二 WHERE BINARY 指定的表列="内容";
 --可以找出大写的或者小写的
 
```

## delete删除表内数据

```sql
delete from 表名 where 删除条件 and 其他条件;
delete from user_info where use_id=2;
```

## 通配符%模糊查找

```sql
select *from 表名  where 位置 like %（%表示通配符）
-- 有'%xx%'无论是否在一起
-- 有'xx%'Z在首个位置
select *from 表名  where 位置 like %xx%（%表示通配符）
select *from user_info where use_name like "ke%";

```

## distinct 唯一值

在表中，可能会包含重复值。这并不成问题，不过，有时您也许希望仅仅列出不同（distinct）的值。

关键词 DISTINCT 用于返回唯一不同的值。

```sql
SELECT DISTINCT 列名称 FROM 表名称
```

```sql
SELECT distinct user_decribe FROM user_info;
--会把同列中重复的部分不显示
```



## group by 分组查询

配合函数 conunt（feild)获取符合条件的非null值

```sql
-- GROUP BY分组查询
SELECT COUNT(use_id) from user_info

-- 3
```

sum获取相加值

```sql
--不带条件
SELECT SUM(use_height) 用户总身高 from user_info
--可以带条件
SELECT SUM(use_height) 用户总身高 from user_info WHERE use_name like '%xiao%';
--- 
```

avg平均值

```sql
SELECT AVG(Use_height)用户平均身高 from user_info WHERE use_name like '%xiao%';
-- 1.733333
```

以性别分组

```sql

SELECT AVG(Use_height)用户男女各平均身高 from user_info group by use_gener;
--1.800000
--1.700000
SELECT use_gener as 性别,AVG(Use_height)用户总身高 from user_info group by use_gener;
```

![](images\test7.png)

如果想让其他分组有显示需要在group by 后面添加需要的

```sql
SELECT use_state,use_gener as 性别,AVG(Use_height)用户总身高 from user_info group by use_state,use_gener;
```

![test8](images\test8.png)

## order by 排序查询

默认升序排列ASC / desc

```sql
-- 升序
SELECT *FROM user_info order by use_id ASC;
--降序
SELECT *FROM user_info order by use_id desc;
```

带条件查询

```sql
SELECT *FROM user_info WHERE use_id>2 order by use_id ASC;
```

![](images\test9.png)

配合使用order by 一般放GROUP BY后面

```sql
select use_gener as 性别,avg(use_height)平均,SUM(use_height)总身高 from user_info GROUP BY use_gener ORDER BY 总身高 ASC;
```

![test10](images\test10.png)

## limt

> sql 中叫 top

```sql
SELECT * FROM user_info LIMIT 1,3;
--查找1-3列的数据
--搭配offset可以跨行
SELECT * FROM user_info LIMIT 5 OFFSET 2 ;
--从第二行开始显示
```

```sql
SELECT column_name(s)
FROM table_name
LIMIT number
```

## between 

BETWEEN 操作符在 WHERE 子句中使用，作用是选取介于两个值之间的数据范围。

```sql
SELECT * FROM 表名 WHERE 表列 BETWEEN 'xx' AND 'xx'
```

## in

IN 操作符允许我们在 WHERE 子句中规定多个值。

```sql
SELECT *FROM 表名 WHERE 列名 IN (value1,value2,...)
```

## 多表查询

查询多张表的语法是：`SELECT * FROM <表1> <表2>`。

## join   

连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。

SELECT COUNT(*) FROM 表名;

- **INNER JOIN（内连接,或等值连接）**：获取两个表中字段匹配关系的记录。

有时为了得到完整的结果，我们需要从两个或更多的表中获取结果。我们就需要执行 join。

```
FROM 表名1 INNER JOIN 表名2 ON 表名.主键 = Orders.主键
ORDER BY Persons.LastName
```

```sql
--创建两个表
连接查询
 SELECT a.use_gener, b.use_state FROM user_info a INNER JOIN score b ON a.use_id = b.use_id;
```

![score](images\score.png)<img src="D:\workspace\weidaidai.github.io\docs\images\use_info.png" alt="use_info" />

查询如下

![test11](images\test11.png)

**LEFT JOIN（左连接）：**获取左表所有记录，即使右表没有对应匹配的记录。

以 **runoob_tbl** 为左表，**tcount_tbl** 为右表

runoob_tb1表

![test12](images\test12.png)

tcount_tb1表

![test13](images\test13.png)

```sql
SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a LEFT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
```

![test14](images\test14.png)

**RIGHT JOIN（右连接）：** 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录

```sql
SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a RIGHT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
```

以 **runoob_tbl** 为左表，**tcount_tbl** 为右表

![test15](images\test15.png)

## 索引

索引分单列索引和组合索引。单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。组合索引，即一个索引包含多个列。

创建索引时，你需要确保该索引是应用在 SQL 查询语句的条件(一般作为 WHERE 子句的条件)。

## 普通索引

#### 创建索引

这是最基本的索引，它没有任何限制。它有以下几种创建方式：

```sql
CREATE INDEX indexName ON 表名 (表列名)
```

#### 删除索引的语法

```
DROP INDEX [indexName] ON 表名; 
```

如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length。

#### 修改表结构(添加索引)

```
ALTER table 表名 ADD INDEX indexName(表列名)
```

## 事务处理

参考：https://www.runoob.com/mysql/mysql-transaction.html

*在 MySQL 命令行的默认设置下，事务都是自动提交的，即执行 SQL 语句后就会马上执行 COMMIT 操作。因此要显式地开启一个事务务须使用命令 BEGIN 或 START TRANSACTION，或者执行命令 SET AUTOCOMMIT=0，用来禁止使用当前会话的自动提交。*

一般来说，事务是必须满足4个条件（ACID）：：原子性（**A**tomicity，或称不可分割性）、一致性（**C**onsistency）、隔离性（**I**solation，又称独立性）、持久性（**D**urability）。

- **原子性：**一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
- **一致性：**在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。
- **隔离性：**数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。
- **持久性：**事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

MYSQL 事务处理主要有两种方法：

> 1、用 BEGIN, ROLLBACK, COMMIT来实现

- **BEGIN** 开始一个事务
- **ROLLBACK** 事务回滚(rollback 回到原来的状态)
- **COMMIT** 事务确认 （提交）

> 2、直接用 SET 来改变 MySQL 的自动提交模式:

- **SET AUTOCOMMIT=0** 禁止自动提交
- **SET AUTOCOMMIT=1** 开启自动提交

事务测试