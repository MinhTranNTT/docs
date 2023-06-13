# No Feign Client for loadBalancing defined. Did you forget to include spring-cloud-starter-netflix-ribbon

```xml
<!-- 服务注册/发现 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

spring-cloud-starter-openfeign

nacos 引用 openfeign 启动项目报错：

```
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'memberController': Unsatisfied dependency expressed through field 'couponFeignService';  nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.example.gulimall.member.feign. CouponFeignService': FactoryBean threw exception on object creation;  nested exception is java.lang.IllegalStateException: No Feign Client for loadBalancing defined.  Did you forget to include spring-cloud-starter-netflix-ribbon?
```

解决：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
```
