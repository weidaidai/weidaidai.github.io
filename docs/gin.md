### 绑定的注意事项

> - ShouldBindJSON()

**只会返回错误信息，不会往header里面写400的错误状态码**

```go
//ShouldBindWith，根据b的类型去绑定json，query，去绑定
func (c *Context) ShouldBindWith(obj interface{}, b binding.Binding) error {
	return b.Bind(c.Request, obj)
}
```

> - BindJSON()

**返回错误，并在header里面写400的状态码**

```go
// BindJSON is a shortcut for c.MustBindWith(obj, binding.JSON).
func (c *Context) BindJSON(obj interface{}) error {
	return c.MustBindWith(obj, binding.JSON)
}
// MustBindWith binds the passed struct pointer using the specified binding engine.
// It will abort the request with HTTP 400 if any error ocurrs.
// See the binding package.
func (c *Context) MustBindWith(obj interface{}, b binding.Binding) (err error) {
	if err = c.ShouldBindWith(obj, b); err != nil {
		c.AbortWithError(http.StatusBadRequest, err).SetType(ErrorTypeBind)
	}

	return
}

```

	//内部根据Content-Type去解析
	c.ShouldBind(obj interface{})
	//内部替你传递了一个binding.JSON，对象去解析
	c.ShouldBindJSON(obj interface{})
	//解析哪一种绑定的类型，根据你的选择
	c.ShouldBindWith(obj interface{}, b binding.Binding)
表单的解析和绑定

### 同步和异步执行

```go
package main

import (
	"github.com/gin-gonic/gin"
	"log"
	"time"
)

func main()  {
	r:=gin.Default()

	//asynchronization 异步
	r.GET("/async", func(c *gin.Context) {
		//创建副本
		copycontext:=c.Copy()
		//goroutine
		go func() {
			time.Sleep(3*time.Second)
			log.Println("异步执行"+copycontext.Request.URL.Path)
		}()
	})
	// 2.同步
	r.GET("/long_sync", func(c *gin.Context) {
		time.Sleep(3 * time.Second)
		log.Println("同步执行：" + c.Request.URL.Path)
	})

	r.Run()
}
```



```go
2021/12/23 17:16:57 同步执行：/long_sync //休眠了3秒
[GIN] 2021/12/23 - 17:16:57 | 200 |    3.0174735s |       127.0.0.1 | GET      "/long_sync"
2021/12/23 17:19:39 异步执行/async
[GIN] 2021/12/23 - 17:19:39 | 200 |            0s |       127.0.0.1 | GET      "/async"
//同步的逻辑可以看到，服务的进程睡眠了。异步的逻辑则看到响应返回了，然后程序还在后台的协程处理
```



### 定全局全局中间件

```bash
#控制台
start 2021-12-23 18:25:16.4467267 +0800 CST m=+8.935206301
end 200
time 9.6043ms
request: middle
[GIN] 2021/12/23 - 18:25:16 | 200 |      9.6043ms |       127.0.0.1 | GET      "/ce"
```

```go
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"time"
)

//定义中间件
//HandlerFunc定义gin middleware使用的处理程序作为返回值。
func middle()gin.HandlerFunc  {
	return func(c *gin.Context) {
		//开始时间
        t:=time.Now()
		fmt.Println("start",t)
        // 设置变量到Context的key中，可以通过Get()取
		c.Set("request","middle")
		status:=c.Writer.Status()
		fmt.Println("end",status)
		
        //计算时间差（没有执行）需要加上c.Next()继续执行剩余的
        t2:=time.Since(t)
		fmt.Println("time",t2)


	}
}

func main() {
	// 1.创建路由
	// 默认使用了2个中间件Logger(), Recovery()
	r := gin.Default()
	// 注册中间件
	r.Use(middle())
	// {}为了代码规范
	{
		r.GET("/ce", func(c *gin.Context) {
			// 取key
			req, _ := c.Get("request")
			fmt.Println("request:", req)
			// 页面接收
			c.JSON(200, gin.H{"request": req})
		})

	}
	r.Run()
}
//{"request":"middle"}
```

//加上c.Next

```go
start 2021-12-23 18:32:47.9576201 +0800 CST m=+2.374964601
end 200
request: middle
time 10.8624ms//时间差执行
```

```go
//加上一个请求，也会经过该中间件

r.GET("/ec", func(c *gin.Context) {
			c.JSON(200,gin.H{"我是其他请求":"有中间件"})
		})

start 2021-12-24 09:23:59.8884641 +0800 CST m=+17.207882601
end 200
time 10.081ms
[GIN] 2021/12/24 - 09:23:59 | 200 |      10.081ms |       127.0.0.1 | GET      "/ec"
start 2021-12-24 09:25:04.5094995 +0800 CST m=+81.828918001
end 200
request: middle
time 0s
[GIN] 2021/12/24 - 09:25:04 | 200 |            0s |       127.0.0.1 | GET      "/ce"
```

```go
func main()  {
	r:=gin.Default()
	r.GET("/jubu",middle(), func(c *gin.Context) {

		rep,_:=c.Get("request")

		fmt.Println("request:",rep)
		//页面显示
		c.JSON(200,gin.H{"request":rep})

	})
	r.GET("/ec", func(c *gin.Context) {
		c.JSON(200,gin.H{"我是其他请求":"没有中间件"})
	})
	r.Run()
}
```

### 中间件小练习

> *gin.Context是传指针

```go
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"time"
)

func Mytime(ctx *gin.Context){
	start:=time.Now()

	ctx.Next()
	since:=time.Since(start)
	fmt.Println(since)


}
func main()  {
	r:=gin.Default()
	r.Use(Mytime)
	r.GET("/one",shopone)
	r.GET("/two",shoptwo)
	r.Run()
}
func shopone(ctx *gin.Context)  {
	time.Sleep(5*time.Second)

}
func shoptwo(ctx *gin.Context)  {
	time.Sleep(3*time.Second)
}

```

重定向

```go
r := gin.Default()
//http重定向
r.GET("/index", func(c *gin.Context) {
   //c.JSON(http.StatusOK, gin.H{
   // "status": "ok",
   //})
   //跳转到sogo
   c.Redirect(http.StatusMovedPermanently, "https://www.sogo.com")
})

//路由重定向
r.GET("/luyou", func(c *gin.Context) {
   //跳转到/luyou2对应的路由处理函数
   c.Request.URL.Path = "/luyou2"  //把请求的URL修改
   r.HandleContext(c)  //继续后续处理
})
r.GET("/luyou2", func(c *gin.Context) {
   c.JSON(http.StatusOK, gin.H{
      "message":"路由重定向",
   })
})
r.Run(":9090")
```

```go
ErrServiceNotFound         = errors.New("service not found")
ErrServiceDisabled         = errors.New("service disabled")
ErrInvalidServiceUrl       = errors.New("invalid service url")
ErrAccountServiceForbidden = errors.New("account not allowed to login service")
```

```
package mysql

import (
   "database/sql"
   "errors"
)

type User struct {
   Username string
   Password string
}

// GetStudentById   通过用户名查询用户
func GetStudentById(db *sql.DB) (*User, error) {
   sqlStr := "select *from User where Username=?"
   U := &User{}
   err := db.QueryRow(sqlStr, &U.Username).Scan(&U.Username)
   if err == sql.ErrNoRows {
      errors.New("没有该用户")
   }
   return U, nil
}
```

1. 用户访问 服务A，302 跳转到 CAS。（GET 参数包含 服务A 回调地址）
2. 用户在 CAS 登录，登录成功后带着 Service Ticket 跳转到 服务A 的回调地址。（同时 CAS 的 Session 里也保存了用户 CAS 的登录状态了）
3. 服务A 后端使用 Service Ticket 请求 CAS 验证是否正确。
4. CAS 后端返回 Service Ticket 正确，并且提供用户的唯一标识。（之后 服务A 根据唯一标识就知道是自己系统里的哪个用户登录了，顺便再设置一下自己的 Session）
   以上便结束了 服务A 的登录。
5. 用户访问 服务B，302 跳转到 CAS。（GET 参数包含 服务B 回调地址）
6. 因为之前已在 CAS 登录过，直接带着 Service Ticket 跳转到 服务B 的回调地址。（连跳两下挺快的，用户可能都没察觉到）
7. 服务B 后端使用 Service Ticket 请求 CAS 验证是否正确。
8. 同上，CAS 后端返回 Service Ticket 正确，并且提供用户的唯一

- `/register` 新用户注册
- `/login` 【CAS 协议】登录
- `/authorize` 服务授权
- `/profile` 个人信息修改
- `/validate`【CAS 协议】服务验证接口



