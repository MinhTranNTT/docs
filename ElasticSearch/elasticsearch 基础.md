# elasticsearch 基础

## Elasticsearch-配置参数：

## 1. elasticsearch 配置文件说明：

```yaml
elasticsearch: 
	bin:
	lib:
	modules:
	logs:
	plugins:
	config:
		elasticsearch.yml              #  elasticsearch 配置文件
		jvm.options                      # jvm 配置文件
		log4j2.properties             # 日志配置文件
```

## 2. jvm.options参数

```bash
################################################################
## IMPORTANT: JVM heap size
################################################################
##
## The heap size is automatically configured by Elasticsearch
## based on the available memory in your system and the roles
## each node is configured to fulfill. If specifying heap is
## required, it should be done through a file in jvm.options.d,
## and the min and max should be set to the same value. For
## example, to set the heap to 4 GB, create a new file in the
## jvm.options.d directory containing these lines:
##
## -Xms4g # 默认4g
## -Xmx4g
##
## See https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html
## for more information
##
################################################################
```

## 3.elasticsearch.yml

```yaml
# 集群使用描述性名称:
cluster.name: my-application
# 使用节点的描述性名称:
node.name: node-1
# 存放数据的目录的路径(多个位置用逗号分隔)
path.data: /path/to/data
# 日志文件路径
path.logs: /path/to/logs
# 默认情况下Elasticsearch只能在本地主机上访问。在这里设置一个不同的地址来公开网络上的这个节点:
network.host: 192.168.0.1
# 默认 http 端口 9200
http.port: 9200

# 配置跨域
http.cors.enabled: true
http.cors.allow-origin: "*"
```

## 4. 访问`curl "http://127.0.0.1:9200"`

```shell
[root@8f1930bdze131 ~]# curl "http://127.0.0.1:9200"
{
  "name" : "8f1930bdze131", # 主机名
  "cluster_name" : "elasticsearch", # 默认一个也是集群，默认集群名 elasticsearch
  "cluster_uuid" : "XjL5BIXbRrOY0VR4HfloEQ", # 集群 uuid
  "version" : {
    "number" : "7.12.0", # 当前版本
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "78722783c38caa25a70982b5b042074cde5d3b3a",
    "build_date" : "2021-03-18T06:17:15.410153305Z",
    "build_snapshot" : false,
    "lucene_version" : "8.8.0", # 基于 lucene 版本
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search" # 标语：你知道了，为了收索
}

```

# 📝elasticsearch 自定义字典

## 1. 配置词典

```sh
[root@localhost config]# pwd
/opt/elasticsearch/plugins/ik/config
[root@localhost config]# cat username.dic # 配置自己字典
柳宗林
[root@localhost config]# cat IKAnalyzer.cfg.xml 
﻿<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">username.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords"></entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<!-- <entry key="remote_ext_dict">words_location</entry> -->
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>
[root@localhost config]# 
```

![image](assets/elasticsearch%20%E5%9F%BA%E7%A1%80/2402369-20221008125621315-1053577461.png)

## 2. 重启 elasticsearch

docker 容器重启会报错，重启 docker 服务

**索引操作（Restful风格）**

![image](assets/elasticsearch%20%E5%9F%BA%E7%A1%80/image-20230302142345-b0a138x.png)​  
​![image](assets/elasticsearch%20%E5%9F%BA%E7%A1%80/image-20230302142342-5axrt1j.png)​

ik_smart 最少切分

```json
# 创建分词器
# get 请求，_analyze 分词

GET _analyze
{
  "analyzer": "ik_smart",
  "text": "Elasticsearch是Elastic Stack核心的分布式搜索和分析引擎。Logstash和Beats有助于收集，聚合和丰富您的数据并将其存储在Elasticsearch中。使用Kibana，您可以交互式地探索，可视化和共享对数据的见解，并管理和监视堆栈。Elasticsearch是建立索引，搜索和分析魔术的地方"
}
```

ik_max_word 最细粒度划分

```json
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "Elasticsearch是Elastic Stack核心的分布式搜索和分析引擎。Logstash和Beats有助于收集，聚合和丰富您的数据并将其存储在Elasticsearch中。使用Kibana，您可以交互式地探索，可视化和共享对数据的见解，并管理和监视堆栈。Elasticsearch是建立索引，搜索和分析魔术的地方"
}
```

## 把“柳宗林” 添加到 ik 分词器字典当中

```json
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "柳宗林"
}

# 柳宗琳 未添加到分词器当中
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "柳宗琳"
}
```

# 📦RestFul 风格

## ✨put一个索引

```json
# Rest 风格
# 创建一个索引
# put /索引名/类型名称/文档id {请求体}
PUT /test1/type1/1
{
  "name": "liuzonglin",
  "age": 18
}
```

## put _doc 默认类型

```json
PUT /test3/_doc/1
{
  "name": "liuzonglin",
  "age": 18,
  "birth": "1997-07-27"
}
```

## put 修改索引

```json
# put 修改索引 覆盖原有信息 更新版本（?v）每次 put 都会跟 _version 信息
PUT /test3/_doc/1
{
  "name": "liuzonglin",
  "age": 18,
  "birth": "1997-07-27"
}
```

## put创建索引规则

```json
# 创建一个库
# 方法体 创建索引规则
PUT /test2
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "age": {
        "type": "long"
      },
      "birthday": {
        "type": "date"
      }
    }
  }
}
```

## GET 查询索引

```json
# 查询索引
# http://192.168.1.102:9200/test1/type1/1
# curl "http://127.0.0.1:9200/test1/type1/1"
GET /test1/type1/1
GET test3/_doc/1
```

## GET 获取索引的信息

```json
GET test2
```

## GET 获取 elasticsearch 健康值

```json
GET _cat/health
```

## GET 查看所有索引库版本信息

```json
GET _cat/indices?v
```

## post 修改文档

```json
POST /test3/_doc/1/_update
{
  "doc": {
    "name": "liuzonglin",
    "age": 25
  }
}
```

## DELETE 删除索引

```json
DELETE /test1
```

# 📝查询练习

## 1. 写入数据

```json
# 文档操作
PUT /liuzonglin/user/1
{
  "name": "liuzonglin",
  "age": 26,
  "desc": "天天都在学习，还算有点进步",
  "tages": ["技术仔", "好色", "指南"]
  
}

PUT /liuzonglin/user/2
{
  "name": "张三",
  "age": 26,
  "desc": "还算有点进步",
  "tages": ["技术仔", "渣男"]
  
}

PUT /liuzonglin/user/3
{
  "name": "李四",
  "age": 26,
  "desc": "还算有点进步",
  "tages": ["技术仔"]
  
}


```

## 2. 查询数据

```json
GET _cat/indices?v
GET /liuzonglin/user/3
```

## 3. 更新数据

```json
# 文档更新操作不加”_update“会跟put 命令一样效果，覆盖原有数据
POST /liuzonglin/user/1/
{
  "doc": {
    "name": "liuzonglin",
    "age": 26,
    "desc": "天天都在学习，还算有点进步",
    "tages": ["技术仔", "擅长java", "指南学习"]
  }
}

POST /liuzonglin/user/1/_update
{
  "doc": {
    "name": "liuzonglin",
    "age": 26,
    "desc": "天天都在学习，还算有点进步",
    "tages": ["技术仔", "擅长java", "指南学习"]
  }
}

PUT /liuzonglin/user/1
{
  "name": "张三",
  "age": 26,
  "desc": "还算有点进步",
  "tages": ["技术仔", "渣男"]
}
```

## get 查询某个字段

```json
GET liuzonglin/_search
{
  "query": {
    "match": {
      "name": "棕"
    }
  }
}

# 错误语句：不支持在搜索请求中指定类型。
GET liuzonglin/_doc/_search
{
  "query": {
    "match": {
      "name": "棕"
    }
  }
}
```

## get 通过 age 进行排序

```json
GET liuzonglin/_search
{
  "query": {
    "match": {
      "name": "棕"
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc" # 降序 ; asc 升序 
      }
    }
  ]
}
```

## GET 分页查询

```json
GET liuzonglin/_search
{
  "query": {
    "match": {
      "name": "棕"
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ],
  "from": 0, # 从第几个开始
  "size": 20 # 每页面数据个数
}
```

数据索引下标还是从 0 开始的

## GET 布尔值查询（多条件精确查询）

**1. must (and) 所有条件都要符合**

```json
GET liuzonglin/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "刘" # 模糊匹配
          }
        },
        {
          "match": {
            "age": 26 # age 相当于唯一标识
          }
        }
      ]
    }
  }
}
```

**2. should（or）只要满足一条**

```json
GET liuzonglin/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "name": "刘" # 模糊匹配
          }
        },
        {
          "match": {
            "age": 26 # age 相当于唯一标识
          }
        }
      ]
    }
  }
}
```

**3. must_not（no）返回不满足的信息**

```json
GET liuzonglin/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "age": 2 # age 相当于唯一标识
          }
        }
      ]
    }
  }
}
```

**4. filter 数据过滤**

```json
GET liuzonglin/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "刘"
          }
        },
        {
          "match": {
            "age": 26
          }
        }
      ],
      "filter": [
        {
          "range": {
            "age": {
              "gte": 10, # 大于等于 10
              "lte": 20  # 小于等于 20
            }
          }
        }
      ]
    }
  }
}
```

参数说明：

- gt    大于
- gte   大于等于
- lt      小于
- lte    小于等于

**5. 匹配多个条件**

```json
GET liuzonglin/_search
{
  "query": {
    "match": {
      "tags": "天 三"
    }
  }
}
```

多个参数用空格分开

**6. term 精确查询**

![img](assets/elasticsearch%20%E5%9F%BA%E7%A1%80/2402369-20221009124320282-944259033.png)

**关于分词：**

- term 直接精确查询
- match 使用分词器解析（先分析文档，然后通过分析的文档进行查询）

**text 可以分分词 keyword 不会被分词**

**7. term 精确多个值查询**

```json
GET liuzonglin/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "term": {
            "t1": 2 
          }
        },
        {
          "term": {
            "t1": 3 
          }
        }
      ]
    }
  }
}
```

**8. 高亮查询**

```json
GET liuzonglin/_search
{
  "query": {
    "match": {
      "name": "棕"
    }
  },
  "highlight": {
    "fields": {
      "name": {}
    }
  }
}
```

![img](assets/elasticsearch%20%E5%9F%BA%E7%A1%80/2402369-20221009130634993-805532018.png)

# elasticsearch 集群搭建

## elasticsearch.yml

```yml
cluster.name: bigdata
node.name: node-1
path.data: /usr/local/las/data/elasticsearch
path.logs: /usr/local/las/log/elasticsearch
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
network.host: 0.0.0.0
network.publish_host: 172.100.19.155
http.port: 9200
http.max_content_length: 100mb
http.cors.enabled: true
discovery.seed_hosts: ["172.100.19.155","172.100.19.156"]
cluster.initial_master_nodes: ["172.100.19.155","172.100.19.156"]
node.master: true
node.data: true
transport.tcp.port: 9300
transport.tcp.compress: true
indices.fielddata.cache.size: 10%
path.repo: ["/usr/local/las/data/backup/es"]
indices.query.bool.max_clause_count: 10240
```

## 参考文档

[https://blog.csdn.net/qq_50227688/article/details/115379121](https://blog.csdn.net/qq_50227688/article/details/115379121)

‍
