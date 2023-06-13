# spring boot 整合 ES

[Installation | Elasticsearch Java API Client [8.6] | Elastic](https://www.elastic.co/guide/en/elasticsearch/client/java-api-client/8.6/installation.html#maven)

[SpringBoot整合ES(8.0版本)(一) - 我心如雷 - 博客园 (cnblogs.com)](https://www.cnblogs.com/TamAlan/p/16193590.html)

---

官网地址api文档地址：[https://www.elastic.co/guide/en/elasticsearch/client/index.html](https://www.elastic.co/guide/en/elasticsearch/client/index.html)

![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221009143553314-1978524853.png)

**1. 依赖引入**

![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221009144428713-712918718.png)

**2. 初始化对象**

![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221009144545010-411497571.png)

**3. 创建项目导入依赖**

![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221009150057029-23054127.png)

**导入依赖版本和实际使用版本不一致（ES 版本必须一致）**

![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221009150454833-574821360.png)

**自定义版本**
![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221009152412599-2006939674.png)

**源码**
![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221009155049526-1244278333.png)
![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221009155708230-399251058.png)

## ES api 测试

### 1. 初始化 ES 创建索引

```java
package com.example.es.config;

import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/*
  ES配置

  @author liuzonglin
 * @date 2022/10/09
 */

/**
 * 配置配置类注解
 * @author liuzonglin
 */
@Configuration
public class ElasticsearchConfig {

    /**
     * 配置 Bean
     * spring <beans id="restHighLevelClient" class="RestHighLevelClient"/>
     */
    @Bean
    public RestHighLevelClient restHighLevelClient() {
        return new RestHighLevelClient(
                RestClient.builder(
                        // 集群配置多个 new HttpHost("localhost", 9201, "http")
                        new HttpHost("192.168.1.102", 9200, "http")));
    }
}

```

### 2. 创建 查询 删除 （索引 文档）

```java
package com.example.es.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.stereotype.Component;

/**
 * 用户
 *
 * @author liuzonglin
 * @date 2022/10/09
 */

/**
 * Component 注入 spring 中
 * AllArgsConstructor 有参
 * NoArgsConstructor 无参
 */
@Component
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private Integer age;
}

```

```java
package com.example.es.utis;

/**
 * elasticsearch 工具栏
 *
 * @author liuzonglin
 * @date 2022/10/09
 */
public class ElasticsearchUtils {
    public final static String ES_INDEX = "liuzonglin_index";

}

```

```java
package com.example.es;

import com.alibaba.fastjson.JSON;
import com.example.es.pojo.User;
import com.example.es.utis.ElasticsearchUtils;
import lombok.SneakyThrows;
import org.elasticsearch.action.admin.indices.delete.DeleteIndexRequest;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.action.get.GetRequest;
import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.action.support.master.AcknowledgedResponse;
import org.elasticsearch.action.update.UpdateRequest;
import org.elasticsearch.action.update.UpdateResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.client.indices.CreateIndexRequest;
import org.elasticsearch.client.indices.CreateIndexResponse;
import org.elasticsearch.client.indices.GetIndexRequest;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.TermQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.FetchSourceContext;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.test.context.SpringBootTest;

import java.io.IOException;
import java.util.ArrayList;
import java.util.concurrent.TimeUnit;


@SpringBootTest
class EsApplicationTests {

    /**
     * 客户端
     * Autowired 默认选择他的一个类型
     * client 不是 RestHighLevelClient 的默认类型 名字问题，如果需要这样设置需要加 @Qualifier("restHighLevelClient")
     */
    @Autowired
    @Qualifier("restHighLevelClient")
    private RestHighLevelClient client;

    /**
     * 创建索引
     */
    @SneakyThrows
    @Test
    void createIndex() {
        // 创建索引请求
        CreateIndexRequest request = new CreateIndexRequest("liuzonglin_index");
        // 执行索引请求 indicesClient 请求后获得响应
        CreateIndexResponse response = client.indices().create(request, RequestOptions.DEFAULT);
        System.out.println("response = " + response);

    }

    /**
     * 查询索引
     */
    @SneakyThrows
    @Test
    void queryIndex() {
        // 获取索引请求
        GetIndexRequest request = new GetIndexRequest("liuzonglin_index");
        boolean exists = client.indices().exists(request, RequestOptions.DEFAULT);
        System.out.println(exists);
    }


    /**
     * 删除索引
     */
    @Test
    void deleteIndex() throws IOException {
        // 获取删除索引请求
        DeleteIndexRequest request = new DeleteIndexRequest("liuzonglin_index2");
        AcknowledgedResponse delete = client.indices().delete(request, RequestOptions.DEFAULT);
        System.out.println(delete.isAcknowledged());

    }

    /**
     * 创建文档
     */
    @SneakyThrows
    @Test
    void createDocument() {
        // 创建对象
        User user = new User("liuzonglin", 26);
        // 创建请求
        IndexRequest request = new IndexRequest("liuzonglin_index");
        // 设置规则
        request.id("1");
        // 设置过期时间 1s
        request.timeout(TimeValue.timeValueSeconds(1));
        request.timeout("1s");

        // 放入请求数据 json
        // 引入 str 转 josn 的依赖
        request.source(JSON.toJSONString(user), XContentType.JSON);
        // 客户端发送请求, 获取相应结果
        IndexResponse index = client.index(request, RequestOptions.DEFAULT);
        System.out.println(index.toString());
        System.out.println(index.status());
    }

    /**
     * 得到文档
     * get/liuzonglin/_doc/1
     */
    @SneakyThrows
    @Test
    void getDocument() {
        // 获取请求
        GetRequest getRequest = new GetRequest("liuzonglin_index", "1");
        // 不获取返回的 _source 的上下文
        getRequest.fetchSourceContext(new FetchSourceContext(false));
        getRequest.storedFields("_none_");
        boolean exists = client.exists(getRequest, RequestOptions.DEFAULT);
        System.out.println(exists);
    }

    /**
     * 获取文件信息
     */
    @SneakyThrows
    @Test
    void getDocumentInformation() {
        GetRequest getRequest = new GetRequest("liuzonglin_index", "1");
        GetResponse getResponse = client.get(getRequest, RequestOptions.DEFAULT);
        // 打印文档
        System.out.println(getResponse.getSourceAsString());
        System.out.println(getResponse.getSource());
        System.out.println(getRequest.version());
    }

    /**
     * 更新文档
     */
    @SneakyThrows
    @Test
    void updateDocument() {
        UpdateRequest updateRequest = new UpdateRequest("liuzonglin_index", "1");
        updateRequest.timeout("1s");
        User user = new User("liuzonglin1", 27);
        updateRequest.doc(JSON.toJSONString(user), XContentType.JSON);
        UpdateResponse update = client.update(updateRequest, RequestOptions.DEFAULT);
        System.out.println(update);
    }

    /**
     * 删除文档
     */
    @SneakyThrows
    @Test
    void deleteDocument() {
        DeleteRequest deleteRequest = new DeleteRequest("liuzonglin_index", "1");
        deleteRequest.timeout("1s");
        DeleteResponse delete = client.delete(deleteRequest, RequestOptions.DEFAULT);
        System.out.println(delete);
    }

    /**
     * 批量插入
     */// 批量插入数据
    @SneakyThrows
    @Test
    void bulkInsert() {
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("100s");
        ArrayList<User> list = new ArrayList<User>() {
            {
                this.add(new User("liuzonglin1", 1));
                this.add(new User("liuzonglin2", 2));
                this.add(new User("liuzonglin3", 3));
                this.add(new User("liuzonglin4", 4));
                this.add(new User("liuzonglin5", 5));
                this.add(new User("liuzonglin6", 6));
                this.add(new User("liuzonglin7", 7));
                this.add(new User("liuzonglin8", 8));
                this.add(new User("liuzonglin9", 9));
                this.add(new User("liuzonglin10", 10));
            }
        };
        int size = list.size();
        for (int i = 0; i < size; i++) {
            bulkRequest.add(
                    new IndexRequest("liuzonglin_index")
                    .id(" " + (i + 1))
                    .source(JSON.toJSONString(list.get(i)),XContentType.JSON));
        }
        BulkResponse bulk = client.bulk(bulkRequest, RequestOptions.DEFAULT);
        // 是否失败
        System.out.println(bulk.hasFailures());
    }

    /**
     * 获取文件信息
     */
    @SneakyThrows
    @Test
    void batchQuery() {
        // 1. 搜索请求
        SearchRequest searchRequest = new SearchRequest(ElasticsearchUtils.ES_INDEX);
        // 2. 搜索条件构建
        SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
        // QueryBuilders.termQuery(); 快速查询精确匹配
        // QueryBuilders.matchAllQuery(); 匹配所有
        TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("name", "liuzonglin");
        searchSourceBuilder.query(termQueryBuilder);
        searchSourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
        // 放入请求
        searchRequest.source(searchSourceBuilder);
        // 执行请求
        SearchResponse search = client.search(searchRequest, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(search.getHits()));
        for (SearchHit documentFields : search.getHits().getHits()) {
            System.out.println(documentFields);
        }
    }
}


```

## 实战案例

> 导入依赖

![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221010054916981-1326200322.png)

> 导入 fastjson 数据

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.83</version>
</dependency>
```

注意导入的 ES 版本

> 导入静态文件

> 测试

```java
package com.example.elasticsearch.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

/**
 * 指数控制器
 *
 * @author liuzonglin
 * @date 2022/10/10
 */
@Controller
public class IndexController {

    /**
     * 指数
     * getMapping 需要返回一个页面 index ({})
     * @return {@link String}
     */
    @GetMapping({"/", "index"})
    public String index() {
        return "index";
    }
}

```

> 导入 jsoup（获取请求返回的页面信息，帅选出想要的数据）

```xml
<!-- tika 爬取视频音乐-->
<!-- 解析网页 jsoup 爬取网页-->
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.11.3</version>
</dependency>
```

> 爬取数据

```java
package com.example.elasticsearch.utils;

import com.example.elasticsearch.pojo.Content;
import lombok.SneakyThrows;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.net.URL;
import java.util.ArrayList;
import java.util.List;

/**
 * 解析网页
 *
 * @author liuzonglin
 * @date 2022/10/10
 */
public class HtmlParseUtil {

    public static void main(String[] args) {
        new HtmlParseUtil().parsingPage("java").forEach(System.out::println);
    }


    /**
     * 解析页面
     */
    @SneakyThrows
    public List<Content> parsingPage(String keywords) {
        // 联网 获取请求
        String url = "https://search.jd.com/Search?keyword=" + keywords;
        // 解析网页 Jsoup 返回 document(整个文档页面) 浏览器对象
        Document document = Jsoup.parse(new URL(url), 3000);
        // 根据 id 查找 页面中所需信息
        Element element = document.getElementById("J_goodsList");
        // System.out.println(element.html());
        // 获取所有元素
        Elements li = element.getElementsByTag("li");

        ArrayList<Content> contents = new ArrayList<>();


        // 或取元素中内容
        for (Element l : li) {
            // 获取图片 无法获取图片 source-data-lazy-img
            String img = l.getElementsByTag("img").eq(0).attr("data-lazy-img");
            // 商品价格
            String price = l.getElementsByClass("p-price").eq(0).text();
            // 商品名称
            String title = l.getElementsByClass("p-name").eq(0).text();

            Content content = new Content();
            content.setImg(img);
            content.setTitle(title);
            content.setPrice(price);
            contents.add(content);
        }
        return contents;
    }

}

```

> 封装数据

```java
package com.example.elasticsearch.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.stereotype.Component;

/**
 * 内容
 *
 * @author liuzonglin
 * @date 2022/10/10
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
@Component
public class Content {
    private String title;
    private String img;
    private String price;
}

```

![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221010070052644-1518107587.png)
![img](https://img2022.cnblogs.com/blog/2402369/202210/2402369-20221010064637863-1222952285.png)

```java
package com.example.elasticsearch.config;

import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/*
  ES配置

  @author liuzonglin
 * @date 2022/10/09
 */

/**
 * 配置配置类注解
 * @author liuzonglin
 */
@Configuration
public class ElasticsearchConfig {

    /**
     * 配置 Bean
     * spring <beans id="restHighLevelClient" class="RestHighLevelClient"/>
     */
    @Bean
    public RestHighLevelClient restHighLevelClient() {
        return new RestHighLevelClient(
                RestClient.builder(
                        new HttpHost("192.168.1.103", 9200, "http")));
    }
}

```

```java
package com.example.elasticsearch.controller;

import com.example.elasticsearch.service.ContentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Map;

/**
 * 内容控制器
 *
 * @author liuzonglin
 * @date 2022/10/10
 */
@RestController
public class ContentController {

    @Autowired
    private ContentService contentService;


    /**
     * ES 批量创建文档
     *
     * @param keywords 关键字
     * @return {@link Boolean}
     */
    @GetMapping("/parse/{keywords}")
    public Boolean parse(@PathVariable("keywords") String keywords) {
        return contentService.parseContent(keywords);
    }


    /**
     * 搜索
     *
     * @param pageNo   页面没有
     * @param pageSize 页面大小
     * @param keyword  关键字
     * @return {@link List}<{@link Map}<{@link String}, {@link Object}>>
     */
    @GetMapping("/search/{keyword}/{pageNo}/{pageSize}")
    public List<Map<String, Object>> search(@PathVariable String keyword,
                                            @PathVariable int pageNo,
                                            @PathVariable int pageSize) {
        return contentService.searchPage(keyword, pageNo, pageSize);
    }
}

```

```java
package com.example.elasticsearch.service;

import com.example.elasticsearch.pojo.Content;
import com.example.elasticsearch.utils.HtmlParseUtil;
import lombok.SneakyThrows;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.TermQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.concurrent.TimeUnit;

/**
 * 内容服务
 *
 * @author liuzonglin
 * @date 2022/10/10
 */
@Service
public class ContentService {

    /**
     * 客户端
     */
    @Autowired
    private RestHighLevelClient client;



    /**
     * 解析内容
     * 解析数据放入 ES 当中
     *
     * @param keywords 关键字
     * @return {@link Boolean}
     */
    @SneakyThrows
    public Boolean parseContent(String keywords) {
        // 爬取数据
        List<Content> contents = new HtmlParseUtil().parsingPage(keywords);
        // 数据放入 ES 当中
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("2m");
        for (Content content : contents) {
            bulkRequest.add(new IndexRequest("liuzonglin_jd")
                    .source(content, XContentType.JSON));
        }
        // 批量创建文档
        BulkResponse bulk = client.bulk(bulkRequest, RequestOptions.DEFAULT);
        // 是否成功
        return !bulk.hasFailures();

    }

    /**
     * 搜索页面
     * 搜索数据分页
     *
     * @param keyword  关键字
     * @param pageNo   页面没有
     * @param pageSize 页面大小
     * @return {@link List}<{@link Map}<{@link String}, {@link Object}>>
     */
    @SneakyThrows
    public List<Map<String, Object>> searchPage(String keyword, int pageNo, int pageSize) {
        // 如果页面数量小于等于 1
        if (pageNo <= 1) {
            pageNo = 1;
        }
        // 条件查询
        SearchRequest searchRequest = new SearchRequest("liuzonglin_jd");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        // 分页
        sourceBuilder.from(pageNo);
        sourceBuilder.size(pageSize);
        // 精准匹配
        TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", keyword);
        sourceBuilder.query(termQueryBuilder);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
        // 执行搜索
        searchRequest.source(sourceBuilder);
        SearchResponse search = client.search(searchRequest, RequestOptions.DEFAULT);

        // 解析结果
        ArrayList<Map<String, Object>> list = new ArrayList<>();
        for (SearchHit hit : search.getHits().getHits()) {
            System.out.println("hit = " + hit);
            list.add(hit.getSourceAsMap());

        }
        return list;
    }
}

```

‍
