# Docker 容器数据卷

## 1. 方式一：直接使用命令来挂载 -v

```bash
# docker run -it -v 主机目录：容器目录
[root@ home]# docker run -it -v /home/ceshi:/home centos /bin/bash
```

​![image](assets/image-20230221223450-r698x3e.png)​

总结：容器运行时，挂载目录数据一直同步

---

```bash
# 获取镜像
[root@ home]# docker pull mysql:5.7
# 运行容器， 需要做数据挂载！ # 安装启动mysql，需要配置密码（注意）
# 官方测试， docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
[root@ home]# docker run -d -p 3344:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
9552bf4eb2b69a2ccd344b5ba5965da4d97b19f2e1a78626ac1f2f8d276fc2ba

	# 参数说明：
	-d      # 后台运行
	-p      # 端口隐射
	-v      # 卷挂载
	-e      # 环境配置
	--name  # 容器的名字
# 启动成功之后，我们在本地使用navicat链接测试一下
# navicat链接到服务器的3344 --- 3344 和 容器的3306映射，这个时候我们就可以连接上mysql喽！
 
# 在本地测试创建一个数据库，查看下我们的路径是否ok！
```

## 2. 匿名和具名挂载

```bash
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx     # -P 随机指定端口
 
# 查看所有volume的情况
[root@ ~]# docker volume ls
DRIVER              VOLUME NAME
local               561b81a03506f31d45ada3f9fb7bd8d7c9b5e0f826c877221a17e45d4c80e096
local               36083fb6ca083005094cbd49572a0bffeec6daadfbc5ce772909bb00be760882
 
# 这里发现，这种情况就是匿名挂载，我们在-v 后面只写了容器内的路径，没有写容器外的路径！
 
# 具名挂载
[root@ ~]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
26da1ec7d4994c76e80134d24d82403a254a4e1d84ec65d5f286000105c3da17
[root@ ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
26da1ec7d499        nginx               "/docker-entrypoint.…"   3 seconds ago       Up 2 seconds        0.0.0.0:32769->80/tcp   nginx02
486de1da03cb        nginx               "/docker-entrypoint.…"   3 minutes ago       Up 3 minutes        0.0.0.0:32768->80/tcp   nginx01
[root@ ~]# docker volume ls
DRIVER              VOLUME NAME
local               561b81a03506f31d45ada3f9fb7bd8d7c9b5e0f826c877221a17e45d4c80e096
local               36083fb6ca083005094cbd49572a0bffeec6daadfbc5ce772909bb00be760882
local               juming-nginx
 
# 通过-v 卷名：容器内的路径
# 查看一下这个卷
# docker volume inspect juming-nginx

[root@ ~]# docker volume inspect juming-nginx
[
  {
      "CreatedAt": "2020-08-12T18:15:21+08:00",
      "Driver": "local",
      "Labels": null,
      "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
      "Name": "juming-nginx",
      "Options": null,
      "Scope": "local"
  }
]
```

```sh
# 通过 -v 容器内容路径 ro rw 改变读写权限
ro  readonly    # 只读
rw  readwrite   # 可读可写
 
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx
 
# ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内容无法操作
```

## 从容器内拷贝文件到主机上

```bash
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
redis        latest    7614ae9453d1   9 months ago    113MB
mysql        latest    3218b38490ce   9 months ago    516MB
centos       latest    5d0da3dc9764   12 months ago   231MB
[root@localhost ~]# docker run -it centos /bin/bash
[root@30dde7df25fb /]# cd /root/
[root@30dde7df25fb ~]# ls
anaconda-ks.cfg  anaconda-post.log  original-ks.cfg
[root@30dde7df25fb ~]# > Test.java
[root@30dde7df25fb ~]# ls
Test.java  anaconda-ks.cfg  anaconda-post.log  original-ks.cfg
[root@30dde7df25fb ~]# exit
exit
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
redis        latest    7614ae9453d1   9 months ago    113MB
mysql        latest    3218b38490ce   9 months ago    516MB
centos       latest    5d0da3dc9764   12 months ago   231MB
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@localhost ~]# docker ps -qa
30dde7df25fb
[root@localhost ~]# docker cp 30dde7df25fb:/root/Test.java /root/
[root@localhost ~]# ls
anaconda-ks.cfg  dump.rdb  Test.java
```

‍
