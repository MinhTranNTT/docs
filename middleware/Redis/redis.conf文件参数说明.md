# redis.conf文件参数说明

‍

## 1、单位(Units)

配置大小单位,开头定义了一些基本的度量单位，只支持bytes，不支持bit

大小写不敏感

```sh
# 不区分大小写
# units are case insensitive so 1GB 1Gb 1gB are all the same.
```

## 2、包含(INCLUDES)

类似jsp中的include，多实例的情况可以把公用的配置文件提取出来

```sh
# include 组合多个配置问题
# include /path/to/local.conf
# include /path/to/other.conf
# include /path/to/fragments/*.conf
```

## 3、网络相关配置

### 3.1、bind

默认情况bind=127.0.0.1只能接受本机的访问请求

不写的情况下，无限制接受任何ip地址的访问

生产环境肯定要写你应用服务器的地址；服务器是需要远程访问的，所以需要将其注释掉

```sh
# 默认绑定 ip
bind 127.0.0.1 -::1
```

如果开启了protected-mode，那么在没有设定bind ip且没有设密码的情况下，Redis只允许接受本机的响应

### 3.2、protected-mode

```sh
# 保护模式
protected-mode yes
```

将本机访问保护模式设置no

### 3.3、port

端口号，默认 6379

```sh
# 端口
port 6379
```

### 3.4、tcp-backlog

设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列 + 已经完成三次握手队列。

在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。

注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值（128），所以需要确认增大/proc/sys/net/core/somaxconn和/proc/sys/net/ipv4/tcp_max_syn_backlog（128）两个值来达到想要的效果

```sh
tcp-backlog 511
```

### 3.5、timeout

一个空闲的客户端维持多少秒会关闭，0表示关闭该功能。即永不关闭。

```sh
timeout 0
```

### 3.6、tcp-keepalive

对访问客户端的一种心跳检测，每个n秒检测一次。

单位为秒，如果设置为0，则不会进行Keepalive检测，建议设置成60

```sh
tcp-keepalive 300
```

## 4、通用(GENERAL)

### 4.1、daemonize

是否为后台进程，设置为yes

守护进程，后台启动

```sh
daemonize yes
```

### 4.2、pidfile

存放pid文件的位置，每个实例会产生一个不同的pid文件

```sh
pidfile /var/run/redis_6379.pid
```

### 4.3、loglevel

指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为**notice**

四个级别根据使用阶段来选择，生产环境选择notice 或者warning

```sh
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
# 默认日志输出级别 notice
loglevel notice
```

### 4.4、logfile

```sh
# 日志文件名称
logfile ""
```

### 4.5、databases

设定库的数量 默认16，默认数据库为0，可以使用`SELECT <dbid>`命令在连接上指定数据库id

```sh
databases 16
```

## 5、SECURITY(安全)

### 5.1、设置密码

```sh
# 设置密码（默认null）
# requirepass foobared
```

访问密码的查看、设置和取消

在命令中设置密码，只是临时的。重启redis服务器，密码就还原了。

永久设置，需要再配置文件中进行设置。

## 6、LIMITS(限制)

### 6.1、maxclients

- 设置redis同时可以与多少个客户端进行连接。
- 默认情况下为10000个客户端。
- 如果达到了此限制，redis则会拒绝新的连接请求，并且向这些连接请求方发出“max number of clients reached”以作回应。

```sh
# 默认最大客户端数量
# maxclients 10000
```

### 6.2、maxmemory

- 建议**必须设置**，否则，将内存占满，造成服务器宕机
- 设置redis可以使用的内存量。一旦到达内存使用上限，redis将会试图移除内部数据，移除规则可以通过maxmemory-policy来指定。
- 如果redis无法根据移除规则来移除内存中的数据，或者设置了“不允许移除”，那么redis则会针对那些需要申请内存的指令返回错误信息，比如SET、LPUSH等。
- 但是对于无内存申请的指令，仍然会正常响应，比如GET等。如果你的redis是主redis（说明你的redis有从redis），那么在设置内存使用上限时，需要在系统中留出一些内存空间给同步队列缓存，只有在你设置的是“不移除”的情况下，才不用考虑这个因素。

```sh
# 默认最大内存限制
# maxmemory <bytes>
```

### 6.3、maxmemory-policy

- volatile-lru：使用LRU算法移除key，只对设置了过期时间的键；（最近最少使用）
- allkeys-lru：在所有集合key中，使用LRU算法移除key
- volatile-random：在过期集合中移除随机的key，只对设置了过期时间的键
- allkeys-random：在所有集合key中，移除随机的key
- volatile-ttl：移除那些TTL值最小的key，即那些最近要过期的key
- noeviction：不进行移除。针对写操作，只是返回错误信息

```sh
# maxmemory-policy 六种方式
# 1. volatile-lru：只对设置了过期时间的key进行LRU（默认值）
# 2. allkeys-lru ： 删除lru算法的key
# 3. volatile-random：随机删除即将过期key
# 4. allkeys-random：随机删除
# 5. volatile-ttl ： 删除即将过期的
# 6. noeviction ： 永不过期，返回错误
# 内存达到限制值的处理策略（redis 中的默认的过期策略是 volatile-lru ）
# maxmemory-policy noeviction
```

### 6.4、maxmemory-samples

- 设置样本数量，LRU算法和最小TTL算法都并非是精确的算法，而是估算值，所以你可以设置样本的大小，redis默认会检查这么多个key并选择其中LRU的那个。
- 一般设置3到7的数字，数值越小样本越不准确，但性能消耗越小。

```sh
maxmemory-samples 5
```

## 7、其他设置

```bash
# 持久化错误是否继续工作
stop-writes-on-bgsave-error yes

# 是否压缩 *.rdp 文件
rdbcompression yes

# 是否校验 rdp 文件
rdbchecksum yes

# rdp 文件的默认保存目录（当前目录）
dir ./

# 是否开启 AOF （默认不开启）
appendonly no
# 默认 AOF 文件名
appendfilename "appendonly.aof"
# 每次修改进行同步
# appendfsync always
# 每秒执行一次同步（数据同步策略）
appendfsync everysec
# 不进行同步 由操作系统进行同步 速度最快
# appendfsync no
```

## redis AOF

开启`appendonly yes`当 `appendonly.aof` 文件存在问题导致 redis 数据无法加载可以使用`redis-check-aof --fix appendonly.aof`恢复文件

‍
