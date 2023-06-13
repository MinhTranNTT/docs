# Dubbo整合Spring Boot

https://cn.dubbo.apache.org/zh-cn/overview/quickstart/java/spring-boot/

---

**引入依赖：**

```shell
<dependency>
    <groupId>com.alibaba.boot</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>0.2.0</version>
</dependency>
```

**注意starter版本适配：**

|Java|Spring Boot<br />|Dubbo<br />|dubbo-spring-boot-starter|
| ------| ---------------| ---------| ---------------------------|
|1.8+|2.0.x|2.6.2+|0.2.0|
|1.7+|1.5.x|2.6.2+|0.1.1|

‍

**配置 application.properties**

* **提供者配置：**

```shell

dubbo.application.name=gmall-user
dubbo.registry.protocol=zookeeper
dubbo.registry.address=192.168.67.159:2181
dubbo.scan.base-package=com.atguigu.gmall
dubbo.protocol.name=dubbo
application.name		# 就是服务名，不能跟别的dubbo提供端重复
registry.protocol 		# 是指定注册中心协议
registry.address 		# 是注册中心的地址加端口号
protocol.name 			# 是分布式固定是dubbo,不要改。
base-package    		# 注解方式要扫描的包
```

* **消费者配置：**

```shell
dubbo.application.name=gmall-order-web
dubbo.registry.protocol=zookeeper
dubbo.registry.address=192.168.67.159:2181
dubbo.scan.base-package=com.atguigu.gmall
dubbo.protocol.name=dubbo
```

‍

**dubbo注解**

@Service、@Reference

**【如果没有在配置中写dubbo.scan.base-package,还需要使用@EnableDubbo注解】**

‍
