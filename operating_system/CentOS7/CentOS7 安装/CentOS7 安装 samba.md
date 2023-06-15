# CentOS7 安装 samba

开机自启：[https://blog.csdn.net/weixin_28713299/article/details/116825515](https://blog.csdn.net/weixin_28713299/article/details/116825515)

## 1. 安装 samba

```
cat /etc/redhat-release  # 检查系统版本
rpm -qa | grep samba
yum install -y samba        # 安装samba
rpm -qi samba            # 检测是否安装好
```

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230144959715-37972264.png)

## 2. 关闭selinux和防火墙

```
setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
systemctl stop firewalld
systemctl disable firewalld
```

## 3.1. 直接添加samba用户

```
useradd willy       # 新建共享文件夹的用户willy
passwd willy        # 对用户添加密码
```

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230145321109-115962507.png)

## 3.2. 添加liuzonglin账户--》用户是系统已有用户

```
useradd -d /home/liuzonglin -s /sbin/nologin liuzonglin  # 添加用户
```

## 3.2.1. 将liuzonglin添加为samba用户

<details>
<summary>点击查看代码</summary>

```
[root@samba-server ~]# pdbedit -a -u durant
new password:                         //设置durant使用的samba账号密码，比如123456
retype new password:                  //确认密码
Unix username:        durant
NT username:          
Account Flags:        [U          ]
User SID:             S-1-5-21-3966910846-3390734216-1763763463-1001
Primary Group SID:    S-1-5-21-3966910846-3390734216-1763763463-513
Full Name:            
Home Directory:       \\samba-server\durant
HomeDir Drive:        
Logon Script:         
Profile Path:         \\samba-server\durant\profile
Domain:               SAMBA-SERVER
Account desc:         
Workstations:         
Munged dial:          
Logon time:           0
Logoff time:          Wed, 06 Feb 2036 23:06:39 CST
Kickoff time:         Wed, 06 Feb 2036 23:06:39 CST
Password last set:    Thu, 13 Jun 2019 16:26:31 CST
Password can change:  Thu, 13 Jun 2019 16:26:31 CST
Password must change: never
Last bad password   : 0
Bad password count  : 0
Logon hours         : FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF

接着修改samba用户的家目录权限
[root@samba-server ~]# chown -Rf durant:durant /home/durant

```

</details>

```
mkdir /home/willy/myshare   # 在用户willy文件夹下新建“myshare”共享文件夹
chmod -R 777 /home/willy/myshare/         # 对文件夹设置权限

```

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230150608418-1899055183.png)
vim smb.conf

## 4. 配置samba配置文件

```
# 共享文件夹设置
# 共享文件描述
# 共享路径
# 是否可查看
# 是否可写入
[liuzonglin]    
        comment = Public Stuff    
        path = /usr/local/liuzonglin/   
        browseable = yes   
        writable = yes    


```

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211230151120459-1460669807.png)

## 5. 启动

```
testparm -s /etc/samba/smb.conf
systemctl status smb
```

## 6. windows 连接用户

Win + R》\+ip
![image](https://img2020.cnblogs.com/blog/2402369/202201/2402369-20220103004937763-1944901332.png)
输入设置的samba账户密码，登录
![image](https://img2020.cnblogs.com/blog/2402369/202201/2402369-20220103005058773-2113177828.png)

<details>
<summary>点击查看代码</summary>

```
[root@desktop-v4v6dsu ~]# yum install samba -y
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.bupt.edu.cn
 * updates: mirrors.bupt.edu.cn
base                                                                                | 3.6 kB  00:00:00     
extras                                                                              | 2.9 kB  00:00:00     
updates                                                                             | 2.9 kB  00:00:00     
正在解决依赖关系
--> 正在检查事务
---> 软件包 samba.x86_64.0.4.10.16-17.el7_9 将被 安装
--> 正在处理依赖关系 samba-libs = 4.10.16-17.el7_9，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 samba-common-tools = 4.10.16-17.el7_9，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 samba-common-libs = 4.10.16-17.el7_9，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 samba-common = 4.10.16-17.el7_9，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 samba-common = 4.10.16-17.el7_9，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 samba-client-libs = 4.10.16-17.el7_9，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 libwbclient = 4.10.16-17.el7_9，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 libwbclient = 4.10.16-17.el7_9，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 libxattr-tdb-samba4.so(SAMBA_4.10.16)(64bit)，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 libxattr-tdb-samba4.so()(64bit)，它被软件包 samba-4.10.16-17.el7_9.x86_64 需要
--> 正在检查事务
---> 软件包 libwbclient.x86_64.0.4.10.16-5.el7 将被 升级
--> 正在处理依赖关系 libwbclient = 4.10.16-5.el7，它被软件包 libsmbclient-4.10.16-5.el7.x86_64 需要
--> 正在处理依赖关系 libwbclient = 4.10.16-5.el7，它被软件包 samba-client-4.10.16-5.el7.x86_64 需要
---> 软件包 libwbclient.x86_64.0.4.10.16-17.el7_9 将被 更新
---> 软件包 samba-client-libs.x86_64.0.4.10.16-5.el7 将被 升级
---> 软件包 samba-client-libs.x86_64.0.4.10.16-17.el7_9 将被 更新
---> 软件包 samba-common.noarch.0.4.10.16-5.el7 将被 升级
---> 软件包 samba-common.noarch.0.4.10.16-17.el7_9 将被 更新
---> 软件包 samba-common-libs.x86_64.0.4.10.16-5.el7 将被 升级
---> 软件包 samba-common-libs.x86_64.0.4.10.16-17.el7_9 将被 更新
---> 软件包 samba-common-tools.x86_64.0.4.10.16-17.el7_9 将被 安装
---> 软件包 samba-libs.x86_64.0.4.10.16-17.el7_9 将被 安装
--> 正在处理依赖关系 libpytalloc-util.so.2(PYTALLOC_UTIL_2.1.9)(64bit)，它被软件包 samba-libs-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 libpytalloc-util.so.2(PYTALLOC_UTIL_2.1.6)(64bit)，它被软件包 samba-libs-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 libpytalloc-util.so.2(PYTALLOC_UTIL_2.0.6)(64bit)，它被软件包 samba-libs-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 libpytalloc-util.so.2()(64bit)，它被软件包 samba-libs-4.10.16-17.el7_9.x86_64 需要
--> 正在处理依赖关系 libpyldb-util.so.1()(64bit)，它被软件包 samba-libs-4.10.16-17.el7_9.x86_64 需要
--> 正在检查事务
---> 软件包 libsmbclient.x86_64.0.4.10.16-5.el7 将被 升级
---> 软件包 libsmbclient.x86_64.0.4.10.16-17.el7_9 将被 更新
---> 软件包 pyldb.x86_64.0.1.5.4-2.el7 将被 安装
--> 正在处理依赖关系 libldb(x86-64) = 1.5.4-2.el7，它被软件包 pyldb-1.5.4-2.el7.x86_64 需要
--> 正在处理依赖关系 python-tdb(x86-64) >= 1.3.18，它被软件包 pyldb-1.5.4-2.el7.x86_64 需要
---> 软件包 pytalloc.x86_64.0.2.1.16-1.el7 将被 安装
---> 软件包 samba-client.x86_64.0.4.10.16-5.el7 将被 升级
---> 软件包 samba-client.x86_64.0.4.10.16-17.el7_9 将被 更新
--> 正在检查事务
---> 软件包 libldb.x86_64.0.1.5.4-1.el7 将被 升级
---> 软件包 libldb.x86_64.0.1.5.4-2.el7 将被 更新
---> 软件包 python-tdb.x86_64.0.1.3.18-1.el7 将被 安装
--> 解决依赖关系完成

依赖关系解决

===========================================================================================================
 Package                        架构               版本                          源                   大小
===========================================================================================================
正在安装:
 samba                          x86_64             4.10.16-17.el7_9              updates             719 k
为依赖而安装:
 pyldb                          x86_64             1.5.4-2.el7                   updates              49 k
 pytalloc                       x86_64             2.1.16-1.el7                  base                 18 k
 python-tdb                     x86_64             1.3.18-1.el7                  base                 20 k
 samba-common-tools             x86_64             4.10.16-17.el7_9              updates             466 k
 samba-libs                     x86_64             4.10.16-17.el7_9              updates             271 k
为依赖而更新:
 libldb                         x86_64             1.5.4-2.el7                   updates             149 k
 libsmbclient                   x86_64             4.10.16-17.el7_9              updates             146 k
 libwbclient                    x86_64             4.10.16-17.el7_9              updates             116 k
 samba-client                   x86_64             4.10.16-17.el7_9              updates             646 k
 samba-client-libs              x86_64             4.10.16-17.el7_9              updates             5.0 M
 samba-common                   noarch             4.10.16-17.el7_9              updates             216 k
 samba-common-libs              x86_64             4.10.16-17.el7_9              updates             182 k

事务概要
===========================================================================================================
安装  1 软件包 (+5 依赖软件包)
升级           ( 7 依赖软件包)

总下载量：7.9 M
Downloading packages:
No Presto metadata available for updates
警告：/var/cache/yum/x86_64/7/updates/packages/pyldb-1.5.4-2.el7.x86_64.rpm: 头V3 RSA/SHA256 Signature, 密 钥 ID f4a80eb5: NOKEY
pyldb-1.5.4-2.el7.x86_64.rpm 的公钥尚未安装
(1/13): pyldb-1.5.4-2.el7.x86_64.rpm                                                |  49 kB  00:00:00     
pytalloc-2.1.16-1.el7.x86_64.rpm 的公钥尚未安装
(2/13): pytalloc-2.1.16-1.el7.x86_64.rpm                                            |  18 kB  00:00:00     
(3/13): libldb-1.5.4-2.el7.x86_64.rpm                                               | 149 kB  00:00:00     
(4/13): libwbclient-4.10.16-17.el7_9.x86_64.rpm                                     | 116 kB  00:00:00     
(5/13): samba-4.10.16-17.el7_9.x86_64.rpm                                           | 719 kB  00:00:00     
(6/13): samba-common-4.10.16-17.el7_9.noarch.rpm                                    | 216 kB  00:00:00     
(7/13): python-tdb-1.3.18-1.el7.x86_64.rpm                                          |  20 kB  00:00:00     
(8/13): samba-common-libs-4.10.16-17.el7_9.x86_64.rpm                               | 182 kB  00:00:00     
(9/13): samba-client-4.10.16-17.el7_9.x86_64.rpm                                    | 646 kB  00:00:00     
(10/13): samba-libs-4.10.16-17.el7_9.x86_64.rpm                                     | 271 kB  00:00:00     
(11/13): samba-common-tools-4.10.16-17.el7_9.x86_64.rpm                             | 466 kB  00:00:00     
(12/13): samba-client-libs-4.10.16-17.el7_9.x86_64.rpm                              | 5.0 MB  00:00:00     
(13/13): libsmbclient-4.10.16-17.el7_9.x86_64.rpm                                   | 146 kB  00:00:01     
-----------------------------------------------------------------------------------------------------------
总计                                                                       5.7 MB/s | 7.9 MB  00:00:01     
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
  正在更新    : libldb-1.5.4-2.el7.x86_64                                                             1/20 
  正在更新    : samba-common-4.10.16-17.el7_9.noarch                                                  2/20 
  正在更新    : samba-client-libs-4.10.16-17.el7_9.x86_64                                             3/20 
  正在更新    : libwbclient-4.10.16-17.el7_9.x86_64                                                   4/20 
  正在更新    : samba-common-libs-4.10.16-17.el7_9.x86_64                                             5/20 
  正在更新    : libsmbclient-4.10.16-17.el7_9.x86_64                                                  6/20 
  正在安装    : python-tdb-1.3.18-1.el7.x86_64                                                        7/20 
  正在安装    : pyldb-1.5.4-2.el7.x86_64                                                              8/20 
  正在安装    : pytalloc-2.1.16-1.el7.x86_64                                                          9/20 
  正在安装    : samba-libs-4.10.16-17.el7_9.x86_64                                                   10/20 
  正在安装    : samba-common-tools-4.10.16-17.el7_9.x86_64                                           11/20 
  正在安装    : samba-4.10.16-17.el7_9.x86_64                                                        12/20 
  正在更新    : samba-client-4.10.16-17.el7_9.x86_64                                                 13/20 
  清理        : samba-client-4.10.16-5.el7.x86_64                                                    14/20 
  清理        : libsmbclient-4.10.16-5.el7.x86_64                                                    15/20 
  清理        : samba-common-libs-4.10.16-5.el7.x86_64                                               16/20 
  清理        : samba-client-libs-4.10.16-5.el7.x86_64                                               17/20 
  清理        : libwbclient-4.10.16-5.el7.x86_64                                                     18/20 
  清理        : samba-common-4.10.16-5.el7.noarch                                                    19/20 
  清理        : libldb-1.5.4-1.el7.x86_64                                                            20/20 
  验证中      : libwbclient-4.10.16-17.el7_9.x86_64                                                   1/20 
  验证中      : samba-libs-4.10.16-17.el7_9.x86_64                                                    2/20 
  验证中      : samba-common-4.10.16-17.el7_9.noarch                                                  3/20 
  验证中      : samba-common-tools-4.10.16-17.el7_9.x86_64                                            4/20 
  验证中      : libldb-1.5.4-2.el7.x86_64                                                             5/20 
  验证中      : libsmbclient-4.10.16-17.el7_9.x86_64                                                  6/20 
  验证中      : samba-client-libs-4.10.16-17.el7_9.x86_64                                             7/20 
  验证中      : samba-4.10.16-17.el7_9.x86_64                                                         8/20 
  验证中      : pyldb-1.5.4-2.el7.x86_64                                                              9/20 
  验证中      : pytalloc-2.1.16-1.el7.x86_64                                                         10/20 
  验证中      : samba-client-4.10.16-17.el7_9.x86_64                                                 11/20 
  验证中      : samba-common-libs-4.10.16-17.el7_9.x86_64                                            12/20 
  验证中      : python-tdb-1.3.18-1.el7.x86_64                                                       13/20 
  验证中      : samba-client-libs-4.10.16-5.el7.x86_64                                               14/20 
  验证中      : libldb-1.5.4-1.el7.x86_64                                                            15/20 
  验证中      : samba-common-4.10.16-5.el7.noarch                                                    16/20 
  验证中      : libwbclient-4.10.16-5.el7.x86_64                                                     17/20 
  验证中      : samba-client-4.10.16-5.el7.x86_64                                                    18/20 
  验证中      : samba-common-libs-4.10.16-5.el7.x86_64                                               19/20 
  验证中      : libsmbclient-4.10.16-5.el7.x86_64                                                    20/20 

已安装:
  samba.x86_64 0:4.10.16-17.el7_9                                                                          

作为依赖被安装:
  pyldb.x86_64 0:1.5.4-2.el7                       pytalloc.x86_64 0:2.1.16-1.el7                          
  python-tdb.x86_64 0:1.3.18-1.el7                 samba-common-tools.x86_64 0:4.10.16-17.el7_9            
  samba-libs.x86_64 0:4.10.16-17.el7_9            

作为依赖被升级:
  libldb.x86_64 0:1.5.4-2.el7                            libsmbclient.x86_64 0:4.10.16-17.el7_9           
  libwbclient.x86_64 0:4.10.16-17.el7_9                  samba-client.x86_64 0:4.10.16-17.el7_9           
  samba-client-libs.x86_64 0:4.10.16-17.el7_9            samba-common.noarch 0:4.10.16-17.el7_9           
  samba-common-libs.x86_64 0:4.10.16-17.el7_9           

完毕！
[root@desktop-v4v6dsu ~]# systemctl restart smb
[root@desktop-v4v6dsu ~]# groupadd -g 888 admin
[root@desktop-v4v6dsu ~]# useradd  -d  /home/admin -g admin  admin
[root@desktop-v4v6dsu ~]# pdbedit -a admin
new password:
retype new password:
Unix username:        admin
NT username:          
Account Flags:        [U          ]
User SID:             S-1-5-21-4213895885-1385645773-3482405199-1000
Primary Group SID:    S-1-5-21-4213895885-1385645773-3482405199-513
Full Name:            
Home Directory:       \\desktop-v4v6dsu\admin
HomeDir Drive:        
Logon Script:         
Profile Path:         \\desktop-v4v6dsu\admin\profile
Domain:               DESKTOP-V4V6DSU
Account desc:         
Workstations:         
Munged dial:          
Logon time:           0
Logoff time:          三, 06 2月 2036 07:06:39 PST
Kickoff time:         三, 06 2月 2036 07:06:39 PST
Password last set:    三, 29 12月 2021 08:46:07 PST
Password can change:  三, 29 12月 2021 08:46:07 PST
Password must change: never
Last bad password   : 0
Bad password count  : 0
Logon hours         : FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
[root@desktop-v4v6dsu ~]# cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
[root@desktop-v4v6dsu ~]# vim /etc/samba/smb.conf
[root@desktop-v4v6dsu ~]# mkdir -p /home/share
[root@desktop-v4v6dsu ~]# chmod 777 /home/share
[root@desktop-v4v6dsu ~]# /usr/sbin/setsebool -P samba_export_all_rw on
[root@desktop-v4v6dsu ~]# systemctl restart smb

```

</details>

开机自启
`chkconfig --level 3 smb on`

## 参考博客

[https://blog.csdn.net/wc1695040842/article/details/91866500](https://blog.csdn.net/wc1695040842/article/details/91866500)
[https://www.jianshu.com/p/e78c01bbde28](https://www.jianshu.com/p/e78c01bbde28)
[https://jingyan.baidu.com/article/f3ad7d0f2ea14409c2345b69.html](https://jingyan.baidu.com/article/f3ad7d0f2ea14409c2345b69.html)
[https://blog.csdn.net/qianglei6077/article/details/86575224](https://blog.csdn.net/qianglei6077/article/details/86575224)
[https://blog.csdn.net/liuyunshengsir/article/details/89518042](https://blog.csdn.net/liuyunshengsir/article/details/89518042)
[https://blog.csdn.net/doadao/article/details/378900](https://blog.csdn.net/doadao/article/details/378900)
