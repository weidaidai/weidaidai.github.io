## grpc

### RPC是干什么的？

当用户的请求到来时，我们需要将用户的请求分散到多个服务去各自处理，然后又需要将这些子服务的结果汇总起来呈现给用户。那么服务之间该使用何种方式进行交互就是需要解决的核心问题。RPC 就是为解决服务之间信息交互而发明和存在的。

在 gRPC 中，客户端应用程序可以直接调用不同机器上的服务器应用程序上的方法，就像它是本地对象一样，使您可以更轻松地创建分布式应用程序和服务

 客户端a，b 相加方法函数在服务端，调用服务端的函数得到a+b

是谷歌开源的一个 RPC 框架，面向移动和 HTTP/2 设计。

内容交换格式采用ProtoBuf(Google Protocol Buffers)，开源已久，提供了一种灵活、高效、自动序列化结构数据的机制，作用与XML，Json类似，但使用二进制，（反）序列化速度快，压缩效率高。
传输协议 采用http2，和很多RPC系统一样，服务端负责实现定义好的接口并处理客户端的请求，客户端根据接口描述直接调用需要的服务。客户端和服务端可以分别使用gPRC支持的不同语言实现。

ProtoBuf 具有强大的IDL（interface description language，接口描述语言）和相关工具集（主要是protoc）。用户写好.proto描述文件后，protoc可以将其编译成众多语言的接口代码。

### 安装Protocol Buffers v3

安装用于生成gRPC服务代码的协议编译器，最简单的方法是从下面的链接：https://github.com/google/protobuf/releases下载适合你平台的预编译好的二进制文件（`protoc-<version>-<platform>.zip`）。

下载完之后，执行下面的步骤：

1. 解压下载好的文件

2. 把`protoc`二进制文件的路径加到环境变量中

3. 安装插件

   ```sh
   $ go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.26
   $ go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1
   ```

```protobuf
//指定版本
syntax="proto3";
//给proto分包用的
package person;

//配置以go mod 路径为开始；别名
option go_package="src/pd/person;person";

//相当于结构体，先声明变量后给唯一值,
message Person{
  string name=1;
  int64  age=2;
  bool sex=3;
  //切片
  repeated string test=4;
  //map key只能用string
  map<string,string>test_map=5;
}

//嵌套
message Home{
  repeated Person person=1;
  message  v {
    string name =1;

  }

}
```

```protobuf
syntax = "proto3"; // 版本声明，使用Protocol Buffers v3版本

package pb; // 包名


// 定义一个打招呼服务
service Greeter {
  // SayHello 方法
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// 包含人名的一个请求消息
message HelloRequest {
  string name = 1;
}

// 包含问候语的响应消息
message HelloReply {
  string message = 1;
}
option go_package = "/proto";
```

执行下面的命令，生成go语言源代码：

```bash
protoc -I helloworld/ helloworld/pb/helloworld.proto --go_out=plugins=grpc:helloworld
```

会在pd自动生成

pb/helloworld.pd.go

protoc-gen-go的不同版本兼容性问题。

解决办法：
一是，在proto文件中加上`option go_package = "/proto";`

### 编写Server端Go代码

```go
package main

import (
	"fmt"
	"net"

	pb "gRPC_demo/helloworld/pb"
	"golang.org/x/net/context"
	"google.golang.org/grpc"
	"google.golang.org/grpc/reflection"
)

type server struct{}

func (s *server) SayHello(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
	return &pb.HelloReply{Message: "Hello " + in.Name}, nil
}

func main() {
	// 监听本地的8972端口
	lis, err := net.Listen("tcp", ":8972")
	if err != nil {
		fmt.Printf("failed to listen: %v", err)
		return
	}
	s := grpc.NewServer() // 创建gRPC服务器
	pb.RegisterGreeterServer(s, &server{}) // 在gRPC服务端注册服务

	reflection.Register(s) //在给定的gRPC服务器上注册服务器反射服务
	// Serve方法在lis上接受传入连接，为每个连接创建一个ServerTransport和server的goroutine。
	// 该goroutine读取gRPC请求，然后调用已注册的处理程序来响应它们。
	err = s.Serve(lis)
	if err != nil {
		fmt.Printf("failed to serve: %v", err)
		return
	}
}
```

将上面的代码保存到`gRPC_demo/helloworld/server/server.go`文件中，编译并执行：

```bash
cd helloworld/server
go build
./server
```

### 编写Client端Go代码

```go
package main

import (
	"context"
	"fmt"

	pb "gRPC_demo/helloworld/pb"
	"google.golang.org/grpc"
)

func main() {
	// 连接服务器
	conn, err := grpc.Dial(":8972", grpc.WithInsecure())
	if err != nil {
		fmt.Printf("faild to connect: %v", err)
	}
	defer conn.Close()

	c := pb.NewGreeterClient(conn)
	// 调用服务端的SayHello
	r, err := c.SayHello(context.Background(), &pb.HelloRequest{Name: "q1mi"})
	if err != nil {
		fmt.Printf("could not greet: %v", err)
	}
	fmt.Printf("Greeting: %s !\n", r.Message)
}
```

将上面的代码保存到`gRPC_demo/helloworld/client/client.go`文件中，编译并执行：

```bash
cd helloworld/client/
go build
./client
```

得到输出如下（注意要先启动server端再启动client端）：

```bash
$ ./client 
Greeting: Hello q1mi!
```

此时我们的目录结构如下：

```bash
./gRPC_demo
├── go.mod
├── go.sum
└── helloworld
    ├── client
    │   ├── client
    │   └── client.go
    │   ├── client.py
    ├── pb
    │   ├── helloworld.pb.go
    │   └── helloworld.proto
    └── server
        ├── server
        └── server.go
```

### gRPC跨语言调用

接下来，我们演示一下如何使用gRPC实现跨语言的RPC调用。

我们使用`Python`语言编写`Client`，然后向上面使用`go`语言编写的`server`发送RPC请求。

### 生成Python代码

在`gRPC_demo`目录执行下面的命令：

```bash
python -m grpc_tools.protoc -I helloworld/pb/ --python_out=helloworld/client/ --grpc_python_out=helloworld/client/ helloworld/pb/helloworld.proto
```

上面的命令会在`gRPC_demo/helloworld/client/`目录生成如下两个python文件：

```bash
helloworld_pb2.py
helloworld_pb2_grpc.py
```

### 编写Python版Client

在``gRPC_demo/helloworld/client/`目录闯将`client.py`文件，其内容如下：

```python
# coding=utf-8

import logging

import grpc

import helloworld_pb2
import helloworld_pb2_grpc


def run():
    # 注意(gRPC Python Team): .close()方法在channel上是可用的。
    # 并且应该在with语句不符合代码需求的情况下使用。
    with grpc.insecure_channel('localhost:8972') as channel:
        stub = helloworld_pb2_grpc.GreeterStub(channel)
        response = stub.SayHello(helloworld_pb2.HelloRequest(name='q1mi'))
    print("Greeter client received: {}!".format(response.message))


if __name__ == '__main__':
    logging.basicConfig()
    run()
```

将上面的代码保存执行，得到输出结果如下：

```bash
gRPC_demo $ python helloworld/client/client.py 
Greeter client received: Hello q1mi
```