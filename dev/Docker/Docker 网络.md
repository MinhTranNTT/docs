# Docker 网络

参考文档

[(20条消息) docker 删除 网桥命令_莫回首�的博客-CSDN博客_docker 删除网络](https://blog.csdn.net/weixin_43841155/article/details/123821258)

---

## 一、自定义网络

‍

### 1.1、查看所有网桥

```sh
# docker network ls
```

### 1.2、查看网桥详细信息

```sh
# docker network inspect <NAME>
```

### 1.3、创建网桥

```sh
# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 <NAME>
```

### 1.4、运行容器使用自定义网桥

```sh
# docker run -d  -p 8081:8080 --name tomcat-net-01 --net <NAME> tomcat
```

### 1.5、容器连接指定网络

```sh
# docker network connect <NAME> <REPOSITORY>
```

‍

## 二、删除网桥

‍

### 2.1、Ubuntu系统 安装 bridge-utils

```sh
# apt install bridge-utils
```

### 2.2、查看网桥状态

```sh
# brctl show
```

### 2.3、关闭此网桥

```sh
# ifconfig <bridge name> down
```

### 2.4、删除网桥

```sh
# brctl delbr <bridge name>
```

### 2.5、删除 docker 网桥

```sh
# docker network rm <NETWORK ID>
```

‍
