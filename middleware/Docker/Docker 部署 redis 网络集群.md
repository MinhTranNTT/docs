# Docker 部署 redis 网络集群

​![image](assets/image-20230221223656-vaq32g5.png)​

```sh
## 1. 创建网卡
docker network create redis --subnet 172.38.0.0/16
 
# 2. 通过脚本创建六个redis配置
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done
# 3.0 创建结点1
docker run -p 6371:6379 -p 16371:16379 --name redis-1 \
-v /mydata/redis/node-1/data:/data \
-v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.11 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
 
# 3.1 创建结点2
docker run -p 6372:6379 -p 16372:16379 --name redis-2 \
-v /mydata/redis/node-2/data:/data \
-v /mydata/redis/node-2/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.12 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
# 3.2 创建结点3
docker run -p 6373:6379 -p 16373:16379 --name redis-3 \
-v /mydata/redis/node-3/data:/data \
-v /mydata/redis/node-3/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.13 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
# 3.3 创建结点4
docker run -p 6374:6379 -p 16374:16379 --name redis-4 \
-v /mydata/redis/node-4/data:/data \
-v /mydata/redis/node-4/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.14 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
# 3.4 创建结点5
docker run -p 6375:6379 -p 16375:16379 --name redis-5 \
-v /mydata/redis/node-5/data:/data \
-v /mydata/redis/node-5/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.15 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
# 3.5创建结点6
docker run -p 6376:6379 -p 16376:16379 --name redis-6 \
-v /mydata/redis/node-6/data:/data \
-v /mydata/redis/node-6/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.16 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
 
# 4. 创建集群
[root@ ~]# docker exec -it redis-1 /bin/sh
/data # ls
appendonly.aof  nodes.conf
/data # redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.38.0.15:6379 to 172.38.0.11:6379
Adding replica 172.38.0.16:6379 to 172.38.0.12:6379
Adding replica 172.38.0.14:6379 to 172.38.0.13:6379
M: 541b7d237b641ac2ffc94d17c6ab96b18b26a638 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
M: a89c1f1245b264e4a402a3cf99766bcb6138dbca 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: 259e804d6df74e67a72e4206d7db691a300c775e 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: 9b19170eea3ea1b92c58ad18c0b5522633a9e271 172.38.0.14:6379
   replicates 259e804d6df74e67a72e4206d7db691a300c775e
S: 061a9d38f22910aaf0ba1dbd21bf1d8f57bcb7d5 172.38.0.15:6379
   replicates 541b7d237b641ac2ffc94d17c6ab96b18b26a638
S: 7a16b9bbb0615ec95fc978fa62fc054df60536f0 172.38.0.16:6379
   replicates a89c1f1245b264e4a402a3cf99766bcb6138dbca
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 172.38.0.11:6379)
M: 541b7d237b641ac2ffc94d17c6ab96b18b26a638 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
M: a89c1f1245b264e4a402a3cf99766bcb6138dbca 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: 7a16b9bbb0615ec95fc978fa62fc054df60536f0 172.38.0.16:6379
   slots: (0 slots) slave
   replicates a89c1f1245b264e4a402a3cf99766bcb6138dbca
S: 061a9d38f22910aaf0ba1dbd21bf1d8f57bcb7d5 172.38.0.15:6379
   slots: (0 slots) slave
   replicates 541b7d237b641ac2ffc94d17c6ab96b18b26a638
M: 259e804d6df74e67a72e4206d7db691a300c775e 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 9b19170eea3ea1b92c58ad18c0b5522633a9e271 172.38.0.14:6379
   slots: (0 slots) slave
   replicates 259e804d6df74e67a72e4206d7db691a300c775e
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
 
 
```

‍
