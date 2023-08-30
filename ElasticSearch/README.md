# ElasticSearch

# 第1章 ElasticSearch概述

## 1.1 什么是搜索？

百度：我们比如说想找寻任何的信息的时候，就会上百度去搜索一下，比如说找一部自己喜欢的电影，或者说找一本喜欢的书，或者找一条感兴趣的新闻（提到搜索的第一印象）。百度 != 搜索

1）互联网的搜索：电商网站，招聘网站，新闻网站，各种app

2）IT系统的搜索：OA软件，办公自动化软件，会议管理，日程管理，项目管理。

搜索，就是在任何场景下，找寻你想要的信息，这个时候，会输入一段你要搜索的关键字，然后就期望找到这个关键字相关的一些信息。

## 1.2 如果用数据库做搜索会怎么样？

用数据库来实现搜索，是不太靠谱的。通常来说，性能会很差的。

## 1.3 什么是全文检索和Lucene？

1）全文检索，倒排索引

[全文检索](https://baike.baidu.com/item/%E5%85%A8%E6%96%87%E6%A3%80%E7%B4%A2/8028630) 是指计算机索引程序通过扫描文章中的每一个词，对每一个词建立一个索引，指明该词在文章中出现的次数和位置，当用户查询时，检索程序就根据事先建立的索引进行查找，并将查找的结果反馈给用户的检索方式。这个过程类似于通过字典中的检索字表查字的过程。全文搜索搜索引擎数据库中的数据。

​![image](assets/image-20230211193123-vtfx44s.png)​

2）lucene，就是一个jar包，里面包含了封装好的各种建立倒排索引，以及进行搜索的代码，包括各种算法。我们就用java开发的时候，引入lucene jar，然后基于lucene的api进行去进行开发就可以了。

### 1.3.1 Lucene 倒排索引结构

可以看到Lucene 为倒排索引(Term Dictionary)部分又增加一层Term Index结构，用于快速定位，而这Term Index是缓存在内存中的，但MySQL的B+tree不在内存中，所以整体来看ES速度更快，但同时也更消耗资源（内存、磁盘）。

### 1.3.2 B+Tree

## 1.4 什么是 ElasticSearch？

Elasticsearch，基于Lucene，隐藏复杂性，提供简单易用的RestfulAPI接口、JavaAPI接口（还有其他语言的API接口）。

关于Elasticsearch的一个传说，有一个程序员失业了，陪着自己老婆去英国伦敦学习厨师课程。程序员在失业期间想给老婆写一个菜谱搜索引擎，觉得Lucene实在太复杂了，就开发了一个封装了Lucene的开源项目：Compass。后来程序员找到了工作，是做分布式的高性能项目的，觉得Compass不够，就写了Elasticsearch，让Lucene变成分布式的系统。

Elasticsearch是一个实时分布式搜索和分析引擎。它用于全文搜索、结构化搜索、分析。

全文检索：将非结构化数据中的一部分信息提取出来,重新组织,使其变得有一定结构,然后对此有一定结构的数据进行搜索,从而达到搜索相对较快的目的。

结构化检索：我想搜索商品分类为日化用品的商品都有哪些，select * from products where category_id='日化用品'。

数据分析：电商网站，最近7天牙膏这种商品销量排名前10的商家有哪些；新闻网站，最近1个月访问量排名前3的新闻版块是哪些。

## 1.5 ElasticSearch 的适用场景

1）维基百科，类似百度百科，牙膏，牙膏的维基百科，全文检索，高亮，搜索推荐。

2）The Guardian（国外新闻网站），类似搜狐新闻，用户行为日志（点击，浏览，收藏，评论）+ 社交网络数据（对某某新闻的相关看法），数据分析，给到每篇新闻文章的作者，让他知道他的文章的公众反馈（好，坏，热门，垃圾，鄙视，崇拜）。

3）Stack Overflow（国外的程序异常讨论论坛），IT问题，程序的报错，提交上去，有人会跟你讨论和回答，全文检索，搜索相关问题和答案，程序报错了，就会将报错信息粘贴到里面去，搜索有没有对应的答案。

4）GitHub（开源代码管理），搜索上千亿行代码。

5）国内：站内搜索（电商，招聘，门户，等等），IT系统搜索（OA，CRM，ERP，等等），数据分析（ES热门的一个使用场景）。

## 1.6 Elasticsearch的特点

1）天然分片，天然集群

ES把数据分成多个shard，下图中的P0-P2，多个shard可以组成一份完整的数据，这些shard可以分布在集群中的各个机器节点中。随着数据的不断增加，集群可以增加多个分片，把多个分片放到多个机子上，已达到负载均衡，横向扩展。

​![](assets/clip_image002-20230211193350-hvt8as1.jpg)​

2）Elasticsearch不是什么新技术，主要是将全文检索、数据分析以及分布式技术，合并在了一起，才形成了独一无二的ES；lucene（全文检索），商用的数据分析软件（也是有的），分布式数据库（mycat）

3）对用户而言，是开箱即用的，非常简单，作为中小型的应用，直接3分钟部署一下ES，就可以作为生产环境的系统来使用了，数据量不大，操作不是太复杂

4）数据库的功能面对很多领域是不够用的（事务，还有各种联机事务型的操作）；特殊的功能，比如全文检索，同义词处理，相关度排名，复杂数据分析，海量数据的近实时处理；Elasticsearch作为传统数据库的一个补充，提供了数据库所不能提供的很多功能

## 1.7 Elasticsearch的核心概念

### 1.7.1 近实时

近实时，两个意思，从写入数据到数据可以被搜索到有一个小延迟（大概1秒）；基于es执行搜索和分析可以达到秒级。

### 1.7.2 Cluster（集群）

集群包含多个节点，每个节点属于哪个集群是通过一个配置（集群名称，默认是elasticsearch）来决定的，对于中小型应用来说，刚开始一个集群就一个节点很正常

### 1.7.3 Node（节点）

集群中的一个节点，节点也有一个名称（默认是随机分配的），节点名称很重要（在执行运维管理操作的时候），默认节点会去加入一个名称为“elasticsearch”的集群，如果直接启动一堆节点，那么它们会自动组成一个elasticsearch集群，当然一个节点也可以组成一个elasticsearch集群。

### 1.7.4 Index（索引-数据库）

索引包含一堆有相似结构的文档数据，比如可以有一个客户索引，商品分类索引，订单索引，索引有一个名称。一个index包含很多document，一个index就代表了一类类似的或者相同的document。比如说建立一个product index，商品索引，里面可能就存放了所有的商品数据，所有的商品document。

### 1.7.5 Type（类型-表）

6.0版本之前每个索引里都可以有多个type；

6.0版本之后每个索引里面只能有一个Type，一般使用_doc代替了。

商品index，里面存放了所有的商品数据，商品document

商品type：product_id，product_name，product_desc，category_id，category_name，service_period

每一个type里面，都会包含一堆document

```json
{
  "product_id": "1",
  "product_name": "长虹电视机",
  "product_desc": "4k高清",
  "category_id": "3",
  "category_name": "电器",
  "service_period": "1年"
}

```

```json
{
  "product_id": "2",
  "product_name": "基围虾",
  "product_desc": "纯天然，冰岛产",
  "category_id": "4",
  "category_name": "生鲜",
  "eat_period": "7天"
}

```

### 1.7.6 Document（文档-行）

文档是ES中的最小数据单元，一个document可以是一条客户数据，一条商品分类数据，一条订单数据，通常用JSON数据结构表示，每个index下的type中，都可以去存储多个document。

### 1.7.7 Field（字段-列）

Field是Elasticsearch的最小单位。一个document里面有多个field，每个field就是一个数据字段。

```json
product document
{
  "product_id": "1",
  "product_name": "高露洁牙膏",
  "product_desc": "高效美白",
  "category_id": "2",
  "category_name": "日化用品"
}

```

### 1.7.8 Mapping（映射-约束）

数据如何存放到索引对象上，需要有一个映射配置，包括：数据类型、是否存储、是否分词等。

Mapping用来定义Document中每个字段的类型，即所使用的分词器、是否索引等属性，非常关键等。创建Mapping 的代码示例如下：

```json
PUT student(index_name->database)
{
  "mappings": {
    "_doc":{(type_name->table)
      "properties":{
        "stu_id":{(field_name->colume)
          "type":"keyword",
          "store":"true"
        },
        "name":{(field_name->colume)
          "type":"keyword"
        },
        "birth":{(field_name->colume)
          "type":"date"(yyyy-MM-dd HH:mm)
        }
      }
    }
  }
}

```

### 1.7.9 ElasticSearch与数据库的类比

|关系型数据库（比如Mysql）|非关系型数据库（Elasticsearch）|
| ---------------------------| ----------------------------------------------------------------------|
|数据库Database|索引Index|
|表Table|类型Type(6.0版本之后在一个索引下面只能有一个，7.0版本之后取消了Type)|
|数据行Row|文档Document(JSON格式)|
|数据列Column|字段Field|
|约束Schema|映射Mapping|

### 1.7.10 ElasticSearch存入数据和搜索数据机制

1）索引对象（blog）：存储数据的表结构，任何搜索数据，存放在索引对象上 。

2）映射（mapping）：数据如何存放到索引对象上，需要有一个映射配置， 包括：数据类型、是否存储、是否分词等。

3）文档（document）：一条数据记录，存在索引对象上。

4）文档类型（type）：一个索引对象，存放多种类型数据，数据用文档类型进行标识。

# 第2章 Elasticsearch快速入门

## 2.1 安装包下载

1）ElasticSearch官网：[https://www.elastic.co/cn/downloads/elasticsearch](https://www.elastic.co/cn/downloads/elasticsearch)

​![](assets/clip_image002-20230211193608-e2fgr0b.png)​

## 2.2 Elasticsearch安装

### 2.2.1 解压安装ElasticSearch

1）解压elasticsearch-6.6.0.tar.gz到/opt/module目录下

```shell
[atguigu@hadoop102 software]$ tar -zxvf elasticsearch-6.6.0.tar.gz -C /opt/module/
[atguigu@hadoop102 module]$ mv elasticsearch-6.6.0 elasticsearch

```

2）在/opt/module/elasticsearch路径下创建data文件夹

```shell
[atguigu@hadoop102 elasticsearch]$ mkdir data
```

3）修改配置文件/opt/module/elasticsearch/config/elasticsearch.yml

```yml
[atguigu@hadoop102 config]$ pwd
/opt/module/elasticsearch/config
[atguigu@hadoop102 config]$ vi elasticsearch.yml

#-----------------------Cluster-----------------------
cluster.name: my-application
#-----------------------Node-----------------------
node.name: node-102
#-----------------------Paths-----------------------
path.data: /opt/module/elasticsearch/data
path.logs: /opt/module/elasticsearch/logs
#-----------------------Memory-----------------------
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
#-----------------------Network-----------------------
network.host: 192.168.9.102
#-----------------------Discovery-----------------------
discovery.zen.ping.unicast.hosts: ["192.168.9.102"]

```

（1）cluster.name

如果要配置集群需要两个节点上的elasticsearch配置的cluster.name相同，都启动可以自动组成集群，这里如果不改cluster.name则默认是cluster.name=my-application，

（2）nodename随意取但是集群内的各节点不能相同

（3）修改后的每行前面不能有空格，修改后的“：”后面必须有一个空格

（4）修改配置文件/opt/module/elasticsearch/config/jvm.options

```yml
修改
-Xms1g
-Xmx1g
为
-Xms256m
-Xmx256m

```

分发至hadoop103以及hadoop104，分发之后修改：

```shell
[atguigu@hadoop102 module]$ xsync elasticsearch/

node.name: node-103
network.host: 192.168.9.103

node.name: node-104
network.host: 192.168.9.104

```

### 2.2.2 配置linux系统环境

（参考：http://blog.csdn.net/satiling/article/details/59697916）

1）切换到root用户，编辑limits.conf 添加类似如下内容

```shell
[root@hadoop105 elasticsearch]# vi /etc/security/limits.conf
```

添加如下内容:

```shell
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096

```

注意：“*” 不要省略掉

2）切换到root用户，进入limits.d目录下修改配置文件。（CentOS7.x不用改）1

```shell
[root@hadoop102 elasticsearch]# vi /etc/security/limits.d/90-nproc.conf
```

修改如下内容：

```shell
* soft nproc 1024
```

#修改为

```shell
* soft nproc 4096
```

3）切换到root用户修改配置sysctl.conf

​`[root@hadoop102 elasticsearch]# vi /etc/sysctl.conf`​

添加下面配置：

​`vm.max_map_count=655360`​

并执行命令：

​`[root@hadoop102 elasticsearch]# sysctl -p`​

以上修改的Linux配置需要分发至其他节点

```shell
[root@hadoop102 elasticsearch]# xsync /etc/security/limits.conf

[root@hadoop102 elasticsearch]# xsync /etc/security/limits.d/90-nproc.conf  #(Centos7.X不需要)

[root@hadoop102 elasticsearch]# xsync /etc/sysctl.conf

```

然后，重新启动Linux，必须重启！！！

4）启动Elasticsearch

​`[atguigu@hadoop102 elasticsearch]$ bin/elasticsearch`​

5）测试elasticsearch

```shell
[atguigu@hadoop102 elasticsearch]$ curl http://hadoop102:9200

{

  "name" : "node-102",

  "cluster_name" : "my-application",

  "cluster_uuid" : "w9UFspEFR4-F5gLrTYMHIw",

  "version" : {

    "number" : "6.6.0",

    "build_flavor" : "default",

    "build_type" : "tar",

    "build_hash" : "a9861f4",

    "build_date" : "2019-01-24T11:27:09.439740Z",

    "build_snapshot" : false,

    "lucene_version" : "7.6.0",

    "minimum_wire_compatibility_version" : "5.6.0",

    "minimum_index_compatibility_version" : "5.0.0"

  },

  "tagline" : "You Know, for Search"

}
```

8）停止集群

        `kill -9 进程号`​

9）群起脚本

```bash
[atguigu@hadoop102 bin]$ vi es.sh
#!/bin/bash
es_home=/opt/module/elasticsearch
case $1  in
 "start") {
  for i in hadoop102 hadoop103 hadoop104
  do
    echo "==============$i=============="
ssh $i  "source /etc/profile;${es_home}/bin/elasticsearch >/dev/null 2>&1 &"
sleep 4s;
  done

};;
"stop") {
  for i in hadoop102 hadoop103 hadoop104
  do
    echo "==============$i=============="
    ssh $i "ps -ef|grep $es_home |grep -v grep|awk '{print \$2}'|xargs kill" >/dev/null 2>&1
  done

};;
esac

```

## 2.3 Elasticsearch操作工具

### 2.3.1 浏览器

​![](assets/clip_image002-20230211194214-2m28cyn.jpg)​

### 2.3.2 Linux命令行

请求：

​`[atguigu@hadoop102 elasticsearch]$ curl -XPOST 'http://192.168.9.102:9200/atguigu/_doc' -i -H "Content-Type:application/json" -d '{"name":"haha","age":"10"}'`​

响应：

```bash
HTTP/1.1 201 Created
Location: /atguigu/_doc/1nZkMnMB7pjbwZuyA8Ge
Warning: 299 Elasticsearch-6.6.0-a9861f4 "the default number of shards will change from [5] to [1] in 7.0.0; if you wish to continue using the default of [5] shards, you must manage this on the create index request or with an index template" "Thu, 09 Jul 2020 07:04:41 GMT"
content-type: application/json; charset=UTF-8
content-length: 174

{"_index":"atguigu","_type":"_doc","_id":"1nZkMnMB7pjbwZuyA8Ge","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":0,"_primary_term":1}

```

### 2.3.3 Kibana的Dev Tools

​![](assets/clip_image002-20230211194309-ueq4w0h.jpg)​

# 第3章 ElasticSearch使用

## 3.1 数据类型

### 3.1.1 核心数据类型

字符串型：text(分词)、keyword(不分词)

数值型：long、integer、short、byte、double、float、half_float、scaled_float

日期类型：date

布尔类型：boolean

二进制类型：binary

范围类型：integer_range、float_range、long_range、double_range、date_range

### 3.1.2 复杂数据类型

数组类型：array

对象类型：object

嵌套类型：nested object

### 3.1.3 地理位置数据类型

geo_point(点)、geo_shape(形状)

### 3.1.4 专用类型

记录IP地址ip

实现自动补全completion

记录分词数：token_count

记录字符串hash值murmur3

多字段特性multi-fields

## 3.2 Mapping

### 3.2.1 手动创建

1）创建操作

```json
PUT my_index1
{
  "mappings": {
    "_doc":{
      "properties":{
        "username":{
          "type": "text"
        }
      }
    }
  }
}

```

2）创建文档

```json
PUT my_index1/_doc/1
{
  "username":"haha heihei"
}

```

3）查询

```json
GET my_index1/_search
{
  "query": {
    "match": {
      "username": "haha"
    }
  }
}

```

### 3.2.2 自动创建

ES可以自动识别文档字段类型，从而降低用户使用成本

1）直接插入文档

```json
PUT /test_index/_doc/1
{
  "username":"alfred",
  "age":1,
"birth":"1991-12-15"
}

```

2）查看mapping

```json
GET /test_index/doc/_mapping

{
  "test_index": {
    "mappings": {
      "doc": {
        "properties": {
          "age": {
            "type": "long"
          },
          "birth": {
            "type": "date"
          },
          "username": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }
        }
      }
    }
  }
}

```

age自动识别为long类型，username识别为text类型

3）日期类型的自动识别

日期的自动识别可以自行配置日期格式，以满足各种需求。

（1）自定义日期识别格式

```json
PUT my_index
{
  "mappings":{
    "_doc":{
      "dynamic_date_formats": ["yyyy-MM-dd","yyyy/MM/dd"]
    }
  }
}

```

（2）关闭日期自动识别

```json
PUT my_index
{
  "mappings": {
    "_doc": {
      "date_detection": false
    }
  }
}

```

4）字符串是数字时，默认不会自动识别为整形，因为字符串中出现数字时完全合理的

Numeric_datection可以开启字符串中数字的自动识别

```json
PUT my_index
{
  "mappings":{
    "_doc":{
      "numeric_datection": true
    }
  }
}

```

## 3.3 IK分词器

### 3.3.1 为什么使用分词器

分词器主要应用在中文上，在ES中字符串类型有keyword和text两种。keyword默认不进行分词，而text是将每一个汉字拆开称为独立的词，这两种都是不适用于生产环境，所以我们需要有其他的分词器帮助我们完成这些事情，其中IK分词器是应用最为广泛的一个分词器。

1）keyword类型的分词

```json
GET _analyze
{
  "keyword":"我是程序员"
}

```

结果展示（报错）

```json
{
  "error": {
    "root_cause": [
      {
        "type": "illegal_argument_exception",
        "reason": "Unknown parameter [keyword] in request body or parameter is of the wrong type[VALUE_STRING] "
      }
    ],
    "type": "illegal_argument_exception",
    "reason": "Unknown parameter [keyword] in request body or parameter is of the wrong type[VALUE_STRING] "
  },
  "status": 400
}

```

2）text类型的分词

```json
GET _analyze
{
  "text":"我是程序员"
}

```

结果展示：

```json
{
  "tokens": [
    {
      "token": "我",
      "start_offset": 0,
      "end_offset": 1,
      "type": "<IDEOGRAPHIC>",
      "position": 0
    },
    {
      "token": "是",
      "start_offset": 1,
      "end_offset": 2,
      "type": "<IDEOGRAPHIC>",
      "position": 1
    },
    {
      "token": "程",
      "start_offset": 2,
      "end_offset": 3,
      "type": "<IDEOGRAPHIC>",
      "position": 2
    },
    {
      "token": "序",
      "start_offset": 3,
      "end_offset": 4,
      "type": "<IDEOGRAPHIC>",
      "position": 3
    },
    {
      "token": "员",
      "start_offset": 4,
      "end_offset": 5,
      "type": "<IDEOGRAPHIC>",
      "position": 4
    }
  ]
}

```

### 3.3.2 IK分词器安装`

1）下载与安装的ES相对应的版本

2）在plugins目录下创建目录ik，之后解压elasticsearch-analysis-ik-6.6.0.zip至ik目录

```shell
[atguigu@hadoop102 plugins]$ mkdir ik
[atguigu@hadoop102 software]$ unzip elasticsearch-analysis-ik-6.6.0.zip -d /opt/module/elasticsearch-6.6.0/plugins/ik/

```

3）分发分词器目录

​`[atguigu@hadoop102 elasticsearch-6.6.0]$ xsync plugins/`​

4）重新启动Elasticsearch，即可加载IK分词器

### 3.3.3 IK分词器测试

IK提供了两个分词算法ik_smart 和 ik_max_word，其中 ik_smart 为最少切分，ik_max_word为最细粒度划分。

1）最少划分ik_smart

```json
get _analyze

{

  "analyzer": "ik_smart",

  "text":"我是程序员"

}
```

结果展示

```json
{
    "tokens" : [
        {
            "token" : "我",
            "start_offset" : 0,
            "end_offset" : 1,
            "type" : "CN_CHAR",
            "position" : 0
        },
        {
            "token" : "是",
            "start_offset" : 1,
            "end_offset" : 2,
            "type" : "CN_CHAR",
            "position" : 1
        },
        {
            "token" : "程序员",
            "start_offset" : 2,
            "end_offset" : 5,
            "type" : "CN_WORD",
            "position" : 2
        }
    ]
}

```

2）最细切分ik_max_word

```json
get _analyze
{
  "analyzer": "ik_max_word",
  "text":"我是程序员"
}

```

输出的结果为：

```json
{
    "tokens" : [
        {
            "token" : "我",
            "start_offset" : 0,
            "end_offset" : 1,
            "type" : "CN_CHAR",
            "position" : 0
        },
        {
            "token" : "是",
            "start_offset" : 1,
            "end_offset" : 2,
            "type" : "CN_CHAR",
            "position" : 1
        },
        {
            "token" : "程序员",
            "start_offset" : 2,
            "end_offset" : 5,
            "type" : "CN_WORD",
            "position" : 2
        },
        {
            "token" : "程序",
            "start_offset" : 2,
            "end_offset" : 4,
            "type" : "CN_WORD",
            "position" : 3
        },
        {
            "token" : "员",
            "start_offset" : 4,
            "end_offset" : 5,
            "type" : "CN_CHAR",
            "position" : 4
        }
    ]
}

```

## 3.4 检索文档

向Elasticsearch增加数据

```json
PUT /atguigu/doc/1
{
    "first_name" : "John",
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": ["sports", "music"]
}

```

如果在关系型数据库Mysql中主键查询数据一般会执行下面的SQL语句

​`select * from atguigu where id = 1;`​

但在Elasticsearch中需要采用特殊的方式

```json
# 协议方法 索引/类型/文档编号

GET /atguigu/doc/1
```

响应

```json
{
  "_index": "atguigu",
  "_type": "doc",
  "_id": "1",
  "_version": 1,
  "found": true,
  "_source": { // 文档的原始数据JSON数据
    "first_name": "John",
    "last_name": "Smith",
    "age": 25,
    "about": "I love to go rock climbing",
    "interests": [
      "sports",
      "music"
    ]
  }
}

```

我们通过HTTP方法GET来检索文档，同样的，我们可以使用DELETE方法删除文档，使用HEAD方法检查某文档是否存在。如果想更新已存在的文档，我们只需再PUT一次。

### 3.4.1 元数据查询

​`GET _cat/indices`​

​`GET _cat/indices?v`​

|health|green(集群完整) yellow(单点正常、集群不完整) red(单点不正常)|
| ----------------| --------------------------------------------------------------|
|status|是否能使用|
|index|索引名|
|uuid|索引统一编号|
|pri|主节点几个|
|rep|从节点几个|
|docs.count|文档数|
|docs.deleted|文档被删了多少|
|store.size|整体占空间大小|
|pri.store.size|主节点占空间大小|

### 3.4.2 全文档检索

如果在关系型数据库Mysql中查询所有数据一般会执行下面的SQL语句

​`select * from user;`​

但在Elasticsearch中需要采用特殊的方式

```json
# 协议方法 索引/类型/_search
GET /atguigu/_doc/_search

```

响应内容不仅会告诉我们哪些文档被匹配到，而且这些文档内容完整的被包含在其中—我们在给用户展示搜索结果时需要用到的所有信息都有了。

### 3.4.3 字段全值匹配检索

如果在关系型数据库Mysql中查询多字段匹配数据（字段检索）

一般会执行下面的SQL语句

​`select * from atguigu where name = 'haha';`​

但在Elasticsearch中需要采用特殊的方式

```json
GET atguigu/_search
{
  "query": {
    "bool": {
      "filter": {
        "term": {
          "about": "I love to go rock climbing"
        }
      }
    }
  }
}

```

### 3.4.4 字段分词匹配检索

```json
GET atguigu/_search
{
  "query": {
    "match": {
      "about": "I"
    }
  }
}

```

### 3.4.5 字段模糊匹配检索

如果在关系型数据库Mysql中模糊查询多字段数据

一般会执行下面的SQL语句

​`select * from user where name like '%haha%'`​

但在Elasticsearch中需要采用特殊的方式，查询出所有文档字段值分词后包含haha的文档

```json
GET  test/_search
{
  "query": {
    "fuzzy": {
      "aa": {
        "value": "我是程序"
      }
    }
  }
}

```

### 3.4.6 聚合检索

```json
GET test/_search
{
  "aggs": {
    "groupby_aa": {
      "terms": {
        "field": "aa",
        "size": 10
      }
    }
  }
}

```

### 3.4.7 分页检索

```json
GET movie_index/movie/_search
{
  "query": { "match_all": {} },
  "from": 1,
  "size": 1
}

```

## 3.5 索引别名 _aliases

索引别名就像一个快捷方式或软连接，可以指向一个或多个索引，也可以给任何一个需要索引名的API来使用。别名带给我们极大的灵活性，允许我们做下面这些：

1）给多个索引分组 (例如， last_three_months)

2）给索引的一个子集创建视图

3）在运行的集群中可以无缝的从一个索引切换到另一个索引

### 3.5.1 创建索引别名

1）建索引时直接声明

```json
PUT movie_chn_2020
{  "aliases": {
      "movie_chn_2020-query": {}
  },
  "mappings": {
    "movie":{
      "properties": {
        "id":{
          "type": "long"
        },
        "name":{
          "type": "text",
	  "analyzer": "ik_smart"
        },
        "doubanScore":{
          "type": "double"
        }
      }
    }
  }
}

```

为已存在的索引增加别名

```json
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "movie",
        "alias": "movie_query"
      }
    }
  ]
}

```

也可以通过加过滤条件缩小查询范围，建立一个子集视图

```json
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "movie",
        "alias": "movie_query",
        "filter": {
          "term": {
            "sex": "male"
          }
        }
      }
    }
  ]
}

```

### 3.5.2 查询别名

与使用普通索引没有区别

​`GET movie_chn_2020-query/_search`​

### 3.5.3 删除某个索引的别名

```json
POST _aliases
{
  "actions": [
    {
      "remove": {
        "index": "test1",
        "alias": "alias1"
      }
    }
  ]
}

```

### 3.5.4 为某个别名进行无缝切换

```json
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "test1",
        "alias": "alias1"
      }
    },
      "remove": {
        "index": "test1",
        "alias": "alias2"
    }
  ]
}

```

### 3.5.5 查询别名列表

​`GET  _cat/aliases?v`​

## 3.6 索引模板

Index Template 索引模板，顾名思义，就是创建索引的模具，其中可以定义一系列规则来帮助我们构建符合特定业务需求的索引的mappings和 settings，通过使用 Index Template 可以让我们的索引具备可预知的一致性。

### 3.6.1 常见的场景: 分割索引

     分割索引就是根据时间间隔把一个业务索引切分成多个索引。比如把order_info 变成 order_info_20200101,order_info_20200102 …..

这样做的好处有两个：

结构变化的灵活性：因为elasticsearch不允许对数据结构进行修改。但是实际使用中索引的结构和配置难免变化，那么只要对下一个间隔的索引进行修改，原来的索引位置原状。这样就有了一定的灵活性。

查询范围优化：因为一般情况并不会查询全部时间周期的数据，那么通过切分索引，物理上减少了扫描数据的范围，也是对性能的优化。

### 3.6.2 创建模板

```json
PUT _template/template_movie2020
{
  "index_patterns": ["movie_test*"],
  "settings": {
    "number_of_shards": 1
  },
  "aliases" : { 
    "{index}-query": {},
    "movie_test-query":{}
  },
  "mappings": { 
"_doc": {
      "properties": {
        "id": {
          "type": "keyword"
        },
        "movie_name": {
          "type": "text",
          "analyzer": "ik_smart"
        }
      }
    }
  }
}

```

其中"index_patterns": ["movie_test*"],  的含义就是凡是往movie_test开头的索引写入数据时，如果索引不存在，那么es会根据此模板自动建立索引。

在"aliases" 中用{index}表示，获得真正的创建的索引名。

测试

```json
POST movie_test_2020xxxx/_doc
{
  "id":"333",
  "name":"zhang3"
}

```

### 3.6.3 查看系统中已有的模板清单

​`GET  _cat/templates`​

### 3.6.4 查看某个模板详情

​`GET  _template/template_movie2020`​

或者

​`GET  _template/template_movie*`​

## 3.7 API操作

新建工程并导入依赖：

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.5</version>
</dependency>

<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpmime</artifactId>
    <version>4.3.6</version>
</dependency>

<dependency>
    <groupId>io.searchbox</groupId>
    <artifactId>jest</artifactId>
    <version>5.3.3</version>
</dependency>

<dependency>
    <groupId>net.java.dev.jna</groupId>
    <artifactId>jna</artifactId>
    <version>4.5.2</version>
</dependency>

<dependency>
    <groupId>org.codehaus.janino</groupId>
    <artifactId>commons-compiler</artifactId>
    <version>2.7.8</version>
</dependency>

<dependency>
<groupId>org.elasticsearch</groupId>
<artifactId>elasticsearch</artifactId>
<version>6.6.0</version>
</dependency>

```

### 3.7.1 写数据

```java
	//1.创建ES客户端构建器
        JestClientFactory factory = new JestClientFactory();

        //2.创建ES客户端连接地址
        HttpClientConfig httpClientConfig = new HttpClientConfig.Builder("http://hadoop102:9200").build();

        //3.设置ES连接地址
        factory.setHttpClientConfig(httpClientConfig);

        //4.获取ES客户端连接
        JestClient jestClient = factory.getObject();

        //5.构建ES插入数据对象
        Index index = new Index.Builder("{\n" +
                "  \"name\":\"zhangsan\",\n" +
                "  \"age\":17\n" +
                "}").index("test5").type("_doc").id("2").build();

        //6.执行插入数据操作
        jestClient.execute(index);

        //7.关闭连接
        jestClient.shutdownClient();

```

### 3.7.2 读数据

```java
	//1.创建ES客户端连接池
        JestClientFactory factory = new JestClientFactory();

        //2.创建ES客户端连接地址
        HttpClientConfig httpClientConfig = new HttpClientConfig.Builder("http://hadoop102:9200").build();

        //3.设置ES连接地址
        factory.setHttpClientConfig(httpClientConfig);

        //4.获取ES客户端连接
        JestClient jestClient = factory.getObject();

        //5.构建查询数据对象
        Search search = new Search.Builder("{\n" +
                "  \"query\": {\n" +
                "    \"match\": {\n" +
                "      \"name\": \"zhangsan\"\n" +
                "    }\n" +
                "  }\n" +
                "}").addIndex("test5").addType("_doc").build();

        //6.执行查询操作
        SearchResult searchResult = jestClient.execute(search);

        //7.解析查询结果
        System.out.println(searchResult.getTotal());
        List<SearchResult.Hit<Map, Void>> hits = searchResult.getHits(Map.class);
        for (SearchResult.Hit<Map, Void> hit : hits) {
            System.out.println(hit.index + "--" + hit.id);
        }

        //8.关闭连接
        jestClient.shutdownClient();

```

# 第4章 Kibana

Kibana的安装

将kibana压缩包上传到虚拟机指定目录

​`[atguigu@hadoop102 software]$ tar -zxvf kibana-6.6.0-linux-x86_64.tar.gz -C /opt/module/`​

修改相关配置，连接Elasticsearch

```yml
[atguigu@hadoop102 kibana]$ vi config/kibana.yml

# Kibana is served by a back end server. This setting specifies the port to use.
server.port: 5601
# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: "192.168.9.102"
... ...
... ...
# The URL of the Elasticsearch instance to use for all your queries.
elasticsearch.hosts: "http://192.168.9.102:9200"

```

启动Kibana

​`[atguigu@hadoop102 kibana]$ bin/kibana`​

​![](assets/clip_image002-20230211195808-lhmm8ps.jpg)​

修改之前的ES启动脚本为：

```bash
#!/bin/bash
es_home=/opt/module/elasticsearch
kibana_home=/opt/module/kibana
case $1  in
 "start") {
  for i in hadoop102 hadoop103 hadoop104
  do
    echo "==============$i=============="
ssh $i  "source /etc/profile;${es_home}/bin/elasticsearch >/dev/null 2>&1 &"
sleep 4s;
  done
  sleep 2s;
  nohup ${kibana_home}/bin/kibana > ${kibana_home}/kibana.log 2>&1 &
};;
"stop") {
  ps -ef | grep ${kibana_home} | grep -v grep | awk '{print \$2}'| xargs kill
  for i in hadoop102 hadoop103 hadoop104
  do
    echo "==============$i=============="
    ssh $i "ps -ef|grep $es_home |grep -v grep|awk '{print \$2}'|xargs kill" >/dev/null 2>&1
  done
};;
esac

```
