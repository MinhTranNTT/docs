# CentOS7 安装 docker-compose

docker-compose github 下载地址：[Releases · docker/compose (github.com)](https://github.com/docker/compose/releases)

### 安装

```shell
# 下载安装
sudo curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

# 文件授权
sudo chmod +x /usr/local/bin/docker-compose

# 查看版本
docker-compose -v
```

### 基础使用

```shell
docker-compose up //启动yml文件定义的container
docker-compose up -d //后台运行
docker-compose up --help //查看up帮助
docker-compose -f docker-compose.yml up //-f 指定yml文件
docker-compose stop //停止
docker-compose start 
docker-compose ls  //查看
docker-compose down //停止删除
docker-compose ps
docker-compose images
docker-compose exec {service_name} {bash}
```

`docker-compose.yml`

案例：

```yaml
version: '3.4'

services:
  zoo1:
    image: zookeeper
    restart: always
    hostname: zoo1
    container_name: zoo1
    ports:
    - 2184:2181
    volumes:
    - "/home/zookeeper/zoo1/data:/data"
    - "/home/zookeeper/zoo1/log:/datalog"
    environment:
      ZOO_MY_ID: 1
      ZOO_SERVERS: server.1=0.0.0.0:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888
    networks:
      nt17219:
        ipv4_address: 172.19.0.11

  zoo2:
    image: zookeeper
    restart: always
    hostname: zoo2
    container_name: zoo2
    ports:
    - 2185:2181
    volumes:
    - "/home/zookeeper/zoo2/data:/data"
    - "/home/zookeeper/zoo2/log:/datalog"
    environment:
      ZOO_MY_ID: 2
      ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=0.0.0.0:2888:3888 server.3=zoo3:2888:3888
    networks:
      nt17219:
        ipv4_address: 172.19.0.12

  zoo3:
    image: zookeeper
    restart: always
    hostname: zoo3
    container_name: zoo3
    ports:
    - 2186:2181
    volumes:
    - "/home/zookeeper/zoo3/data:/data"
    - "/home/zookeeper/zoo3/log:/datalog"
    environment:
      ZOO_MY_ID: 3
      ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=0.0.0.0:2888:3888
    networks:
      nt17219:
        ipv4_address: 172.19.0.13

networks:
  nt17219:
    external:
      name: nt17219
```
