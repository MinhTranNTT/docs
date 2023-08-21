# Nginx 安装

## 版本区别

### 常用版本分为四大阵营

* Nginx开源版：[nginx news](http://nginx.org/)

* Nginx plus 商业版：[Advanced Load Balancer, Web Server, &amp; Reverse Proxy - NGINX](https://www.nginx.com/)

* openresty：[OpenResty® - 开源官方站](http://openresty.org/cn/)

* Tengine：[简介 - The Tengine Web Server (taobao.org)](http://tengine.taobao.org/)

## 安装 Nginx

### 安装环境

```shell
yum install -y gcc
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
```

### 编译安装

```shell
./configure --prefix=/usr/local/nginx && make && make install
```

### 安装成系统服务

创建服务脚本

```shell
vi /usr/lib/systemd/system/nginx.service
```

服务脚本内容

```shell
[Unit]
Description=nginx - web server
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
ExecQuit=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true
[Install]
WantedBy=multi-user.target
```

重新加载系统服务 

```shell
systemctl daemon-reload
```

启动服务 

```shell
systemctl start nginx.service
```

开机启动 

```shell
systemctl enable nginx.service
```

## 启动 Nginx

‍

进入安装好的目录 `/usr/local/nginx/sbin`​

```shell
./nginx 启动
./nginx -s stop 快速停止
./nginx -s quit 优雅关闭，在退出前完成已经接受的连接请求
./nginx -s reload 重新加载配置
```

## 关于防火墙

关闭防火墙

```shell
systemctl stop firewalld.service
```

禁止防火墙开机启动 

```shell
systemctl disable firewalld.service
```

放行端口

```shell
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

‍

重启防火墙

```shell
firewall-cmd --reload
```

## 如果出现警告或报错

提示

```shell
checking for OS
+ Linux 3.10.0-693.el7.x86_64 x86_64
checking for C compiler ... not found
./configure: error: C compiler cc is not found
```

安装 gcc

提示

```shell
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
```

安装perl库

提示：

```shell
./configure: error: the HTTP gzip module requires the zlib library.
You can either disable the module by using --without-http_gzip_module
option, or install the zlib library into the system, or build the zlib library
statically from the source with nginx by using --with-zlib=<path> option.
```

安装zlib库

‍
