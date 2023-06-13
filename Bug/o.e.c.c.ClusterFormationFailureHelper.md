# o.e.c.c.ClusterFormationFailureHelper

原本缓存数据，影响。

```
问题1：
[2022-08-09T10:37:14,478][WARN ][o.e.c.c.ClusterFormationFailureHelper] [fort1] master not discovered yet, this node has not previously joined a bootstrapped (v7+) cluster, and this node must discover master-eligible nodes [node-1] to bootstrap a cluster: have discovered [{fort1}{nhSOkEhLQuaO4R6wlHvobw}{ddGlKTDYQfGnDrw3PXqtqw}{172.100.4.9}{172.100.4.9:9300}{dilmrt}{ml.machine_memory=16703619072, xpack.installed=true, transform.node=true, ml.max_open_jobs=20}]; discovery will continue using [127.0.0.1:9300] from hosts providers and [{fort1}{nhSOkEhLQuaO4R6wlHvobw}{ddGlKTDYQfGnDrw3PXqtqw}{172.100.4.9}{172.100.4.9:9300}{dilmrt}{ml.machine_memory=16703619072, xpack.installed=true, transform.node=true, ml.max_open_jobs=20}] from last-known cluster state; node term 0, last-accepted version 0 in term 0

问题2：
ERROR: [2] bootstrap checks failed
[1]: memory locking requested for elasticsearch process but memory is not locked
[2]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
ERROR: Elasticsearch did not exit normally - check the logs at /usr/local/las/log/elasticsearch/bigdata.log
```

‍
