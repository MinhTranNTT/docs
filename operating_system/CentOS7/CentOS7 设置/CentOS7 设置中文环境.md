# CentOS7 设置中文环境

1. 查看当前字符集

```
locale
```

2. 查看有没有zh_CN.utf8

```
locale -a |grep CN
```

3. 安装 langpacks-zh_CN

```
yum install -y langpacks-zh_CN
```

4. 配置

```
 > /etc/locale.conf; echo 'LANG="zh_CN.UTF-8"' >>/etc/locale.conf  
```

5. 重启系统

```
shutdown -r now
```

‍
