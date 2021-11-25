## docker

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

docker  rm  镜像id

运行镜像并映射端口号

```bash
docker run -d -p :8080:8080 myimage（镜像名）记得将本地的-p 端口号映射

可以正常访问
```

hello  /

> 退出docker  

exit或者ctrl+D（其他容器也一样）



## 说明

> Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。
> 在Dockerfile 中命令书写对先后顺序及表示其执行对顺序，在书写时需注意。

参考[Dockerfile 中对常用命令详解 - Earen - 博客园 (cnblogs.com)](https://www.cnblogs.com/Earen/p/15337421.html)

## 约定

命令不区分大小写，但是命名约定为全部大写。

## 常用命令

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
CMD ["<可执行文件或命令>","<param1>","<param2>",...] 
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

## 部署多个 docker compose





















