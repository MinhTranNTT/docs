# Nginx扩容

通过扩容提升整体吞吐量

## 1.单机垂直扩容：硬件资源增加

```
云服务资源增加
整机：IBM、浪潮、DELL、HP等
CPU/主板：更新到主流
网卡：10G/40G网卡
磁盘：SAS(SCSI) HDD（机械）、HHD（混合）、SATA SSD、PCI-e SSD、 MVMe SSD
SSD
多副本机制
系统盘/热点数据/数据库存储
HDD
冷数据存储

```

## 2.水平扩展：集群化

### 会话管理

#### Nginx高级负载均衡

**ip_hash**

**hash    $cookie_jsessionid;**

**hash    $request_uri;**

**使用lua逻辑定向分发**

**Redis + SpringSession**

```nginx
   upstream httpds {
   ip_hash;
   server 192.168.44.102 ;
   server 192.168.44.103 ;
   }


    server {
        listen       80;
        server_name  localhost;

        location / {
	    proxy_pass http://httpds;

      	# root   html;
        }
    
    
    
       location ~*/(css|img|js) {
     
        root   /usr/local/nginx/html;

    }
```

#### 使用sticky模块完成对Nginx的负载均衡

**使用参考**

[http://nginx.org/en/docs/http/ngx_http_upstream_module.html#sticky](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#sticky)

tengine中有session_sticky模块我们通过第三方的方式安装在开源版本中

sticky是第三方模块，需要重新编译Nginx,他可以对Nginx这种静态文件服务器使用基于cookie的负载均衡

##### 1.下载模块

**项目官网**

[https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/src/master/](https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/src/master/)

另外一个版本

[https://github.com/bymaximus/nginx-sticky-module-ng](https://github.com/bymaximus/nginx-sticky-module-ng)

**下载**

[https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/get/1.2.6.zip](https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/get/1.2.6.zip)

##### 2.上传解压

##### 3.重新编译Nginx

依赖`openssl-devel`

**进到源码目录重新编译**

```shell
./configure --prefix=/usr/local/nginx --add-module=/root/nginx-goodies-nginx-sticky-module-ng-c78b7dd79d0d
```

**执行make**

**如遇报错修改源码**

打开 `ngx_http_sticky_misc.c`文件

在12行添加

```c
#include <openssl/sha.h>
#include <openssl/md5.h>
```

**备份之前的程序**

```
mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.old
```

**把编译好的Nginx程序替换到原来的目录里**

```shell
cp objs/nginx /usr/local/nginx/sbin/
```

**升级检测**

```
make upgrade
```

检查程序中是否包含新模块

```
nginx -V
```

配置方法

```
upstream httpget {

sticky name=route expires=6h;

server 192.168.44.102;
server 192.168.44.103;
}
```

#### 

### KeepAlive

在http协议header中可以看到当前连接状态

#### 测试工具charles

**下载地址**

[https://www.charlesproxy.com/assets/release/4.6.2/charles-proxy-4.6.2-win64.msi?k=fc1457e312](https://www.charlesproxy.com/assets/release/4.6.2/charles-proxy-4.6.2-win64.msi?k=fc1457e312)

**官网**

[https://www.charlesproxy.com](https://www.charlesproxy.com)

#### 什么时候使用？

明显的预知用户会在当前连接上有下一步操作

复用连接，有效减少握手次数，尤其是https建立一次连接开销会更大

#### 什么时候不用？

访问内联资源一般用缓存，不需要keepalive

长时间的tcp连接容易导致系统资源无效占用

#### 对客户端使用keepalive

**keepalive_time**

限制keepalive保持连接的最大时间

1.19.10新功能

**keepalive_timeout**

用于设置Nginx服务器与客户端保持连接的超时时间

用于踢出不活动连接

keepalive_timeout = 0 即关闭

- send_timeout 10;  10秒
- send_timeout 10 10; 同时下发一个header 告诉浏览器

**send_timeout**

两次向客户端写操作之间的间隔 如果大于这个时间则关闭连接 默认60s

**此处有坑**，注意耗时的同步操作有可能会丢弃用户连接

该设置表示Nginx服务器与客户端连接后，某次会话中服务器等待客户端响应超过10s，就会自动关闭连接。

**keepalive_request**

默认1000

单个连接中可处理的请求数

**keepalive_disable**

不对某些浏览器建立长连接

默认msie6

```nginx
http {
    include       mime.types;
    default_type  application/octet-stream;


    sendfile        on;


    keepalive_timeout  65 65; #超过这个时间 没有活动，会让keepalive失效 
    keepalive_time 1h; # 一个tcp连接总时长，超过之后 强制失效
  
    send_timeout 60;# 默认60s  此处有坑！！ 系统中 若有耗时操作，超过 send_timeout 强制断开连接。 注意：准备过程中，不是传输过程


    keepalive_requests 1000;  #一个tcp复用中 可以并发接收的请求个数
```

### 对上游服务器使用keepalive

首先需要配置使用http1.1协议。以便建立更高效的传输，默认使用http1.0，在http1.0中需要配置header才能

在Upstream中所配置的上游服务器默认都是用短连接，即每次请求都会在完成之后断开

#### 相关配置

##### upstream中配置

配置

**keepalive 100;**

向上游服务器的保留连接数

 keepalive_timeout   

连接保留时间

 keepalive_requests  

一个tcp复用中 可以并发接收的请求个数

##### server中配置

proxy_http_version 1.1;
配置http版本号
默认使用http1.0协议，需要在request中增加”Connection： keep-alive“ header才能够支持，而HTTP1.1默认支持。
proxy_set_header Connection "";
清楚close信息

### AB安装

yum install httpd-tools

参数说明：

|参数说明|参数|
| -----------------------------------------------------------------------------------------------------------------------| ------|
|添加一个基本的网络认证信息，用户名和密码之间用英文冒号隔开。|-A|
|添加cookie信息，例如："Apache=1234"(可以重复该参数选项以添加多个)。|-C|
|添加任意的请求头，例如："Accept-Encoding: gzip"，请求头将会添加在现有的多个请求头之后(可以重复该参数选项以添加多个)。|-H|
|添加一个基本的代理认证信息，用户名和密码之间用英文冒号隔开。|-P|
|指定使用的和端口号，例如:"126.10.10.3:88"。|-X|
|不显示预估和警告信息。|-S|
|即requests，用于指定压力测试总共的执行次数。|-n|
|即concurrency，用于指定的并发数。|-c|
|即timelimit，等待响应的最大时间(单位：秒)。|-t|
|即windowsize，TCP发送/接收的缓冲大小(单位：字节)。|-b|
|即postfile，发送POST请求时需要上传的文件，此外还必须设置-T参数。|-p|
|即putfile，发送PUT请求时需要上传的文件，此外还必须设置-T参数。|-u|
|即content-type，用于设置Content-Type请求头信息，例如：application/x-www-form-urlencoded，默认值为text/plain。|-T|
|即verbosity，指定打印帮助信息的冗余级别。|-v|
|以HTML表格形式打印结果。|-w|
|使用HEAD请求代替GET请求。|-i|
|插入字符串作为table标签的属性。|-x|
|插入字符串作为tr标签的属性。|-y|
|插入字符串作为td标签的属性。|-z|
|打印版本号并退出。|-V|
|使用HTTP的KeepAlive特性。|-k|
|不显示百分比。|-d|
|输出结果信息到gnuplot格式的文件中。|-g|
|输出结果信息到CSV格式的文件中。|-e|
|指定接收到错误信息时不退出程序。|-r|
|显示用法信息，其实就是ab -help。|-h|

‍

#### 直连nginx

```
Server Software:        nginx/1.21.6
Server Hostname:        192.168.44.102
Server Port:            80

Document Path:          /
Document Length:        16 bytes

Concurrency Level:      30
Time taken for tests:   13.035 seconds
Complete requests:      100000
Failed requests:        0
Write errors:           0
Total transferred:      25700000 bytes
HTML transferred:       1600000 bytes
Requests per second:    7671.48 [#/sec] (mean)
Time per request:       3.911 [ms] (mean)
Time per request:       0.130 [ms] (mean, across all concurrent requests)
Transfer rate:          1925.36 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.4      0      12
Processing:     1    3   1.0      3      14
Waiting:        0    3   0.9      3      14
Total:          2    4   0.9      4      14

Percentage of the requests served within a certain time (ms)
  50%      4
  66%      4
  75%      4
  80%      4
  90%      5
  95%      5
  98%      6
  99%      7
 100%     14 (longest request)

```

#### 反向代理

```
Server Software:        nginx/1.21.6
Server Hostname:        192.168.44.101
Server Port:            80

Document Path:          /
Document Length:        16 bytes

Concurrency Level:      30
Time taken for tests:   25.968 seconds
Complete requests:      100000
Failed requests:        0
Write errors:           0
Total transferred:      25700000 bytes
HTML transferred:       1600000 bytes
Requests per second:    3850.85 [#/sec] (mean)
Time per request:       7.790 [ms] (mean)
Time per request:       0.260 [ms] (mean, across all concurrent requests)
Transfer rate:          966.47 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.2      0      13
Processing:     3    8   1.4      7      22
Waiting:        1    7   1.4      7      22
Total:          3    8   1.4      7      22

Percentage of the requests served within a certain time (ms)
  50%      7
  66%      8
  75%      8
  80%      8
  90%      9
  95%     10
  98%     12
  99%     13
 100%     22 (longest request)

```

#### 直连Tomcat

```
Server Software:        
Server Hostname:        192.168.44.105
Server Port:            8080

Document Path:          /
Document Length:        7834 bytes

Concurrency Level:      30
Time taken for tests:   31.033 seconds
Complete requests:      100000
Failed requests:        0
Write errors:           0
Total transferred:      804300000 bytes
HTML transferred:       783400000 bytes
Requests per second:    3222.38 [#/sec] (mean)
Time per request:       9.310 [ms] (mean)
Time per request:       0.310 [ms] (mean, across all concurrent requests)
Transfer rate:          25310.16 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.3      0      15
Processing:     0    9   7.8      7     209
Waiting:        0    9   7.2      7     209
Total:          0    9   7.8      7     209

Percentage of the requests served within a certain time (ms)
  50%      7
  66%      9
  75%     11
  80%     13
  90%     18
  95%     22
  98%     27
  99%     36
 100%    209 (longest request)

```

#### nginx反向代理Tomcat + keepalive

```

Server Software:        nginx/1.21.6
Server Hostname:        192.168.44.101
Server Port:            80

Document Path:          /
Document Length:        7834 bytes

Concurrency Level:      30
Time taken for tests:   23.379 seconds
Complete requests:      100000
Failed requests:        0
Write errors:           0
Total transferred:      806500000 bytes
HTML transferred:       783400000 bytes
Requests per second:    4277.41 [#/sec] (mean)
Time per request:       7.014 [ms] (mean)
Time per request:       0.234 [ms] (mean, across all concurrent requests)
Transfer rate:          33688.77 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.2      0       9
Processing:     1    7   4.2      6     143
Waiting:        1    7   4.2      6     143
Total:          1    7   4.2      6     143

Percentage of the requests served within a certain time (ms)
  50%      6
  66%      7
  75%      7
  80%      7
  90%      8
  95%     10
  98%     13
  99%     16
 100%    143 (longest request)

```

#### nginx反向代理Tomcat

```
Server Software:        nginx/1.21.6
Server Hostname:        192.168.44.101
Server Port:            80

Document Path:          /
Document Length:        7834 bytes

Concurrency Level:      30
Time taken for tests:   33.814 seconds
Complete requests:      100000
Failed requests:        0
Write errors:           0
Total transferred:      806500000 bytes
HTML transferred:       783400000 bytes
Requests per second:    2957.32 [#/sec] (mean)
Time per request:       10.144 [ms] (mean)
Time per request:       0.338 [ms] (mean, across all concurrent requests)
Transfer rate:          23291.74 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.2      0       9
Processing:     1   10   5.5      9     229
Waiting:        1   10   5.5      9     229
Total:          1   10   5.5      9     229

Percentage of the requests served within a certain time (ms)
  50%      9
  66%     10
  75%     11
  80%     11
  90%     13
  95%     14
  98%     17
  99%     19
 100%    229 (longest request)

```

### UpStream工作流程

proxy_pass 向上游服务器请求数据共有6个阶段

- 初始化
- 与上游服务器建立连接
- 向上游服务器发送请求
- 处理响应头
- 处理响应体
- 结束

**set_header**

设置header

**proxy_connect_timeout**

与上游服务器连接超时时间、快速失败

**proxy_send_timeout**

定义nginx向后端服务发送请求的间隔时间(不是耗时)。默认60秒，超过这个时间会关闭连接

**proxy_read_timeout**

后端服务给nginx响应的时间，规定时间内后端服务没有给nginx响应，连接会被关闭，nginx返回504 Gateway Time-out。默认60秒

##### 缓冲区

**proxy_requset_buffering**

是否完全读到请求体之后再向上游服务器发送请求

**proxy_buffering**

是否缓冲上游服务器数据

**proxy_buffers 32 64k;**

缓冲区大小 32个 64k大小内存缓冲块

**proxy_buffer_size**

header缓冲区大小

```
proxy_requset_buffering on;
proxy_buffering on;

proxy_buffer_size 64k;

proxy_buffers 32 128k;
proxy_busy_buffers_size 8k;
proxy_max_temp_file_size 1024m;
```

**proxy_temp_file_write_size 8k**

当启用从代理服务器到临时文件的响应的缓冲时，一次限制写入临时文件的数据的大小。 默认情况下，大小由proxy_buffer_size和proxy_buffers指令设置的两个缓冲区限制。 临时文件的最大大小由proxy_max_temp_file_size指令设置。

**proxy_max_temp_file_size 1024m;**

临时文件最大值

**proxy_temp_path**

> ```
> proxy_temp_path /spool/nginx/proxy_temp 1 2;
> ```

a temporary file might look like this:

> ```
> /spool/nginx/proxy_temp/7/45/00000123457
> ```

#### 对客户端的限制

可配置位置

- http
- server
- location

**client_body_buffer_size**

对客户端请求中的body缓冲区大小

默认32位8k 64位16k

如果请求体大于配置，则写入临时文件

**client_header_buffer_size**

设置读取客户端请求体的缓冲区大小。 如果请求体大于缓冲区，则将整个请求体或仅将其部分写入临时文件。 默认32位8K。 64位平台16K。

**client_max_body_size 1000M;**

默认1m，如果一个请求的大小超过配置的值，会返回413 (request Entity Too Large)错误给客户端

将size设置为0将禁用对客户端请求正文大小的检查。

**client_body_timeout**

指定客户端与服务端建立连接后发送 request body 的超时时间。如果客户端在指定时间内没有发送任何内容，Nginx 返回 HTTP 408（Request Timed Out）

**client_header_timeout**

客户端向服务端发送一个完整的 request header 的超时时间。如果客户端在指定时间内没有发送一个完整的 request header，Nginx 返回 HTTP 408（Request Timed Out）。

**client_body_temp_path** *path*​`​ [`​*level1*​`​ [`​*level2*​`​ [`​*level3*`]]]

在磁盘上客户端的body临时缓冲区位置

**client_body_in_file_only on;**

把body写入磁盘文件，请求结束也不会删除

**client_body_in_single_buffer**

尽量缓冲body的时候在内存中使用连续单一缓冲区，在二次开发时使用`$request_body`读取数据时性能会有所提高

**client_header_buffer_size**

设置读取客户端请求头的缓冲区大小

如果一个请求行或者一个请求头字段不能放入这个缓冲区，那么就会使用large_client_header_buffers

**large_client_header_buffers**

默认8k

### 反向代理中的容错机制

#### 参考文档

[https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)

[http://nginx.org/en/docs/stream/ngx_stream_proxy_module.html#proxy_bind](http://nginx.org/en/docs/stream/ngx_stream_proxy_module.html#proxy_bind)

**proxy_timeout**

#### 重试机制

**proxy_next_upstream**

作用：

当后端服务器返回指定的错误时，将请求传递到其他服务器。

`error`与服务器建立连接，向其传递请求或读取响应头时发生错误;

`timeout`在与服务器建立连接，向其传递请求或读取响应头时发生超时;

`invalid_header`服务器返回空的或无效的响应;

`http_500`服务器返回代码为500的响应;

`http_502`服务器返回代码为502的响应;

`http_503`服务器返回代码为503的响应;

`http_504`服务器返回代码504的响应;

`http_403`服务器返回代码为403的响应;

`http_404`服务器返回代码为404的响应;

`http_429`服务器返回代码为429的响应;

不了解这个机制，在日常开发web服务的时候，就可能会踩坑。

比如有这么一个场景：一个用于导入数据的web页面，上传一个excel，通过读取、处理excel，向数据库中插入数据，处理时间较长（如1分钟），且为同步操作（即处理完成后才返回结果）。暂且不论这种方式的好坏，若nginx配置的响应等待时间（proxy_read_timeout）为30秒，就会触发超时重试，将请求又打到另一台。如果处理中没有考虑到重复数据的场景，就会发生数据多次重复插入！（当然，这种场景，内网可以通过机器名访问该服务器进行操作，就可以绕过nginx了，不过外网就没办法了。）

### 获取客户端真实IP

#### X-Real-IP

额外模块，不推荐使用

#### setHeader

```
proxy_set_header X-Forwarded-For $remote_addr;
```

### Gzip

作用域 `http, server, location`

##### gzip on;

开关，默认关闭

##### gzip_buffers 32 4k|16 8k

缓冲区大小

**gzip_comp_level** 1；

压缩等级 1-9，数字越大压缩比越高

##### gzip_http_version 1.1;

使用gzip的最小版本

##### gzip_min_length

设置将被gzip压缩的响应的最小长度。 长度仅由“Content-Length”响应报头字段确定。

##### gzip_proxied 多选

off 为不做限制

作为反向代理时，针对上游服务器返回的头信息进行压缩

expired - 启用压缩，如果header头中包含 "Expires" 头信息
no-cache - 启用压缩，如果header头中包含 "Cache-Control:no-cache" 头信息
no-store - 启用压缩，如果header头中包含 "Cache-Control:no-store" 头信息
private - 启用压缩，如果header头中包含 "Cache-Control:private" 头信息
no_last_modified - 启用压缩,如果header头中不包含 "Last-Modified" 头信息
no_etag - 启用压缩 ,如果header头中不包含 "ETag" 头信息
auth - 启用压缩 , 如果header头中包含 "Authorization" 头信息
any - 无条件启用压缩

##### gzip_vary on;

增加一个header，适配老的浏览器 `Vary: Accept-Encoding`

##### gzip_types

哪些mime类型的文件进行压缩

##### gzip_disable

禁止某些浏览器使用gzip

##### 完整实例

```
  gzip on;
  gzip_buffers 16 8k;
  gzip_comp_level 6;
  gzip_http_version 1.1;
  gzip_min_length 256;
  gzip_proxied any;
  gzip_vary on;
  gzip_types text/plain application/x-javascript text/css application/xml;
  gzip_types
    text/xml application/xml application/atom+xml application/rss+xml application/xhtml+xml image/svg+xml
    text/javascript application/javascript application/x-javascript
    text/x-json application/json application/x-web-app-manifest+json
    text/css text/plain text/x-component
    font/opentype application/x-font-ttf application/vnd.ms-fontobject
    image/x-icon;
  gzip_disable "MSIE [1-6]\.(?!.*SV1)";
```

```
HTTP/1.1 200
Server: nginx/1.21.6
Date: Wed, 18 May 2022 17:42:35 GMT
Content-Type: text/html;charset=utf-8
Content-Length: 7832
Connection: keep-alive
Keep-Alive: timeout=65
```

### ngx_http_gunzip_module

帮助不支持gzip的客户端解压本地文件

#### http_gzip_static_module

需要重新编译nginx

```
./configure --with-http_gzip_static_module
```

### Brotli

#### 安装

- 官网

  - `https://github.com/google/ngx_brotli`
  - `https://codeload.github.com/google/brotli/tar.gz/refs/tags/v1.0.9`
- 下载 两个项目
- 解压缩

模块化编译

```shell
./configure --with-compat --add-dynamic-module=/root/ngx_brotli-1.0.0rc --prefix=/usr/local/nginx/
```

或

```
--add-dynamic-module=brotli目录
```

- make
- 将`ngx_http_brotli_filter_module.so` `ngx_http_brotli_static_module.so`拷贝到`/usr/local/nginx/modules/`
- 复制nginx主程序
- 配置文件中添加

```
load_module "/usr/local/nginx/modules/ngx_http_brotli_filter_module.so";
load_module "/usr/local/nginx/modules/ngx_http_brotli_static_module.so";
```

```
brotli on;
    brotli_static on;
	brotli_comp_level 6;
	brotli_buffers 16 8k;
	brotli_min_length 20;
	brotli_types text/plain text/css text/javascript application/javascript text/xml application/xml application/xml+rss application/json image/jpeg image/gif image/png;

```

- 测试

默认http协议是没有br的

```
curl -H 'Accept-Encoding: gzip' -I http://localhost
```

### 合并客户端请求

Concat模块

Tengine

Nginx官方介绍

[https://www.nginx.com/resources/wiki/modules/concat/](https://www.nginx.com/resources/wiki/modules/concat/)

git地址

[https://github.com/alibaba/nginx-http-concat](https://github.com/alibaba/nginx-http-concat)

- 安装

下载源码解压缩编译安装

- 配置

```
    concat on;
    concat_max_files 30;
```

### 资源静态化

•高并发系统资源静态化方案

•一致性问题

•合并文件输出

•集群文件同步

### SSI合并服务器端文件

官方文档

[http://nginx.org/en/docs/http/ngx_http_ssi_module.html](http://nginx.org/en/docs/http/ngx_http_ssi_module.html)

#### 配置

**ssi_min_file_chunk**

向磁盘存储并使用sendfile发送，文件大小最小值

**ssi_last_modified**

是否保留lastmodified

**ssi_silent_errors**

不显示逻辑错误

**ssi_value_length**

限制脚本参数最大长度

**ssi_types**

默认text/html;如果需要其他mime类型 需要设置

#### include file

```
<!--# include file="footer.html" -->
```

静态文件直接引用

#### include virtual

可以指向location，而不一定是具体文件

#### include wait

阻塞请求

#### include set

在virtual基础上设置变量

#### set

设置临时变量

#### block

可以声明一个ssi的命令块，里面可以包裹其他命令

#### config errmsg

在模板中配置报错情况

#### config timefmt

日期格式化

#### echo

直接输出变量

- var变量名称
- encoding 是否使用特殊编码格式
- default 变量没有值的时候使用默认值

#### if

逻辑判断

### rsync

[https://www.samba.org/ftp/rsync/rsync.html](https://www.samba.org/ftp/rsync/rsync.html)

remote synchronize是一个远程数据同步工具，可通过 LAN/WAN 快速同步多台主机之间的文件。也可以使用 rsync 同步本地硬盘中的不同目录。
rsync 是用于替代 rcp 的一个工具，rsync 使用所谓的 rsync算法 进行数据同步，这种算法只传送两个文件的不同部分，而不是每次都整份传送，因此速度相当快。

rsync 基于inotify 开发

Rsync有三种模式：

- 本地模式（类似于cp命令）
- 远程模式（类似于scp命令）
- 守护进程（socket进程：是rsync的重要功能）

## rsync 常用选项

|选项|含义|
| :--------| :-------------------------------------------------------------------------------|
|-a|包含-rtplgoD|
|-r|同步目录时要加上，类似cp时的-r选项|
|-v|同步时显示一些信息，让我们知道同步的过程|
|-l|保留软连接|
|-L|加上该选项后，同步软链接时会把源文件给同步|
|-p|保持文件的权限属性|
|-o|保持文件的属主|
|-g|保持文件的属组|
|-D|保持设备文件信息|
|-t|保持文件的时间属性|
|–delete|删除DEST中SRC没有的文件|
|–exclude|过滤指定文件，如–exclude “logs”会把文件名包含logs的文件或者目录过滤掉，不同步|
|-P|显示同步过程，比如速率，比-v更加详细|
|-u|加上该选项后，如果DEST中的文件比SRC新，则不同步|
|-z|传输时压缩|

#### 安装

两端安装

```
yum install -y rsync
```

‍

#### 密码文件

创建文件`/etc/rsync.password`

内容

```
hello:123
```

修改权限

```
chmod 600 /etc/rsync.password
```

修改配置

```
auth users = sgg
secrets file = /etc/rsyncd.pwd
```

#### 开机启动

在`/etc/rc.local`文件中添加

```
rsync --daemon
```

- 修改权限

echo "sgg:111" >> /etc/rsyncd.passwd

#### 查看远程目录

rsync --list-only 192.168.44.104::www/

#### 拉取数据到指定目录

rsync -avz [rsync://192.168.44.104/www](rsync://192.168.44.104:873/www)

rsync -avz 192.168.44.104::www/ /root/w

##### 使用SSH方式

rsync -avzP /usr/local/nginx/html/ root@192.168.44.105:/www/

#### 客户端免密

客户端只放密码

```
echo "111" >> /etc/rsyncd.passwd
```

此时在客户端已经可以配合脚本实现定时同步了

#### 如何实现推送？

修改配置

```
rsync -avz --password-file=/etc/rsyncd.passwd.client /usr/local/nginx/html/ rsync://sgg@192.168.44.105:/www
```

`--delete 删除目标目录比源目录多余文件`

#### 实时推送

推送端安装inotify

依赖

```
yum install -y automake
```

```
wget http://github.com/downloads/rvoicilas/inotify-tools/inotify-tools-3.14.tar.gz
./configure --prefix=/usr/local/inotify
make && make install
```

监控目录

```
/usr/local/inotify/bin/inotifywait -mrq --timefmt '%Y-%m-%d %H:%M:%S' --format '%T %w%f %e' -e close_write,modify,delete,create,attrib,move //usr/local/nginx/html/
```

#### 简单自动化脚本

```shell
#!/bin/bash

/usr/local/inotify/bin/inotifywait -mrq --timefmt '%d/%m/%y %H:%M' --format '%T %w%f %e' -e close_write,modify,delete,create,attrib,move //usr/local/nginx/html/ | while read file
do
       
        rsync -az --delete --password-file=/etc/rsyncd.passwd.client /usr/local/nginx/html/ sgg@192.168.44.102::ftp/
done

```

#### inotify常用参数

|参数|说明|含义|
| ----------| ----------------------------------------------------------------------------------| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|-r|--recursive|#递归查询目录|
|-q|--quiet|#打印很少的信息，仅仅打印监控事件信息|
|-m|--monitor|#始终保持事件监听状态|
|--excludei||#排除文件或目录时，不区分大小写|
|--timefmt||#指定事件输出格式|
|--format||#打印使用指定的输出类似格式字符串|
|-e|--event[ -e|--event ... ]accessmodifyattribcloseopenmove_tomove createdeleteumount|#通过此参数可以指定要监控的事件 #文件或目录被读取#文件或目录的内容被修改#文件或目录属性被改变#文件或目录封闭，无论读/写模式#文件或目录被打开#文件或目录被移动至另外一个目录#文件或目录被移动另一个目录或从另一个目录移动至当前目录#文件或目录被创建在当前目录#文件或目录被删除#文件系统被卸载|

## 多级缓存

#### 静态资源缓存

#### 浏览器缓存

##### 什么时候可以用缓存？

1. 不常改变的内容
2. 过期时间
3. 针对post/get请求都可以
4. 存储位置
5. 磁盘使用空间限制

观察京东缓存及加载速度

- deskcache

字面理解是从内存中，其实也是字面的含义，这个资源是直接从内存中拿到的，**不会请求服务器**一般已经加载过该资源且缓存在了内存当中，当关闭该页面时，此资源就被内存释放掉了，再次重新打开相同页面时不会出现from memory cache的情况

- memorycache

是从磁盘当中取出的，也是在已经在之前的某个时间加载过该资源，**不会请求服务器**但是此资源不会随着该页面的关闭而释放掉，因为是存在硬盘当中的，下次打开仍会from disk cache

##### Age

是CDN添加的属性表示在CDN中缓存了多少秒

##### **via**

用来标识CDN缓存经历了哪些服务器，缓存是否命中，使用的协议

#### Nginx默认缓存

Nginx版本不同会默认配置

#### 强制缓存与协商缓存

强制缓存：直接从本机读取，不请求服务器

协商缓存：发送请求header中携带Last-Modified，服务器可能会返回304 Not Modified

#### 浏览器强制缓存

##### **cache-control**

http1.1的规范，使用max-age表示文件可以在浏览器中缓存的时间以秒为单位

|标记|功能|类型|
| ------------------------| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ------------|
|public|响应的数据可以被缓存，客户端和代理层都可以缓存|响应头|
|private|可私有缓存，客户端可以缓存，代理层不能缓存（CDN，proxy_pass）|响应头|
|no-cache|可以使用本地缓存，但是必须发送请求到服务器回源验证|请求头|
|no-store|应禁用缓存|请求和响应|
|max-age|文件可以在浏览器中缓存的时间以秒为单位|请求和响应|
|s-maxage|用户代理层缓存，CDN下发，当客户端数据过期时会重新校验|请求和响应|
|max-stale|缓存最大使用时间，如果缓存过期，但还在这个时间范围内则可以使用缓存数据|请求和响应|
|min-fresh|缓存最小使用时间，|请求和响应|
|must-revalidate|当缓存过期后，必须回源重新请求资源。比no-cache更严格。因为HTTP 规范是允许客户端在某些特殊情况下直接使用过期缓存的，比如校验请求发送失败的时候。那么带有must-revalidate的缓存必须校验，其他条件全部失效。|请求和响应|
|proxy-revalidate|和must-revalidate类似，只对CDN这种代理服务器有效，客户端遇到此头，需要回源验证|请求和响应|
|stale-while-revalidate|表示在指定时间内可以先使用本地缓存，后台进行异步校验|响应|
|stale-if-error|在指定时间内，重新验证时返回状态码为5XX的时候，可以用本地缓存|响应|
|only-if-cached|那么只使用缓存内容，如果没有缓存 则504 getway timeout|响应|

在浏览器和服务器端验证文件是否过期的时候，浏览器在二次请求的时候会携带IF-Modified-Since属性

##### Expires

过期时间

```
expires 30s;   #缓存30秒
expires 30m;  #缓存30分钟   
expires 2h;     #缓存2小时
expires 30d;    #缓存30天
```

#### 协商缓存

##### **last-modified**

##### **etag**

http1.1支持

在HTTP协议中If-Modified-Since和If-None-Match分别对应Last-Modified和ETag

Entity Tag 的缩写，中文译过来就是实体标签的意思.

HTTP中并没有指定如何生成ETag，哈希是比较理想的选择。

在计算Etag的时候，会产生CPU的耗费，所以也可以用时间戳，但这样直接使用Last-Modified即可。

ETag 用来校验用户请求的资源是否有变化，作用和lastmodified很像，区别是lastmodified精确到秒，ETag可以用hash算法来生成更精确的比对内容。

当用户首次请求资源的时候返回给用户数据和200状态码并生成ETag，再次请求的时候服务器比对ETag，没有发生变化的话返回304

Cache-Control直接是通过不请求来实现，而ETag是会发请求的，只不过服务器根据请求的东西的内容有无变化来判断是否返回请求的资源

### 总结：

#### cache-control expires 强制缓存

页面首次打开，直接读取缓存数据，刷新，会向服务器发起请求

#### etag lastmodify  协商缓存

没发生变化 返回304 不发送数据

##### last-modified 与ssi的冲突

#### 浏览器缓存原则

- 多级集群负载时last-modified必须保持一致
- 还有一些场景下我们希望禁用浏览器缓存。比如轮训api上报数据数据
- 浏览器缓存很难彻底禁用，大家的做法是加版本号，随机数等方法。
- 只缓存200响应头的数据，像3XX这类跳转的页面不需要缓存。
- 对于js，css这类可以缓存很久的数据，可以通过加版本号的方式更新内容
- 不需要强一致性的数据，可以缓存几秒
- 异步加载的接口数据，可以使用ETag来校验。
- 在服务器添加Server头，有利于排查错误
- 分为手机APP和Client以及是否遵循http协议
- 在没有联网的状态下可以展示数据
- 流量消耗过多
- 提前下发  避免秒杀时同时下发数据造成流量短时间暴增
- 兜底数据 在服务器崩溃和网络不可用的时候展示
- 临时缓存  退出即清理
- 固定缓存  展示框架这种，可能很长时间不会更新，可用随客户端下发

  - **首页**有的时候可以看做是框架 应该禁用缓存，以保证加载的资源都是最新的
- 父子连接 页面跳转时有一部分内容不需要重新加载，可用从父菜单带过来
- 预加载     某些逻辑可用判定用户接下来的操作，那么可用异步加载那些资源
- 漂亮的加载过程 异步加载 先展示框架，然后异步加载内容，避免主线程阻塞

### GEOip

#### 1 下载数据库

官网需注册登录

下载数据库

maxmind.com

#### 2 安装依赖

##### 官方git

[https://github.com/maxmind/libmaxminddb](https://github.com/maxmind/libmaxminddb)

下载后执行编译安装之后

```
$ echo /usr/local/lib  >> /etc/ld.so.conf.d/local.conf 
$ ldconfig
```

#### Nginx模块

[https://github.com/leev/ngx_http_geoip2_module](https://github.com/leev/ngx_http_geoip2_module)

更完整的配置可参考官方文档

[http://nginx.org/en/docs/http/ngx_http_geoip_module.html#geoip_proxy](http://nginx.org/en/docs/http/ngx_http_geoip_module.html#geoip_proxy)

#### Nginx配置

```
    geoip2 /root/GeoLite2-ASN_20220524/GeoLite2-ASN.mmdb {
$geoip2_country_code country iso_code;
    }
add_header country $geoip2_country_code;
```

### 正向代理与反向代理缓存

#### 正向代理配置

```
proxy_pass $scheme://$host$request_uri;
resolver 8.8.8.8;
```

#### 代理https请求

需要第三方模块

[https://github.com/chobits/ngx_http_proxy_connect_module](https://github.com/chobits/ngx_http_proxy_connect_module)

配置

```
 server {
     listen                         3128;

     # dns resolver used by forward proxying
     resolver                       8.8.8.8;

     # forward proxy for CONNECT request
     proxy_connect;
     proxy_connect_allow            443 563;
     proxy_connect_connect_timeout  10s;
     proxy_connect_read_timeout     10s;
     proxy_connect_send_timeout     10s;

     # forward proxy for non-CONNECT request
     location / {
         proxy_pass http://$host;
         proxy_set_header Host $host;
     }
 }
```

### proxy缓存

官网解释

[http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache)

#### 配置

```
http模块：
proxy_cache_path /ngx_tmp levels=1:2 keys_zone=test_cache:100m inactive=1d max_size=10g ;
location模块：
add_header  Nginx-Cache "$upstream_cache_status";
proxy_cache test_cache;
proxy_cache_valid 168h;
```

**proxy_cache_use_stale**

默认off

在什么时候可以使用过期缓存

可选`error` | `timeout` | `invalid_header` | `updating` | `http_500` | `http_502` | `http_503` | `http_504` | `http_403` | `http_404` | `http_429` | `off`

**proxy_cache_background_update**

默认off

运行开启子请求更新过期的内容。同时会把过期的内容返回给客户端

**proxy_no_cache**  **proxy_cache_bypass**

指定什么时候不使用缓存而直接请求上游服务器

```
proxy_no_cache $cookie_nocache $arg_nocache$arg_comment;
proxy_no_cache $http_pragma    $http_authorization;
```

如果这些变量如果存在的话不为空或者不等于0，则不使用缓存

**proxy_cache_convert_head**

默认 on

是否把head请求转换成get请求后再发送给上游服务器 以便缓存body里的内容

如果关闭 需要在 `cache key` 中添加 $request_method 以便区分缓存内容

**proxy_cache_lock**

默认off

缓存更新锁

**proxy_cache_lock_age**

默认5s

缓存锁超时时间

#### 断点续传缓存 range

当有完整的content-length之后即可断点续传

在反向代理服务器中需向后传递header

```
proxy_set_header Range $http_range;
```

proxy_cache_key中增加range

**proxy_cache_max_range_offset**

range最大值，超过之后不做缓存，默认情况下 不需要对单文件较大的资源做缓存

**proxy_cache_methods**

默认 head get

**proxy_cache_min_uses**

默认1

被请求多少次之后才做缓存

**proxy_cache_path**

path 指定存储目录

以cache_key取md5值

- **levels=1:2**

目录层级数及目录名称位数

取mdb5后几位

TMPFS

- **use_temp_path**

默认创建缓存文件时，先向缓冲区创建临时文件，再移动到缓存目录

是否使用缓冲区

- **inactive**

指定时间内未被访问过的缓存将被删除

#### 缓存清理

**purger**

需要第三方模块支持

[https://github.com/FRiCKLE/ngx_cache_purge](https://github.com/FRiCKLE/ngx_cache_purge)

配置

```

        location ~ /purge(/.*) {

            proxy_cache_purge  test_cache  $1;
        }
        自定义cachekey
         proxy_cache_key $uri;
```

**proxy_cache_key**

默认`$scheme$proxy_host$request_uri`

缓存的key

**proxy_cache_revalidate**

如果缓存过期了，向上游服务器发送“If-Modified-Since” and “If-None-Match来验证是否改变，如果没有就不需要重新下载资源了

**proxy_cache_valid**

可以针对不容http状态码设置缓存过期时间

不设置状态码会默认200, 301, 302

```
proxy_cache_valid 200 302 10m;
proxy_cache_valid 301      1h;
proxy_cache_valid any      1m;
```

any指其他任意状态码

‍
