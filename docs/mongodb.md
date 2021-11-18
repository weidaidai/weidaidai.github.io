### 简介

介于sql和Nosql之间

将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组

面向文档存储的数据库

### mongodb 镜像容器

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

创建数据库

> use+创建数据库名，有使用，无自动创

```bash

use runoob
#switched to db runoob
#查看数据库show dbs
```

> 查看数据库show dbs

admin   0.000GB
config  0.000GB
local   0.000GB
runoob  0.000GB

> 插入数据 db.数据库名.insert({"key":"val"}) 

```bash
db.runoob.insert({"name":"小可爱"})
```

> 删除数据库

```bash
db.dropDatabase()
```

