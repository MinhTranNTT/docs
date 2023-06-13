# Nginx 高效

## Nginx内存缓存

**strace**

一般应用为静态文件元数据信息缓存

sendfile执行过程

```
epoll_wait(8, [{EPOLLIN, {u32=1904243152, u64=140709327827408}}, {EPOLLIN, {u32=1904242704, u64=140709327826960}}], 512, 25215) = 2
recvfrom(10, "GET / HTTP/1.1\r\nHost: 192.168.44"..., 1024, 0, NULL, NULL) = 475
stat("/usr/local/nginx//html/index.html", {st_mode=S_IFREG|0644, st_size=1429, ...}) = 0
open("/usr/local/nginx//html/index.html", O_RDONLY|O_NONBLOCK) = 11
fstat(11, {st_mode=S_IFREG|0644, st_size=1429, ...}) = 0
writev(10, [{iov_base="HTTP/1.1 200 OK\r\nServer: nginx/1"..., iov_len=263}], 1) = 263
sendfile(10, 11, [0] => [1429], 1429)   = 1429
write(4, "192.168.44.1 - - [27/May/2022:14"..., 193) = 193
close(11) 
```

**open_file_cache**

```
open_file_cache max=500 inactive=60s
open_file_cache_min_uses 1; 
open_file_cache_valid 60s; 
open_file_cache_errors on
```

**max**缓存最大数量，超过数量后会使用LRU淘汰

**inactive** 指定时间内未被访问过的缓存将被删除

**pen_file_cache_min_uses**

被访问到多少次后会开始缓存

**open_file_cache_valid**

间隔多长时间去检查文件是否有变化

**open_file_cache_errors**

对错误信息是否缓存

## Nginx外置缓存缓存

[http://nginx.org/en/docs/http/ngx_http_memcached_module.html](http://nginx.org/en/docs/http/ngx_http_memcached_module.html)

### error_page

指定状态码

```
	error_page 404 =302 http://www.atguigu.com;
```

默认指向location

### 匿名location

### nginx + memcached

#### memcached安装

```shell
yum -y install memcached
```

默认配置文件在

```shell
/etc/sysconfig/memcached
```

查看状态

```
memcached-tool 127.0.0.1:11211  stats
```

#### nginx配置

```nginx
  
   upstream backend {
  
#   server 192.168.44.102 weight=8 down;
   server 192.168.44.104:8080;
   }

location / {

        set            $memcached_key "$uri?$args";
        memcached_pass 127.0.0.1:11211;

	add_header X-Cache-Satus HIT;

	add_header Content-Type 'text/html; charset=utf-8'; # 强制响应数据格式为html



	# root   html;
        }

```

### nginx + redis

#### Redis安装

7.0下载地址

[https://codeload.github.com/redis/redis/tar.gz/refs/tags/7.0.0](https://codeload.github.com/redis/redis/tar.gz/refs/tags/7.0.0)

#### 安装

```
依赖
yum install -y tcl-devel

解压
make
make install 
```

### redis2-nginx-module

redis2-nginx-module是一个支持 Redis 2.0 协议的 Nginx upstream 模块，它可以让 Nginx 以非阻塞方式直接防问远方的 Redis 服务，同时支持 TCP 协议和 Unix Domain Socket 模式，并且可以启用强大的 Redis 连接池功能。

[https://www.nginx.com/resources/wiki/modules/redis2/](https://www.nginx.com/resources/wiki/modules/redis2/)

[https://github.com/openresty/redis2-nginx-module](https://github.com/openresty/redis2-nginx-module)

redis快速安装

```
yum install epel-release
yum install -y redis
```

redis2-nginx-module 安装

#### test

```nginx
location = /foo {

default_type text/html;

     redis2_query auth 123123;

     set $value 'first';

     redis2_query set one $value;

     redis2_pass 192.168.199.161:6379;

 }
```

#### get

```nginx
location = /get {

default_type text/html;

     redis2_pass 192.168.199.161:6379;

     redis2_query auth 123123;

     set_unescape_uri $key $arg_key;  # this requires ngx_set_misc

     redis2_query get $key;

}
```

#### set

```nginx
# GET /set?key=one&val=first%20value

location = /set {

default_type text/html;

redis2_pass 192.168.199.161:6379;

redis2_query auth 123123;
 

     set_unescape_uri $key $arg_key;  # this requires ngx_set_misc

     set_unescape_uri $val $arg_val;  # this requires ngx_set_misc

     redis2_query set $key $val;

 }
```

#### pipeline

```nginx
     set $value 'first';

     redis2_query set one $value;

     redis2_query get one;

     redis2_query set one two;

     redis2_query get one;

redis2_query del key1;
```

#### list

```lua
    redis2_query lpush key1 C;

    redis2_query lpush key1 B;

    redis2_query lpush key1 A;

redis2_query lrange key1 0 -1;
```

#### 集群

```nginx
upstream redis_cluster {

     server 192.168.199.161:6379;

     server 192.168.199.161:6379;

 }

location = /redis {

default_type text/html;

         redis2_next_upstream error timeout invalid_response;

         redis2_query get foo;

         redis2_pass redis_cluster;
   }
```

## Stream模块

[http://nginx.org/en/docs/stream/ngx_stream_core_module.html](http://nginx.org/en/docs/stream/ngx_stream_core_module.html)

### 限流

#### QPS限制

官方文档

[http://nginx.org/en/docs/http/ngx_http_limit_req_module.html](http://nginx.org/en/docs/http/ngx_http_limit_req_module.html)

测试工具

[https://jmeter.apache.org/](https://jmeter.apache.org/)

配置

```
limit_req_zone $binary_remote_addr zone=test:10m rate=15r/s;
limit_req zone=req_zone_wl burst=20 nodelay;
```

### 日志

### ngx_http_log_module

[http://nginx.org/en/docs/http/ngx_http_log_module.html](http://nginx.org/en/docs/http/ngx_http_log_module.html)

#### ngx_http_empty_gif_module

[http://nginx.org/en/docs/http/ngx_http_empty_gif_module.html](http://nginx.org/en/docs/http/ngx_http_empty_gif_module.html)

#### json

```json
log_format  ngxlog json '{"timestamp":"$time_iso8601",'
                    '"source":"$server_addr",'
                    '"hostname":"$hostname",'
                    '"remote_user":"$remote_user",'
                    '"ip":"$http_x_forwarded_for",'
                    '"client":"$remote_addr",'
                    '"request_method":"$request_method",'
                    '"scheme":"$scheme",'
                    '"domain":"$server_name",'
                    '"referer":"$http_referer",'
                    '"request":"$request_uri",'
                    '"requesturl":"$request",'
                    '"args":"$args",'
                    '"size":$body_bytes_sent,'
                    '"status": $status,'
                    '"responsetime":$request_time,'
                    '"upstreamtime":"$upstream_response_time",'
                    '"upstreamaddr":"$upstream_addr",'
                    '"http_user_agent":"$http_user_agent",'
                    '"http_cookie":"$http_cookie",'
                    '"https":"$https"'
                    '}';
```

#### errorlog

[http://nginx.org/en/docs/ngx_core_module.html#error_log](http://nginx.org/en/docs/ngx_core_module.html#error_log)

#### 日志分割

1.脚本

2.Logrotate

### 重试机制

[http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream)

**max_fails**

最大失败次数

0为标记一直可用，不检查健康状态

**fail_timeout**

失败时间

当fail_timeout时间内失败了max_fails次，标记服务不可用

fail_timeout时间后会再次激活次服务

**proxy_next_upstream**

**proxy_next_upstream_timeout**

重试最大超时时间

**proxy_next_upstream_tries**

重试次数，包括第一次

proxy_next_upstream_timeout时间内允许proxy_next_upstream_tries次重试

### 主动健康检查

tengine版

[https://github.com/yaoweibin/nginx_upstream_check_module](https://github.com/yaoweibin/nginx_upstream_check_module)

nginx商业版

[http://nginx.org/en/docs/http/ngx_http_upstream_hc_module.html](http://nginx.org/en/docs/http/ngx_http_upstream_hc_module.html)

### 配置

```
      upstream backend {
  
#   server 192.168.44.102 weight=8 down;
   server 192.168.44.104:8080;
   server 192.168.44.105:8080;
 check interval=3000 rise=2 fall=5 timeout=1000 type=http;
       check_http_send "HEAD / HTTP/1.0\r\n\r\n";
       check_http_expect_alive http_2xx http_3xx;
   }

   
   
   
   
   
   
   location /status {
                check_status;
                access_log off;
        }


        location / {

proxy_pass http://backend;

	 root   html;
        }
```

## Openresty

### Lua

Lua 是由巴西里约热内卢天主教大学（Pontifical Catholic University of Rio de Janeiro）里的一个研究小组于1993年开发的一种轻量、小巧的脚本语言，用标准 C 语言编写，其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。

官网：[http://www.lua.org/](http://www.lua.org/)

### IDE

#### EmmyLua插件

[https://github.com/EmmyLua/IntelliJ-EmmyLua](https://github.com/EmmyLua/IntelliJ-EmmyLua)

[https://emmylua.github.io/zh_CN/](https://emmylua.github.io/zh_CN/)

#### LDT 基于eclipse

[https://www.eclipse.org/ldt/](https://www.eclipse.org/ldt/)

### Lua基础语法

#### hello world

```lua
print("hello world!")
```

#### 保留关键字

​`and`​       `break`​     `do   ​`​ `else`​      `elseif`​      `end`​       `false`​    `​ for`​       `function  if`​      `in`​        `local`​     `nil`​      `not`​      `or`​      `repeat`​    `return`​    `then`​     `​ true`​      `until`​    `​ while`​

#### 注释

```lua
-- 两个减号是行注释

--[[

 这是块注释

 这是块注释.

 --]]
```

#### 变量

##### 数字类型

Lua的数字只有double型，64bits

你可以以如下的方式表示数字

```lua
num = 1024

num = 3.0

num = 3.1416

num = 314.16e-2

num = 0.31416E1

num = 0xff

num = 0x56
```

##### 字符串

可以用单引号，也可以用双引号

也可以使用转义字符‘\n’ （换行）， ‘\r’ （回车）， ‘\t’ （横向制表）， ‘\v’ （纵向制表）， ‘\’ （反斜杠）， ‘\”‘ （双引号）， 以及 ‘\” （单引号)等等

下面的四种方式定义了完全相同的字符串（其中的两个中括号可以用于定义有换行的字符串）

```
a = 'alo\n123"'

a = "alo\n123\""

a = '\97lo\10\04923"'

a = [[alo

123"]]
```

##### 空值

C语言中的NULL在Lua中是nil，比如你访问一个没有声明过的变量，就是nil

##### 布尔类型

只有nil和false是 false

数字0，‘’空字符串（’\0’）都是true

##### 作用域

lua中的变量如果没有特殊说明，全是全局变量，那怕是语句块或是函数里。

变量前加local关键字的是局部变量。

#### 控制语句

##### while循环

```lua
local i = 0

local max = 10

while i <= max do

print(i)

i = i +1

end
```

##### if-else

```lua
local function main()


local age = 140

local sex = 'Male'
 

  if age == 40 and sex =="Male" then
    print(" 男人四十一枝花 ")
  elseif age > 60 and sex ~="Female" then
   
    print("old man!!")
  elseif age < 20 then
    io.write("too young, too simple!\n")
  
  else
  print("Your age is "..age)
  end

end


-- 调用
main()

```

##### for循环

```lua
sum = 0

for i = 100, 1, -2 do

	sum = sum + i

end
```

##### 函数

```lua
function myPower(x,y)

  return      y+x

end

power2 = myPower(2,3)

 print(power2)
```

```lua
function newCounter()

   local i = 0
   return function()     -- anonymous function

        i = i + 1

        return i

    end
end

 

c1 = newCounter()

print(c1())  --> 1

print(c1())  --> 2

print(c1())
```

#### 返回值

```lua
name, age,bGay = "yiming", 37, false, "yimingl@hotmail.com"

print(name,age,bGay)
```

```lua
function isMyGirl(name)
 return name == 'xiao6' , name
end

local bol,name = isMyGirl('xiao6')

print(name,bol)
```

#### Table

key，value的键值对 类似 map

```lua
local function main()
dog = {name='111',age=18,height=165.5}

dog.age=35

print(dog.name,dog.age,dog.height)

print(dog)
end
main()

```

#### 数组

```lua
local function main()
arr = {"string", 100, "dog",function() print("wangwang!") return 1 end}

print(arr[4]())
end
main()

```

#### 遍历

```lua
arr = {"string", 100, "dog",function() print("wangwang!") return 1 end}

for k, v in pairs(arr) do

   print(k, v)
end
```

#### 成员函数

#### 

```lua
local function main()

person = {name='旺财',age = 18}

  function  person.eat(food)

    print(person.name .." eating "..food)

  end
person.eat("骨头")
end
main()

```

## Openresty Nginx + Lua

Nginx是一个主进程配合多个工作进程的工作模式，每个进程由单个线程来处理多个连接。

在生产环境中，我们往往会把cpu内核直接绑定到工作进程上，从而提升性能。

### 安装

#### 预编译安装

以CentOS举例 其他系统参照：[http://openresty.org/cn/linux-packages.html](http://openresty.org/cn/linux-packages.html)

你可以在你的 CentOS 系统中添加 openresty 仓库，这样就可以便于未来安装或更新我们的软件包（通过 yum update 命令）。运行下面的命令就可以添加我们的仓库：

      yum install yum-utils

      yum-config-manager --add-repo [https://openresty.org/package/centos/openresty.repo](https://openresty.org/package/centos/openresty.repo)

然后就可以像下面这样安装软件包，比如 openresty：

   yum install openresty

如果你想安装命令行工具 resty，那么可以像下面这样安装 openresty-resty 包：

      sudo yum install openresty-resty

#### 源码编译安装

#### 下载

[http://openresty.org/cn/download.html](http://openresty.org/cn/download.html)

最小版本基于nginx1.21

​`./configure`​

然后在进入 `openresty-VERSION/ ​`​目录, 然后输入以下命令配置:

​`./configure`​

默认, `--prefix=/usr/local/openresty`​ 程序会被安装到`/usr/local/openresty`​目录。

依赖 `gcc openssl-devel pcre-devel zlib-devel`​

安装：`yum install gcc openssl-devel pcre-devel zlib-devel postgresql-devel`​

您可以指定各种选项，比如

```
./configure --prefix=/opt/openresty \

           --with-luajit \

           --without-http_redis2_module \

           --with-http_iconv_module \

           --with-http_postgres_module
```

试着使用 `./configure --help`​ 查看更多的选项。

​`make && make install`​

#### 服务命令

##### 启动

​`Service openresty start`​

##### 停止

​`Service openresty stop`​

##### 检查配置文件是否正确

​`Nginx -t`​

重新加载配置文件

​`Service openresty reload`​

##### 查看已安装模块和版本号

​`Nginx -V`​

### 测试lua脚本

```nginx
在Nginx.conf 中写入
   location /lua {

        default_type text/html;
        content_by_lua '
           ngx.say("<p>Hello, World!</p>")
         ';
      }
```

### lua-nginx-module

#### 创建配置文件lua.conf

```nginx
   server {
        listen       80;
        server_name  localhost;

   location /lua {

        default_type text/html;

        content_by_lua_file conf/lua/hello.lua;

         }
}
```

#### 在Nginx.conf下引入lua配置

​`include       lua.conf;`​

#### 创建外部lua脚本

​`conf/lua/hello.lua`​

内容：

​`ngx.say("<p>Hello, World!</p>")`​

#### 获取Nginx uri中的单一变量

```nginx
    location /nginx_var {

         default_type text/html;

        content_by_lua_block {

            ngx.say(ngx.var.arg_a)

        }
    }
```

#### 获取Nginx uri中的所有变量

```lua
local uri_args = ngx.req.get_uri_args()  

for k, v in pairs(uri_args) do  

    if type(v) == "table" then  

        ngx.say(k, " : ", table.concat(v, ", "), "<br/>")  

    else  

        ngx.say(k, ": ", v, "<br/>")  

    end  
end
```

#### 在处理http请求时还可以使用

* set_by_lua

修改nginx变量

* rewrite_by_lua

修改uri

* access_by_lua

访问控制

* header_filter_by_lua

修改响应头

* boy_filter_by_lua

修改响应体

* log_by_lua

日志

#### 代码热部署

```
lua_code_cache off
```

#### 获取Nginx请求头信息

```lua
local headers = ngx.req.get_headers()                     

ngx.say("Host : ", headers["Host"], "<br/>")  

ngx.say("user-agent : ", headers["user-agent"], "<br/>")  

ngx.say("user-agent : ", headers.user_agent, "<br/>")

for k,v in pairs(headers) do  

    if type(v) == "table" then  

        ngx.say(k, " : ", table.concat(v, ","), "<br/>")  

    else  

        ngx.say(k, " : ", v, "<br/>")  

    end  

end  
```

#### 获取post请求参数

```lua
ngx.req.read_body()  

ngx.say("post args begin", "<br/>")  

local post_args = ngx.req.get_post_args()  

for k, v in pairs(post_args) do  

    if type(v) == "table" then  

        ngx.say(k, " : ", table.concat(v, ", "), "<br/>")  

    else  

        ngx.say(k, ": ", v, "<br/>")  

    end  
end
```

#### http协议版本

```lua
ngx.say("ngx.req.http_version : ", ngx.req.http_version(), "<br/>")
```

#### 请求方法

```lua
ngx.say("ngx.req.get_method : ", ngx.req.get_method(), "<br/>")  
```

#### 原始的请求头内容

```lua
ngx.say("ngx.req.raw_header : ",  ngx.req.raw_header(), "<br/>")  
```

#### body内容体

```lua
ngx.say("ngx.req.get_body_data() : ", ngx.req.get_body_data(), "<br/>")
```

### Nginx缓存

#### Nginx全局内存缓存

```lua
lua_shared_dict shared_data 1m;

local shared_data = ngx.shared.shared_data  

  

local i = shared_data:get("i")  

if not i then  

    i = 1  

    shared_data:set("i", i)  

    ngx.say("lazy set i ", i, "<br/>")  
end  
 

i = shared_data:incr("i", 1)  

ngx.say("i=", i, "<br/>")
```

#### lua-resty-lrucache

Lua 实现的一个简单的 LRU 缓存，适合在 Lua 空间里直接缓存较为复杂的 Lua 数据结构：它相比 ngx_lua 共享内存字典可以省去较昂贵的序列化操作，相比 memcached 这样的外部服务又能省去较昂贵的 socket 操作

[https://github.com/openresty/lua-resty-lrucache](https://github.com/openresty/lua-resty-lrucache)

引用lua文件

```

	            content_by_lua_block {
                require("my/cache").go()
            }
```

自定义函数

```
local _M = {}


lrucache = require "resty.lrucache"

c, err = lrucache.new(200)  -- allow up to 200 items in the cache
ngx.say("count=init")


if not c then
    error("failed to create the cache: " .. (err or "unknown"))
end

function _M.go()

count = c:get("count")


c:set("count",100)
ngx.say("count=", count, " --<br/>")


if not count then  


    c:set("count",1)

    ngx.say("lazy set count ", c:get("count"), "<br/>")  

else


c:set("count",count+1)
 


ngx.say("count=", count, "<br/>")
end


end
return _M


```

打开lua_code_cache

### lua-resty-redis访问redis

[https://github.com/openresty/lua-resty-redis](https://github.com/openresty/lua-resty-redis)

#### 常用方法

```lua
local res, err = red:get("key")

local res, err = red:lrange("nokey", 0, 1)

ngx.say("res:",cjson.encode(res))
```

#### 创建连接

```lua
red, err = redis:new()

ok, err = red:connect(host, port, options_table?)
```

#### timeout

```lua
red:set_timeout(time)
```

#### keepalive

```lua
red:set_keepalive(max_idle_timeout, pool_size)
```

#### close

```
ok, err = red:close()
```

#### pipeline

```nginx
red:init_pipeline()

results, err = red:commit_pipeline()
```

#### 认证

```nginx
    local res, err = red:auth("foobared")

    if not res then

        ngx.say("failed to authenticate: ", err)

        return
end
```

```
  local redis = require "resty.redis"
                local red = redis:new()

                red:set_timeouts(1000, 1000, 1000) -- 1 sec

  local ok, err = red:connect("127.0.0.1", 6379)
 if not ok then
                    ngx.say("failed to connect: ", err)
                    return
                end

                ok, err = red:set("dog", "an animal")
                if not ok then
                    ngx.say("failed to set dog: ", err)
                    return
                end

                ngx.say("set result: ", ok)

                local res, err = red:get("dog")
                if not res then
                    ngx.say("failed to get dog: ", err)
                    return
                end

                if res == ngx.null then
                    ngx.say("dog not found.")
                    return
                end


              ngx.say("dog: ", res)
```

#### redis-cluster支持

[https://github.com/steve0511/resty-redis-cluster](https://github.com/steve0511/resty-redis-cluster)

### redis2-nginx-module

redis2-nginx-module是一个支持 Redis 2.0 协议的 Nginx upstream 模块，它可以让 Nginx 以非阻塞方式直接防问远方的 Redis 服务，同时支持 TCP 协议和 Unix Domain Socket 模式，并且可以启用强大的 Redis 连接池功能。

#### test

```nginx
location = /foo {

default_type text/html;

     redis2_query auth 123123;

     set $value 'first';

     redis2_query set one $value;

     redis2_pass 192.168.199.161:6379;

 }
```

#### get

```nginx
location = /get {

default_type text/html;

     redis2_pass 192.168.199.161:6379;

     redis2_query auth 123123;

     set_unescape_uri $key $arg_key;  # this requires ngx_set_misc

     redis2_query get $key;

}
```

#### set

```nginx
# GET /set?key=one&val=first%20value

location = /set {

default_type text/html;

redis2_pass 192.168.199.161:6379;

redis2_query auth 123123;
 

     set_unescape_uri $key $arg_key;  # this requires ngx_set_misc

     set_unescape_uri $val $arg_val;  # this requires ngx_set_misc

     redis2_query set $key $val;

 }
```

#### pipeline

```nginx
     set $value 'first';

     redis2_query set one $value;

     redis2_query get one;

     redis2_query set one two;

     redis2_query get one;

redis2_query del key1;
```

#### list

```lua
    redis2_query lpush key1 C;

    redis2_query lpush key1 B;

    redis2_query lpush key1 A;

redis2_query lrange key1 0 -1;
```

#### 集群

```nginx
upstream redis_cluster {

     server 192.168.199.161:6379;

     server 192.168.199.161:6379;

 }

location = /redis {

default_type text/html;

         redis2_next_upstream error timeout invalid_response;

         redis2_query get foo;

         redis2_pass redis_cluster;
   }
```

### lua-resty-mysql

[https://github.com/openresty/lua-resty-mysql](https://github.com/openresty/lua-resty-mysql)

```
local mysql = require "resty.mysql"
                local db, err = mysql:new()
                if not db then
                    ngx.say("failed to instantiate mysql: ", err)
                    return
                end

                db:set_timeout(1000) -- 1 sec


                local ok, err, errcode, sqlstate = db:connect{
                    host = "192.168.44.211",
                    port = 3306,
                    database = "zhangmen",
                    user = "root",
                    password = "111111",
                    charset = "utf8",
                    max_packet_size = 1024 * 1024,
                }


                ngx.say("connected to mysql.<br>")



                local res, err, errcode, sqlstate = db:query("drop table if exists cats")
                if not res then
                    ngx.say("bad result: ", err, ": ", errcode, ": ", sqlstate, ".")
                    return
                end


                res, err, errcode, sqlstate =
                    db:query("create table cats "
                             .. "(id serial primary key, "
                             .. "name varchar(5))")
                if not res then
                    ngx.say("bad result: ", err, ": ", errcode, ": ", sqlstate, ".")
                    return
                end

                ngx.say("table cats created.")



                res, err, errcode, sqlstate =
                    db:query("select * from t_emp")
                if not res then
                    ngx.say("bad result: ", err, ": ", errcode, ": ", sqlstate, ".")
                    return
                end

                local cjson = require "cjson"
                ngx.say("result: ", cjson.encode(res))


                local ok, err = db:set_keepalive(10000, 100)
                if not ok then
                    ngx.say("failed to set keepalive: ", err)
                    return
                end
 

```

## 模板实时渲染 lua-resty-template

[https://github.com/bungle/lua-resty-template](https://github.com/bungle/lua-resty-template)

如果学习过JavaEE中的servlet和JSP的话，应该知道JSP模板最终会被翻译成Servlet来执行；

而lua-resty-template模板引擎可以认为是JSP，其最终会被翻译成Lua代码，然后通过ngx.print输出。

lua-resty-template大体内容有：

l   模板位置：从哪里查找模板；

l   变量输出/转义：变量值输出；

l   代码片段：执行代码片段，完成如if/else、for等复杂逻辑，调用对象函数/方法；

l   注释：解释代码片段含义；

l   include：包含另一个模板片段；

l   其他：lua-resty-template还提供了不需要解析片段、简单布局、可复用的代码块、宏指令等支持。

基础语法

l   {(include_file)}：包含另一个模板文件；

l   {* var *}：变量输出；

l   {{ var }}：变量转义输出；

l   {% code %}：代码片段；

l   {# comment #}：注释；

l   {-raw-}：中间的内容不会解析，作为纯文本输出；

### lua代码热加载

在http模块中加入

```
lua_code_cache off;
```

reload后Nginx会提示影响性能，记得在生产环境中关掉。

​![1569585068623](1569585068623.png)​

### 测试

### 一、初始化

```
-- Using template.new
local template = require "resty.template"
local view = template.new "view.html"
view.message = "Hello, World!"
view:render()

-- Using template.render
-- template.render("view.html", { message = "Hel11lo, Worl1d!" })


```

### 二、执行函数，得到渲染之后的内容

```
local func = template.compile("view.html")  

local content = func(context)  

ngx.say("xx:",content) 
```

#### 模板文件存放位置

##### nginx.conf中配置

```
set $template_root /usr/local/openresty/nginx/tmp;
```

### resty.template.html

```lua
local template = require("resty.template")
local html = require "resty.template.html"

template.render([[
<ul>
{% for _, person in ipairs(context) do %}
    {*html.li(person.name)*} --
{% end %}
</ul>
<table>
{% for _, person in ipairs(context) do %}
    <tr data-sort="{{(person.name or ""):lower()}}">
        {*html.td{ id = person.id }(person.name)*}
    </tr>
{% end %}
</table>]], {
    { id = 1, name = "Emma"},
    { id = 2, name = "James" },
    { id = 3, name = "Nicholas" },
    { id = 4 }
})

```

### 模板内容

```html
<!DOCTYPE html>
<html>
<body>
  <h1>{{message}}</h1>
</body>
</html>

```

### 多值传入

```lua
template.caching(false)
local template = require("resty.template")
local context = {
    name = "lucy",
    age = 50,
}
template.render("view.html", context)

```

### 模板内容

```lua
<!DOCTYPE html>
<html>
<body>
  <h1>name:{{name}}</h1>
  <h1>age:{{age}}</h1>
</body>
</html>

```

### 模板管理与缓存

模板缓存：默认开启，开发环境可以手动关闭

​`template.caching(true)`​

模板文件需要业务系统更新与维护，当模板文件更新后，可以通过模板版本号或消息通知Openresty清空缓存重载模板到内存中

​`template.cache = {}`​

### 完整页面

```lua
local template = require("resty.template")
template.caching(false)
local context = {
    title = "测试",
    name = "lucy",
    description = "<script>alert(1);</script>",
    age = 40,
    hobby = {"电影", "音乐", "阅读"},
    score = {语文 = 90, 数学 = 80, 英语 = 70},
    score2 = {
        {name = "语文", score = 90},
        {name = "数学", score = 80},
        {name = "英语", score = 70},
    }
}

template.render("view.html", context)

```

### 模板

```html
{(header.html)}  
   <body>  
      {# 不转义变量输出 #}  
      姓名：{* string.upper(name) *}<br/>  
      {# 转义变量输出 #}  
      简介：{{description}}
           简介：{* description *}<br/>  
      {# 可以做一些运算 #}  
      年龄: {* age + 10 *}<br/>  
      {# 循环输出 #}  
      爱好：  
      {% for i, v in ipairs(hobby) do %}  
         {% if v == '电影' then  %} - xxoo
        
              {%else%}  - {* v *} 
{% end %}  
     
      {% end %}<br/>  
  
      成绩：  
      {% local i = 1; %}  
      {% for k, v in pairs(score) do %}  
         {% if i > 1 then %}，{% end %}  
         {* k *} = {* v *}  
         {% i = i + 1 %}  
      {% end %}<br/>  
      成绩2：  
      {% for i = 1, #score2 do local t = score2[i] %}  
         {% if i > 1 then %}，{% end %}  
          {* t.name *} = {* t.score *}  
      {% end %}<br/>  
      {# 中间内容不解析 #}  
      {-raw-}{(file)}{-raw-}  
{(footer.html)}  

```

### layout 布局统一风格

使用模板内容嵌套可以实现全站风格同一布局

#### lua

​`local template = require "resty.template"`​

一、

```
local layout   = template.new "layout.html"

layout.title   = "Testing lua-resty-template"

layout.view    = template.compile "view.html" { message = "Hello, World!" }

layout:render()
```

二、

```
template.render("layout.html", {

  title = "Testing lua-resty-template",

  msg = "type=2",

  view  = template.compile "view.html" { message = "Hello, World!" }

})
```

三、

此方式重名变量值会被覆盖

```
local view     = template.new("view.html", "layout.html")

view.title     = "Testing lua-resty-template"

view.msg = "type=3"

view.message   = "Hello, World!"

view:render()
```

四、

可以区分一下

```
local layout   = template.new "layout.html"

layout.title   = "Testing lua-resty-template"

layout.msg = "type=4"

local view     = template.new("view.html", layout)

view.message   = "Hello, World!"

view:render()
```

#### layout.html

```
<!DOCTYPE html>

<html>

<head>

    <title>{{title}}</title>

</head>

<h1>layout</h1>

<body>

    {*view*}

</body>

</html>
```

#### view.html·

​`msg:{{message}}`​

#### 多级嵌套

lua

```
local view     = template.new("view.html", "layout.html")

view.title     = "Testing lua-resty-template"

view.message   = "Hello, World!"

view:render()

view.html

{% layout="section.html" %}
```

<div>
<h1>msg:{{message}}</h1>
</div>

section.html

<div>
<div
id="section">
</div>

    {*view*} - sss

<div>
</div>

layout.html

<div>
<!DOCTYPE html>
</div>

<div>
<html>
</div>

<div>
<head>
</div>

    <title>{{title}}</title>

<div>
</head>
</div>

<div>
<h1>layout {{msg}}</h1>
</div>

<div>
<body>
</div>

    {*view*}

<div>
</body>
</div>

<div>
</html>
</div>

### Redis缓存+mysql+模板输出

lua

```
  cjson = require "cjson"
sql="select * from t_emp"


local redis = require "resty.redis"
                local red = redis:new()

                red:set_timeouts(1000, 1000, 1000) -- 1 sec

  local ok, err = red:connect("127.0.0.1", 6379)
 if not ok then
                    ngx.say("failed to connect: ", err)
                    return
                end


    
                local res, err = red:get(sql)
                if not res then
                    ngx.say("failed to get sql: ", err)
                    return
                end

                if res == ngx.null then
                    ngx.say("sql"..sql.." not found.")




--mysql查询
local mysql = require "resty.mysql"
                local db, err = mysql:new()
                if not db then
                    ngx.say("failed to instantiate mysql: ", err)
                    return
                end

                db:set_timeout(1000) -- 1 sec


                local ok, err, errcode, sqlstate = db:connect{
                    host = "192.168.44.211",
                    port = 3306,
                    database = "zhangmen",
                    user = "root",
                    password = "111111",
                    charset = "utf8",
                    max_packet_size = 1024 * 1024,
                }


                ngx.say("connected to mysql.<br>")


 res, err, errcode, sqlstate =
                    db:query(sql)
                if not res then
                    ngx.say("bad result: ", err, ": ", errcode, ": ", sqlstate, ".")
                    return
                end


          --ngx.say("result: ", cjson.encode(res))



      ok, err = red:set(sql, cjson.encode(res))
                if not ok then
                    ngx.say("failed to set sql: ", err)
                    return
                end

                ngx.say("set result: ", ok)

                    return
                end








local template = require("resty.template")
template.caching(false)
local context = {
    title = "测试",
    name = "lucy",
    description = "<script>alert(1);</script>",
    age = 40,
    hobby = {"电影", "音乐", "阅读"},
    score = {语文 = 90, 数学 = 80, 英语 = 70},
    score2 = {
        {name = "语文", score = 90},
        {name = "数学", score = 80},
        {name = "英语", score = 70},
    },

zhangmen=cjson.decode(res)

}





template.render("view.html", context)
```

模板

```

{(header.html)}  
   <body>  
      {# 不转义变量输出 #}  
      姓名：{* string.upper(name) *}<br/>  
      {# 转义变量输出 #}  

      年龄: {* age + 10 *}<br/>  
      {# 循环输出 #}  
      爱好：  
      {% for i, v in ipairs(hobby) do %}  
         {% if v == '电影' then  %} - xxoo
        
              {%else%}  - {* v *} 
{% end %}  
     
      {% end %}<br/>  
  
      成绩：  
      {% local i = 1; %}  
      {% for k, v in pairs(score) do %}  
         {% if i > 1 then %}，{% end %}  
         {* k *} = {* v *}  
         {% i = i + 1 %}  
      {% end %}<br/>  
      成绩2：  
      {% for i = 1, #score2 do local t = score2[i] %}  
         {% if i > 1 then %}，{% end %}  
          {* t.name *} = {* t.score *}  
      {% end %}<br/>  
      {# 中间内容不解析 #}  
      {-raw-}{(file)}{-raw-}  




掌门：
{* zhangmen *}



   {% for i = 1, #zhangmen do local z = zhangmen[i] %}  
         {* z.deptId *},{* z.age *},{* z.name *},{* z.empno *},<br>
      {% end %}<br/>  

{(footer.html)}  

```

## Lua 开源项目

### WAF

[https://github.com/unixhot/waf](https://github.com/unixhot/waf)

[https://github.com/loveshell/ngx_lua_waf](https://github.com/loveshell/ngx_lua_waf)

l   防止 SQL 注入，本地包含，部分溢出，fuzzing 测试，XSS/SSRF 等 Web 攻击

l   防止 Apache Bench 之类压力测试工具的攻击

l   屏蔽常见的扫描黑客工具，扫描器

l   屏蔽图片附件类目录执行权限、防止 webshell 上传

l   支持 IP 白名单和黑名单功能，直接将黑名单的 IP 访问拒绝

l   支持 URL 白名单，将不需要过滤的 URL 进行定义

l   支持 User-Agent 的过滤、支持 CC 攻击防护、限制单个 URL 指定时间的访问次数

l   支持支持 Cookie 过滤，URL 与 URL 参数过滤

l   支持日志记录，将所有拒绝的操作，记录到日志中去

### Kong 基于Openresty的流量网关

[https://konghq.com/](https://konghq.com/)

[https://github.com/kong/kong](https://github.com/kong/kong)

Kong 基于 OpenResty，是一个云原生、快速、可扩展、分布式的微服务抽象层（Microservice Abstraction Layer），也叫 API 网关（API Gateway），在 Service Mesh 里也叫 API 中间件（API Middleware）。

Kong 开源于 2015 年，核心价值在于高性能和扩展性。从全球 5000 强的组织统计数据来看，Kong 是现在依然在维护的，在生产环境使用最广泛的 API 网关。

Kong 宣称自己是世界上最流行的开源微服务 API 网关（The World’s Most Popular Open Source Microservice API Gateway）。

核心优势：

l   可扩展：可以方便的通过添加节点水平扩展，这意味着可以在很低的延迟下支持很大的系统负载。

l   模块化：可以通过添加新的插件来扩展 Kong 的能力，这些插件可以通过 RESTful Admin API 来安装和配置。

l   在任何基础架构上运行：Kong 可以在任何地方都能运行，比如在云或混合环境中部署 Kong，单个或全球的数据中心。

### APISIX

### ABTestingGateway

[https://github.com/CNSRE/ABTestingGateway](https://github.com/CNSRE/ABTestingGateway)

ABTestingGateway 是一个可以动态设置分流策略的网关，关注与灰度发布相关领域，基于 Nginx 和 ngx-lua 开发，使用 Redis 作为分流策略数据库，可以实现动态调度功能。

ABTestingGateway 是新浪微博内部的动态路由系统 dygateway 的一部分，目前已经开源。在以往的基于 Nginx 实现的灰度系统中，分流逻辑往往通过 rewrite 阶段的 if 和 rewrite 指令等实现，优点是性能较高，缺点是功能受限、容易出错，以及转发规则固定，只能静态分流。ABTestingGateway 则采用 ngx-lua，通过启用 lua-shared-dict 和 lua-resty-lock 作为系统缓存和缓存锁，系统获得了较为接近原生 Nginx 转发的性能。

l   支持多种分流方式，目前包括 iprange、uidrange、uid 尾数和指定uid分流

l   支持多级分流，动态设置分流策略，即时生效，无需重启

l   可扩展性，提供了开发框架，开发者可以灵活添加新的分流方式，实现二次开发

l   高性能，压测数据接近原生 Nginx 转发

l   灰度系统配置写在 Nginx 配置文件中，方便管理员配置

l   适用于多种场景：灰度发布、AB 测试和负载均衡等
