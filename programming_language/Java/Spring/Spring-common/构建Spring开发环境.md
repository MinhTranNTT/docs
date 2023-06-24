# 构建Spring开发环境

[https://spring.io/projects/spring-framework#learn](https://spring.io/projects/spring-framework#learn)

---

## 构建Spring开发环境

* JDK：Java17+**（Spring6要求JDK最低版本是Java17）**
* Maven：3.6+
* Spring：6.0.2

* **构建父模块spring6**

  在idea中，依次单击 File -> New -> Project -> New Project

  ![image](assets/%E6%9E%84%E5%BB%BASpring%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/image-20230209131607-wky89sk.png)​

  点击“Create”

  ![image](assets/%E6%9E%84%E5%BB%BASpring%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/image-20230209131830-3yvm8cm.png)​

  删除src目录

* **构建子模块 spring6-first**

  ![image](assets/%E6%9E%84%E5%BB%BASpring%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/image-20230209132021-ddgzepz.png)​

  点击 Create 完成

  ![image](assets/%E6%9E%84%E5%BB%BASpring%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/image-20230209132143-0bnoy95.png)​

  父工厂 自动加载依赖

  ```xml
      <packaging>pom</packaging>
      <modules>
          <module>spring-first</module>
      </modules>
  ```

* **添加依赖：**

  ```xml
      <dependencies>
          <!--spring context依赖-->
          <!--当你引入Spring Context依赖之后，表示将Spring的基础依赖引入了-->
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-context</artifactId>
              <version>6.0.2</version>
          </dependency>

          <!--junit5测试-->
          <dependency>
              <groupId>org.junit.jupiter</groupId>
              <artifactId>junit-jupiter-api</artifactId>
              <version>5.3.1</version>
              <scope>test</scope>
          </dependency>
      </dependencies>
  ```

* **扩展 Kotlin 依赖**

  ```xml
      <properties>
          <kotlin.version>1.8.0</kotlin.version>
      </properties>

      <dependencies>
          <dependency>
              <groupId>org.jetbrains.kotlin</groupId>
              <artifactId>kotlin-stdlib-jdk8</artifactId>
              <version>${kotlin.version}</version>
          </dependency>
          <dependency>
              <groupId>org.jetbrains.kotlin</groupId>
              <artifactId>kotlin-test</artifactId>
              <version>${kotlin.version}</version>
              <scope>test</scope>
          </dependency>
      </dependencies>

      <build>
          <plugins>
              <plugin>
                  <groupId>org.jetbrains.kotlin</groupId>
                  <artifactId>kotlin-maven-plugin</artifactId>
                  <version>${kotlin.version}</version>
                  <executions>
                      <execution>
                          <id>compile</id>
                          <phase>compile</phase>
                          <goals>
                              <goal>compile</goal>
                          </goals>
                      </execution>
                      <execution>
                          <id>test-compile</id>
                          <phase>test-compile</phase>
                          <goals>
                              <goal>test-compile</goal>
                          </goals>
                      </execution>
                  </executions>
                  <configuration>
                      <jvmTarget>${maven.compiler.target}</jvmTarget>
                  </configuration>
              </plugin>
          </plugins>
      </build>

  </project>
  ```

* **查看依赖：**

  ![image](assets/%E6%9E%84%E5%BB%BASpring%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/image-20230209132721-pypc7r3.png)​

* 创建java类

  ```shell
  package com.atguigu.spring6.bean;

  public class HelloWorld {
    
      public void sayHello(){
          System.out.println("helloworld");
      }
  }
  ```

* 创建配置文件

  在resources目录创建一个 Spring 配置文件 beans.xml（配置文件名称可随意命名，如：springs.xml）

  ![img007](assets/%E6%9E%84%E5%BB%BASpring%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/img007-20230209125529-83cjuqh.png)​

* **bean.xml 初始格式**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

      <!--
      配置HelloWorld所对应的bean，即将HelloWorld的对象交给Spring的IOC容器管理
      通过bean标签配置IOC容器所管理的bean
      属性：
          id：设置bean的唯一标识，`id 可以随便起名，一般为类的小写`
          class：设置bean所对应类型的全类名
  	-->
      <bean id="helloWorld" class="org.example.HelloWorld"/>
    
  </beans>
  ```

* Java 测试

  ```java
  package org.example

  import org.junit.jupiter.api.Test;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;

  public class HelloWorldTest {

      @Test
      public void testHelloWorld(){
          ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
          HelloWorld helloworld = (HelloWorld) ac.getBean("helloWorld");
          helloworld.sayHello();
      }
  }
  ```

* kotlin 测试

  ```kotlin
  package org.example
  
  import org.junit.jupiter.api.Test
  import org.springframework.context.ApplicationContext
  import org.springframework.context.support.ClassPathXmlApplicationContext


  class HelloWorldTest {

      @Test
      fun testHelloWorld() {
    // ClassPathXmlApplicationContext 加载 spring 配置文件，对象创建
          val ac: ApplicationContext = ClassPathXmlApplicationContext("bean.xml")
    
    // 获取创建的对象
          val bean = ac.getBean("helloWorld") as HelloWorld
    
    // 使用对象调用方法进行测试
          bean.sayHello()
      }
  }
  ```

* 运行测试程序

  ​![image-20221031172354535](assets/image-20221031172354535-20230209125529-jjz9kxg.png)​

## Spring实例化细节

1. **底层是怎么创建对象的，是通过反射机制调用无参数构造方法吗？**

    修改HelloWorld类：

    ```java
    package com.atguigu.spring6.bean;

    public class HelloWorld {

        public HelloWorld() {
            System.out.println("无参数构造方法执行"); // 创建对象时确实调用了无参数构造方法
        }

        public void sayHello(){
            System.out.println("helloworld");
        }
    }
  ```

    执行结果：
    
    ​![image-20221031181430720](assets/image-20221031181430720-20230209125529-ihgrajo.png)​
    
    **测试得知：创建对象时确实调用了无参数构造方法。**

2. **Spring是如何创建对象的呢？原理是什么？**

    ![01-入门案例分析](assets/%E6%9E%84%E5%BB%BASpring%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/01-%E5%85%A5%E9%97%A8%E6%A1%88%E4%BE%8B%E5%88%86%E6%9E%90-20230209142636-2u0mcsk.png)​

    ‍

    **反射案例**

    ```java
    // dom4j解析bean.xml文件，从中获取class属性值，类的全类名
    // 通过反射机制调用无参数构造方法创建对象
    Class clazz = Class.forName("com.atguigu.spring6.bean.HelloWorld");

    // newInstance() 方法过时了
    // Object obj = clazz.newInstance();

    Object object = clazz.getDeclaredConstructor().newInstance();
    ```

3. **把创建好的对象存储到一个什么样的数据结构当中了呢？**

    bean对象最终存储在spring容器中，在spring源码底层就是一个map集合，存储bean的map在 `DefaultListableBeanFactory`​​ 类中：

    ```java
    package org.springframework.beans.factory.support;
    
    @SuppressWarnings("serial")
    public class DefaultListableBeanFactory extends AbstractAutowireCapableBeanFactory
    		implements ConfigurableListableBeanFactory, BeanDefinitionRegistry, Serializable {
    
    	/** Map of bean definition objects, keyed by bean name. */
    	// key: 唯一标识， bean-id
    	// value: Bean 的描述信息
    	private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<>(256);
    }
    ```

    Spring容器加载到Bean类时 , 会把这个类的描述信息, 以包名加类名的方式存到beanDefinitionMap 中,  
    Map<String,BeanDefinition> , 其中 String是Key , 默认是类名首字母小写 , BeanDefinition , 存的是类的定义(描述信息) , 我们通常叫BeanDefinition接口为 : bean的定义对象。

‍

‍
