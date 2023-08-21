# Elasticsearch 配置参数

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




