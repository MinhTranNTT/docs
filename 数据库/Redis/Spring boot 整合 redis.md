# Spring boot 整合 redis

我们接着来看如何在SpringBoot项目中整合Redis操作框架，只需要一个starter即可，但是它底层没有用Jedis，而是Lettuce：

## 1、依赖导入

> Maven

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

> Gradle

```groovy
implementation group: 'org.springframework.boot', name: 'spring-boot-starter-data-redis', version: '3.0.0'
```

[Maven Central Repository Search](https://search.maven.org/)

‍

starter提供的默认配置会去连接本地的Redis服务器，并使用0号数据库，当然你也可以手动进行修改：

## 2、application.properties 中加入 redis 相关配置

```properties
# Redis数据库索引（默认为0）  
spring.redis.database=0

# Redis服务器地址  
spring.redis.host=192.168.1.102

# Redis服务器连接端口  
spring.redis.port=6379

# Redis服务器连接密码（默认为空）  
spring.redis.password=

# 连接池最大连接数（使用负值表示没有限制）  
spring.redis.pool.max-active=200

# 连接池最大阻塞等待时间（使用负值表示没有限制）  
spring.redis.pool.max-wait=-1

# 连接池中的最大空闲连接  
spring.redis.pool.max-idle=10

# 连接池中的最小空闲连接  
spring.redis.pool.min-idle=0

# 连接超时时间（毫秒）  
spring.redis.timeout=1000
```

[Common Application Properties (spring.io)](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties)

## 3、redis 配置类

```java
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnClass({RedisOperations.class})
@EnableConfigurationProperties({RedisProperties.class})
@Import({LettuceConnectionConfiguration.class, JedisConnectionConfiguration.class})
public class RedisAutoConfiguration {
    public RedisAutoConfiguration() {
    }

    @Bean
    @ConditionalOnMissingBean(
        name = {"redisTemplate"}
    )
    @ConditionalOnSingleCandidate(RedisConnectionFactory.class)
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<Object, Object> template = new RedisTemplate();
        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }

    @Bean
    @ConditionalOnMissingBean
    @ConditionalOnSingleCandidate(RedisConnectionFactory.class)
    public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
        return new StringRedisTemplate(redisConnectionFactory);
    }
}
```

那么如何去使用这两个模板类呢？我们可以直接注入`StringRedisTemplate`​来使用模板：

```java
@SpringBootTest
class SpringBootTestApplicationTests {

    @Autowired
    StringRedisTemplate template;

    @Test
    void contextLoads() {
        ValueOperations<String, String> operations = template.opsForValue();
        operations.set("c", "xxxxx");   //设置值
        System.out.println(operations.get("c"));   //获取值
  
        template.delete("c");    //删除键
        System.out.println(template.hasKey("c"));   //判断是否包含键
    }

}
```

实际上所有的值的操作都被封装到了`ValueOperations`​对象中，而普通的键操作直接通过模板对象就可以使用了，大致使用方式其实和Jedis一致。

我们接着来看看事务操作，由于Spring没有专门的Redis事务管理器，所以只能借用JDBC提供的，只不过无所谓，正常情况下反正我们也要用到这玩意：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

```java
@Service
public class RedisService {

    @Resource
    StringRedisTemplate template;

    @PostConstruct
    public void init(){
        template.setEnableTransactionSupport(true);   //需要开启事务
    }

    @Transactional    //需要添加此注解
    public void test(){
        template.multi();
        template.opsForValue().set("d", "xxxxx");
        template.exec();
    }
}
```

我们还可以为RedisTemplate对象配置一个Serializer来实现对象的JSON存储：

```java
@Test
void contextLoad2() {
    //注意Student需要实现序列化接口才能存入Redis
    template.opsForValue().set("student", new Student());
    System.out.println(template.opsForValue().get("student"));
}
```

---

‍
