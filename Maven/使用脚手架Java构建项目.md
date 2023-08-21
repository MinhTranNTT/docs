# 使用脚手架Java构建项目

* [Spring Initializr](https://start.spring.io/) 构建项目

* [Cloud Native App Initializer (aliyun.com)](https://start.aliyun.com/) 构建项目
* 配置依赖

  ```xml
      <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.3.4.RELEASE</version>
      </parent>
      <dependencies>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
          </dependency>
      </dependencies>  
      <build>
          <plugins>
              <plugin>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-maven-plugin</artifactId>
              </plugin>
          </plugins>
      </build>
  ```

* 编写测试：接口

  ![image](assets/%E4%BD%BF%E7%94%A8%E8%84%9A%E6%89%8B%E6%9E%B6Java%E6%9E%84%E5%BB%BA%E9%A1%B9%E7%9B%AE/image-20230307184242-12s0skh.png)​

* 修改 Maven 核心配置文件

  ```shell
  vim /root/.m2/wrapper/dists/apache-maven-3.8.6-bin/1ks0nkde5v1pk9vtc31i9d0lcd/apache-maven-3.8.6/conf/settings.xml
  ```

* 使用 Maven 命令行构建 Java 项目 并启动

  ```shell
  ./mvnw clean package
  ```

  ```shell
  java -jar demo-0.0.1-SNAPSHOT.jar
  ```

  ![image](assets/%E4%BD%BF%E7%94%A8%E8%84%9A%E6%89%8B%E6%9E%B6Java%E6%9E%84%E5%BB%BA%E9%A1%B9%E7%9B%AE/image-20230307184841-fx67uuk.png)​

* 利用 curl命令 – 文件传输工具 测试

  ![image](assets/%E4%BD%BF%E7%94%A8%E8%84%9A%E6%89%8B%E6%9E%B6Java%E6%9E%84%E5%BB%BA%E9%A1%B9%E7%9B%AE/image-20230307184941-3ko4a1y.png)​

‍
