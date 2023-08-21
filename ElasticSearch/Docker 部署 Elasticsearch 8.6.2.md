# Docker 部署 Elasticsearch 8.6.2

```bash
docker pull elasticsearch:8.6.2
mkdir -pv /home/zonglin/elasticsearch/plugins

sudo docker run --name elasticsearch -p 9200:9200  -p 9300:9300 \
 --restart=always \
 -e "discovery.type=single-node" \
 -e ES_JAVA_OPTS="-Xms84m -Xmx512m" \
 -v /home/zonglin/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
 -d elasticsearch:8.6.2
```

修改默认开启了ssl认证[SpringBoot整合ES(8.0版本)(一) - 我心如雷 - 博客园 (cnblogs.com)](https://www.cnblogs.com/TamAlan/p/16193590.html)

```bash
# 拷贝到容器文件本地
docker cp 240e18143687:/usr/share/elasticsearch/config/elasticsearch.yml ./elasticsearch.yml

# 本地文件拷贝容器
docker cp ./elasticsearch.yml 240e18143687:/usr/share/elasticsearch/config/elasticsearch.yml
```

## Elasticsearch 配置参数说明

```bash
# 集群名称
cluster.name: "docker-cluster"

# 设置节点绑定地址 (IPv4 or IPv6)
network.host: 0.0.0.0

#----------------------- BEGIN SECURITY AUTO CONFIGURATION -----------------------
#
# The following settings, TLS certificates, and keys have been automatically    
# generated to configure Elasticsearch security features on 23-03-2023 12:31:51
#
# --------------------------------------------------------------------------------

  # 启用安全功能默认是https需要密码， false: http 不需用密码
xpack.security.enabled: false

xpack.security.enrollment.enabled: true

# Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents
xpack.security.http.ssl:
  enabled: true
  keystore.path: certs/http.p12

# Enable encryption and mutual authentication between cluster nodes
xpack.security.transport.ssl:
  enabled: true
  verification_mode: certificate
  keystore.path: certs/transport.p12
  truststore.path: certs/transport.p12
#----------------------- END SECURITY AUTO CONFIGURATION -------------------------
```

‍
