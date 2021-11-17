## docker

镜像（image），容器（container），仓库

> dockerfile



![](images\docker.jpg)

开始一个小实战

建立一个server.go

```go
package main

import (
	"fmt"
	"html"
	"log"
	"net/http"
)

func main() {
	fmt.Println("开启服务端口8080")
	http.HandleFunc("/",func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w,"hello,%q",html.EscapeString(r.URL.Path))
	})
	log.Fatal(http.ListenAndServe(":8080",nil))
}
```

创建 Dockerfile

```dockerfile
FROM golang:latest #最近版本

RUN mkdir /app #创建目录

WORKDIR /app

add . /app

RUN go build -o main ./server.go

EXPOSE 8080

CMD /app/main
```

访问[localhost:8080]

hello  /

## 基础命令

开始创建在当前目录下

docker build .

命名为myimage

docker build . -t myimage

> 查看镜像

docker image ls

> 查看所有镜像容器

docker ps -a

> 启动镜像

docker run -d myimage

> 查看容器

docker container ls

> 关闭容器

docker stop 容器id

> 删除容器

docker rm 容器id

> 查看所有容器状态

 docker container ls -a

> 执行镜像删除命令： 

运行镜像并映射端口号

```bash
docker run -d -p :8080:8080 myimage（镜像名）记得将本地的-p 端口号映射

可以正常访问
```

hello  /

#### 退出docker  

exit或者ctrl+D（其他容器也一样）

## 部署多个 docker compose





















