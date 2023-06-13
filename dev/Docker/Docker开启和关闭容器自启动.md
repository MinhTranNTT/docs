# Docker开启和关闭容器自启动

[(36条消息) Docker开启和关闭容器自启动-CSDN博客](https://blog.csdn.net/m0_67265464/article/details/126580949)

---

1. 开启自启

在docker启动容器可以增加参数来达到，当docker 服务重启之后 自动启动容器，命令如下：

```shell
docker run –restart=always
```

当然如果你的容器已经启动,可以通过update命令进行修改,命令如下：

```shell
docker update –restart=always <CONTAINER ID>
```

2. 关闭自启

对某一个容器关闭自启动：

```shell
docker update --restart=no <CONTAINER ID>
```

取消所有自启动，命令如下：

```shell
docker update --restart=no $(docker ps -q)
```

3. docker-compose配置容器自启动

配置启动容器时添加下述配置项，docker-compose 关机或者重启docker时就会生效

‍
