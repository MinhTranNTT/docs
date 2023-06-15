CentOS 7 的几个常用的 yum 源



这些 yum 源都是稳定可靠的，您可以根据实际情况选择使用。使用方法是将对应的源文件复制到 `/etc/yum.repos.d/` 目录下，并进行相应的配置。比如以阿里云为例，

```sh
sudo wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
sudo yum makecache
```

其中，第一行命令会将阿里云的 CentOS 7 镜像源文件下载到 `/etc/yum.repos.d/` 目录下，第二行命令会重新生成缓存，以便使用新的镜像源。