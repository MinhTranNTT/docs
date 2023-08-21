# docker-compose 部署 RabbitMQ

### docker-compose.yml

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

### 部署运行

```yaml
docker-compose up -d
```

### 开启图形化界面

```shell
docker exec -it rabbitmq /bin/bash
```

```shell
/opt/rabbitmq/sbin/rabbitmq-plugins enable rabbitmq_management	# 进入rabbitmq的可执行命令目录 图形化界面默认是关闭的，这里需要开启
```

### 测试

```shell
curl localhost:5672 --output run.log |cat run.log
```
