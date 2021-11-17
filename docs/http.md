HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）应用层的协议

<img src="images\http.png" alt="http" style="zoom: 67%;" />

<img src="images\http.jpg" alt="http" style="zoom:67%;" />

http一般都是明文传输 

请求报文，

百度的首页

user请求方式图为命令行

接收方式任何数据类型

接收报文内容

http协议，状态码200 ok

。。。

<img src="images\http(1).png" alt="http(1)" style="zoom: 67%;" />

   websocker是以底层使用http协议完成连接，以二进制的形式发送数据包

​    HTTP 协议中共定义了八种方法或者叫“动作”来表明对 Request-URI 指定的资源的不同操作方式，具体介绍如下：

-  **OPTIONS**：返回服务器针对特定资源所支持的HTTP请求方法。也可以利用向Web服务器发送'*'的请求来测试服务器的功能性。
-  **HEAD**：向服务器索要与GET请求相一致的响应，只不过响应体将不会被返回。这一方法可以在不必传输整个响应内容的情况下，就可以获取包含在响应消息头中的元信息。
-  **GET**：向特定的资源发出请求。
-  **POST**：向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的创建和/或已有资源的修改。
-  **PUT**：向指定资源位置上传其最新内容。
-  **DELETE**：请求服务器删除 Request-URI 所标识的资源。
-  **TRACE**：回显服务器收到的请求，主要用于测试或诊断。
-  **CONNECT**：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。

虽然 HTTP 的请求方式有 8 种，但是我们在实际应用中常用的也就是 **get** 和 **post**，其他请求方式也都可以通过这两种方式间接的来实现。