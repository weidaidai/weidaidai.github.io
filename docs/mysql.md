> docker上安装
>
> 遇到power 无法识别中文 可以输入临时命令chcp 65001

mysql中文显示乱码问题

<img src="images\mysql.PNG" style="zoom:60%;" />

首先查看

```sql
 show variables like '%char%';
 
 -- 设置
 set character_set_results=utf8;
 set character_set_connection=utf8;
 set character_set_client=utf8;
 ....
 -- 除了
 character_set_filesystem
```

如果还是不行查看表或者行是否没设置utf8

```text
---查看表
show table status from test like '表名'\G;
如果有
 ALTER TABLE 表名 CONVERT TO CHARACTER SET utf8 需要修改的项;

---查看列 
show full columns from product\G;
```

安装mysql-docker

docker 可以执行如下命令一步搭建MySQL数据库：

```
docker run --name mysql -v $PWD/mysql:/var/lib/mysql -p3306:3306 -eMYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

命令中显示我们使用的是Docker技术并创建一个名字为mysql的容器，然后在容器中把数据关联到本地的MySQL，并把MySQL的3306接口映射到外面的3306，同时为用户设置一个账户密码

- –name：容器名，此处命名为`mysql`

- -e：配置信息，此处配置mysql的root用户的登陆密码

- -p：端口映射，此处映射 主机3306端口 到 容器的3306端口

- -d：后台运行容器，保证在退出终端后容器继续运行

  > docker exec -it mysql(或者容器id) bash

  > mysql -u root -p  在设置密码的时候使用

## sql语句的在mysql基本操作

>创建数据库和判断

数据库中`column`和`field`的区别
Field: smallest unit of application data recognized by system software指数据字段，数据记录中`已有定义的部分`，例如数据库表中的一列，但一般特指`某列某行`（数据库里的`特定一格`）

column 指列，`一列所有数据`

column name a column is a set of data values of a particular simple type, one value for each row of the database.



`drop database if exists testdb`;//不存在就创建

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

tinyint` 一个字节 ,小整数

`default` 默认为8888,初始密码

`integer `int长度限制为10

`varchar`(20)可字符串最大长度为255,现在设置为20

`char`定长字符串最大255字节没有给定长度用空格补充

单精度`decimal`(4,1) 表示4位数小数保留2 位-最大（6，3）

`int`不能后面设置其他数据类型只能填充int类型

`auto_increment` 自增长

### 

### mysql支持的数据类型

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

> 日期和时间类型

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

> 字符串类型

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

### 创建临时表

创建临时表的基本语法如下：

多一个temporary

```sql
CREATE TEMPORARY TABLE table_name(
   column1 datatype,
   column2 datatype,
   column3 datatype,
   .....
   columnN datatype,
   PRIMARY KEY( one or more columns )
);
```

删表也一样

drop table table_name

### 外键

> 外键foreign key 和外键references

假设两张表,
表1(学号,姓名,性别),学号为主键.
表2(学号,课程,成绩).
可以为表2的学号定义外键(FOREIGN KEY),该外键的取值范围参照(REFERENCES)表1的学号

比如我们创两个表

```sql
-- 分类表
CREATE TABLE category ( cid VARCHAR ( 32 ) PRIMARY KEY, cname VARCHAR ( 50 ) );

-- 商品表
CREATE TABLE products (
	pid VARCHAR ( 32 ) PRIMARY KEY,
	pname VARCHAR ( 50 ),
	price INT,
	flag VARCHAR ( 2 ),-- 是否上架标记为：1表示上架、0表示下架
	category_id VARCHAR ( 32 ),
	FOREIGN KEY ( category_id ) REFERENCES category ( cid ) --外键的取值范围参照
);

-- 分类数据
INSERT INTO category(cid,cname) VALUES('c001','家电');
INSERT INTO category(cid,cname) VALUES('c002','鞋服');
INSERT INTO category(cid,cname) VALUES('c003','化妆品');
INSERT INTO category(cid,cname) VALUES('c004','汽车');

-- 商品数据
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p001','小米电视机',5000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p002','格力空调',3000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p003','美的冰箱',4500,'1','c001');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p004','篮球鞋',800,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p005','运动裤',200,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p006','T恤',300,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p007','冲锋衣',2000,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p008','神仙水',800,'1','c003');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p009','大宝',200,'1','c003');

--查询
SELECT *FROM products;

```

![外键](images\外键.png)

> Navicat Premium常用快捷键

如果有外键在执行drop table 操作的时候是有外键约束无法执行删表操作

SET foreign_key_checks = 0;  // 先设置外键约束检查关闭

drop table table1;  // 删除表，如果要删除视图，也是如此

SET foreign_key_checks = 1; // 开启外键约束检查，以保持表结构完整性

> 注意在添加外键的时候要注意和主键的类型要相同

### 表存在时候添加外键

`user` 表：id 为主键

`profile` 表： uid 为主键

简单来说，若表 `profile` 的 `uid` 列 作为表外键（外建名称：`user_profile`），以表 `user` 做为主表，以其 id列 做为参照（`references`），且联动删除/更新操作（`on delete/update cascade`）。则 `user` 表 中删除 `id` 为 1 的记录，会联动删除 `profile` 中 `uid` 为 1 的记录。

B 存在外键 `b_f_k`，以 A 表的 `a_k` 作为参照列，则 A 为主表，B 为从表，A 中某记录更新或删除时将会联动 B 中外键与其关联对应的记录做更新或删除操作。

```sql
alter table `profile` 
add constraint `user_profile` foreign key (`uid`) 
references `user`(`id`) on delete cascade on update cascade;
```

> 小结

首先要创建一个字段：alter table 表名 add 字段名 字段类型;
 再添加外键约束:alter table 需加外键的表 add constraint 外键名 foreign key(需加外键表的字段名) references 关联表名(关联字段名);
外键名不能重复

> navicat 快捷键

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

查询表列属性

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

## insert into 

> insert into +(column1,column2...)+ values+(values1,values2...)插入表数据
>
> insert into 表名（字段名称，。。。）values（插入的值）

```sql
insert into user_info(use_name,use_birthday,use_gener,use_state,use_height,user_decribe)values("dada","2022-07-08","2",1.78,4,"golang")

```

插入类型要匹配

在插入指定的列的值的时候要看其他列是否有not null的约束不然会报错

NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

## update table_name +set 

> update +table_name +set +field  where primary key();

```sql
update user_info set use_birthday="2021-04-06" where use_id=1; 
update(表名)set修改字段=值，。。。where（主键不变值或者其他）
-- 一般用id作为主键来标识位，有些主键不是int类型有可能是varchar这时候要注意加引号
```

## where筛选查询和全部查询

```sql
SELECT 列名称, 列名称2,...fieldN 
FROM table_name
WHERE field1 LIKE condition1 [AND [OR]] 列名称 = 'somevalue'
-- 查询

select use_gener from user_info2 where use_gener="男";

-- 和in 配合(查id为2到3数据)
select user_decribe,use_id from user_info2  where  use_id in(1,3);
select user_decribe,use_id from user_info2  where  user_decribe in('a','b','c');
---和or 配合
select user_decribe,use_id from user_info2  where  user_decribe='a'or user_decribe='b'
--和like 配合
select user_decribe from user_info2  where user_decribe like "%a%";  
---加上order by 排序
select user_decribe,use_id from user_info2  where user_decribe like "%a%" order by use_id DESC;  
```



```sql

 select *from 表名；
 -- where可以通过比较（>,<,<=,>=,!=,=）来查询结果
 select *from user_info where use_gener="男";
  select  user_info2 where use_height>=1;
```

```sql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
 -- WHERE BINARY 可以区分大小写
 -- SELECT * from 表一，表二 WHERE BINARY 指定的表列="内容";
 --可以找出大写的或者小写的
 
```

替代如果表

```sql
--replace 和 update 功能差不多
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

## delete删除表内数据

```sql
sql语法：DELETE FROM table_name [WHERE Clause]
delete from 表名 where 删除条件 and 其他条件;
delete from user_info where use_id=2;
-- 最后，要特别小心的是，和UPDATE类似，不带WHERE条件的DELETE语句会删除整个表的数据：

DELETE FROM table_name;(会把表内全部删除)
```

## 通配符%模糊查找

```sql
select column from TABLE_NAME  where column like %（%表示通配符）
-- 有'%xx%'无论是否在一起
-- 有'xx%'在首个位置
select *from 表名  where 位置 like %xx%（%表示通配符）
select *from user_info where use_name like "ke%";
-- 单一表查询
SELECT user_decribe from user_info2 WHERE user_decribe LIKE "e%";

```

```sql
'%a'     //以a结尾的数据
'a%'     //以a开头的数据
'%a%'    //含有a的数据
'_a_'    //三位且中间字母是a的
'_a'     //两位且结尾字母是a的
'a_'     //两位且开头字母是a的
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

## union 两表不重复元素

注意：要求两表的列长一致

> sql语法：select+列名称 +from +table1 union select +列名称+from +table2;

```SQL
select name from user_2 union SELECT use_gener FROM user_info2;
```

**UNION 语句**：用于将不同表中相同列中查询的数据展示出来；（不包括重复数据）

**UNION ALL 语句**：用于将不同表中相同列中查询的数据展示出来；（包括重复数据）

```

SELECT 列名称 FROM 表名称 UNION ALL SELECT 列名称 FROM 表名称 ORDER BY 列名称；
```

## group by 分组查询

> 配合函数 conunt（feild)获取符合条件的非null值

```sql
-- GROUP BY分组查询
SELECT COUNT(use_id) from user_info

-- 3
```

## 函数

内建 SQL 函数的语法是：

```sql
SELECT function(列) FROM 表 
```

> length（）--字符串长度 ，sql中为len()

```sql
select length(列) from table_name;
```

> round() 对数字进行四舍五入，最接近的整数，后面+保留多少个分数

```sql
 select round(use_height,1) from user;
```

>ucase() 转换大写
>
>lcase() 转换为小写

> now()显示现在的日期时间

```bash
mysql> select round(use_height,1) ,now() 现在的日期 from user;
+---------------------+---------------------+
| round(use_height,1) | 现在的日期          |
+---------------------+---------------------+
| 1.6                 | 2021-11-29 09:17:27 |
| 1.8                 | 2021-11-29 09:17:27 |
| 5.3                 | 2021-11-29 09:17:27 |
| 1.7                 | 2021-11-29 09:17:27 |
| 1.6                 | 2021-11-29 09:17:27 |
+---------------------+---------------------+
```



> MySQL不支持FIRST()，LAST()函数。可以返回第一个值和最后一个值

> sum获取相加值

```sql
--不带条件
SELECT SUM(use_height) 用户总身高 from user_info
--可以带条件
SELECT SUM(use_height) 用户总身高 from user_info WHERE use_name like '%xiao%';
--- 
```

> avg平均值

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
SELECT use_state 状态,use_gener 性别,AVG(Use_height)用户总身高 from user_info group by use_state,use_gener;
```

![test8](images\test8.png)

## having 分组筛选

如果想要从 GROUP BY 分组中进行筛选的话，不是用 WHERE 而是使用 HAVING 来进行聚合函数的筛选。

HAVING 子句写法：

**HAVING 子句必须写在GROUP BY 子句之后，其在DBMS 内部的执行顺序也排在GROUP BY 子句之后。**

```xml
SELECT <列名1>, <列名2>, <列名3>, ……
FROM <表名>
GROUP BY <列名1>, <列名2>, <列名3>, ……
HAVING <分组结果对应的条件>
```

- WHERE 子句 = 指定行所对应的条件

- HAVING 子句 = 指定组所对应的条件

- WHERE 处理速度比 HAVING 处理速度高

  

```sql

mysql> select use_height,count(use_height)
    -> from user
    -> group by use_height;
+------------+-------------------+
| use_height | count(use_height) |
+------------+-------------------+
| 1.55       |                 1 |
| 1.65       |                 1 |
| 1.80       |                 1 |
| 5.30       |                 1 |
+------------+-------------------+
4 rows in set (0.02 sec)

mysql> select use_height,count(*)
    -> from user
    -> group by use_height
    -> having use_height=1.55;
+------------+----------+
| use_height | count(*) |
+------------+----------+
| 1.55       |        1 |
+------------+----------+
1 row in set (0.02 sec)
-- 条数为2的
mysql> select use_height,count(*)
    -> from user
    -> group by use_height
    -> having count(*)=2;
Empty set

```



## as 别名（可以不写直接起别名）

**Alias** 别名

对table 进行取别名，用别名可以直接调用列

```sql
select s.use_height,count(*)

FROM user as s --user起别名

group by s.use_height;
```



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
select use_gener 性别,avg(use_height)平均,SUM(use_height)总身高 from user_info GROUP BY use_gener ORDER BY 总身高 ASC;
-- 注意：对于ORDER BY +COMMENT 一定要对应前面对应的 COMMENT 
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
SELECT 列名 from 表名 WHERE 列名 BETWEEN val AND val;

-- 查询
SELECT use_id,user_decribe FROM user_info2 WHERE use_id BETWEEN 2 AND 3;
```

![](D:\workspace\weidaidai.github.io\docs\images\between.png)

## in

IN 操作符允许我们在 WHERE 子句中规定多个值。

```sql
SELECT *FROM 表名 WHERE 列名 IN (value1,value2,...)
SELECT *FROM user_info WHERE use_id IN (1,2);
```

![](images\MYSQL_IN.PNG)

```sql
SELECT *FROM user_info WHERE use_id=1 OR use_id=2; //结果一样
```

```
SELECT use_id,user_decribe FROM user_info2 limit 1,3;
```



## 多表查询

查询多张表的语法是：`SELECT * FROM <表1> <表2>`。

> 聚合查询 w

除了`COUNT()`函数外，SQL还提供了如下聚合函数：

| 函数 | 说明                                   |
| :--- | :------------------------------------- |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值                     |
| MIN  | 计算某一列的最小值                     |

查看有多少条记录

```sql
SELECT COUNT(*) from user_info2;

--20
```



## join   

连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。

SELECT COUNT(*) FROM 表名;

- **INNER JOIN（内连接,或等值连接）**：获取两个表中字段匹配关系的记录。

有时为了得到完整的结果，我们需要从两个或更多的表中获取结果。我们就需要执行 join。

MySQL的INNER JOIN(也可以省略 INNER 使用 JOIN，效果一样)来连接以上两张表来读取runoob_tbl表中所有runoob_author字段在tcount_tbl表对应的runoob_count字段值,相同部分

```
SELECT  A.column1,b.column2 FROM TABLE_NAME1 A JOIN TABLE_NAME2 B ON A.column1=b.column2
//以上是使用别名的
select column1,column2  TABLE_NAME1 JOIN TABLE_NAME2 ON TABLE_NAME1 .column1=TABLE_NAME2.column2

```

```sql
--创建两个表
--连接查询
--接下来我们就使用MySQL的INNER JOIN(也可以省略 INNER 使用 JOIN，效果一样)来连接以上两张表来读取runoob_tbl表中所有runoob_author字段在tcount_tbl表对应的runoob_count字段值：
 SELECT a.runoob_id , a.runoob_author, b.runoob_count FROM runoob_tbl a INNER JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
```

![](images\JOIN.PNG)

> join查看交集元素 比如现在查看手机

```sql
SELECT b.SId 学生id,student_phone 手机 FROM user_info A JOIN Student B ON B.student_phone=A.change_use2; 
```

| 苹果<br/>改变值<br/>小米<br/>魅族<br/>vivo<br/>小米     change_use2内容 |
| ------------------------------------------------------------ |
| 三星<br/>小米<br/>华为         student_phone内容             |
| `SELECT b.SId 学生id,student_phone 手机 FROM user_info A JOIN Student B ON B.student_phone=A.change_use2;` |

查询结果

![](images\join3.PNG)

**LEFT JOIN（左连接）：**获取左表a表所有记录，即使右b表没有对应匹配的记录。

以 **runoob_tbl** 为a表，**tcount_tbl** 为b表

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

## SQL逻辑运算

[Mysql 逻辑运算符详解文章来源](https://www.cnblogs.com/xuchunlin/p/6235373.html)

逻辑运算符又称为布尔运算符，用来确认表达式的真和假。MySQL 支持4 种逻辑运算符，如表4-3 所示。

表4-3             MySQL 中的逻辑运算符

| 运算符     | 作用     |
| ---------- | -------- |
| NOT 或！   | 逻辑非   |
| AND 或&&   | 逻辑与   |
| OR 或 \|\| | 逻辑或   |
| XOR        | 逻辑异或 |



 　““NOT”或“！”表示逻辑非。返回和操作数相反的结果：当操作数为0（假），则返回值为1，否则值为0。但是有一点除外，那就是NOT NULL 的返回值为NULL，这一点请大家注意。如下例所示：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
mysql> select not 0, not 1, not null ;
+-------+-------+----------+
| not 0 | not 1 | not null |
+-------+-------+----------+
| 1 | 0 | NULL |
+-------+-------+----------+
1 row in set (0.00 sec)
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 ““AND”或“&&”表示逻辑与运算。当所有操作数均为非零值并且不为NULL 时，计算所得结果为1，当一个或多个操作数为0 时，所得结果为0，操作数中有任何一个为NULL 则返回值为NULL。如下例所示：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
mysql> select (1 and 1),(0 and 1) ,(3 and 1 ) ,(1 and null);
+-----------+-----------+------------+--------------+
| (1 and 1) | (0 and 1) | (3 and 1 ) | (1 and null) |
+-----------+-----------+------------+--------------+
| 1 | 0 | 1 | NULL |
+-----------+-----------+------------+--------------+
1 row in set (0.00 sec)
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 “OR”或“||”表示逻辑或运算。当两个操作数均为非NULL 值时，如有任意一个操作数为非零值，则结果为1，否则结果为0。当有一个操作数为NULL 时，如另一个操作数为非零值，则结果为1，否则结果为NULL。假如两个操作数均为NULL，则所得结果为NULL。如下例所示：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
mysql> select (1 or 0) ,(0 or 0),(1 or null) ,(1 or 1),(null or null);
+----------+----------+-------------+----------+----------------+
| (1 or 0) | (0 or 0) | (1 or null) | (1 or 1) | (null or null) |
+----------+----------+-------------+----------+----------------+
| 1 | 0 | 1 | 1 | NULL |
+----------+----------+-------------+----------+----------------+
1 row in set (0.00 sec)
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 “XOR”表示逻辑异或。当任意一个操作数为NULL 时，返回值为NULL。对于非NULL 的操作数，如果两个的逻辑真假值相异，则返回结果1；否则返回0。如下例所示：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
mysql> select 1 xor 1 ,0 xor 0,1 xor 0,0 xor 1,null xor 1;
+---------+---------+---------+---------+------------+
| 1 xor 1 | 0 xor 0 | 1 xor 0 | 0 xor 1 | null xor 1 |
+---------+---------+---------+---------+------------+
| 0 | 0 | 1 | 1 | NULL |
+---------+---------+---------+---------+------------+
1 row in set (0.00 sec)
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

## 索引

索引分单列索引和组合索引。单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。组合索引，即一个索引包含多个列。

创建索引时，你需要确保该索引是应用在 SQL 查询语句的条件(一般作为 WHERE 子句的条件) primary key

或者unique也可以达到不重复的效果

```sql
CREATE TABLE person_tbl
(
   first_name CHAR(20) NOT NULL,
   last_name CHAR(20) NOT NULL,
   sex CHAR(10),
   UNIQUE (last_name, first_name)
);-- 向last_name ,first_name INSERT INTO 同样的 values 会报错
```

## 普通索引

#### 创建索引

这是最基本的索引，它没有任何限制。它有以下几种创建方式：

```sql
CREATE INDEX indexName ON 表名 (表列名)
---
create index field on user_info2(user_decribe(250));-- 因为user_decribe 是text类型没有指定长度使用需要指定一个长度不然会报错
```



#### 删除索引的语法

```
DROP INDEX [indexName] ON 表名; 
```

如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length。

#### 修改表结构(添加索引)表创建完以后添加

```
ALTER table 表名 ADD INDEX indexName(表列名)

//普通索引
alter table table_name add index index_name (column_list) ;
//唯一索引
alter table table_name add unique (column_list) ;
//主键索引
alter table table_name add primary key (column_list) ;
```

> show index from table_name; 查看索引

强制使用index查询

```sql
select *FROM user_info2 force index(field) WHERE user_decribe="golang";
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