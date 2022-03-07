## Docker

**docker** 包括三个基本概念

> **镜像**（`Image`）

操作系统分为 **内核** 和 **用户空间**。对于 `Linux` 而言，内核启动后，会挂载 `root` 文件系统为其提供用户空间支持。而 **Docker 镜像**（`Image`），就相当于是一个 `root` 文件系统。比如官方镜像 `ubuntu:18.04` 就包含了完整的一套 Ubuntu 18.04 最小系统的 `root` 文件系统。

**Docker 镜像** 是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像 **不包含** 任何动态数据，其内容在构建之后也不会被改变。

> **容器**（`Container`）

镜像（`Image`）和容器（`Container`）的关系，就像是面向对象程序设计中的 `类` 和 `实例` 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 [命名空间](https://en.wikipedia.org/wiki/Linux_namespaces)。因此容器可以拥有自己的 `root` 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样

> **仓库**（`Repository`）

镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，[Docker Registry]() 就是这样的服务。

一个 **Docker Registry** 中可以包含多个 **仓库**（`Repository`）；每个仓库可以包含多个 **标签**（`Tag`）；每个标签对应一个镜像。

通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过 `<仓库名>:<标签>` 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 `latest` 作为默认标签。

## container容器制作

```bash
docker run -p 6379:6379 --name redis -v $PWD/data:/data -d redis:latest redis-server --appendonly yes
```

例如制作以上redis的容器

> -e 设置环境变量

> -- name 容器名

> -v &PWD目录挂载本地:容器
>
> PWD是一个环境variables，您的shell将扩展到您当前的工作目录。 因此，在这个例子中，它会将当前工作目录（从执行此命令的位置）挂载到容器中的/data。
>
> 例如，假设你的当前工作目录是/ home / youruser / somedir，那么你的命令行将在执行之前通过你的shell扩展到这个目录：

> -d +镜像  后台运行

> -m +val m设置容器的内存上限，--memory-swap=600M

> -p 6666:8888 本机端口映射到容器端口，127.0.0.1:6666可访问

> --rm 容器停止后删除

> 连接到container内部的tty 语法如下
> $ docker exec -it <container name/id> /bin/bash

比如连接redis和MySQL

docker exec -it container id redis-cli/mysql bash

> -- wget -o-表示使用docker的wget命令

[docker容器]wget命令是什么？wget命令用来从指定的URL下载文件

使用wget -O下载并以不同的文件名保存(-O：下载文件到对应目录，并且修改文件名称)

```
wget -O wordpress.zip http://www.minjieren.com/download.aspx?id=1080
```

使用wget -b后台下载

```
wget -b <a href="http://www.minjieren.com/wordpress-3.1-zh_CN.zip">http://www.minjieren.com/wordpress-3.1-zh_CN.zip</a>
```

备注： 你可以使用以下命令来察看下载进度：tail -f wget-log

> docker tag **用于给镜像打标签** 

docker会员可以自动更新镜像，但是普通用户不会

<name>[:<tag>]，<tag>不写默认为latest

```bash
#语法
docker tag old-image[:old-tag] new-image[:new-tag]
```

![](images\tag.PNG)

tag似乎更加灵活，docker将文件等信息的变动抽象为一次次的commit，每一次commit以后可能走向不同的分支，当我们完成dockerfile的构建后，会生成一串无规则的字符串代表此次生成的ID，此时，tag的作用就是为他创建一个友好的NAME，方便我们对镜像库的管理。

> 查看容器log

docker logs +container id

> 推送镜像

登录后通过docker push 来推送自己的镜像

docker tag xx(镜像) 用户名/xx(镜像名)

## 规范

Don’t run as root 
Timezone problem  根据时区
Minify image size （使用alpine系列的Base image, alpine是一个精简版linux，镜像基于alpine构建所以体积也小。docker pull redis:6.2.4-alpine 仅为23M ）
Always pull （docker build --pull ...指定版本,`因为本地镜像不会自动更新`）
PID=1  PID namespace（名空间）

docker 是推崇一个容器一个进程(one process per container)”的方式 

比如查看redis容器的PID

![](images\PID.PNG)

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

## docker基础命令

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

docker  rm  镜像id

运行镜像并映射端口号

```bash
docker run -d -p :8080:8080 myimage（镜像名）记得将本地的-p 端口号映射

可以正常访问
```

hello  /

> 退出docker  

exit或者ctrl+D（其他容器也一样）

**docker inspect :** 获取容器/镜像的元数据。

docker inspect + container id

> docker attach + container_id 查看容器被访问状态



 [参考](https://www.cnblogs.com/Earen/p/15337421.html)

[文档](https://yeasy.gitbook.io/docker_practice/)

> 约定

命令不区分大小写，但是命名约定为全部大写。

## registry （私有仓库配置）

```dockerfile
$ docker run -d -p 5000:5000 --restart=always --name registry registry -v /opt/data/registry:/var/lib/registry
```





### volume数据卷

`数据卷` 是被设计用来持久化数据的，它的生命周期独立于容器



```
$ docker volume rm +volume_name
### 
```

## dockerfile构建image

> Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。
> 在Dockerfile 中命令书写对先后顺序及表示其执行对顺序，在书写时需注意。

在cmd中



### dockerfile常用command

### FROM

> 所有Dockerfile 都必须以 `FROM` 命令开始。
> `FROM` 命令会指定镜像基于哪个基础镜像来进行创建，后面对命令也会机会这个镜像来执行。
> `FROM` 命令可以多次使用，表示创建多个镜像。
> 语法如下：

```bash
FROM [images name]
```

例如：

```bash
FROM mcr.microsoft.com/dotnet/aspnet:5.0-focal AS base
```

### WORKDIR

> `WORKDIR` 命令用于指定 `RUN`、`CMD`、`ENTRYPOINT` 等命令对工作目录。
> 语法如下：

```bash
WORKDIR [path]
```

例如：

```bash
WORKDIR /app
```

### EXPOSE

> `EXPOSE` 用于指定容器在运行时监听对端口
> 语法如下：

```bash
EXPOSE [port]
```

例如：

```bash
EXPOSE 8080
```

### COPY

> `COPY` 复制指令，从上下文目录中复制文件或者目录到容器里指定路径。
> 语法如下：

```bash
COPY [path,newpath]
```

例如：

```bash
COPY src/nuget.config nuget.config
```

### ADD

> `ADD` 指令和 `COPY` 的使用格类似（同样需求下，官方推荐使用 `COPY`）。功能也类似
> 在执行 <源文件> 为 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，会自动复制并解压到 <目标路径>。
> 在不解压的前提下，无法复制 tar 压缩文件。会令镜像构建缓存失效，从而可能会令镜像构建变得比较缓慢。具体是否使用，可以根据是否需要自动解压来决定。
> 语法如下：

```bash
ADD [path,newpath]
```

例如：

```bash
ADD src/nuget.config nuget.config
```

### RUN

用于在image里执行指令，安装软件和下载文件

> `RUN` 在shell或者exec的环境下执行的命令。`RUN` 指令会在新创建的镜像上添加新的层面，接下来提交的结果用在Dockerfile的下一条指令中。
> 语法如下：

```bash
RUN ["<可执行文件或命令>","<param1>","<param2>",...] 
```

例如：

```bash
-- 还原项目资源包
RUN dotnet restore "HanddayRetail.MallApi/HanddayRetail.MallApi.csproj" --configfile "nuget.config"
-- 构建项目
RUN dotnet build "HanddayRetail.MallApi/HanddayRetail.MallApi.csproj" -c Release -o /app/build
```

### CMD

> `CMD` 类似于 RUN 指令，用于运行程序,CMD 指令指定的程序可被 `docker run` 命令行参数中指定要运行的程序所覆盖。，但二者运行的时间点不同:
> CMD 在docker run 时运行。
> RUN 是在 docker build。

```bash
CMD ["<可执行文件或命令>","<param1>","<param2>",...] #推荐
CMD ["param1","param2"]
CMD command param1 param2
```

注意：如果 Dockerfile 中如果存在多个 `CMD` 指令，仅最后一个生效。

### ENTRYPOINT

> `ENTRYPOINT` 配置给容器一个可执行的命令，这意味着在每次使用镜像创建容器时一个特定的应用程序可以被设置为默认程序。同时也意味着该镜像每次被调用时仅能运行指定的应用。
> 类似于CMD，Docker只允许一个ENTRYPOINT，多个ENTRYPOINT会抵消之前所有的指令，只执行最后的ENTRYPOINT指令。
> 语法如下：

```bash
ENTRYPOINT ["<可执行文件或命令>","<param1>","<param2>",...] 
ENTRYPOINT command param1 param2
```

例如：

```bash
ENTRYPOINT ["dotnet", "HanddayRetail.MallApi.dll"]
```

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", HandleHello)
	server := &http.Server{
		Addr: ":9090",
	}
  fmt.Println("Server startup...")
	if err := server.ListenAndServe(); err != nil {
		fmt.Printf("Server startup failed, err:%v\n", err)
	}
}

func HandleHello(w http.ResponseWriter, _ *http.Request) {
	w.Write([]byte("Hello World"))
}

```

dockerfile

```go
FROM golang:alpine

# 设置镜像环境变量
ENV GO111MODULE=on \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# 移动到工作目录：/build
WORKDIR /build

# 将本目录下代码复制到容器中
COPY . .

# 将我们的代码编译成二进制可执行文件app
RUN go build -o app .

# 声明服务端口
EXPOSE 9090

# 启动容器时运行的命令
CMD ["/app"]

```

#### 构建镜像[#](https://www.niuwx.cn/posts/go/docker_go/#构建镜像)

使用命令构建镜像。

docker build . -t goweb

#### 使用镜像

docker run -p 9090:9090 --name goweb-app goweb

二进制文件

```bash
FROM golang:alpine AS builder

# 设置镜像环境变量
ENV GO111MODULE=on \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# 移动到工作目录：/build
WORKDIR /build

# 将本目录下代码复制到容器中
COPY . .

# 将我们的代码编译成二进制可执行文件app
RUN go build -o app .

###################
# 最终的小镜像
###################
FROM scratch

# 从builder镜像中把/app 拷贝到当前目录
COPY --from=builder /build/app /

# 需要运行的命令
ENTRYPOINT ["/app"]

```

如果需要部署的程序还需要用到静态资源，那么还需要将静态资源拷贝到镜像中。

```bash
FROM golang:alpine AS builder

# 设置镜像环境变量
ENV GO111MODULE=on \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# 移动到工作目录：/build
WORKDIR /build

# 将本目录下代码复制到容器中
COPY . .

# 将我们的代码编译成二进制可执行文件app
RUN go build -o app .

###################
# 最终的小镜像
###################
FROM scratch

COPY /templates /templates
COPY /static /static

# 从builder镜像中把/app 拷贝到当前目录
COPY --from=builder /build/app /

# 需要运行的命令
ENTRYPOINT ["/app"]

```



## 部署多个 docker compose

#### nginx

#### 拉取 nginx 镜像

```
docker pull nginx
```

#### 编写 docker-compose.yml 文件

```
version: '3'

services:
  nginx:
    image: nginx
    container_name: nginx
    volumes: ["/mnt/nginx/config.d:/etc/nginx/conf.d", "/mnt/static:/static"]
    ports:
      - "80:80"
```

#### conf 文件配置静态文件服务

```
server {
        listen 80;
        server_name localhost;
        location /vehicle-service/static/image/ {
                root /static;
                index index.html;
        }
}
```

#### mysql

### 拉取 mysql 镜像

```
docker pull mysql
```

## 编写 docker-compose.yml 文件

```
version: '3.8'
services:
  mysql:
    image: mysql:5.7.26
    container_name: mysql-stu
    ports:
      - "3306:3306"
    volumes:
      - /mnt/mysql/data:/var/lib/mysql
      - /mnt/mysql/initdb:/docker-entrypoint-initdb.d
      - /mnt/mysql/cnf/my.cnf:/etc/my.cnf
      - /mnt/mysql/cnf/mysql:/etc/mysql
      - /mnt/mysql/mysql-files:/var/lib/mysql-files
    command: [
        '--character-set-server=utf8mb4',
        '--collation-server=utf8mb4_unicode_ci',
        '--max_connections=3000'
    ]
    environment:
      MYSQL_ROOT_PASSWORD: "123456"
      TZ: "Asia/Shanghai"

```

## 配置 mysql root 账户可远程连接

```
docker exec -it mysql /bin/bash #进入容器
use mysql;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'xxxxxxxxxxxxx';
grant all privileges on *.* to 'root'@'%';
flush privileges;
```

# redis

#### 拉取 redis 镜像

```
docker pull redis
```

#### 编写 docker-compose.yml 文件

```
version: '3.5'
services:
  redis:
    image: "redis:5"
    container_name: redis
    command: redis-server /etc/redis/redis.conf
    ports:
      - "6379:6379"
    volumes:
      - /mnt/redis/data:/data
      - /mnt/redis/redis.conf:/etc/redis/redis.conf
```

#### 常用命令

```
docker-compose up -d   #启动当前目录下 docker-compose.yml 文件的容器
docker-compose down    #移除镜像，如果修改了 docker-compose.yml 需要重新创建容器才生效
docker-compose restart #重启容器，不会删除容器，在修改 conf 文件后，需要 restart 生效 
```



## 微服务

> Nginx

Nginx是一款自由的、开源的、高性能的HTTP服务器和反向代理服务器；同时也是一个IMAP、POP3、SMTP代理服务器；Nginx可以作为一个HTTP服务器进行网站的发布处理，另外Nginx可以作为反向代理进行负载均衡的实现

1.使用docker 下载nginx 镜像 docker pull nginx

2.启动nginx

docker run --name nginx -p 80:80 -d nginx

这样就简单的把nginx启动了，但是我们想要改变配置文件nginx.conf ，进入容器,命令：

docker exec -it nginx bash

nginx.conf配置文件在 /etc/nginx/ 下面，但是你使用vim nginx.conf 或者vi nginx.conf

apt-get update 完成之后 apt-get install vim

docker pull nginx

docker run --name nginx -p 80:80 -d nginx

>错误问题

配置mysql和redis的环境变量

5.7版本

```bash
panic: Error 1045: Access denied for user '"root'@'172.17.0.1' (using password: YES)
#忘记密码然后用
update mysql_test.user set authentication_string=password('Aa123456') where user='root';
 #刷新权限
flush privileges;
 #退出
 quit
# 重新进入
 mysql -u root -p
```

> -net host 来启动docker项目

   查询

```sql
 select host,user,plugin,authentication_string from mysql.user;
```

1. 添加密码IP远程访问权限：

   ```sql
   GRANT ALL PRIVILEGES ON *.* TO 'root'@'host' IDENTIFIED BY 'Aa123456' WITH GRANT OPTION;
   ```

   表示设置指定用户名为root，密码为123456，可访问所有数据库*，只有IP为172.33.5.46这台机器有权限访问。

```javascript
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'Aa123456';
```