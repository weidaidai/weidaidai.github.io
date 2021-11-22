

## goland使用redis

```
"github.com/go-redis/redis"
```

普通模式连接

```go
var rdb *redis.Client //定义一个全局的rbd变量，redis.Client自带连接池
//初始化连接
func initclient() (err error) {
    //这里不要使用:=，我们是给全局变量赋值，然后在main函数中使用全局变量rdb
   rdb = redis.NewClient(&redis.Options{
      Addr:     "127.0.0.1:6379",
      Password: "", //不设置密码
      DB:       0,  //使用默认bd
      PoolSize: 100,//连接池的大小由业务决定
   })
   _, err = rdb.Ping().Result()
      return err 
}
func main() {
   if err :=initclient();err!=nil{
      fmt.Println("inti redis failed" ,err)
   }
   fmt.Println("连接成功")
   //程序退出时要释放相关资源
   defer rdb.Close()
}
```

#### set/get示例

```go
func redisExample() {
	err := rdb.Set("score", 100, 0).Err()//设置key：score，value：100
	if err != nil {
		fmt.Printf("set score failed, err:%v\n", err)
		return
	}

	val, err := rdb.Get("score").Result()//获取key
	if err != nil {
		fmt.Printf("get score failed, err:%v\n", err)
		return
	}
	fmt.Println("score", val)

	val2, err := rdb.Get("name").Result()//获取key，但不存在
	if err == redis.Nil {
		fmt.Println("name does not exist")
	} else if err != nil {
		fmt.Printf("get name failed, err:%v\n", err)
		return
	} else {
		fmt.Println("name", val2)
	}
}

//连接成功
//score 100
//name does not exist
```

#### 哈希表获取查询

```bash
创建哈希表的key vul
127.0.0.1:6379> hset user name "dongdong"
(integer) 1
127.0.0.1:6379> hgetall user
1) "name"
2) "dongdong"
127.0.0.1:6379> hset user age 18
(integer) 1
127.0.0.1:6379> hgetall user
1) "name"
2) "dongdong"
3) "age"
4) "18"
```



```GO
func hgetdemo() {
   v, err := rdb.HGetAll("user").Result()//返回map和err
   if err != nil {
      //redis.nil
      //其他err
      fmt.Println("hgetdemo failed", err)
      return
   }
   fmt.Println(v)
   v1 := rdb.HGet("user",  "age").Val()//返回string
   fmt.Println(v1)
}
```

#### zset示例

```go
func redisExample2() {
	zsetKey := "language_rank"
    //ZADD 命令在key后面分数/成员（score/member）自带
	languages := []redis.Z{
		redis.Z{Score: 90.0, Member: "Golang"},
		redis.Z{Score: 98.0, Member: "Java"},
		redis.Z{Score: 95.0, Member: "Python"},
		redis.Z{Score: 97.0, Member: "JavaScript"},
		redis.Z{Score: 99.0, Member: "C/C++"},
	}
	// ZADD
	num, err := rdb.ZAdd(zsetKey, languages...).Result()
	if err != nil {
		fmt.Printf("zadd failed, err:%v\n", err)
		return
	}
	fmt.Printf("zadd %d succ.\n", num)

	// 把Golang的分数加10
	newScore, err := rdb.ZIncrBy(zsetKey, 10.0, "Golang").Result()//float 64
	if err != nil {
		fmt.Printf("zincrby failed, err:%v\n", err)
		return
	}
	fmt.Printf("Golang's score is %f now.\n", newScore)

	// 取分数最高的3个
	ret, err := rdb.ZRevRangeWithScores(zsetKey, 0, 2).Result()//返回[]z切片集合
	if err != nil {
		fmt.Printf("zrevrange failed, err:%v\n", err)
		return
	}
	for _, z := range ret {
		fmt.Println(z.Member, z.Score)
	}

	// 取95~100分的
	op := redis.ZRangeBy{
		Min: "95",
		Max: "100",
	}
	ret, err = rdb.ZRangeByScoreWithScores(zsetKey, op).Result()//返回[]z切片集合
	if err != nil {
		fmt.Printf("zrangebyscore failed, err:%v\n", err)
		return
	}
	for _, z := range ret {
		fmt.Println(z.Member, z.Score)
	}
}tKey, op).Result()
	if err != nil {
		fmt.Printf("zrangebyscore failed, err:%v\n", err)
		return
	}
	for _, z := range ret {
		fmt.Println(z.Member, z.Score)
	}
}
```

输出结果如下：

```bash
$ ./06redis_demo 
zadd 0 succ.
Golang's score is 100.000000 now.
Golang 100
C/C++ 99
Java 98
JavaScript 97
Java 98
C/C++ 99
Golang 100
```

redis

```bash
127.0.0.1:6379> zrevrange language_rank 0 2//查看0-2的范围member
1) "Golang"
2) "C/C++"
3) "Java"
127.0.0.1:6379> zincrby language_rank 1 "python"//本来应该是增加score分数小写p增加一个score和member
"1"
127.0.0.1:6379> zrange language_rank 0 2 withscores//查看0-2的范围member和score
1) "python"
2) "1"
3) "Python"
4) "95"
5) "JavaScript"
6) "97"
```



#### 根据前缀获取Key

```go
vals, err := rdb.Keys(ctx, "prefix*").Result()
```

#### 执行自定义命令

```go
res, err := rdb.Do(ctx, "set", "key", "value").Result()
```

#### 按通配符删除key

当通配符匹配的key的数量不多时，可以使用`Keys()`得到所有的key在使用`Del`命令删除。 如果key的数量非常多的时候，我们可以搭配使用`Scan`命令和`Del`命令完成删除。

```go
ctx := context.Background()
iter := rdb.Scan(ctx, 0, "prefix*", 0).Iterator()
for iter.Next(ctx) {
	err := rdb.Del(ctx, iter.Val()).Err()
	if err != nil {
		panic(err)
	}
}
if err := iter.Err(); err != nil {
	panic(err)
}
```

### Pipeline（批量传输）

`Pipeline` 主要是一种网络优化。它本质上意味着客户端缓冲一堆命令并一次性将它们发送到服务器。这些命令不能保证在事务中执行。这样做的好处是节省了每个命令的网络往返时间（RTT）。

`Pipeline` 基本示例如下：

```go
pipe := rdb.Pipeline()

incr := pipe.Incr("pipeline_counter")
pipe.Expire("pipeline_counter", time.Hour)

_, err := pipe.Exec()
fmt.Println(incr.Val(), err)
```

上面的代码相当于将以下两个命令一次发给redis server端执行，与不使用`Pipeline`相比能减少一次RTT。

```bash
INCR pipeline_counter
EXPIRE pipeline_counts 3600
```

也可以使用`Pipelined`：

```go
var incr *redis.IntCmd
_, err := rdb.Pipelined(func(pipe redis.Pipeliner) error {
	incr = pipe.Incr("pipelined_counter")
	pipe.Expire("pipelined_counter", time.Hour)
	return nil
})
fmt.Println(incr.Val(), err)
```

在某些场景下，当我们有多条命令要执行时，就可以考虑使用pipeline来优化。事务

Redis是单线程的，因此单个命令始终是原子的，但是来自不同客户端的两个给定命令可以依次执行，例如在它们之间交替执行。但是，`Multi/exec`能够确保在`multi/exec`两个语句之间的命令之间没有其他客户端正在执行命令。

在这种场景我们需要使用`TxPipeline`。`TxPipeline`总体上类似于上面的`Pipeline`，但是它内部会使用`MULTI/EXEC`包裹排队的命令。例如：

```go
pipe := rdb.TxPipeline()

incr := pipe.Incr("tx_pipeline_counter")
pipe.Expire("tx_pipeline_counter", time.Hour)

_, err := pipe.Exec()
fmt.Println(incr.Val(), err)
```

上面代码相当于在一个RTT下执行了下面的redis命令：

```bash
MULTI
INCR pipeline_counter
EXPIRE pipeline_counts 3600
EXEC
```

还有一个与上文类似的`TxPipelined`方法，使用方法如下：

```go
var incr *redis.IntCmd
_, err := rdb.TxPipelined(func(pipe redis.Pipeliner) error {
	incr = pipe.Incr("tx_pipelined_counter")
	pipe.Expire("tx_pipelined_counter", time.Hour)
	return nil
})
fmt.Println(incr.Val(), err)
```

