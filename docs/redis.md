

## Redis

 是一个开源的使用 ANSI C 语言编写、遵守 BSD 协议、支持网络、可基于内存、分布式、可选持久性的键值对(Key-Value)存储数据库

因此 Redis被广泛应用于**缓存**方向

支持数据结构

#### string（字符串）

一个 key 对应一个 value

string 类型的值最大能存储 512MB

#### hash（哈希）

hash 是一个键值(key=>value)对集合。

 hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象

#### list（列表）

简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

#### set（集合）

Redis 的 Set 是 string 类型的无序集合。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)

#### zset(sorted set：有序集合)。

Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数(score)却可以重复。

### 安装redis-docker

Step1:搜索redis：  `docker search redis`

Step2:拉取官方镜像：docker pull docker.io/redis

Step3:下载完成后，使用`docker images`查看是否拉去成功

Step4:运行容器

```
docker run -p 6379:6379 -v $PWD/data:/data -d redis:latest redis-server --appendonly yes
```

命令说明：
 `docker run`：启动命令

`-p 6379:6379` : 将容器的6379端口映射到主机的6379端口

`-v $PWD/data:/data` : 将主机中当前目录下的data挂载到容器的/data
 `- d` ： 后台运行
 `redis-server --appendonly yes` : 在容器执行redis-server启动命令，并打开redis持久化配置

### 进入容器

docker exec -it 容器名/id  redis-cli

redis有16个数据库，默认为0开始

### string基本命令

string是最经常用的

```redis
127.0.0.1:6379> select 3//选择数据库
OK
127.0.0.1:6379[3]> dbsize//查看大小
(integer) 0
127.0.0.1:6379[3]> get name
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
127.0.0.1:6379> keys *//
1) "age"
2) "name"
127.0.0.1:6379> expire name 10//设置过期时间单位秒
(integer) 1
127.0.0.1:6379> ttl name//查看时间
(integer) 1
```

