# Java 整合 Redis

## 1、依赖导入

```xml
<dependency>
	<groupId>redis.clients</groupId>
	<artifactId>jedis</artifactId>
	<version>4.2.3</version>
</dependency>
<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>fastjson</artifactId>
	<version>1.2.83</version>
</dependency>
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-simple</artifactId>
	<version>1.7.22</version>
</dependency>
```

### 1.2、创建Jedis对象

```java
Jedis jedis = new Jedis("192.168.1.102",6379);
```

### 1.3、基础API测试

```java
    @Test
    public void ping() {
        System.out.println("测试连通是否连通 " + jedis.ping());
        jedis.close();
    }


    /*操作zset*/
    @Test
    public void demo5() {
        jedis.zadd("china",100d,"shanghai");
        Set<String> china = jedis.zrange("china", 0, -1);
        System.out.println(china);
        jedis.close();
    }

    /*操作hash*/
    @Test
    public void demo4() {
        jedis.hset("users","age","20");
        String hget = jedis.hget("users", "age");
        System.out.println(hget);
        jedis.close();
    }

    /*操作set*/
    @Test
    public void demo3() {
        jedis.sadd("names","lucy");
        jedis.sadd("names","mary");
        Set<String> names = jedis.smembers("names");
        System.out.println(names);
        jedis.close();
    }

    /*操作list*/
    @Test
    public void demo2() {
        jedis.lpush("key1","lucy","mary","jack");
        List<String> values = jedis.lrange("key1", 0, -1);
        System.out.println(values);
        jedis.close();
    }

```

‍
