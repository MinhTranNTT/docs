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

![image](assets/Elasticsearch%20自定义字典//2402369-20221008125621315-1053577461.png)

## 2. 重启 elasticsearch

docker 容器重启会报错，重启 docker 服务

**索引操作（Restful风格）**

![image](assets/Elasticsearch%20自定义字典/image-20230302142345-b0a138x.png)​  
​![image](assets/Elasticsearch%20自定义字典/image-20230302142342-5axrt1j.png)​

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