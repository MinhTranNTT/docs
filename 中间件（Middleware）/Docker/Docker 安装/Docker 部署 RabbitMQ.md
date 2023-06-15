# Docker 部署 RabbitMQ

[(35条消息) docker-compose安装RabbitMQ_rabbitmq dockerfile_嫣夜来的博客-CSDN博客](https://blog.csdn.net/qq_41865652/article/details/123263529)

[Docker系列之RabbitMQ安装部署教程 - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1612598)

---

### 拉取镜像

```shell
docker pull rabbitmq:management
```

### 运行容器

```shell
docker run --restart=always -d -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin --name lcloud-rabbitmq --hostname=rabbitmqhostone  rabbitmq:management
```

### 访问路径

[RabbitMQ Management](http://192.168.1.102:15672/#/)

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

```yaml
version: '3.5'
services:
  rabbitmq:
    restart: always
    image: rabbitmq:management
    container_name: rabbitmq
    hostname: rabbit
    ports:
      - 5672:5672
      - 15672:15672
    environment:
      TZ: Asia/Shanghai
      RABBITMQ_DEFAULT_USER: root
      RABBITMQ_DEFAULT_PASS: root
    volumes:
      - ./data:/var/lib/rabbitmq
      - ./conf:/etc/rabbitmq
```

```yaml
docker-compose up -d
```

‍

‍

```shell
curl localhost:5672 --output run.log |cat run.log

```

​![image](assets/image-20230225123022-vjdrg1b.png)​

```shell
docker exec -it rabbitmq /bin/bash
```

```shell
/opt/rabbitmq/sbin/rabbitmq-plugins enable rabbitmq_management	# 进入rabbitmq的可执行命令目录 图形化界面默认是关闭的，这里需要开启
```

‍
