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
