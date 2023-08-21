# IDEA构建SpringBoot程序镜像

‍

## 1、构建 springboot 项目

这里我们创建一个新的SpringBoot项目，现在我们希望能够使用Docker快速地将我们的SpringBoot项目部署到安装了Docker的服务器上，我们就可以将其打包为一个Docker镜像。

## 2、打包 *.jar

好了，现在基本环境搭建好了，我们接着就需要将我们的SpringBoot项目打包然后再容器启动时运行了，打开Maven执行打包命令：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213001947-uqmag0f.png)​

‍

这里需要使用 ADD 命令来将文件拷贝到镜像中，`ADD`​`./target/docker1-0.0.1-SNAPSHOT.jar ./`​ 第一个参数是我们要拷贝的本地文件，第二个参数是存放在Docker镜像中的文件位置。

CMD 命令可以设定容器启动后执行的命令，EXPOSE 可以指定容器需要暴露的端口

## 3、((20230307200409-izf70s5 'Dockerfile 构建 镜像'))

‍

## 4、docker 开启远程客户端访问

配置Docker的服务器开启远程客户端访问：

`sudo vim /etc/systemd/system/multi-user.target.wants/docker.service`​

打开后，添加高亮部分：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213001614-eor6bmz.png)​​

```livecodeserver
[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0 --containerd=/run/containerd/containerd.sock
ExecReload=/bin/kill -s HUP $MAINPID
TimeoutSec=0
RestartSec=2
Restart=always
```

修改完成后，重启Docker服务，如果是云服务器，记得开启 2375 TCP连接端口：

```sh
sudo systemctl daemon-reload
sudo systemctl restart docker.service 
```

现在接着在IDEA中进行配置：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230212232935-iz74o0m.png)​

在引擎 API URL 处填写我们 Docker 服务器的IP地址：

```
tcp://IP:2375
```

```shell
[root@localhost local]# ss -lnp | grep 2375
tcp    LISTEN     0      128      :::2375                 :::*                   users:(("dockerd",pid=6690,fd=3))
```

显示连接成功后，表示配置正确，点击保存即可，接着就开始在我们的Docker服务器上进行构建了：

## 5、构建镜像、发布运行、测试

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230212235515-1pht2rt.png)​​

最后成功

可以看到，Docker服务器上已经有了我们刚刚构建好的镜像：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230212235637-zxsz3zf.png)​

可以看到历史中已经出现新的步骤了：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213000501-qfobut8.png)​​

接着启动我们的镜像，我们可以直接在IDEA中进行操作，不用再去敲命令了，有点累：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230212235938-lad0gad.png)​​

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213000010-j577mp3.png)​​

启动后可以在右侧看到容器启动的日志信息：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213000103-zrzyz96.png)​​

我们就可以成功访问到容器中运行的SpringBoot项目了：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213000220-rmrl18q.png)​​

当然，为了以后方便使用，我们可以直接将其推送到 Docker Hub 中，这里我们还是创建一个新的公开仓库：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213002623-524z1w5.png)​

这次我们就使用IDEA来演示直接进行镜像的上传，直接点击：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213002643-4piq67i.png)​​

接着我们需要配置一下我们的Docker Hub相关信息：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213002703-spcyb48.png)​​

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213002718-vqw38sm.png)​​

OK，远程镜像仓库配置完成，直接推送即可，等待推送完成。

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213002733-vzjjyo1.png)​​

可以看到远程仓库中已经出现了我们的镜像，然后IDEA中也可以同步看到：

![image](assets/IDEA%E6%9E%84%E5%BB%BASpringBoot%E7%A8%8B%E5%BA%8F%E9%95%9C%E5%83%8F/image-20230213002746-1u6gkly.png)​​

这样，我们就完成了使用 IDEA 将 SpringBoot 项目打包为 Docker 镜像。

‍
