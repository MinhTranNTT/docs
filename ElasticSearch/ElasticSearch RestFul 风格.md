
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