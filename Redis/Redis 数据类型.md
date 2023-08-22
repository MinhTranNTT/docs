# Redis 数据类型

参考文档：

- [Redis命令中心（Redis commands） -- Redis中国用户组（CRUG）](http://www.redis.cn/commands.html)
- [redis基本操作命令 - 简书 (jianshu.com)](https://www.jianshu.com/p/32b9fe8c20e1)

---

Redis 支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及 zsetsorted set（有序集合）。

我们实际项目中比较常用的是 string，hash 如果你是 Redis 中高级用户，还需要加上下面几种数据结构 HyperLogLog、Geo、Pub/Sub。

‍

## 1、Redis key(键)

### keys *

```sh
> keys * # 查看当前库所有 key(键)
name
```

### exists

```sh
> exists name # 判断某个 key(键) 是否存在
1
```

### type

```sh
> type name # 查看 key(键) 是什么类型
string
```

### del

```sh
> del name # 删除指定的 key(键) 数据
1
```

### unlink

```sh
> unlink name # 根据 value(值) 选择非阻塞删除
1
```

### expire / ttl

```sh
> expire name 10 # 10秒钟：为给定的 key(键) 设置过期时间
1
> ttl name # 查看还有多少秒过期，-1表示永不过期，-2表示已过期
0
```

### ping / exit

```sh
> ping 
PONG
```

### flushdb / flushall

```sh
> flushdb # 清空当前数据库
> flushall # 通杀全部库
```

### select

```sh
> select 3 # 切换数据库
```

### dbsize

```sh
> dbsize # 查看当前数据库的 key(键) 的数量
```

## 2、Redis String(字符串)

### 2.1、简介

- String 是 Redis 最基本的类型，你可以理解成与 Memcached 一模一样的类型，一个 key(键) 对应一个 value 。
- String 类型是**二进制安全的**。意味着 Redis 的 string 可以包含任何数据。比如 jpg 图片或者序列化的对象。
- String 类型是 Redis 最基本的数据类型，一个 Redis 中字符串 value 最多可以是**512M**。

### 2.2、数据结构

String的数据结构为简单动态字符串(Simple Dynamic String,缩写SDS)。是可以修改的字符串，内部结构实现上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配。

![image-20221210004617257](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210004617257-20230113215812-ves6qz1.png)

如图中所示，内部为当前字符串实际分配的空间能力 capacity(能力) 一般要高于实际字符串 len(长度) 。当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容时一次只会多扩1M的空间。需要注意的是字符串最大长度为512M。

### 2.3、常用命令

#### set / get / move / mset / mget / getset / setex / setnx / msetnx

```sh
> set name liuzonglin # 添加键值对
OK
> get name # 查询对应键值
"liuzonglin"
> move name 1 # 移除 key
0
> mset h1 1 h2 2 h3 3 h4 4 h5 5 # 一次性设置 key value 对
OK
> keys *
1) "h1"
2) "h2"
3) "h3"
4) "h4"
5) "h5"
> mget h1 h2 h3 h4 h5 # 同时获取一个或多个 value  
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
> mset user:1:name liuzonglin user:1:age 18 user:1:pd 123 # 设置user:1 的参数{name, age, pd}
OK
> mget user:1:name user:1:age user:1:pd
1) "liuzonglin"
2) "18"
3) "123"
> keys *
1) "user:1:age"
2) "user:1:name"
3) "user:1:pd"
> keys *
(empty array)
> getset he hello # 先获取在设置，不存在创建，存在返回原有值，在设置新的值
(nil)
> get he
"hello"
> keys * # 获取所有 key
1) "name"
> setex he 30 "hello" # 设置过期时间，存在直接替换
OK
> get he
"hello"
> setex he 30 "hell"
OK
> get he
"hell"
> ttl he
-2
> get he
(nil)
> setnx re redis # 只有在 key 不存在时    设置 key 的值
1
> keys *
1) "re"
2) "views"
3) "name"
> setnx re redis
0
> get re
"redis"
>
> msetnx h5 5 h6 6 h7 7 # 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在
0
> keys *
1) "h1"
2) "h2"
3) "h3"
4) "h4"
5) "h5"
```

#### incr / decr / incrby

```sh
> set views 0
OK
> incr views # 将 key 中储存的数字值增1 ;只能对数字值操作，如果为空，新增值为1
1
> incr views
2
> incr views
3
> get views
"3"
> decr views # 将 key 中储存的数字值减1; 只能对数字值操作，如果为空，新增值为-1
2
> get views
"2"
> incrby views 5 # 将 key 中储存的数字值增减。自定义步长。
7
> incrby views 5
12
```

#### append

```sh
> set name liuzonglin
OK
> append name hello # 字符串追加，如果字符串不存在，就相当于 set key
15 # 15 字符串长度
> get name
"liuzonglinhello"
```

#### strlen

```sh
> strlen name # 获得 value 的长度
15
```

#### getrange

```sh
> set he 'hello, world'
OK
> get he
"hello, world"
> getrange he 0 3 # 字符串截取下标[0-3]
"hell"
> getrange he 0 -1 # 查看全部字符串
"hello, world"
> get he
"hello, world"
> setrange he 1 xxx # 替换指定位置的字符串，1 为下标，xxx 为替换位数
12
> get he
"hxxxo, world"
```

#### exists

```sh
> exists name # 判断 key 是否存在
1 # 存在
> flushdb # 清空库
OK
> exists name
0 # 不存在
>
```

---

## 3、Redis 列表(List)

### 3.1、简介

单键多值：

- Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
- 它的底层实际是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差。

![image-20221210005138481](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210005138481-20230113215812-hiz34fc.png)

### 3.2、数据结构

- List(列表) 的数据结构为 quickList(快速链表)。
- 首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是 ziplist(压缩列表)，也即是压缩列表。
- 它将所有的元素紧挨着一起存储，分配的是一块连续的内存。
- 当数据量比较多的时候才会改成 quicklist(快速列表)。
- 因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里存的只是int类型的数据，结构上还需要两个额外的指针 prev(上一个) 和 next(下一个) 。

![image-20221210005736431](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210005736431-20230113215812-00v5to8.png)

Redis将链表和ziplist结合起来组成了quicklist。也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余。

### 3.3、常用命令

#### lpush / lrange / rpush / lpop / rpop / lindex / llen / lrem / ltrim

```sh
> lpush list one # 从左边/右边将一个或多个值插入到列表头部
1
> lpush list two # 将一个或多个值插入到列表头部
2
> lpush list three # 数据存储类似进栈出栈（头部）
3
> lrange list 0 -1 # 获取 list 中值
1) "three"
2) "two"
3) "one"
> lrange list 0 1 # 0左边第一个，-1右边第一个，（0-1表示获取所有）
1) "three"
2) "two"
> rpush list right # 在列表中添加一个或多个值
4
> lpop list # 移除左侧头部一位信息
"three"
> lrange list 0 -1
1) "two"
2) "one"
3) "right"
> rpop list # 移除右侧尾部一位信息
"right"
> lrange list 0 -1
1) "two"
2) "one"
> lindex list 0 # 通过下标获取值
"two"
> lindex list 1
"one"
> llen list # list 长度
2
> keys *
(empty array)
> lpush list one
1
> lpush list two
2
> lpush list three
3
> lpush list right
4
> lrange list 0 -1
1) "right"
2) "three"
3) "two"
4) "one"
> lrem list 1 one
1
> lrange list 0 -1
1) "right"
2) "three"
3) "two"
> lpush list three
4
> lrange list 0 -1
1) "three"
2) "right"
3) "three"
4) "two"
> lpush list three
5
> lrange list 0 -1
1) "three"
2) "three"
3) "right"
4) "three"
5) "two"
> lrem list 2 three # 移除指定个数的value 
2
> lrange list 0 -1
1) "right"
2) "three"
3) "two"
> lpush list hello
1
> lpush list hell1
2
> lpush list hell2
3
> lpush list hell3
4

```

#### ltrim

```sh
> ltrim list 1 2 # 截取
OK
> lrange list 0 -1
1) "hell2"
2) "hell1"
> flushdb
OK
> lpush list 1
1
> lpush list 2
2
> lpush list 3
3
> lpush list 4
4
> lpush list 5
5
> lpush list 6
6
> lrange list 0 -1
1) "6"
2) "5"
3) "4"
4) "3"
5) "2"
6) "1"
> ltrim list 1 2 # 截取list[1 -> 2] 内部参数
OK
> lrange list 0 -1
1) "5"
2) "4"
```

#### rpoplpush

```sh
> flushdb
OK
> rpush list 1
1
> rpush list 2
2
> rpush list 3
3
> rpush list 4
4
> rpush list 5
5
> rpush list 6
6
> lrange list 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
> rpush li I
1
> rpush li II
2
> rpush li III
3
> lrange li 0 -1
1) "I"
2) "II"
3) "III"
> rpoplpush list li # 把 list 末尾值移动到新的列表中
"6"
> lrange li 0 -1
1) "6"
2) "I"
3) "II"
4) "III"
> lrange list 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
```

#### lset

```sh
> exists list # 判断列表是否存在
0
> lpush list 1
1
> lset list 0 2 # 替换，前提 value 存在
OK
> lrange list 0 -1
1) "2"
>
> lrange list 0 -1
1) "6"
2) "5"
3) "4"
4) "3"
5) "2"
6) "1"
```

#### linsert

```sh
> linsert list before 1 liuz # 像指定位置前插入
7
> lrange list 0 -1
1) "6"
2) "5"
3) "4"
4) "3"
5) "2"
6) "liuz"
7) "1"
> linsert list after 1 liuz # 向自动位置后面插入
8
> lrange list 0 -1
1) "6"
2) "5"
3) "4"
4) "3"
5) "2"
6) "liuz"
7) "1"
8) "liuz"
```

## 4、Redis 集合(Set)

### 4.1、简介

- Redis set对外提供的功能与list类似是一个列表的功能，特殊之处在于set是可以**自动排重**的，当你需要存储一个列表数据，**又不希望出现重复数据时**，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。
- Redis的Set是string类型的无序集合。它底层其实是一个value为null的hash表，所以添加，删除，查找的**复杂度都是O(1)**。
- 一个算法，随着数据的增加，执行时间的长短，如果是O(1)，数据增加，查找数据的时间不变

### 4.2、数据结构

- Set 数据结构是 dict(字典)，字典是用哈希表实现的。
- Java 中 HashSet 的内部实现使用的是 HashMap，只不过所有的 value 都指向同一个对象。Redis 的 set 结构也是一样，它的内部也使用 hash 结构，所有的 value 都指向同一个内部值。

### 4.3、常用命令

#### sadd / smembers / scard / srem / srandmember / spop

```sh
> sadd set hello liu zong lin # 向集合添加一个或多个成员
4
> smembers set # 返回集合中的所有成员
1) "zong"
2) "liu"
3) "hello"
4) "lin"
> scard set # 查看key有几个元素
4
> srem set hello # 移除某个元素
1
> smembers set
1) "zong"
2) "liu"
3) "lin"
> srandmember set 1 # 随机从该集合中取出n个值。不会从集合中删除 。
1) "liu"
> spop set 1 # 随机移除一个元素
1) "lin"
```

#### sismember

```sh
> sismember set hello # 判断 hello 是否存在 有1，没有0
1
> sismember set set
0
```

#### smove

```sh
redis 127.0.0.1:6379> SADD myset1 "hello"
(integer) 1
redis 127.0.0.1:6379> SADD myset1 "world"
(integer) 1
redis 127.0.0.1:6379> SADD myset1 "bar"
(integer) 1
redis 127.0.0.1:6379> SADD myset2 "foo"
(integer) 1
redis 127.0.0.1:6379> SMOVE myset1 myset2 "bar" # 把集合中一个值从一个集合移动到另一个集合
(integer) 1
redis 127.0.0.1:6379> SMEMBERS myset1
1) "World"
2) "Hello"
redis 127.0.0.1:6379> SMEMBERS myset2
1) "foo"
2) "bar"
```

#### sinter / sunion / sdiff

```sh
> sinter <key1> <key2> # 返回两个集合的交集元素。
> sunion <key1> <key2> # 返回两个集合的并集元素。
> sdiff <key1> <key2>  # 返回两个集合的差集元素(key1中的，不包含key2中的)
```

## 5、Redis哈希(Hash)

### 5.1、介绍

- Redis hash 是一个键值对集合。
- Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。类似 Java 里面的 Map<String,Object>
- 用户 ID 为查找的 key，存储的 value 用户对象包含姓名，年龄，生日等信息，如果用普通的 key/value 结构来存储

主要有以下2种存储方式：

|![image-20221210013016325](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210013016325-20230113215812-4d9t50o.png)|每次修改用户的某个属性需要，先反序列化改好后再序列化回去。开销较大。|
| :-----------------------------------------------------------------------------------: | :-------------------------------------------------------------------|
|![image-20221210013035918](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210013035918-20230113215812-5zh9iqh.png)|**用户ID数据冗余**|
|![image-20221210013059835](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210013059835-20230113215812-yf7jzq3.png)|**通过 key(用户ID) + field(属性标签) 就可以操作对应属性数据了，既不需要重复存储数据，也不会带来序列化和并发修改控制的问题**|

### 5.2、数据结构

Hash 类型对应的数据结构是两种：

- ziplist(压缩列表)
- hashtable(哈希表)

当 field-value(字段值) 长度较短且个数较少时，使用 ziplist(压缩列表)，否则使用 hashtable(哈希表)。

### 5.3、常用指令

#### hset / hget / hmset / hexists

```sh
hset <key><field><value> # 给<key>集合中的  <field>键赋值<value>
hget <key1><field> # 从<key1>集合<field>取出 value 
hmset <key1><field1><value1><field2><value2>... # 批量设置hash的值
hexists<key1><field> # 查看哈希表 key 中，给定域 field 是否存在。 
hkeys <key> # 列出该hash集合的所有field
hvals <key> # 列出该hash集合的所有value
hincrby <key><field><increment> # 为哈希表 key 中的域 field 的值加上增量 1   -1
hsetnx <key><field><value> # 将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在 .
```

## 6、Redis  Zset(有序集合)

### 6.1、简介

Redis **Zset(有序集合)**  与 **set(普通集合)** 非常相似，是一个没有重复元素的字符串集合。

不同之处是 **Zset(有序集合)** 的每个成员都关联了一个 **score(评分)** ,这个 **score(评分)** 被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可以是重复了 。

因为元素是有序的, 所以你也可以很快的根据 **score(评分)** 或者 **position(次序)** 来获取一个范围的元素。

访问有序集合的中间元素也是非常快的, 因此你能够使用有序集合作为一个没有重复成员的智能列表。

### 6.2、数据结构

Zset(有序集合) 是 Redis 提供的一个非常特别的数据结构，一方面它等价于 Java 的数据结构 `Map<String, Double>` ，可以给每一个元素 value(值) 赋予一个权重score(评分)，另一方面它又类似于`TreeSet`，内部的元素会按照权重 score(评分) 进行排序，可以得到每个元素的名次，还可以通过 score(评分) 的范围来获取元素的列表。

zset底层使用了两个数据结构：

1. hash，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值。
2. 跳跃表，跳跃表的目的在于给元素value排序，根据score的范围获取元素列表。

### 6.3、跳跃表

#### 6.3.1、简介

有序集合在生活中比较常见，例如根据成绩对学生排名，根据得分对玩家排名等。对于有序集合的底层实现，可以用数组、平衡树、链表等。数组不便元素的插入、删除；平衡树或红黑树虽然效率高但结构复杂；链表查询需要遍历所有效率低。Redis采用的是跳跃表。跳跃表效率堪比红黑树，实现远比红黑树简单。

#### 6.3.2、实例

对比有序链表和跳跃表，从链表中查询出51

##### 1. 有序链表

![image-20221210145708847](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210145708847-20230113215812-jspuhes.png)

要查找值为51的元素，需要从第一个元素开始依次查找、比较才能找到。共需要6次比较。

##### 2. 跳跃表?

![image-20221210145741519](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210145741519-20230113215812-26l7jvf.png)

从第2层开始，1节点比51节点小，向后比较。

21节点比51节点小，继续向后比较，后面就是NULL了，所以从21节点向下到第1层

在第1层，41节点比51节点小，继续向后，61节点比51节点大，所以从41向下

在第0层，51节点为要查找的节点，节点被找到，共查找4次。

从此可以看出跳跃表比有序链表效率要高

### 6.4、常用指令

```sh
zadd  <key><score1><value1><score2><value2>… # 将一个或多个 member 元素及其 score 值加入到有序集 key 当中。
zrange <key><start><stop>  [WITHSCORES]   # 返回有序集 key 中，下标在<start><stop>之间的元素; 带WITHSCORES，可以让分数一起和值返回到结果集。
zrangebyscore key minmax [withscores] [limit offset count]# 返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。 
zrevrangebyscore key maxmin [withscores] [limit offset count]               # 同上，改为从大到小排列。 
zincrby <key><increment><value>      # 为元素的score加上增量
zrem  <key><value> # 删除该集合下，指定值的元素 
zcount <key><min><max> # 统计该集合，分数区间内的元素个数 
zrank <key><value> # 返回该值在集合中的排名，从0开始。
```

## redis 性能测试

```sh
[root@localhost ~]# /usr/local/bin/redis-benchmark -h 127.0.0.1 -p 6379 -c 100 -n 100000 # redis 性能测试

====== SET ======
100000 requests completed in 0.65 seconds # 对我们的10万个请求进行写入测试
100 parallel clients # 100 个并发客户端
3 bytes payload # 每次写入3个字节
keep alive: 1 # 只有一台服务器处理请求，单机性能
host configuration "save": 3600 1 300 100 60 10000
host configuration "appendonly": no
multi-thread: no

Latency by percentile distribution:
0.000% <= 0.071 milliseconds (cumulative count 2)
50.000% <= 0.319 milliseconds (cumulative count 50028)
75.000% <= 0.375 milliseconds (cumulative count 78160)
87.500% <= 0.407 milliseconds (cumulative count 89038)
93.750% <= 0.439 milliseconds (cumulative count 93779)
96.875% <= 0.495 milliseconds (cumulative count 97049)
98.438% <= 0.551 milliseconds (cumulative count 98537)
99.219% <= 0.607 milliseconds (cumulative count 99246)
99.609% <= 0.671 milliseconds (cumulative count 99621)
99.805% <= 0.767 milliseconds (cumulative count 99822)
99.902% <= 0.839 milliseconds (cumulative count 99908)
99.951% <= 0.879 milliseconds (cumulative count 99958)
99.976% <= 0.911 milliseconds (cumulative count 99979)
99.988% <= 0.927 milliseconds (cumulative count 99988)
99.994% <= 0.967 milliseconds (cumulative count 99995)
99.997% <= 0.983 milliseconds (cumulative count 99998)
99.998% <= 0.991 milliseconds (cumulative count 100000)
100.000% <= 0.991 milliseconds (cumulative count 100000)

Cumulative distribution of latencies:
0.010% <= 0.103 milliseconds (cumulative count 10)
0.036% <= 0.207 milliseconds (cumulative count 36)
30.447% <= 0.303 milliseconds (cumulative count 30447)
89.038% <= 0.407 milliseconds (cumulative count 89038)
97.349% <= 0.503 milliseconds (cumulative count 97349)
99.246% <= 0.607 milliseconds (cumulative count 99246)
99.716% <= 0.703 milliseconds (cumulative count 99716)
99.867% <= 0.807 milliseconds (cumulative count 99867)
99.974% <= 0.903 milliseconds (cumulative count 99974)
100.000% <= 1.007 milliseconds (cumulative count 100000)

Summary:
throughput summary: 154559.50 requests per second
latency summary (msec):
avg       min       p50       p95       p99       max
0.341     0.064     0.319     0.455     0.591     0.991

[root@localhost ~]#
```

‍

‍

## 5、Redis 的发布和订阅

### 5.1、什么是发布和订阅

Redis 发布/订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。

Redis 客户端可以订阅任意数量的频道。

### 5.2、Redis的发布和订阅

1. 客户端可以订阅频道

![image-20221210154402082](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210154402082-20230113215812-bxcvo6d.png)​

2. 当给这个频道发布消息后，消息就会发送给订阅的客户端

![image-20221210154426333](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210154426333-20230113215812-aqzbvyq.png)​

### 5.3、发布订阅命令行实现

1. 打开一个客户端订阅channel1

```sh
> SUBSCRIBE channel1
```

2. 打开另一个客户端，给channel1发布消息hello

```sh
> publish channel1 hello
1 # 返回的1是订阅者数量
```

3. 打开第一个客户端可以看到发送的消息

注：发布的消息没有持久化，如果在订阅的客户端收不到hello，只能收到订阅后发布的消息

## 6、Redis 新数据类型(?)

### 6.1、Bitmaps

#### 6.1.1、简介

现代计算机用二进制（位） 作为信息的基础单位， 1个字节等于8位， 例如“abc”字符串是由3个字节组成， 但实际在计算机存储时将其用二进制表示， “abc”分别对应的ASCII码分别是97、 98、 99， 对应的二进制分别是01100001、 01100010和01100011，如下图：

![image-20221210161323060](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210161323060-20230113215812-rcxvnvx.png)​

合理地使用操作位能够有效地提高内存使用率和开发效率。

Redis提供了Bitmaps这个“数据类型”可以实现对位的操作：

1. Bitmaps本身不是一种数据类型， 实际上它就是字符串（key-value） ， 但是它可以对字符串的位进行操作。
2. Bitmaps单独提供了一套命令， 所以在Redis中使用Bitmaps和使用字符串的方法不太相同。 可以把Bitmaps想象成一个以位为单位的数组， 数组的每个单元只能存储0和1， 数组的下标在Bitmaps中叫做偏移量。

![image-20221210161444101](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210161444101-20230113215812-j63c9e8.png)​

#### 6.1.2、Bitmaps与set对比

#### 6.1.3、命令

##### setbit

```sh
> setbit<key><offset><value> # 设置Bitmaps中某个偏移量的值（0或1）
```

offset:偏移量从0开始

实例

每个独立用户是否访问过网站存放在Bitmaps中， 将访问的用户记做1， 没有访问的用户记做0， 用偏移量作为用户的id。

设置键的第offset个位的值（从0算起） ， 假设现在有20个用户，userid=1， 6， 11， 15， 19的用户对网站进行了访问， 那么当前Bitmaps初始化结果如图

![image-20221210162009627](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210162009627-20230113215812-983121p.png)​

unique:users:20201106代表2020-11-06这天的独立访问用户的Bitmaps

![image-20221210162042084](assets/Redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/image-20221210162042084-20230113215812-0g0a2fn.png)​

很多应用的用户id以一个指定数字（例如10000） 开头， 直接将用户id和Bitmaps的偏移量对应势必会造成一定的浪费， 通常的做法是每次做setbit操作时将用户id减去这个指定数字。

在第一次初始化Bitmaps时， 假如偏移量非常大， 那么整个初始化过程执行会比较慢， 可能会造成Redis的阻塞。

### 6.2、HyperLogLog

### 6.3、Geospatial

‍

‍
