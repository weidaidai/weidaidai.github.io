### HTTP是什么

超文本传输协议（Hyper Text Transfer Protocol，HTTP）是一个简单的请求-响应协议，它通常运行在TCP之上。 它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。 请求和响应消息的头以ASCII形式给出；而消息内容则具有一个类似MIME的格式

### HTTP消息体结构    

参考https://zhuanlan.zhihu.com/p/137679911

![](images\http的消息结构.jpg)

HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）应用层的协议

<img src="images\http.jpg" alt="http" style="zoom:67%;" />

http一般都是明文传输 

HTTP 请求由四部分组成：

- 请求行：分别是`请求方法`+`空格`+`URL`+`空格`+`协议版本`+`\r\n`
- 请求头部：由多个请求头部`键值对`组成，中间以冒号`:`隔开，每个`键值对`最后是`\r\n`
- 空行：即`\r\n`
- 请求包体：包体部分

> 如图上

请求报文，

百度的首页

user为命令行curl发送

接收方式任何数据类型

HTTP 响应由四部分组成：

- 状态行：分别是`协议版本`+`空格`+`状态码`+`空格`+`状态码描述`+`\r\n`
- 响应头部：由多个响应头部`键值对`组成，中间以冒号`:`隔开，每个`键值对`最后是`\r\n`
- 空行：即`\r\n`
- 响应包体：包体部分

> 如图上

接收报文内容

http协议，状态码200 ok

`Cache-Control`客户端可以在 HTTP 请求中使用的标准指令。

`private`指示该响应是针对单个用户的，并且不能由共享缓存存储。私有缓存可以存储该响应。

`no-cache`在释放缓存副本之前，强制高速缓存将请求提交给原始服务器进行验证。

 `proxy-revalidate` 重新验证和重新加载

`no-transform`不应该对资源进行转换或转换。

。。。

### HTTP 请求头字段

这里介绍一些常用的HTTP 请求头字段：

> `Host`：客户端端请求的域名。
> `Connection`：告诉服务端，处理完本请求后，是否关闭连接。
> `User-Agent`：客户端使用的浏览器或APP 类型/版本。
> `Accept`：客户端支持哪些类型的文档。
> `Accept-Encoding` ：客户端支持的编码类型。
> `Accept-Language` ：客户端支持的语言类型。
> `Referer` ：客户端从哪个网页过来的。
> `Cache-Control`：指定缓存机制。

### HTTP 响应头字段

这里介绍一些常用的HTTP 响应头字段：

> `Allow`：表明服务器支持哪些请求方法，如GET，POST 等。
> `Content-Encoding`：响应内容编码方法。
> `Content-Type`：响应内容属于什么MIME 类型。
> `Content-Length`：响应内容的长度。
> `Date`：当前GMT 时间。
> `Expiress`：响应内容过期时间，过期后将不再缓存内容。
> `Last-Modified`：文档的最后改动时间。
> `Location`：告诉客户端到哪里获取文档，一般用于重定向。
> `Refresh`：浏览器在多少秒后刷新文档。
> `Server`：服务器名字。
> `Set-Cookie`：设置和页面关联的Cookie。
> `Date`：表示消息发送时间。
>
> ### websocker与http

<img src="images\http(1).png" alt="http(1)" style="zoom: 67%;" />

> websocker是以底层使用http协议完成连接，以二进制的形式发送数据包

### HTTP请求方法

HTTP 协议支持9 种`请求方法`，最常用的是`GET` 和`POST` 方法。

![](images\http的请求方法.PNG)

### HTTP GET 与POST 方法区别

`GET 方法`与`POST 方法`是最常用的两个HTTP 方法，来看下其异同点：

- 请求内容存放位置不同：`GET 方法`一般没有请求体，其请求内容放在`URL 参数`中，`POST 方法`则将请求内容放在请求体中。
- `POST 方法` 安全性更高：`GET 请求`一般是明文传输，不利于传输敏感数据。`POST 请求`内容在请求体中，更方便加密，提高安全性。
- `POST 方法`传输的数据量更大：`GET 请求`内容在URL 中，因此有大小限制，而`POST 请求` 内容在请求体中，理论上没有大小限制。

### HTTP的状态码

HTTP 状态码分为5 种类型，由三个十进制数字组成。第一个数字（`1-5`）代表状态码的分类，后两位是其含义。

![](images\状态码.PNG)

![](images\常见状态码.PNG)

### CURL

环境部署参考

https://segmentfault.com/a/1190000022581025

操作指南

https://www.ruanyifeng.com/blog/2019/09/curl-reference.html

curl 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端（client）的 URL 工具的意思。

Windows 下安装curl

不带有任何参数时，curl 就是发出 GET 请求。

> ```bash
> $ curl https://www.example.com
> ```

上面命令向`www.example.com`发出 GET 请求，服务器返回的内容会在命令行输出。

curl -v www.baidu.com

![](images\http请求.PNG)

 -v 选项，--verbose，指定该选项后，可以跟踪URL的连接信息。我们可以根据这个选项看看curl是怎么工作的。

```bash
#保存文件
curl -o 具体文件名 url
curl -o home.html  http://www.sina.com.cn

curl -XGET www.baidu.com
curl -XPOST www.baidu.com
curl -XDELETE www.baidu.com
curl -XPUT www.baidu.com
```

