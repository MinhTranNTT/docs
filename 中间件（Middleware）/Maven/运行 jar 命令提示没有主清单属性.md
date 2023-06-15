# 运行 jar 命令提示没有主清单属性

参考文档：

[解决：运行jar命令提示没有主清单属性 - 码农教程 (manongjc.com)](http://www.manongjc.com/detail/29-etukibtsbwxttjp.html)

‍

‍

在pom.xml中添加/修改maven打包依赖：

```xml
    <build>
        <finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

‍
