Linux下vnc的安装、使用以及设置开机启动：https://blog.csdn.net/muslim377287976/article/details/103820434
Centos7安装VNCserver，并设置为开机自启动服务的方法：https://www.cnblogs.com/zgq123456/articles/15023213.html
VNC客户端下载地址：http://yczm.iis7.net/

1. 关闭防火墙

```sh
sudo systemctl stop firewalld
```

2. 安装vnc服务

```sh
yum install tigervnc-server tigervnc       # 安装相应桌面环境与vnc服务端和客户端   有桌面不执行
yum install -y tigervnc-server   # 下载vnc
```

3. 开启1;并设置密码

```sh
vncserver :1
vncserver -list
```


```sh
[root@root ~]# vnc
vncconfig          vncpasswd          vncserver          vncserver_wrapper  
[root@root ~]# vncserver :1

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
```

设置远程登陆到gnome桌面的配置：（可以不设置）

```sh
vim /etc/sysconfig/vncservers

VNCSERVERS="2:root"

VNCSERVERARGS[2]="-geometry 800x600"
```

修改vnc密码

```sh
[root@root ~]# vncpasswd
```

VNC的启动和重启：

连接服务器

`运行 VNC Viewer`

![image](assets/CentOS7%20%E5%AE%89%E8%A3%85%20VNC/2402369-20211229164600756-359745447.png)
![image](assets/CentOS7%20%E5%AE%89%E8%A3%85%20VNC/2402369-20211229164624504-1628082107.png)

![image](assets/CentOS7%20%E5%AE%89%E8%A3%85%20VNC/2402369-20211229164640277-541900296.png)
