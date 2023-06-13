# CentOS7 镜像

**编写 Dockerfile**

```dockerfile
FROM centos
MAINTAINER xiaofan<11111111@qq.com>
 
ENV MYPATH /usr/local
WORKDIR $MYPATH     # 镜像的工作目录
 
RUN yum -y install vim
RUN yum -y install net-tools
 
EXPOSE 80
 
CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
```

**通过这个文件构建镜像**

```shell
docker build -f mydockerfile-centos -t mycentos:0.1 .
```

格式：`docker build -f dockerfile文件路径 -t 镜像名:[tag] .`​

‍
