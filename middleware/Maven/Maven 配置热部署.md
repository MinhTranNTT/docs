# Maven 配置热部署

参考文档：

[maven配置热部署 - 秒客网 (miaokee.com)](https://www.miaokee.com/397644.html)

[(22条消息) 新版本Idea热部署无效问题处理_全栈高级工程师的博客-CSDN博客](https://blog.csdn.net/weixin_43837268/article/details/128541815)

---

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>


<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
                <addResources>true</addResources>
            </configuration>
        </plugin>
    </plugins>
</build>
```

‍
