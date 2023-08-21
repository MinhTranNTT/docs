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
mkdir -p ./mysql/data
mkdir -p ./mysql/initdb
mkdir -p ./mysql/log
```

### 运行容器设置开机自启

```shell
docker run \
--name mysql \
-d \
-p 3306:3306 \
-v ./mysql/log:/var/log/mysql \
-v ./mysql/data:/var/lib/mysql \
-v ./mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql:latest
```

参数说明：

latest 最新版本

* -p 3306:3306 ：映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 宿主机ip:3306 访问到 MySQL 的服务。
* MYSQL_ROOT_PASSWORD=123456：设置 MySQL 服务 root 用户的密码。
