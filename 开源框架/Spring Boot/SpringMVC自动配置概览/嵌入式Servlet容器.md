# 嵌入式Servlet容器

* 默认支持的webServer

  * Tomcat, Jetty, or Undertow
  * ServletWebServerApplicationContext 容器启动寻找ServletWebServerFactory 并引导创建服务器
* 切换服务器  
  ​![image](assets/image-20230306170339-oew1300.png)​

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

* **原理**

  * SpringBoot应用启动发现当前是Web应用。web场景包-导入tomcat
  * web应用会创建一个web版的ioc容器 `ServletWebServerApplicationContext`​
  * ​`ServletWebServerApplicationContext`​ 启动的时候寻找 `ServletWebServerFactory`​（Servlet 的web服务器工厂---> Servlet 的web服务器）
  * SpringBoot 底层默认有很多的 WebServer 工厂；`TomcatServletWebServerFactory`​, `JettyServletWebServerFactory`​, or `UndertowServletWebServerFactory`
  * ​底层直接会有一个自动配置类。`ServletWebServerFactoryAutoConfiguration`
  * `ServletWebServerFactoryAutoConfiguration`导入了`ServletWebServerFactoryConfiguration`（配置类）
  * ​`ServletWebServerFactoryConfiguration ​`配置类 根据动态判断系统中到底导入了那个Web服务器的包。（默认是web-starter导入tomcat包），容器中就有 `TomcatServletWebServerFactory`
  * ​`TomcatServletWebServerFactory`​ 创建出Tomcat服务器并启动；`TomcatWebServer`​的构造器拥有初始化方法initialize---this.tomcat.start();
  * `内嵌服务器，就是手动把启动服务器的代码调用（tomcat核心jar包存在）`

## 2、定制**Servlet**容器

* 实现  WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>

  * 把配置文件的值和`ServletWebServerFactory ​`进行绑定
* 修改配置文件 server.xxx
* 直接自定义 ConfigurableServletWebServerFactory

**xxxxxCustomizer：定制化器，可以改变xxxx的默认规则**

```java
import org.springframework.boot.web.server.WebServerFactoryCustomizer;
import org.springframework.boot.web.servlet.server.ConfigurableServletWebServerFactory;
import org.springframework.stereotype.Component;

@Component
public class CustomizationBean implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory> {

    @Override
    public void customize(ConfigurableServletWebServerFactory server) {
        server.setPort(9000);
    }

}
```

‍
