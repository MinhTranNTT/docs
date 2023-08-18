# Docker 部署 Nginx

```shell
docker pull nginx:latest
```

Docker来部署一个Nginx服务器

```java
docker run --restart=always -dp 80:80 -v ./nginx/logs:/var/log/nginx -v /etc/localtime:/etc/localtime --name lcloud-nginx nginx:latest
```

可以看到，Nginx服务器已经成功部署了，但是实际上我们并没有在Ubuntu中安装Nginx，而是通过Docker运行的镜像来进行服务器搭建的，是不是感觉玩法挺新奇的。除了Nginx这种简单的应用之外，我们还可以通过Docker来部署复杂应用，之后我们都会一一进行讲解的。

‍访问：http://192.168.1.102:80

‍
