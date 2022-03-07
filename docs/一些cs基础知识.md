### git

合并分支到主分支master

创建commit新分支内容后并提交分支到GitHub后，切换到主分支

git checkout master
然后从GitHub拉回主分支master(自己开发可以不用，保险还是要）

git pull origin master
合并新分支到master ，然后提交master

git merge chgl16
git status  # 仅查看状态
git push origin master

### 栈（Stack）与堆（Heap）

栈和堆是编程语言最核心的数据结构，但是在很多语言中，你并不需要深入了解栈与堆。 但对于Rust这样的系统编程语言，值是位于栈上还是堆上非常重要, 因为这会影响程序的行为和性能。

栈和堆的核心目标就是为程序在运行时提供可供使用的内存空间，关于它们的详细解释和实现方式，请参见[Rust代码鉴赏](https://codes.rs/data-structures/heap.html)一书.

#### 栈

栈按照顺序存储值并以相反顺序取出值，这也被称作 **后进先出**。想象一下一叠盘子：当增加更多盘子时，把它们放在盘子堆的顶部，当需要盘子时，再从顶部拿走。不能从中间也不能从底部增加或拿走盘子！

增加数据叫做 **进栈**，移出数据则叫做 **出栈**。

因为上述的实现方式，栈中的所有数据都必须占用已知且固定大小的内存空间，假设数据大小是未知的，那么在取出数据时，你将无法取到你想要的数据。

#### 堆

与栈不同，对于大小位置或者可能变化的数据，我们需要将它存储在堆上。

当向堆上放入数据时，需要请求一定大小的内存空间。操作系统在堆的某处找到一块足够大的空位，把它标记为已使用，并返回一个表示该位置地址的 **指针**, 该过程被称为 **在堆上分配内存**，有时简称为 “分配”（allocating）。

接着，该指针会被推入`栈`中，因为指针的大小是已知且固定的，在后续使用过程中，你将通过栈中的指针，来获取数据在堆上的实际内存位置，进而访问该数据。

由上可知，堆是一种缺乏组织的数据结构。想象一下去餐馆就座吃饭: 进入餐馆，告知服务员有几个人，然后服务员找到一个够大的空桌子(堆上分配的内存空间)并领你们过去。如果有人来迟了，他们也可以通过桌号(栈上的指针)来找到你们坐在哪。

#### 性能区别

入栈比在堆上分配内存要快，因为入栈时操作系统无需分配新的空间：新数据的位置放入栈顶。相比之下，在堆上分配内存则需要更多的工作，这是因为操作系统必须首先找到一块足够存放数据的内存空间，接着做一些记录为下一次分配做准备。

访问堆上的数据比访问栈上的数据慢，因为必须通过指针来访问内存。得益于CPU高速缓存，现代处理器访问内存的次数越少则越快，栈数据往往可以存储在cpu高速缓存中，而堆数据只能存储在内存中，这两者的访问速度差异在10倍以上！

因此，处理器在处理栈上数据的时候比处理堆上的数据更加高效，同时，在堆上分配大量的空间也可能消耗时间。

### 时序图

> 生命线

 生命线在顺序图中表示为从对象图标向下延伸的一条**虚线，表示对象存在的时间**



> 控制焦点

**控制焦点是顺序图中表示\**时间段\**的符号**，在这个时间段内对象将执行相应的操作。用小矩形表示

![](images\时序图.png)

![](images\时序图1.png)

##  

#### SSO

**什么是单点登录？**

**单点登录**( **SSO** ) 是一种身份验证方案，它允许用户使用单个 ID 和密码登录到多个相关但独立的软件系统中的任何一个[。](https://en.wikipedia.org/wiki/Login)

#### ID Token协议

JWT Access Token

查询   https://docs.authing.cn/v2/concepts/id-token.html

JWT 全称为 [JSON Web Token (opens new window)](https://tools.ietf.org/html/rfc7519)，遵循 JWT 标准。JWT 中包含了**主体、受众、权限、颁发时间、过期时间、用户信息字段**等内容且具备**签名**，**不可篡改**

因此无需发送到服务器，可以本地验证。Authing 在大多数情况下使用此种格式的 Token。

Access Token 示例

```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjF6aXlIVG15M184MDRDOU1jUENHVERmYWJCNThBNENlZG9Wa3VweXdVeU0ifQ.eyJqdGkiOiIzWUJ5eWZ2TDB4b01QNXdwTXVsZ0wiLCJzdWIiOiI2MDE5NDI5NjgwMWRjN2JjMmExYjI3MzUiLCJpYXQiOjE2MTI0NDQ4NzEsImV4cCI6MTYxMzY1NDQ3MSwic2NvcGUiOiJvcGVuaWQgZW1haWwgbWVzc2FnZSIsImlzcyI6Imh0dHBzOi8vc3RlYW0tdGFsay5hdXRoaW5nLmNuL29pZGMiLCJhdWQiOiI2MDE5M2M2MTBmOTExN2U3Y2IwNDkxNTkifQ.cYyZ6buwAjp7DzrYQEhvz5rvUBhkv_s8xzuv2JHgzYx0jbqqsWrA_-gufLTFGmNkZkZwPnF6ktjvPHFT-1iJfWGRruOOMV9QKPhk0S5L2eedtbKJU6XIEkl3F9KbOFwYM53v3E7_VC8RBj5IKqEY0qd4mW36C9VbS695wZlvMYnmXhIopYsd5c83i39fLBF8vEBZE1Rq6tqTQTbHAasR2eUz1LnOqxNp2NNkV2dzlcNIksSDbEGjTNkWceeTWBRtFMi_o9EWaHExdm5574jQ-ei5zE4L7x-zfp9iAe8neuAgTsqXOa6RJswhyn53cW4DwWg_g26lHJZXQvv_RHZRlQ
```

解析后的内容：

```json
{
  "jti": "3YByyfvL0xoMP5wpMulgL",
  "sub": "60194296801dc7bc2a1b2735", // subject 的缩写，为用户 ID
  "iat": 1612444871,
  "exp": 1613654471,
  "scope": "openid email message",
  "iss": "https://steam-talk.authing.cn/oidc",
  "aud": "60193c610f9117e7cb049159"
}
```

**ID Token** 本质上是一个 [`JWT Token`](https://docs.authing.cn/v2/concepts/jwt-token.html)，包含了该用户身份信息相关的 key/value 键值对，

**ID Token** 本质上是一个 `JWT Token` 意味着：

- 用户的身份信息直接被编码进了 `id_token`，你不需要额外请求其他的资源来获取用户信息；

- `id_token` 可以验证其没有被篡改过，

  

#### CAS协议

CAS全称是中心认证服务(Central Authentication Service), 是SSO服务中一种认证协议, 目前有三个版本1.0, 2.0, 3.0

Central Authentication Service简称CAS，是一种常见的B/S架构的SSO协议。和其他任何SSO协议一样，用户仅需登陆一次，访问其他应用则无需再次登陆

上图是CAS官网上的标准流程，具体流程如下：（**用户第一次访问受保护的app系统流程**）

------

1. 用户输入**GET [https://app.example.com/](https://links.jianshu.com/go?to=https%3A%2F%2Fapp.example.com%2F)**想要访问app系统，由于app系统是需要验证才能范文的，但是app系统在收到该请求的时候并没有发现请求中带有JSESSIONID字段，因此app系统判断发送该请求的用户没有登录认证，最后app系统做出的操作是，重定向该请求，让用户去访问SSO系统，因此我们从图上看到**302 Location:[https://cas.example.com/cas/login?service=https%3A%2F](https://links.jianshu.com/go?to=https%3A%2F%2Fcas.example.com%2Fcas%2Flogin%3Fservice%3Dhttps%3A%2F)...**这里加上？service=https:%3A%2F... 就是为了方便用户在SSO认证登录后能够再定向到app系统。
2. 用户（或者这里叫Browser）在收到app系统的重定向后，去访问SSO，这个时候SSO发现该用户的请求中没有ticket信息，因此SSO判定该用户还没有在SSO中登录，因此SSO出示login页面，让用户去输入username、password信息。
3. 用户填写username、password信息，SSO系统对其进行认证后，会生成一个ST（Service Ticket）供url上传输 , 并将该用户信息写入SSO的session（session中的用户登录信息是被TGT封装起来的），而这个SSO系统的session的保存是key，value形式的，这个session的key叫做TGC，对应的value值叫做TGT。当重定向回用户时附带ST信息，并且把TGC的值存储到用户的Cookie中。
4. 接下来用户有了ST信息，就拿着这个ST+app系统链接地址（**GET [https://app.example.com/?ticket=ST-12345678](https://links.jianshu.com/go?to=https%3A%2F%2Fapp.example.com%2F%3Fticket%3DST-12345678)**）去访问app系统。
5. 当app系统收到用户的请求时发现，该request中包含ST信息，app系统会去拿着ST信息到SSO进行验证，如果验证通过，SSO返回给app系统success消息（图上是xml信息，还包含其它的可选参数字段信息）。
6. app系统在得到SSO的肯定回答后，将用户的登录信息存入app系统的session中，此时app系统并没有直接给用户展示要访问的信息，而是把request重定向回用户，只不过redirect 地址中夹带了app系统的session 信息，目的是为了让浏览器存储该session信息到Cookies中，方便后续的二次、三次访问情况。
7. 用户再次根据app系统的redirect 地址（*Cookie：JESESSIONID=ABC-1234567 GET [https://app.example.com](https://links.jianshu.com/go?to=https%3A%2F%2Fapp.example.com)*）去请求app系统。
8. app系统此时收到用户的request是包含JESESSIONID字段的，app系统会在自己的Cookie中验证，如果成功就返回给用户资源信息。

------

**用户再次访问app系统的流程：**

1. 用户请求app系统链接地址，带上JSESSIONID。
2. app系统收到带有JSESSIONID的request后，会在app系统的session中查找对应的value，查找成功后向用户发送相应的资源文件。

这个过程比较简单，和普通的请求访问一样，检查服务端是否有存在session值，如果有就直接允许访问相应的资源。

------

**用户第一次访问 与 app系统相互信任的系统流程：**

1. 用户直接请求app2系统地址链接。[https://app2.example.com/](https://links.jianshu.com/go?to=https%3A%2F%2Fapp2.example.com%2F)
2. app2系统收到request后发现这个request既没有ST也没有cookie，就会重定向到CAS服务器。
3. 用户的请求被重定向后，去访问CAS服务器，这个时候是带着cookie的。
4. CAS服务器收到用户带有cookie的request后，根据这个cookie（也就是TGC）去CAS的session中找TGT，如果查找成功，为该用户的请求颁发一个ST，并且更新TGC的值重写回浏览器端，当然CAS服务器端也是需要更新的哈。最后CAS把请求再重定向到app2系统。
5. 用户按照CAS的重定向，携带ST去访问app2系统，app2系统这次发现request带有ST，赶紧去找CAS验证，CAS验证通过后，给app2系统发送确认消息，最后app2系统才给用户返回要访问的资源信息。

------



![](images\cas.png)

参考 https://www.jianshu.com/p/a58c559bf0e1

因为HTTP协议是一个无状态协议，即Web应用程序无法区分收到的两个HTTP请求是否是同一个浏览器发出的。为了跟踪用户状态，服务器可以向浏览器分配一个唯一ID，并以Cookie的形式发送到浏览器，浏览器在后续访问时总是附带此Cookie，这样，服务器就可以识别用户身份

#### Saml 协议

在*SAML*协议中，一旦用户身份被主网站（身份鉴别服务器，Identity Provider，*IDP*）认证过后，该用户再去访问其他在主站注册过的应用（服务提供者，Service Providers，*SP*）时，都可以直接登录，而不用再输入身份和口令。

`Service Provider 服务提供方`，简称 SP。什么是服务提供方？例如：阿里云控制台、腾讯云控制台、AWS 控制台这些都是服务提供方。

`Identity Provider 身份提供方`，简称 IdP。身份提供者将用户身份验证作为服务提供。简而言之，您可以在身份提供程序中，添加和管理您的用户帐户、组，这使您可以在访问任何服务时对用户进行身份验证。



1.SP 可以返回带参数的重定向 HTTP 响应，

让用户立刻通过参数将信息发给 IdP。而 IdP 会返回一个表单，同时还有一段立即提交表单的 JS 代码，从而让用户立刻将信息发给 SP

2.两个主体通过用户的浏览器进行信息交换。方式上，SP 可以返回带参数的重定向 HTTP 响应，让用户立刻通过参数将信息发给 IdP。

3.而 IdP 会返回一个表单，同时还有一段立即提交表单的 JS 代码，从而让用户立刻将信息发给 SP。

> 总结一下，

SP 提供服务，需要知道用户的身份，就需要向 IdP 询问。

IdP 知道用户的身份，当用户在 IdP 登录成功，IdP 就将用户的身份以 SAML 断言的形式发给 SP。SP 信任 IdP 发来的身份断言，从而赋予该用户在 SP 的相关权限

**SAML 请求**

SAML 请求包含有关尝试使用 IDP 完成身份验证的服务提供商的信息。IDP 可能会使用此信息来验证请求是否来自正确的来源。

**SAML 断言**

SAML 断言由身份提供者在用户成功登录后生成。

断言包含有关经过身份验证的用户的信息，例如他的电子邮件地址、部门或任何其他声明。由于它是由身份提供者签名的，因此它充当声明资源的用户的证书。

服务提供者已经知道身份提供者，它可以验证应用在断言上的签名并接受断言中存在的信息。

#### Session

我们把这种基于唯一ID识别用户身份的机制称为Session。每个用户第一次访问服务器后，会自动获得一个Session ID。如果用户在一段时间内没有访问服务器，那么Session会自动失效，下次即使带着上次分配的Session ID访问，服务器也认为这是一个新用户，会分配新的Session ID。

JavaEE的Servlet机制内建了对Session的支持。我们以登录为例，当一个用户登录成功后，我们就可以把这个用户的名字放入一个`HttpSession`对象，以便后续访问其他页面的时候，能直接从`HttpSession`取出用户名：

对于Web应用程序来说，我们总是通过`HttpSession`这个高级接口访问当前Session。如果要深入理解Session原理，可以认为Web服务器在内存中自动维护了一个ID到`HttpSession`的映射表，我们可以用下图表示：

而服务器识别Session的关键就是依靠一个名为`JSESSIONID`的Cookie。在Servlet中第一次调用`req.getSession()`时，Servlet容器自动创建一个Session ID，然后通过一个名为`JSESSIONID`的Cookie发送给浏览器：

```
Servlet容器提供了Session机制以跟踪用户；

默认的Session机制是以Cookie形式实现的，Cookie名称为`JSESSIONID`；

通过读写Cookie可以在客户端设置用户偏好等。
```

#### 