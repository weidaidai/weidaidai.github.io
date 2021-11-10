## sql语句的在mysql基本操作

## 创建数据库和判断

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

tinyint 一个字节

default 默认为8888

integer int长度限制为10

date 日历

varchar(20)可字符串最大长度为255

char定长字符串最大255字节没有给定长度用空格补充

单精度decimal(4,1) 表示4位数小数保留2 位-最大（6，3）

int不能后面设置其他数据类型只能填充int类型

auto_increment 自增长



## alter和DROP

 删除和修改列名或数据库名

```sql
alter table tb_test2 add use_phone varchar(20) not null comment'添加';
alter table tb_test2 modify use_phone int not null comment'修改表名';
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

添加列add

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
--会把同列中重复的部分删除
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

sql 中叫 top

```sql
SELECT * FROM user_info LIMIT 1,3;
--查找1-3列的数据
--搭配offset
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
SELECT * FROM Persons WHERE use_id BETWEEN 'xx' AND 'xx'
```

## in

IN 操作符允许我们在 WHERE 子句中规定多个值。

```sql
SELECT *FROM 表名 WHERE 列名 IN (value1,value2,...)
```

