# Docker 安装 Elasticsearch

## 1. 拉取镜像：

```bash
sudo docker pull elasticsearch:8.6.2
```

## 2. 创建docker容器挂载目录：

```bash
sudo mkdir -pv /home/zonglin/elasticsearch/config
sudo mkdir -pv /home/zonglin/elasticsearch/data
sudo mkdir -pv /home/zonglin/elasticsearch/plugins
```

`mkdir` 参数说明：

- -p, --parents     如果存在，则没有错误，根据需要创建父目录
- -v, --verbose     为每个创建的目录打印一条消息

## 3. 配置文件（elasticsearch.yml）：

```bash
echo "http.host: 0.0.0.0" > /home/zonglin/elasticsearch/config/elasticsearch.yml
chmod -R 777 /home/zonglin/elasticsearch/
```

参数说明：

- ">"                          如果文件存在，清空文件内容并写入。如果文件不存在，创建新文件并写入。
- -R, --recursive        递归地更改文件和目录
- 777                         可读、可写、可执行权限

## 4. 创建容器：

```bash
sudo docker run --name elasticsearch -p 9200:9200  -p 9300:9300 \
 --restart=always \
 -e "discovery.type=single-node" \
 -e ES_JAVA_OPTS="-Xms84m -Xmx512m" \
 -v /home/zonglin/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
 -v /home/zonglin/elasticsearch/data:/usr/share/elasticsearch/data \
 -v /home/zonglin/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
 -d elasticsearch:8.6.2
```

`docker run`参数说明:

- --name string                    为容器分配一个名称
- -p, --publish list                  向主机发布容器的端口
- -P, --publish-all                    将所有公开的端口发布到随机端口
- -e discovery.type=single-node 单点模式启动
- -e ES_JAVA_OPTS="-Xms84m -Xmx512m"：设置启动占用的内存范围
- -v, --volume list                    绑定挂载卷
- -d, --detach                         在后台运行容器并打印容器ID

## 5. 查看启动详情：

```bash
docker ps  查看是否启动
docker logs elasticsearch  启动日志查询
docker restart elasticsearch   重启
docker exec -it elasticsearch bash 进入
```

**操作明细**

```bash
[root@localhost ~]# docker pull elasticsearch:7.12.0
7.12.0: Pulling from library/elasticsearch
7a0437f04f83: Pull complete 
2b674c951ca3: Pull complete 
06baeb69f25f: Pull complete 
eeff01d19ce5: Pull complete 
a994306398ca: Pull complete 
2c002d76c1f6: Pull complete 
6286f2196f9b: Pull complete 
Digest: sha256:383e9fb572f3ca2fdef5ba2edb0dae2c467736af96aba2c193722aa0c08ca7ec
Status: Downloaded newer image for elasticsearch:7.12.0
docker.io/library/elasticsearch:7.12.0
[root@localhost ~]# docker images
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
elasticsearch   7.12.0    9337ed510a0c   18 months ago   830MB
[root@localhost opt]# sudo mkdir -pv /opt/elasticsearch/config
mkdir: 已创建目录 "/opt/elasticsearch"
mkdir: 已创建目录 "/opt/elasticsearch/config"
[root@localhost opt]# sudo mkdir -pv /opt/elasticsearch/data
mkdir: 已创建目录 "/opt/elasticsearch/data"
[root@localhost opt]# sudo mkdir -pv /opt/elasticsearch/plugins
mkdir: 已创建目录 "/opt/elasticsearch/plugins"
[root@localhost config]# echo "http.host: 0.0.0.0" > /opt/elasticsearch/config/elasticsearch.yml
[root@localhost config]# sudo docker run --name elasticsearch -p 9200:9200  -p 9300:9300 \
>  -e "discovery.type=single-node" \
>  -e ES_JAVA_OPTS="-Xms84m -Xmx512m" \
>  -v /opt/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
>  -v /opt/elasticsearch/data:/usr/share/elasticsearch/data \
>  -v /opt/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
>  -d elasticsearch:7.12.0
8f1930bde13101b5f0412d2e31c7ebc9114c80d95b36da4ead466262574642af
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS             PORTS                                                                                  NAMES
8f1930bde131   elasticsearch:7.12.0   "/bin/tini -- /usr/l…"   13 minutes ago   Up 5 minutes       0.0.0.0:9200->9200/tcp, :::9200->9200/tcp, 0.0.0.0:9300->9300/tcp, :::9300->9300/tcp   elasticsearch

[root@localhost ~]# curl "http://127.0.0.1:9200"
{
  "name" : "8f1930bde131",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "XjL5BIXbRrOY0VR4HfloEQ",
  "version" : {
    "number" : "7.12.0",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "78722783c38caa25a70982b5b042074cde5d3b3a",
    "build_date" : "2021-03-18T06:17:15.410153305Z",
    "build_snapshot" : false,
    "lucene_version" : "8.8.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

# 安装 elasticsearch-ik 分词器

==elasticsearch-ik 分词器版本和 elasticsearch 版本必须一致==

## 1. 拉取安装包

```sh
wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.12.0/elasticsearch-analysis-ik-7.12.0.zip
```

## 2. 创建 ik 目录

```sh
mkdir -pv /opt/elasticsearch/plugins/ik/
unzip elasticsearch-analysis-ik-7.12.0.zip # 解压到ik目录中
```

## 3.重启服务

```sh
docker restart elasticsearch
```

# 🔨kibana 安装

## 可视化界面

[Elasticvue - Microsoft Edge Addons](https://microsoftedge.microsoft.com/addons/detail/elasticvue/geifniocjfnfilcbeloeidajlfmhdlgo)

## 1. 安装kibana

==kibana，elasticsearch需要版本一致==

```sh
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.12.0-linux-x86_64.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.12.0-linux-x86_64.tar.gz
```

## 2. 配置`kibana/config/kibana.yml`

```sh
server.host: "192.168.1.102"
# 配置远程服务器地址
elasticsearch.hosts: ["http://192.168.1.102:9200"]
# 设置中文
i18n.locale: "zh-CN"
# zh-CN
/usr/local/kibana/x-pack/plugins/translations/translations/zh-CN.json
```

## 3. 启动 kibana

```sh
# kibana 不支持root用户启动
./kibana/bin/kibana --allow-root &
```

## 4. 测试连接

`systemctl stop firewalld.service # 关闭防火墙`

![image](assets/Docker%20%E5%AE%89%E8%A3%85%20Elasticsearch/2402369-20221008090548309-87994082.png)

`web访问：http://192.168.1.102:5601`

‍
