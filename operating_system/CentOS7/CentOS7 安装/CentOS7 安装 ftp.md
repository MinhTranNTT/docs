

CentOS7.5上FTP服务的安装与使用：https://www.cnblogs.com/basa/p/11112590.html
Linux设置FTP开机自动启动：https://www.cnblogs.com/chuanqi1995/p/11812524.html



1. 检查是否安装了ftp

```
rpm -qa | grep vsftpd
```



2. 安装 vsftpd

```
yum -y install vsftpd
chkconfig vsftpd on     # 设置开机自启
```

> 执行流程

```
[root@localhost ~]# yum -y intall vsftpd
已加载插件：fastestmirror, langpacks
没有该命令：intall。请使用 /usr/bin/yum --help
[root@localhost ~]# yum -y install vsftpd
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.bupt.edu.cn
 * updates: mirrors.bupt.edu.cn
正在解决依赖关系
--> 正在检查事务
---> 软件包 vsftpd.x86_64.0.3.0.2-29.el7_9 将被 安装
--> 解决依赖关系完成

依赖关系解决

==================================================================================================
 Package             架构                版本                          源                    大小
==================================================================================================
正在安装:
 vsftpd              x86_64              3.0.2-29.el7_9                updates              173 k

事务概要
==================================================================================================
安装  1 软件包

总下载量：173 k
安装大小：353 k
Downloading packages:
警告：/var/cache/yum/x86_64/7/updates/packages/vsftpd-3.0.2-29.el7_9.x86_64.rpm: 头V3 RSA/SHA256 Signature, 密钥 ID f4a80eb5: NOKEY
vsftpd-3.0.2-29.el7_9.x86_64.rpm 的公钥尚未安装
vsftpd-3.0.2-29.el7_9.x86_64.rpm                                           | 173 kB  00:00:00     
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
  正在安装    : vsftpd-3.0.2-29.el7_9.x86_64                                                  1/1 
  验证中      : vsftpd-3.0.2-29.el7_9.x86_64                                                  1/1 

已安装:
  vsftpd.x86_64 0:3.0.2-29.el7_9                                                                  

完毕！
```



3. 修改vsftpd配置 `vim /etc/vsftpd/vsftpd.conf`
   ![image](assets/2402369-20211229173018533-949094984.png)

   

4. 重启 vsftpd服务

   ```sh
   sudo systemctl status vsftpd 或者 service vsftpd restart
   ```

   

5. 为ftp创建用户(问题！)

ftp用户名:testftp，密码testftp,并将用户绑定到 /var/ftp/testftp

```sh
useradd  -d /var/ftp/testftp testftp
```

设置密码:

```sh
passwd testftp
```

输入密码，Linux下输入密码不显示



6. 开启防火墙21端口

```sh
iptables -I INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
```

保存

```sh
service iptables save
```

重启：

```sh
service iptables restart
```



7. 在浏览器测试是否成功
   ![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211229174654377-2035879970.png)

   

8. 权限设置
   如果建新文件夹时出现 `550 Create directory operation failed.` （550报错）时，或者切换目录出错，应该是没有权限导致的！

```sh
vi /etc/selinux/config
```

打开配置将 `SELINUX` 的值设为 `disabled`

重启ftp服务



9. 指定`ftp`家目录
   修改`ftp`的根目录只要修改`/etc/vsftpd/vsftpd.conf`文件即可

加入下面三行

```conf
local_root=/var/www/html
chroot_local_user=YES
anon_root=/var/www/html
```

`local_root` 针对系统用户；

`anon_root` 针对匿名用户

```sh
chmod 755 /var/ftp/testftp
```

然后重启`ftp`服务就可以了

这时任何一个用户访问都会指定到  `/var/ftp/testftp`  下   即：ftp的根目录设置成了 /

FTP配置到此结束！！！

`service vsftpd start` 启动ftp命令

`service vsftpd stop` 停止ftp命令

`service vsftpd restart` 重启ftp命令

### CentOS7 xftp工具无法使用

```shell
vim /etc/ssh/sshd_config
Subsystem sftp internal-sftp
```

‍

## 报错

![image](assets/2402369-20220103143959330-617575353.png)
https://blog.csdn.net/china_skag/article/details/7278923

![image](assets/2402369-20220103144413977-2098771522.png)
https://blog.csdn.net/jiangshuanshuan/article/details/84846611

![image](assets/2402369-20220103145254740-1036715787.png)

