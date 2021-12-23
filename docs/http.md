### HTTP是什么

超文本传输协议（Hyper Text Transfer Protocol，HTTP）是一个简单的请求-响应协议，它通常运行在TCP之上。 它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。 请求和响应消息的头以ASCII形式给出；而消息内容则具有一个类似MIME的格式

### HTTP 工作原理

HTTP协议工作于客户端-服务端架构上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。

Web服务器有：Apache服务器，IIS服务器（Internet Information Services）等。

Web服务器根据接收到的请求后，向客户端发送响应信息。

HTTP默认端口号为80，但是你也可以改为8080或者其他端口。

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

完整

用get 请求 www.google.com.hk

> 请求头部

```yaml
:authority: www.google.com.hk    # 请求的域名（对方的服务器地址）
:method: GET                     # 请求方法，一般浏览器访问网站使用GET请求                     
:path: /?gws_rd=ssl              # 请求路径，也就是 https://ww.google.com.hk/ 后面的内容
:scheme: https                   # 请求的协议，这里使用https协议（使用SSL加密的http协议）
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng响应头,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
# 请求文件类型，可以吧这里的文件类型Google一下，因为篇幅太长这里就不做详细介绍了（如text/html是文档/HTML文档的意思）
accept-encoding: gzip, deflate, br  # 压缩类型，支援gzip,deflate,br 压缩方式
accept-language: zh-HK,zh;q=0.9  # 浏览器语言，我的默认语言是 zh-hk （中国-香港）
cache-control: no-cache          # 缓存讯息，这里是 不缓存（no-cache）
pragma: no-cache                 # 缓存来源
sec-fetch-dest: document         # sec-fetch-* 意为如何使用返回的参数
sec-fetch-mode: navigate
sec-fetch-site: same-origin
sec-fetch-user: ?1
upgrade-insecure-requests: 1     # 不用在意这个东西
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36 Edg/86.0.622.63
# 非常重要的浏览器UA，表明了浏览器的身份：是什么内核，运行在什么
```

> 响应头部

```yaml
alt-svc: h3-Q050=":443"; ma=2592000,h3-29=":443"; ma=2592000,h3-T051=":443"; ma=2592000,h3-T050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"
# 标示这个请求使用http/3 （google已经用上h3了啊）
cache-control: private, max-age=0  # 缓存控制：私有（缓存0秒，也就是不缓存）
content-encoding: gzip  # 压缩使用Gzip压缩
content-length: 67582   #  HTTP消息长度 67582
content-type: text/html; charset=UTF-8  # 返回文件类型 text/html ，字符编码为 UTF-8
date: Tue, 10 Nov 2020 01:06:54 GMT   # 服务器的时间
expires: -1  # 链接过期，-1不过期
p3p: CP="This is not a P3P policy! See g.co/p3phelp for more info."  #个人隐私安全平台项目，可以去wiki查询一下p3p
server: gws  # 服务器名，这个其实可以自定义，一般用于标示自己用的是什么web服务器
set-cookie: 1P_JAR=2020-11-10-01; expires=Thu, 10-Dec-2020 01:06:54 GMT; path=/; domain=.google.com.hk; Secure; SameSite=none
set-cookie: NID=204=Zl-c80ku7NgAF7VKJt84n2Z4DGob3kh9RCnBCaf21NgeIKVA7GsixU8st4FdJvPODEnS_EYoXKbPOrdhxKI_Si4k2PeLInLNoE5KNlBC1DKyZHP1z0GLesmx8sN-H4hFUe3cvrUDJMyUTq6rTyG8DL4X-rRLcgn1Iy32UlaxFDo; expires=Wed, 12-May-2021 01:06:54 GMT; path=/; domain=.google.com.hk; Secure; HttpOnly; SameSite=none
# set-cookie 是给予cookie
status: 200  # 状态码，非必要
strict-transport-security: max-age=31536000
x-frame-options: SAMEORIGIN
x-xss-protection: 0
```

### HTTP 请求头字段

这里介绍一些常用的HTTP 请求头字段：

`Host`：客户端端请求的域名。
`Connection`：告诉服务端，处理完本请求后，是否关闭连接。
`User-Agent`：客户端使用的浏览器或APP 类型/版本。
`Accept`：客户端支持哪些类型的文档。
`Accept-Encoding` ：客户端支持的编码类型。
`Accept-Language` ：客户端支持的语言类型。
`Referer` ：客户端从哪个网页过来的。
`Cache-Control`：指定缓存机制。

### HTTP 响应头字段

这里介绍一些常用的HTTP 响应头字段：

`Allow`：表明服务器支持哪些请求方法，如GET，POST 等。
`Content-Encoding`：响应内容编码方法。
`Content-Type`：响应内容属于什么MIME 类型。
`Content-Length`：响应内容的长度。
`Date`：当前GMT 时间。
`Expiress`：响应内容过期时间，过期后将不再缓存内容。
`Last-Modified`：文档的最后改动时间。
`Location`：告诉客户端到哪里获取文档，一般用于重定向。
`Refresh`：浏览器在多少秒后刷新文档。
`Server`：服务器名字。
`Set-Cookie`：设置和页面关联的Cookie。
`Date`：表示消息发送时间。

### websocket与http 区别

<img src="images\http(1).png" alt="http(1)" style="zoom: 67%;" />

> websocker是以底层使用http协议完成连接，以二进制的形式发送数据包

### HTTP请求方法

HTTP 协议支持9种`请求方法`，最常用的是`GET` 和`POST` 方法。

![](images\http的请求方法.PNG)

### HTTP GET 与POST 方法区别

post 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。

`GET 方法`与`POST 方法`是最常用的两个HTTP 方法，来看下其异同点：

- 请求内容存放位置不同：`GET 方法`一般没有请求体，其请求内容放在`URL 参数`中，`POST 方法`则将请求内容放在请求体中。
- `POST 方法` 安全性更高：`GET 请求`一般是明文传输，不利于传输敏感数据。`POST 请求`内容在请求体中，更方便加密，提高安全性。
- `POST 方法`传输的数据量更大：`GET 请求`内容在URL 中，因此有大小限制，而`POST 请求` 内容在请求体中，理论上没有大小限制。

### RESTful API 

必须有一种统一的机制，方便不同的前端设备与后端进行通信

API与用户的通信协议，总是使用[HTTPs协议](https://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)。

1.应该将API的版本号放入URL。

> ```javascript
> https://api.example.com/v1
> ```

2.举例来说，有一个API提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

```go
- https://api.example.com/v1/zoos
- https://api.example.com/v1/animals
- https://api.example.com/v1/employees
```

对应的sql语句

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
- DELETE（DELETE）：从服务器删除资源

过滤信息

如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。

下面是一些常见的参数。

> - ?limit=10：指定返回记录的数量
> - ?offset=10：指定返回记录的开始位置。
> - ?page=2&per_page=100：指定第几页，以及每页的记录数。
> - ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
> - ?animal_type_id=1：指定筛选条件

## Content-Type的类型常见类型

Content-Type是返回消息中非常重要的内容，表示后面的文档属于什么MIME类型。Content-Type: [type]/[subtype]; parameter。例如最常见的就是text/html

Accept代表发送端（客户端）希望接受的数据类型。
比如：Accept：text/xml;
代表客户端希望接受的数据类型是xml类型

Content-Type代表发送端（客户端|服务器）发送的实体数据的数据类型。
比如：Content-Type：text/html;
代表发送端发送的数据格式是html。

"application/xml" 和 "text/xml"两种类型， 二者功能一模一样，唯一的区别就是编码格式，text/xml忽略xml头所指定编码格式而默认采用us-ascii编码，而application/xml会根据xml头指定的编码格式来编码。

```bash
text/html : HTML格式 (最常见)

text/plain :纯文本格式

text/xml :XML格式

image/gif :gif图片格式

image/jpeg  :jpg图片格式

image/png :png图片格式

Content-Type的类型如下：

`以application开头的媒体格式类型`

application/xhtml+xml :XHTML格式

application/xml : XML数据格式

application/atom+xml ：Atom XML聚合格式

application/json : JSON数据格式

application/pdf :pdf格式

application/msword : Word文档格式

application/octet-stream  :二进制流数据（如常见的文件下载）

application/x-www-form-urlencoded  :中默认的encType，form表(常见post 提交数据方式 数据key1=val1&key2=val2 的方式进行编码)

multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式 (常见post 提交数据方式，需要在表单中进行文件上传时)


`以audio开头的常见媒体格式文件：``

'audio/x-wav' : wav文件
'audio/x-ms-wma' : wma 文件
'audio/mp3' : mp3文件
`以video开头的常见媒体格式文件：

 'video/x-ms-wmv' : wmv文件
 'video/mpeg4' : mp4文件
 'video/avi' : avi文件
```

[参考的原文链接1：](https://blog.csdn.net/qq_24147051/article/details/90477756)

[参考的原文链接2](https://segmentfault.com/a/1190000013056786)

### HTTP的状态码

HTTP 状态码分为5 种类型，由三个十进制数字组成。第一个数字（`1-5`）代表状态码的分类，后两位是其含义。

![](images\状态码.PNG)

![4000](images\常见状态码.PNG)

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

curl -v www.sina.com

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



### URL

www.127.0.0.1:8080/hello

url www.127.0.0.1:8080

uri  /hello(虚拟目录)

![img](images\url.png)

.**参数**：从“？”开始到“#”（或至结束）为止之间的部分为参数部分，又称搜索部分、查询部分。参数间用“&”作为分隔符。

.**锚**：或称片段（fragment），HTTP请求不包括锚部分，从“#”开始到最后，都是锚部分。本例中的锚部分是“r_70732423“。锚部分不是一个URL必须的部分。

　　锚点作用：打开用户页面时滚动到该锚点位置。如：一个html页面中有一段代码【<div name='r_70732423'>...</div>】，该url的hash为r_70732423

### HTTP和HTTPS区别

```bash
HTTP和HTTPS的区别
    • HTTPS是加密传输协议，HTTP是名文传输协议
    • HTTPS需要用到SSL证书，而HTTP不用
    • HTTPS比HTTP更加安全，对搜索引擎更友好，利于SEO
    • HTTPS标准端口443，HTTP标准端口80
    • HTTPS基于传输层，HTTP基于应用层
    • HTTPS在浏览器显示绿色安全锁，HTTP没有显示
    
    
1.证书可以认为就是公钥；
 
2.在Https通信中，需要CA认证中心的证书以及服务器的证书和私钥；
 
3.服务器的证书是用来发送给客户端的；
 
4.CA认证中心的证书需要安装在客户机上，用来验证服务器证书的真实性
```

