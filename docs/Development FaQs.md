开发问题：

go mod download

下载依赖

go mod tidy

同步依赖包，添加需要的，移除多余的

go mod vendor

将依赖包放入vendor

go build 的时候命令如下

-



```bash
C:\Users\SW>docker login harbor.supwisdom.com
Authenticating with existing credentials...
Login Succeeded
C:\Users\SW>docker image ls
REPOSITORY    TAG            IMAGE ID       CREATED        SIZE
student_app   latest         bebea7e10646   21 hours ago   418MB
student_app   v1.0.0         bebea7e10646   21 hours ago   418MB
mysql         5.7            c20987f18b13   4 weeks ago    448MB
redis         latest         40c68ed3a4d2   2 months ago   113MB
nginx         latest         ea335eea17ab   2 months ago   141MB
mongo         latest         fefd78e9381a   3 months ago   699MB
redis         6.2.4-alpine   500703a12fa4   6 months ago   32.3MB
mysql         5.7.26         e9c354083de7   2 years ago    373MB

C:\Users\SW>docker push harbor.supwisdom.com/weidongqi/student_app:v1.0.0
The push refers to repository [harbor.supwisdom.com/weidongqi/student_app]
An image does not exist locally with the tag: harbor.supwisdom.com/weidongqi/student_app

C:\Users\SW>docker push harbor.supwisdom.com/weidongqi/student_app:latest
The push refers to repository [harbor.supwisdom.com/weidongqi/student_app]
An image does not exist locally with the tag: harbor.supwisdom.com/weidongqi/student_app

#推送不成功修改如下
C:\Users\SW>docker tag student_app:v1.0.0 harbor.supwisdom.com/weidongqi/student_app

C:\Users\SW>docker image ls
REPOSITORY                                   TAG            IMAGE ID       CREATED        SIZE
student_app                                  latest         bebea7e10646   22 hours ago   418MB
student_app                                  v1.0.0         bebea7e10646   22 hours ago   418MB
harbor.supwisdom.com/weidongqi/student_app   latest         bebea7e10646   22 hours ago   418MB
mysql                                        5.7            c20987f18b13   4 weeks ago    448MB
redis                                        latest         40c68ed3a4d2   2 months ago   113MB
nginx                                        latest         ea335eea17ab   2 months ago   141MB
mongo                                        latest         fefd78e9381a   3 months ago   699MB
redis                                        6.2.4-alpine   500703a12fa4   6 months ago   32.3MB
mysql                                        5.7.26         e9c354083de7   2 years ago    373MB

C:\Users\SW>docker push harbor.supwisdom.com/weidongqi/student_app
Using default tag: latest
The push refers to repository [harbor.supwisdom.com/weidongqi/student_app]
81dc20d6c547: Pushed
e5edc7d1f2a8: Pushed
5f70bf18a086: Pushed
4cb9ba503b6b: Pushed
19c4d4cefc09: Pushed
46e96c819e17: Pushed
b6f786c730a9: Pushed
63a6bdb95b08: Pushed
8d3ac3489996: Pushed
latest: digest: sha256:77421aa1e46ef28a98c941a317c908e909d8f5f4aa22b2ee204f1c0cda8dd5fd size: 2202
```



grant all privileges on *.* to root@'%' identified by 'Aa123456' with grant option;

Harbor仓库地址

harbor.supwisdom.com





1. `docker tag student_app harbor.supwisdom.com/weidongqi/student_app`

1. `docker push harbor.supwisdom.com/weidongqi/student_app`

技术栈：JavaScript css html http mysql redis docker golang python

```text
# 设置时区为上海
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone
```

 `CMD`

> CMD 的主要目的是为执行容器提供默认值。

`ENTRYPOINT`：

> ENTRYPOINT 可帮助您配置可以作为可执行文件运行的容器。

```bash
"." -- 代表目前所在的目录，相对路径。 如：<a href="./abc">文本</a> 或 <img src="./abc" /> ".." -- 代表上一层目录，相对路径。 如：<a href="../abc">文本</a> 或 <img src="../abc" /> "../../" -- 代表的是上一层目录的上一层目录，相对路径。 如：<img src="../../abc" /> "/" -- 代表根目录,绝对路径。 如：<a href="/abc">文本</a> 或 <img src="/abc" /> "D:/abc/" -- 代表根目录,绝对路径。
```



/bin/sh: home/app/program: not found



docker build -t stu:test .



```
docker run -it --rm --name gin-instance -p 8080:8080 -v

docker run 用于从一个image上启动一个容器

-it 标签以交互的方式启动容器

--rm 标签在容器关闭后会会删除该容器，查看容器命令:sudo docker ps -a

-p 7070:7070标签允许荣过主机的7070端口访问docker容器中的7070端口

-v /home/youngblood/Go/src/ginDocker:/go/src/ginDocker表示将docker容器中
```

docker run -it --rm stu

