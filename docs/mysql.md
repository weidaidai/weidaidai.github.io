# mysql的基本操作

## 创建数据库和判断
drop database if exists testdb;//不存在就创建

create database testdb;
-- 使用数据库
use testdb;
## 创建数据表和判断

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

![image-20210818155419294](C:\Users\16451\AppData\Roaming\Typora\typora-user-images\image-20210818155419294.png)

![image-20210818155557114](C:\Users\16451\AppData\Roaming\Typora\typora-user-images\image-20210818155557114.png)

tinyint 一个字节

default 默认为8888

integer int长度为10

date 日历

varchar(20)可字符串最大长度为255

char定长字符串最大255字节没有给定长度用空格补充

单精度decimal(4,1) 表示4位数小数保留2 位-最大（6，3）

int不能后面设置其他数字只能填充int类型



## 添加和修改查询

```sql
alter table tb_test2 add use_phone varchar(20) not null comment'添加';
alter table tb_test2 modify use_phone int not nullcomment'修改';
alter table tb_test2 change use_phone change_use2 int not null comment'修改表名';
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
alter table tb_test2 drop change_use2;##删除一个存在
```

修改表名

```sql
##修改表名
alter table tb_test2 rename to user_info;
##修改引擎
alter table user_info engine=myisam;##默认为InnoDB
```





## 插入表数据

```sql
insert into user_info(use_name,use_birthday,use_gener,use_state,use_height,user_decribe)values("dada","2022-07-08","2",1.78,4,"golang")
insert into 表名（字段名称，。。。）values（插入的值）
```

插入类型要匹配

在插入指定的列的值的时候要看其他列是否有not null的约束不然会报错

## 字段修改

```sql
update user_info set use_birthday="2021-04-06" where use_id=1; 
update(表名)set修改字段=值，。。。where（主键不变值或者其他）
```

## 筛选查询和全部查询

```sql
 select *from 表名；
 -- where可以通过比较（>,<,<=,>=,!=,=）来查询结果
 select *from user_info where use_gener="男";
  select *from user_info where use_height>=1;
```



## 删除表内数据

```sql
delete from 表名 where 删除条件 and 其他条件;
delete from user_info where use_id=2;
delete from use
```

## 模糊查找

```sql
select *from 表名  where 位置 like %（%表示通配符）
-- 有%xx%无论是否在一起
select *from 表名  where 位置 like %xx%（%表示通配符）
select *from user_info where use_name like "ke%";

```



