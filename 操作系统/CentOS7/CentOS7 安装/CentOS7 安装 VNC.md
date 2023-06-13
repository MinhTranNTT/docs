# CentOS7 安装 VNC

开机自启：[https://blog.csdn.net/muslim377287976/article/details/103820434](https://blog.csdn.net/muslim377287976/article/details/103820434)
centos7: [https://www.cnblogs.com/zgq123456/articles/15023213.html](https://www.cnblogs.com/zgq123456/articles/15023213.html)
VNC客户端下载地址：
[http://yczm.iis7.com/?tscc](http://yczm.iis7.com/?tscc)

1. 关闭防火墙

```
sudo systemctl stop firewalld
```

2. 安装vnc服务

```
yum install tigervnc-server tigervnc       # 安装相应桌面环境与vnc服务端和客户端   有桌面不执行
yum install -y tigervnc-server   # 下载vnc
```

<details>
<summary>点击查看代码</summary>

```
[root@localhost ~]# yum install -y tigervnc-server
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.bupt.edu.cn
 * updates: mirrors.bupt.edu.cn
正在解决依赖关系
--> 正在检查事务
---> 软件包 tigervnc-server.x86_64.0.1.8.0-22.el7 将被 安装
--> 解决依赖关系完成

依赖关系解决

==================================================================================================
 Package                    架构              版本                       源                  大小
==================================================================================================
正在安装:
 tigervnc-server            x86_64            1.8.0-22.el7               updates            211 k

事务概要
==================================================================================================
安装  1 软件包

总下载量：211 k
安装大小：498 k
Downloading packages:
警告：/var/cache/yum/x86_64/7/updates/packages/tigervnc-server-1.8.0-22.el7.x86_64.rpm: 头V3 RSA/SHA256 Signature, 密钥 ID f4a80eb5: NOKEY
tigervnc-server-1.8.0-22.el7.x86_64.rpm 的公钥尚未安装
tigervnc-server-1.8.0-22.el7.x86_64.rpm                                    | 211 kB  00:00:05     
从 file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 检索密钥
导入 GPG key 0xF4A80EB5:
 用户ID     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 指纹       : 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 软件包     : centos-release-7-9.2009.0.el7.centos.x86_64 (@anaconda)
 来自       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : tigervnc-server-1.8.0-22.el7.x86_64                                           1/1 
  验证中      : tigervnc-server-1.8.0-22.el7.x86_64                                           1/1 

已安装:
  tigervnc-server.x86_64 0:1.8.0-22.el7                                                           

完毕！
```

</details>

3. 开启1;并设置密码

```
vncserver :1
vncserver -list
```

<details>
<summary>点击查看代码</summary>

```
[root@localhost ~]# vnc
vncconfig          vncpasswd          vncserver          vncserver_wrapper  
[root@localhost ~]# vncserver :1

You will require a password to access your desktops.

Password:
Verify:
Would you like to enter a view-only password (y/n)? y
Password:
Verify:

New 'localhost.localdomain:1 (root)' desktop is localhost.localdomain:1

Creating default startup script /root/.vnc/xstartup
Creating default config /root/.vnc/config
Starting applications specified in /root/.vnc/xstartup
Log file is /root/.vnc/localhost.localdomain:1.log

[root@localhost ~]# 

```

</details>

设置远程登陆到gnome桌面的配置：（可以不设置）

```
vi /etc/sysconfig/vncservers

VNCSERVERS="2:root"

VNCSERVERARGS[2]="-geometry 800x600"
```

修改vnc密码

```
[root@localhost yum.repos.d]# vncpasswd
Password:
Verify:
```

VNC的启动和重启：

连接服务器
`运行 VNC Viewer`
![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211229164600756-359745447.png)
![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211229164624504-1628082107.png)

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211229164640277-541900296.png)
