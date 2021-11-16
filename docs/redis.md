## Redis

官方文档命令：[Redis命令中心（Redis commands） -- Redis中国用户组（CRUG）](http://www.redis.cn/commands.html) 

是一个开源的使用 ANSI C 语言编写、遵守 BSD 协议、支持网络、可基于内存、分布式、可选持久性的键值对(Key-Value)存储数据库

因此 Redis被广泛应用于**缓存**方向，消息中间件MQ

支持数据结构

### 安装redis-docker

Step1:搜索redis：  `docker search redis`

Step2:拉取官方镜像：docker pull docker.io/redis

Step3:下载完成后，使用`docker images`查看是否拉去成功

Step4:运行容器

```
docker run -p 6379:6379 -v $PWD/data:/data -d redis:latest redis-server --appendonly yes
```

如果需要给容器id命名

--name 名

密码则--requirepass "password"

查看现有的redis密码：config get requirepass

设置redis密码config set requirepass ****（****为你要设置的密码）

若出现(error) NOAUTH Authentication required.错误，则使用 auth 密码 来认证密码

命令说明：
 `docker run`：启动命令

`-p 6379:6379` : 将容器的6379端口映射到主机的6379端口

`-v $PWD/data:/data` : 将主机中当前目录下的data挂载到容器的/data
 `- d` ： 后台运行

latest最新

 `redis-server --appendonly yes` : 在容器执行redis-server启动命令，并打开redis持久化配置



### 进入容器

docker exec -it 容器名/id  redis-cli

redis有16个数据库，默认为0开始

### 基本命令

### string（字符串）

一个 key 对应一个 value

string 类型的值最大能存储 512MB

常用：字符串存储

```bash
127.0.0.1:6379> select 3#选择数据库
OK
127.0.0.1:6379[3]> dbsize#查看大小
(integer) 0
127.0.0.1:6379[3]> get name #查值
(nil)
127.0.0.1:6379[3]> keys *//获取key，多的时候不要用
(empty array)
127.0.0.1:6379[3]> flushall//删除所有
OK
127.0.0.1:6379[3]> flushdb//删库
OK
127.0.0.1:6379> set age 18//设置key和val
OK
127.0.0.1:6379> exists name//以存在的key
(integer) 1
127.0.0.1:6379> keys * #查看所有
1) "age"
2) "name"
127.0.0.1:6379> expire name 10#设置过期时间单位秒
(integer) 1
127.0.0.1:6379> ttl name#查看时间
(integer) 1
127.0.0.1:6379[2]> del name #删
(integer) 1
#无key可设，有key不可设
127.0.0.1:6379[2]> setnx name 1
(integer) 1
127.0.0.1:6379[2]> setnx name 9
(integer) 0
127.0.0.1:6379[2]> get name
"1"
```

### list（列表）

简单理解为链表 可以在left，right都可以插入数值,元素插入两边效率高，中间低

简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

常用：最新消息排行等功能(比如朋友圈的时间线,消息队列

栈，队列，阻塞队列

![](images\list.png)

```bash
#增lpush命令将一个或多个值插入到列表头部。 如果 key 不存在，一个空列表会被创建并执行 LPUSH 操作。 当 key 存在但不是列表类型时，返回一个错误。 查lrange
127.0.0.1:6379[2]> lpush name xiaowei #增加创建
(integer) 1
127.0.0.1:6379[2]> lpush name xiaohua#增加
(integer) 2
127.0.0.1:6379[2]> lpush name xiaohong#增加
(integer) 3
127.0.0.1:6379[2]> lrange name 1 3 ##需要指定开始到结束的元素长度
1) "xiaohua"
2) "xiaowei"
127.0.0.1:6379[2]> lrange name 0 3
1) "xiaohong"
2) "xiaohua"
3) "xiaowei"
#移除第一个元素lpop

127.0.0.1:6379> lrange mylist 0 -1
1) "\xe6\x8f\x92\xe5\x85\xa5"
2) "two"
3) "world"
4) "hello"
5) "tree"
127.0.0.1:6379> lpop mylist
"\xe6\x8f\x92\xe5\x85\xa5"
#移除最后一个元素并显示rpop

127.0.0.1:6379> rpop mylist
"tree"
#移除第一个元素bloop并显示对应元素

127.0.0.1:6379[2]> blpop key2 0 #要删除的下标，并显示对应元素
1) "key2"
2) "mysql"
127.0.0.1:6379[2]> blpop key2 2
1) "key2"
2) "xiaoming"
127.0.0.1:6379[2]> blpop key2 3 #删除不存在，超时退出
(nil)
(3.05s)
#移除最后一个元素brpop 指定对应下标

127.0.0.1:6379> lrange key2 1 3
1) "redis"
2) "mysql"
127.0.0.1:6379> brpop key2 1
1) "key2"
2) "mysql"
127.0.0.1:6379> brpop key2 1
1) "key2"
2) "redis"
127.0.0.1:6379> brpop key2 0
1) "key2"
2) "mogondb"
127.0.0.1:6379> brpop key2 3
(nil)
(3.01s)
#通过lindex索引查询对应下标元素

127.0.0.1:6379> lpush mylist "hello"
(integer) 1
127.0.0.1:6379> lpush mylist "world"
(integer) 2
127.0.0.1:6379> lindex mylist 0
"world"
127.0.0.1:6379> lindex mylist -1
"hello"
#rpush 插入值位于尾部

127.0.0.1:6379> rpush mylist "tree"
(integer) 4
127.0.0.1:6379> lrange mylist 0 -1
1) "two"
2) "world"
3) "hello"
4) "tree"
127.0.0.1:6379> lrange mylist 0 3
1) "two"
2) "world"
3) "hello"
4) "tree"
#获取list长度 llen
127.0.0.1:6379> llen mylist
(integer) 4
#移除lsit第一个元素并返回的对应元素
127.0.0.1:6379> lpop mylist
"mysql"
127.0.0.1:6379> lpop list2 #没有返回nil
(nil)
# Lpushx 将一个值插入到已存在的列表头部，列表不存在时操作无效。
127.0.0.1:6379> lpushx mylist "插入"
(integer) 5
127.0.0.1:6379> lpushx list2 "插入"
(integer) 0
127.0.0.1:6379> lrange list2 0 -1
(empty array)``
#移除指定下标元素lrem 对应下标+移除元素
127.0.0.1:6379> lrem mylist 0 two
(integer) 1
127.0.0.1:6379> lrange mylist 0 -1
1) "world"
2) "hello"

#ltrim 截取部分开始下标+终点下标
127.0.0.1:6379> lrange list1 0 -1
1) "tree"
2) "two"
3) "one"
127.0.0.1:6379> ltrim list1 1 2
OK
127.0.0.1:6379> lrange list1 0 -1
1) "two"
2) "one"
#将尾部的元素添加到其他列表rpoplpush 旧表+新表
127.0.0.1:6379> lrange list1 0 -1
1) "two"
2) "one"
127.0.0.1:6379> rpoplpush list1 list2
"one"
127.0.0.1:6379> lrange list2 0 -1
1) "one"
127.0.0.1:6379> lrange list1 0 -1
1) "two"

#修改值lset 指定下标+元素
127.0.0.1:6379> lrange list1 0 -1
1) "two"
127.0.0.1:6379> lset list 0 redis
(error) ERR no such key
127.0.0.1:6379> lset list1 0 redis
OK
127.0.0.1:6379> lrange list1 0 -1
1) "redis"
#判断list是否存在 exists
127.0.0.1:6379> exists list1
(integer) 1
127.0.0.1:6379> exists list
(integer) 0

#指定插入位置之前或之后linsert +list+before/after +list中元素+要插入的元素
127.0.0.1:6379> lrange list 0 -1
1) "4"
2) "3"
3) "2"
4) "1"
127.0.0.1:6379> linsert list before 3 hello
(integer) 5
127.0.0.1:6379> lrange list 0 -1
1) "4"
2) "hello"
3) "3"
4) "2"
5) "1"


```



### hash（哈希）

hash 是一个键值(key=>value)对集合。

 hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象

常用：存储、读取、修改用户属性

相当于map

```bash
#创建哈希表的key vul
#插hset 查hget
127.0.0.1:6379> hset user name "dongdong"#插入数据
(integer) 1
127.0.0.1:6379> hgetall user
1) "name"
2) "dongdong"
127.0.0.1:6379> hset user age 18
(integer) 1
127.0.0.1:6379> hgetall user#查询user所有
1) "name"
2) "dongdong"
3) "age"
4) "18"
127.0.0.1:6379[2]> hget user age #查
"19"
127.0.0.1:6379[2]> type user#看类型
hash
##hmset设置多个##
127.0.0.1:6379[2]> hmset user1 field1 hello fierld2 world #设置多个
OK
127.0.0.1:6379[2]> hmget user1 field1 fierld2 #查询多个
1) "hello"
2) "world"
127.0.0.1:6379[2]> hmset user1 field3 '小明' feild4  '小韦'
OK
127.0.0.1:6379[2]> hmset user1 name 小花
OK
127.0.0.1:6379[2]> hlen user1 #获取长度
(integer) 5
127.0.0.1:6379[2]> HKEYS user1 # 获取key
1) "name"
2) "field1"
3) "fierld2"
4) "field3"
5) "feild4"
127.0.0.1:6379[2]> Hvals user1 #获取vals
1) "\xe5\xb0\x8f\xe8\x8a\xb1"
2) "hello"
3) "world"
4) "\xe5\xb0\x8f\xe6\x98\x8e"
5) "\xe5\xb0\x8f\xe9\x9f\xa6"
127.0.0.1:6379[2]> hset user1 field1 1 
(integer) 0
127.0.0.1:6379[2]> hincrby user1 field1 1 #增加1
(integer) 2
127.0.0.1:6379[2]> hget user1 field1
"2"
127.0.0.1:6379[2]> hincrby user1 field1 -1#-1
(integer) 1
127.0.0.1:6379[2]> hget user1 field1
"1"

######hsetnx存在不可以重新设置###可以应用于分布式锁
127.0.0.1:6379[2]> hsetnx user1 field1 9
(integer) 0
127.0.0.1:6379[2]> hget user1 field1
"1"
######不存在可以设置###
127.0.0.1:6379[2]> hsetnx user1 field6 9
(integer) 1
127.0.0.1:6379[2]> hget user1 field6
"9"
#设置id
127.0.0.1:6379[2]> hset user1:1 name xiaoming
(integer) 1
127.0.0.1:6379[2]> hget user1:1 name
"xiaoming"



```

### set（集合）

Redis 的 Set 是 string 类型的无序集合，元素不可重复。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)

常用：1.共同好友 2.利用唯一性,统计访问网站的所有独立ip 3.好友推荐时,根据tag求交集,大于某个阈值就可以推荐

```bash
#基本命令
127.0.0.1:6379[2]> sadd key1 xiaoming #增创
(integer) 1
127.0.0.1:6379[2]> sadd key1 xiaohua
(integer) 1
127.0.0.1:6379[2]> sadd key1 xiaohong
(integer) 1
127.0.0.1:6379[2]> sadd key1 xiaohong
(integer) 0
#查看元素smembers
127.0.0.1:6379[2]> smembers key1  #查 无序集合
1) "xiaohua"
2) "xiaohong"
3) "xiaoming"

#查看是否存在某个元素sismember+set+元素 返回1存在返回0不在

127.0.0.1:6379> smembers  key1
1) "tree"
2) "two"
3) "one"
127.0.0.1:6379> sismember key1 one
(integer) 1

#sard+set查看有多少个元素
127.0.0.1:6379> smembers myset
1) "ai"
2) "xiao"
3) "ke"
127.0.0.1:6379> scard myset
(integer) 3

#移除srem+集合名+元素
127.0.0.1:6379> srem myset ai
(integer) 1

#随机选一个值srandmember
127.0.0.1:6379> srandmember myset
"suiji2"
127.0.0.1:6379> smembers myset
1) "suiji"
2) "ke"
3) "xiao"
4) "suiji2"
5) "suiji3"
#删除指定的元素和随机删除 spop +set+指定下标/不指定下标可以随机删除
127.0.0.1:6379> smembers myset
1) "suiji"
2) "ke"
3) "xiao"
4) "suiji2"
5) "suiji3"
127.0.0.1:6379> spop myset
"ke"
127.0.0.1:6379> spop myset 1
1) "xiao"
127.0.0.1:6379> smembers myset
1) "suiji"
2) "suiji2"
3) "suiji3"

#将指定的元素移除到另一个集合smove+旧set+新set+元素 如果集合不存则创
127.0.0.1:6379> smembers myset
1) "two"
2) "tree"
3) "one"
127.0.0.1:6379> smove myset myset1 one
(integer) 1

#集合
-差集（不一样的元素）
-交集（一样的元素）
-并集（所有的元素合并）
127.0.0.1:6379> smembers key1
1) "d"
2) "b"
3) "a"
4) "c"
127.0.0.1:6379> smembers key2
1) "d"
2) "a"
3) "e"
#查看差集sdiff(为什么没有e ?)
127.0.0.1:6379> sdiff key1 key2
1) "b"
2) "c"
#查看交集sinter 共同好友
127.0.0.1:6379> sinter key1 key2
1) "d"
2) "a"
#查看并集
127.0.0.1:6379> sunion key1 key2
1) "c"
2) "d"
3) "b"
4) "a"
5) "e"

```

### Zset（Sorted Set有序集合)

将Set中的元素增加一个权重参数score,元素按score有序排列

数据插入集合时,已经进行天然排序

常用;1、排行榜 2、带权重的消息队列

```bash
#常用命令
#添加值zadd+zset+序号+元素 添加多个值（序号+元素）
127.0.0.1:6379[2]> zadd key1 1 mysql#增
(integer) 1
127.0.0.1:6379[2]> zadd key1 2 redis
(integer) 1
127.0.0.1:6379[2]> zadd key1 2 mogonbd
(integer) 1
#查zrange+zset+(min+max),如果想加序号+withscores
127.0.0.1:6379[2]> zrange key1 0 10 withscores 
1) "mysql"
2) "1"
3) "redis"
4) "2"
5) "mogonbd"
6) "2"
#zrangebyscore +zset+-inf +inf 小到大升序
127.0.0.1:6379> zrange salary 0 3 withscores
1) "apple"
2) "1000"
3) "xiaomi"
4) "2000"
5) "huawei"
6) "3000"

127.0.0.1:6379> zrangebyscore salary -inf +inf
1) "apple"
2) "xiaomi"
3) "huawei"

#指定小于某值的升序排列
127.0.0.1:6379> zrangebyscore salary -inf 2000 withscores
1) "apple"
2) "1000"
3) "xiaomi"
4) "2000"

#移除元素zrem + 元素
127.0.0.1:6379> zrem salary apple
(integer) 1

#获取zset中的个数
127.0.0.1:6379> zcard salary
(integer) 2

#zrevrange默认从大到小 降序
127.0.0.1:6379> zrevrange salary 0 -1 
1) "huawei"
2) "xiaomi"

#获取区间数量zcount +zset +min max
127.0.0.1:6379> zadd myzset 1 hello 2 world 3 nihao
(integer) 3
127.0.0.1:6379> zcount myzset 0 3
(integer) 3
```

![zset](images\zset.png)

### 三种特殊的数据类型

#### geospatial (地理位置)  

```bash
#geoadd添加 key+经度+纬度+城市名称
127.0.0.1:6379> geoadd china:city 120.619585 31.299379 suzhou
(integer) 1

#获取指定城市的位置geopos+key+名称
127.0.0.1:6379> geopos china:city beijing
1) 1) "116.40528291463851929"
   2) "39.9049884229125027"

#查看距离geodist+key+名称+名称 默认距离km
127.0.0.1:6379> geodist china:city beijing shanghai
"1067771.3354"
```

georadius 给定的经纬度为中心找出某一半径的距离

```bash

#georadius+key+经纬度+距离+km
127.0.0.1:6379> georadius china:city 110 30 10000 km
1) "suzhou"
2) "shanghai"
3) "fengtai"
4) "beijing"
5) "fangsan"
```

geohash 返回经纬11个字符串

```bash
127.0.0.1:6379> geohash china:city beijing
1) "wx4g0b7xrt0"
```

可以用zset命令来操作geo

```bash
127.0.0.1:6379> zrange china:city 0 -1
1) "suzhou"
2) "shanghai"
3) "fengtai"
4) "beijing"
5) "fangsan"
```

后续。。。。

#### Hyperloglog 基数统计

#### bitmap 位图场景