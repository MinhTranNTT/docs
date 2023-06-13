# Docker 部署 MySQL

参考资料：

[使用docker-compose的方式部署mysql](https://zhuanlan.zhihu.com/p/384330120)

---

### 拉取镜像

```shell
docker pull mysql:latest
```

### 创建数据卷

```shell
mkdir -p /home/zonglin/mysql/data
mkdir -p /home/zonglin/mysql/initdb
mkdir -p /home/zonglin/mysql/log
```

### 运行容器设置开机自启

```shell
docker run --restart=always --log-opt max-size=100m --log-opt max-file=2 -p 6379:6379 --name lcloud-redis -v /home/zonglin/redis/redis.conf:/etc/redis/redis.conf -v /home/zonglin/redis/data:/data -d redis:latest redis-server /etc/redis/redis.conf --appendonly yes 
```

参数说明：

latest 最新版本

* -p 3306:3306 ：映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 宿主机ip:3306 访问到 MySQL 的服务。
* MYSQL_ROOT_PASSWORD=123456：设置 MySQL 服务 root 用户的密码。

‍

‍

‍

‍
