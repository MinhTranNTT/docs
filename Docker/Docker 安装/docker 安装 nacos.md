# docker 安装 nacos

## 拉取镜像

```shell
docker pull nacos/nacos-server
```

## 设置

```java
mkdir -p ./nacos/{init.d, logs}
touch ./nacos/init.d/custom.properties
```

## 运行容器

```shell
docker run -d -p 8848:8848 -e MODE=standalone -e PREFER_HOST_MODE=hostname -v ./nacos/init.d/custom.properties:/home/nacos/init.d/custom.properties -v ./nacos/logs:/home/nacos/logs --restart always --name nacos nacos/nacos-server
```

访问路径：http://192.168.1.102:15672/#/

‍
