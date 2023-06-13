# Docker 部署 zookeeper

### 拉取ZooKeeper镜像最新版本

```json
docker pull zookeeper:latest
```

### 创建数据卷

```shell
mkdir -p ./zookeeper/data
mkdir -p ./zookeeper/conf
mkdir -p ./zookeeper/logs
```

### 运行容器

```json
docker run -d --name lcloud-zookeeper --privileged=true -p 2181:2181 --restart=always -v ./zookeeper/data:/data -v ./zookeeper/logs:/datalog zookeeper:latest
```

‍
