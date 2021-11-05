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
