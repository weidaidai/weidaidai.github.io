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



