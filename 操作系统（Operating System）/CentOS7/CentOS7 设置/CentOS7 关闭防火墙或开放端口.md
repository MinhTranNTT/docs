# CentOS7 关闭防火墙或开放端口

### 关闭防火墙

```shell
systemctl start firewalld.service
systemctl status firewalld.service
systemctl stop firewalld.service
# 设置开机启用防火墙
systemctl enable firewalld.service
# 设置开机禁用防火墙
systemctl disable firewalld.service
```

### 开放端口

查看开放的端口号

```shell
firewall-cmd --list-all
```

设置开放的端口号

```shell
firewall-cmd --add-service=http --permanent 
firewall-cmd --add-port=3306/tcp --permanent 
```

重启防火墙

```shell
firewall-cmd --reload
```

‍
