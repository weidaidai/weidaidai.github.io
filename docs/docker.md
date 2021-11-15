## docker

镜像（image），容器（container），仓库

dockerfile



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

查看镜像

docker image ls

启动镜像

docker run -d myimage

查看容器

docker container ls

关闭容器

docker stop 容器id

查看所有容器状态

docker container ls -a

运行镜像并映射端口号

docker run -d -p:8080:8080 myimage（镜像名）记得将本地的-p 端口号映射

可以正常访问

## 部署多个 docker compose





















拉取mongodb最新

docker pull mongo:latest

设置目录和端口

docker run -p 27017:27017 -v /data/mongo:/data/db --name mongodb -d mongo

```bash
-p 映射容器服务的 27017 端口到宿主机的 27017 端口。外部可以直接通过 宿主机 ip:27017 访问到 mongo 的服务

-v 为设置容器的挂载目录，这里是将本机的“/data/mongo”目录挂载到容器中的/data/db中，作为 mongodb 的存储目录

--name 为设置该容器的名称

-d 设置容器以守护进程方式运行
```

```bash
设置管理员账号
db.createUser({`
`user:` `'admin',`
`pwd:` `'Aa123456',`
`roles: [ { role:` `"userAdminAnyDatabase", db:` `"admin"` `} ]`
`});　
```

