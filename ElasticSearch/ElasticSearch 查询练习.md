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

![img](assets/ElasticSearch%20查询练习/2402369-20221009124320282-944259033.png)

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

![img](assets/ElasticSearch%20查询练习/2402369-20221009130634993-805532018.png)

