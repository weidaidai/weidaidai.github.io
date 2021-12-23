## template模板

！+tab可以直接生成模板

```html
<!doctype html>
<html lang="zh-cn">//lang表示前页面内容为中文默认为en
<head>//头文件定义标题
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>

</body>
</html>
```

```html
//完整如下
<!doctype html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>hello</title>
</head>
<body>
<p>hello{{.}}</p>
</body>
</html>
```



```html
<p>hello{{.}}</p>//表示{{.}}传变量
```

```go
package main

import (
	"fmt"
	"html/template"
	"net/http"
)

func sayhello(w http.ResponseWriter, r *http.Request) {
	//2.解析模板
	t, err := template.ParseFiles("./hello.tmpl")
	if err != nil {
		fmt.Println("parse template faild", err)
		return
	}

	//3.渲染模板
	name := "小东西"
	err = t.Execute(w, name)
	if err != nil {
		fmt.Println("渲染模板failed", err)
		return
	}
}
func main() {
	http.HandleFunc("/", sayhello  )
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		fmt.Println("http start faild", err)
		return
	}
}

```

![image-20210820175635849](C:\Users\16451\AppData\Roaming\Typora\typora-user-images\image-20210820175635849.png)

## 模板语法

![image-20210821181604821](C:\Users\16451\AppData\Roaming\Typora\typora-user-images\image-20210821181604821.png)

```go
<!doctype html>
<html lang="zh-cn">
<head>
    <title>hello</title>
</head>
<body>
<p>hello{{.Name}}</p>
<p>性别:{{.Gender}}</p>
<p>年龄:{{.Age}}</p>
</body>
</html>
```

```go
type User struct {
	Name   string
	Gender string
	Age    int
}
```

```go
	//使用之前定义user结构体并初始化
u1 := User{
		Name:   "小东西",
		Gender: "男",
		Age:    18,
	}

   err = t.Execute(w, u1)
//或者直接定义
u2:=map[string]interface{}{
	"Name":"meme",

	"Gender":1,
	"Age":18,

	}

	err = t.Execute(w, u2)
```

同时使用两个或者多个

![image-20210821190134831](C:\Users\16451\AppData\Roaming\Typora\typora-user-images\image-20210821190134831.png)

```html
<!doctype html>
<html lang="zh-cn">
<head>
    <title>hello</title>
</head>
<body>
<p>u1</p>
<p>hello{{.u1.Name}}</p>
<p>性别:{{.u1.Gender}}</p>
<p>年龄:{{.u1.Age}}</p>
<p>u2</p>
<p>hello{{.u2.Name}}</p>
<p>性别:{{.u2.Gender}}</p>
<p>年龄:{{.u2.Age}}</p>
</body>
</html>
```

```go
//使用之前定义user结构体并初始化
	u1 := User{
		Name:   "小东西",
		Gender: "男",
		Age:    18,
	}
	u2:=map[string]interface{}{
	"Name":"meme",

	"Gender":1,
	"Age":18,

	}
//传参两个定义一个map key：string vul：interface{}类型

	err = t.Execute(w, map[string]interface{}{
	"u1":u1,
	"u2":u2,

	})
```



```html
{{/*a comment*/}}//注释
<hr>//分页
```

Go gin使用html模板
一、engine.LoadHTMLGlob：推荐
只有一个参数，通配符，如：template/* 意思是找当前项目路径下template文件夹下所有的html文件

e.g.：engine.LoadHTMLGlob(“templates/*”)

二、engine.LoadHTMLFiles：不推荐
不定长参数，可以传多个字符串，使用这个方法需要指定所有要使用的html文件路径

e.g.：engine.LoadHTMLFiles(“templates/index.html”,“template/user.html”)

三.指定模板路径

```go
// 使用*gin.Context下的HTML方法

func Hello(context *gin.Context)  {
    name := "zhiliao"
    context.HTML(http.StatusOK,"index.html",name)
}
```

注意：不要使用golang里面run，否则会报错

```go
panic: html/template: pattern matches no files: `templates/*`
```


在cmd运行即可

在cmd运行即可

四、多级目录的模板指定
如果有多级目录，比如templates下有user和article两个目录，如果要使用里面的html文件，必须得在Load的时候指定多级才可以，比如：engine.LoadHTMLGlob(“templates/**/*”)

1.有几级目录，得在通配符上指明

两级：engine.LoadHTMLGlob("templates/**/*")
三级：engine.LoadHTMLGlob("templates/**/**/*")
1
2
2.指定html文件

// 除了第一级的templates路径不需要指定，后面的路径都要指定
e.g.：context.HTML(http.StatusOK,"user/index.html","zhiliao")
1
2
3.在html中

```go
必须使用
{{ define "user/index.html" }}

html内容

{{ end }}

包起来
```

错误





gin 模板使用错误

> panic: read tem\public: The handle is invalid.

```go
r.LoadHTMLGlob("html/tem/*")//html路径要对应 目录路径html(tem(index),main)
```

>- using env: export GIN_MODE=release - using code: gin.SetMode(gin.ReleaseMode)

将gin的GIN_MODE=release加入环境变量

完整代码

tree

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.LoadHTMLGlob("html/tem/*")
	r.GET("/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", gin.H{"title": "我是测试", "ce": "123456"})
	})
	r.Run()
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>{{.title}}</title>
</head>
<body>
密码{{.ce}}
</body>
</html>
```

//访问127.0.0.1:8080/index 密码123456

> 分离，新建目录为tem

![](images\tem.PNG)



```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
    //二级目录
	r.LoadHTMLGlob("tem/**/**/*")
	r.GET("/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "user/index.html", gin.H{"title": "我是测试", "address": "123456"})
	})
	r.Run()
}
```

```html
{{ define "user/index.html" }}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>{{.title}}</title>
</head>
<body>
我的地址{{.address}}
</body>
</html>
{{ end }}

```

必须使用
{{ define "xxx/xxx.html" }}

html内容

{{ end }}

包起来

![](images\htm.PNG)