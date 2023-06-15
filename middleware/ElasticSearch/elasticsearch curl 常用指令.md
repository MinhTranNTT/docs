# elasticsearch curl 常用指令

## 查询所有节点

```shell
$ curl 'http://127.0.0.1:9200/_cat/nodes'
192.168.31.127 49 61 0 2.16 2.11 2.03 dilmrt * node-1
```

## 查询集群状态

```shell
$ curl -k 'http://127.0.0.1:9200/_cluster/health?pretty'
{
  "cluster_name" : "docker-cluster",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 2,
  "active_shards" : 2,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 1,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 66.66666666666666
}
```

## 查询授权许可

```shell
$ curl -XGET -u elastic:Z37ufZpU -k 'http://127.0.0.1:9200/_xpack/license'

$ curl -k 'http://127.0.0.1:9200/_license'
{
  "license" : {
    "status" : "active",
    "uid" : "f20fdabe-697a-42d4-87ad-0683bbfc2799",
    "type" : "basic",
    "issue_date" : "2023-03-23T12:32:12.047Z",
    "issue_date_in_millis" : 1679574732047,
    "max_nodes" : 1000,
    "max_resource_units" : null,
    "issued_to" : "docker-cluster",
    "issuer" : "elasticsearch",
    "start_date_in_millis" : -1
  }
}
```

## 查询索引（集群转态ip 为真实ip）

```shell
$ curl -k 'http://127.0.0.1:9200/_cat/indices'
green open las-e-2022-08-01 P1hcLjCtRAmfumcUaTGnnA 1 0  57 0 134.1kb 134.1kb
green open las-e-2022-07-11 CHqhzA7ERzi5eBZveJlQVA 1 0  11 0 143.6kb 143.6kb
green open las-e-2022-07-22 sagFseupQBuSSvU-PUyIwQ 1 0  25 0 150.4kb 150.4kb

$ curl -XGET "http://127.0.0.1:9200/_cat/indices?v&pretty"
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   las-e-2022-08-01 P1hcLjCtRAmfumcUaTGnnA   1   0         57            0    134.1kb        134.1kb
green  open   las-e-2022-07-22 sagFseupQBuSSvU-PUyIwQ   1   0         25            0    150.4kb        150.4kb
green  open   las-e-2022-07-11 CHqhzA7ERzi5eBZveJlQVA   1   0         11            0    143.6kb        143.6kb
```

## 查询指定 id

```shell
参数：
	# liuzonglin_jd1 文档索引（_index）
	# _doc 文档类型（_type）
	# 10 文档 id (_id)

$ curl -XGET  'http://127.0.0.1:9200/liuzonglin_jd1/_doc/10/?pretty' -k
```

## 查询指定索引内容

```shell
$ curl 'http://127.0.0.1:9200/las-e-2022-08-07/_search?pretty' -k	# ES查询指定索引内容
```

## POST 查询分页数据

```shell
# from 开启，size 显示条数

$ curl -XPOST http://127.0.0.1:9200/liuzonglin_jd1/_search?pretty -H 'content-Type:application/json' -d 
'{
	"query":{"match_all":{}},
	"from":0,
	"size":10
}'
```

## PUT 创建索引

```shell
# liuzonglin 索引（index）

$ curl -XPUT http://127.0.0.1:9200/liuzonglin?pretty
```

## PUT 创建文档

```shell
$ curl -XPUT http://127.0.0.1:9200/liuzonglin/_doc/2?pretty -H 'content-Type:application/json' -d '{"name":"liuzonglin","age":"26"}'
```

## 删除全部索引

```shell
# las-e-* 全部索引名（index）

$ curl -XDELETE 'http://127.0.0.1:9200/las-e-*/' -k	# ES删除索引
{"acknowledged" : true}
```

## DELETE 删除指定索引 id

```shell
$ curl -XDELETE 'http://127.0.0.1:9200/liuzonglin_jd1/_doc/10/?pretty' -k
{
  "_index" : "liuzonglin_jd1",
  "_type" : "_doc",
  "_id" : "10",
  "found" : false
}
```

‍
