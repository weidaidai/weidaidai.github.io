

## Redis

 是一个开源的使用 ANSI C 语言编写、遵守 BSD 协议、支持网络、可基于内存、分布式、可选持久性的键值对(Key-Value)存储数据库

因此 Redis被广泛应用于**缓存**方向

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

简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

常用：最新消息排行等功能(比如朋友圈的时间线,消息队列

栈，队列，阻塞队列

![](images\list.png)

```bash
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
```



### hash（哈希）

hash 是一个键值(key=>value)对集合。

 hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象

常用：存储、读取、修改用户属性

相当于map

```bash
创建哈希表的key vul
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

Redis 的 Set 是 string 类型的无序集合。

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
127.0.0.1:6379[2]> smembers key1  #查
1) "xiaohua"
2) "xiaohong"
3) "xiaoming"
```

### Sorted Set(有序集合)

将Set中的元素增加一个权重参数score,元素按score有序排列

数据插入集合时,已经进行天然排序

常用;1、排行榜 2、带权重的消息队列

```bash
#常用命令
127.0.0.1:6379[2]> zadd key1 1 mysql#增
(integer) 1
127.0.0.1:6379[2]> zadd key1 2 redis
(integer) 1
127.0.0.1:6379[2]> zadd key1 3 mogonbd
(integer) 1
127.0.0.1:6379[2]> zrange key1 0 10 withscores #查
1) "mysql"
2) "1"
3) "redis"
4) "2"
5) "mogonbd"
6) "3"
```

