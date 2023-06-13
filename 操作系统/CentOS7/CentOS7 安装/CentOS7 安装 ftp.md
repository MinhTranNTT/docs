# CentOS7 安装 ftp

[https://www.cnblogs.com/basa/p/11112590.html](https://www.cnblogs.com/basa/p/11112590.html)
开机自启：[https://www.cnblogs.com/chuanqi1995/p/11812524.html](https://www.cnblogs.com/chuanqi1995/p/11812524.html)

```
[root@desktop-v4v6dsu ~]# setenforce 0 
[root@desktop-v4v6dsu ~]# systemctl stop firewalld.service
[root@desktop-v4v6dsu ~]# yum install -y vsftpd
Loaded plugins: fastestmirror, langpacks
Determining fastest mirrors
 * base: mirrors.bfsu.edu.cn
 * extras: mirrors.bfsu.edu.cn
 * updates: mirrors.bfsu.edu.cn
base                                                                                                                    | 3.6 kB  00:00:00     
extras                                                                                                                  | 2.9 kB  00:00:00     
updates                                                                                                                 | 2.9 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package vsftpd.x86_64 0:3.0.2-29.el7_9 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================================================================
 Package                        Arch                           Version                                   Repository                       Size
===============================================================================================================================================
Installing:
 vsftpd                         x86_64                         3.0.2-29.el7_9                            updates                         173 k

Transaction Summary
===============================================================================================================================================
Install  1 Package

Total download size: 173 k
Installed size: 353 k
Downloading packages:
vsftpd-3.0.2-29.el7_9.x86_64.rpm                                                                                        | 173 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : vsftpd-3.0.2-29.el7_9.x86_64                                                                                                1/1 
  Verifying  : vsftpd-3.0.2-29.el7_9.x86_64                                                                                                1/1 

Installed:
  vsftpd.x86_64 0:3.0.2-29.el7_9                                                                                                               

Complete!
[root@desktop-v4v6dsu ~]# systemctl start vsftpd
[root@desktop-v4v6dsu ~]# find / -name "pub"
/var/ftp/pub
[root@desktop-v4v6dsu ~]# cd /var/ftp/pub/
[root@desktop-v4v6dsu pub]# ls
[root@desktop-v4v6dsu pub]# touch liuzonglin.txt
[root@desktop-v4v6dsu pub]# ls
liuzonglin.txt
[root@desktop-v4v6dsu pub]# vim liuzonglin.txt 
[root@desktop-v4v6dsu pub]# cd
[root@desktop-v4v6dsu ~]# useradd liuzonglin
useradd: user 'liuzonglin' already exists
[root@desktop-v4v6dsu ~]# passwd liuzonglin
Changing password for user liuzonglin.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@desktop-v4v6dsu ~]# systemctl start vsftpd
[root@desktop-v4v6dsu ~]# cd /var/
[root@desktop-v4v6dsu var]# ls
account  cache  db     ftp    gopher    lib    lock  mail  opt       run    target  yp
adm      crash  empty  games  kerberos  local  log   nis   preserve  spool  tmp
[root@desktop-v4v6dsu var]# mkdir /var/www
[root@desktop-v4v6dsu var]# mkdir /var/www/html
[root@desktop-v4v6dsu var]# chmod a-w /var/www && chmod 777 -R /var/www/html
[root@desktop-v4v6dsu var]# vim /etc/vsftpd/vsftpd.conf 
[root@desktop-v4v6dsu var]# vim /etc/vsftpd/vsftpd.conf 
[root@desktop-v4v6dsu var]# vim /etc/vsftpd/vsftpd.conf 
[root@desktop-v4v6dsu var]# vim /etc/vsftpd/vsftpd.conf 
[root@desktop-v4v6dsu var]# systemctl restart vsftpd
[root@desktop-v4v6dsu var]# 

```

</details>

‍
