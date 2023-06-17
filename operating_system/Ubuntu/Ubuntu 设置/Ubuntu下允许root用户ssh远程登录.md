### Ubuntu下允许root用户ssh远程登录

如果你使用的是树莓派或是云服务器，那么你会得到一个公网的IP地址，以及默认的用户名和密码，由于服务器安装的Ubuntu并不是在我们的电脑上运行的，那么我们怎么去远程操作呢？

比如我们要远程操作一台Windows电脑，直接使用远程桌面连接即可，但是Ubuntu上来就是命令行，这种情况下要实现远程连接就只能使用SSH终端。

SSH是一种网络协议，用于计算机之间的加密登录。如果一个用户从本地计算机，使用SSH协议登录另一台远程计算机，我们就可以认为，这种登录是安全的，即使被中途截获，密码也不会泄露。最早的时候，互联网通信都是明文通信，一旦被截获，内容就暴露无疑。1995年，芬兰学者Tatu
Ylonen设计了SSH协议，将登录信息全部加密，成为互联网安全的一个基本解决方案，迅速在全世界获得推广，目前已经成为Linux系统的标准配置。

云服务器上安装的Ubuntu默认都是自带了OpenSSH服务端的，我们可以直接连接，如果你的Ubuntu服务器上没有安装OpenSSH服务器端，那么可以输入命令进行安装：

这里我们使用 WindTerm 来进行 SSH
登陆，官网：https://github.com/kingToolbox/WindTerm

查看 ip 信息

```shell
ifconfig
```

若提示找不到命令，则需安装 net-tools

```shell
sudo apt install net-tools
```

安装openssl

```shell
sudo apt-get install update
sudo apt-get install openssh-server # 输入后还需要你输入当前用户的密码才可以执行，至于为什么我们后面会说
```

启用root用户

```shell
sudo passwd root	# 修改密码后就启用了
```

vim /etc/ssh/sshd_config

```shell
PermitRootLogin yes                   #允许root用户以任何认证方式登录（貌似也就两种认证方式：用户名密码认证，公钥认证）
PermitRootLogin without-password      #只允许root用public key认证方式登录
PermitRootLogin no                    #不允许root用户以任何认证方式登录
```

启动 ssh 服务

```shell
sudo service ssh start  或者  sudo /etc/init.d/ssh start
```

‍

### 参考资料：

[ubuntu下允许root用户ssh远程登录 - 奋斗终生 - 博客园 (cnblogs.com)](https://www.cnblogs.com/ajianbeyourself/p/4220274.html#:~:text=SSH%E6%9C%8D%E5%8A%A1%E5%99%A8%EF%BC%8C%E5%8F%AF%E4%BB%A5%E9%80%9A%E8%BF%87SSH%E5%8D%8F%E8%AE%AE%E7%99%BB%E5%BD%95%E8%BF%9C%E7%A8%8B%E6%9C%8D%E5%8A%A1%E5%99%A8%EF%BC%8C%E4%BD%86%E6%98%AFubuntu%E9%BB%98%E8%AE%A4%E6%98%AF%E5%90%AF%E7%94%A8%E4%BA%86root%E7%94%A8%E6%88%B7%EF%BC%8C%E4%BD%86%E8%A6%81%E9%80%9A%E8%BF%87public%20key%E6%9D%A5%E7%99%BB%E5%BD%95%E3%80%82%20%E5%90%AF%E7%94%A8root%E7%94%A8%E6%88%B7%EF%BC%9Asudo,passwd%20root%20%23%E4%BF%AE%E6%94%B9%E5%AF%86%E7%A0%81%E5%90%8E%E5%B0%B1%E5%90%AF%E7%94%A8%E4%BA%86)

[Ubuntu中开启ssh允许root远程ssh登录的方法 - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1445519#:~:text=Ubuntu%E4%B8%AD%E5%BC%80%E5%90%AFssh%E5%85%81%E8%AE%B8root%E8%BF%9C%E7%A8%8Bssh%E7%99%BB%E5%BD%95%E7%9A%84%E6%96%B9%E6%B3%95%E3%80%82%20%E5%AE%89%E8%A3%85openssh-server%20%E8%AE%BE%E7%BD%AEroot%E7%94%A8%E6%88%B7%E5%AF%86%E7%A0%81%EF%BC%9A%20sudo%20passwd%20root%20%E7%BC%96%E8%BE%91%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%EF%BC%9A%20sudo,yes%20image.png%20%E9%87%8D%E5%90%AFssh%E6%9C%8D%E5%8A%A1%EF%BC%9A%20sudo%20systemctl%20restart%20sshd%20%E5%86%8D%E6%AC%A1%E8%BF%9B%E8%A1%8C%E8%BF%9C%E7%A8%8B%E7%99%BB%E5%BD%95%EF%BC%8C%E5%8D%B3%E5%8F%AF%E6%88%90%E5%8A%9F%EF%BC%9A)
