# Error creating bean with name 'userMapper' defined in file

```xml
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userMapper' defined in file [D:\.github\.dome（开发示例）\spring\springboot-mybatisplus\build\classes\java\main\com\example\mapper\UserMapper.class]: Property 'sqlSessionFactory' or 'sqlSessionTemplate' are required
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1751) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:599) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:521) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:326) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:324) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:200) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:955) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:915) ~[spring-context-6.0.4.jar:6.0.4]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:584) ~[spring-context-6.0.4.jar:6.0.4]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:146) ~[spring-boot-3.0.2.jar:3.0.2]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:730) ~[spring-boot-3.0.2.jar:3.0.2]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:432) ~[spring-boot-3.0.2.jar:3.0.2]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:308) ~[spring-boot-3.0.2.jar:3.0.2]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1302) ~[spring-boot-3.0.2.jar:3.0.2]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1291) ~[spring-boot-3.0.2.jar:3.0.2]
	at com.example.MybatisApplication.main(MybatisApplication.java:12) ~[main/:na]
Caused by: java.lang.IllegalArgumentException: Property 'sqlSessionFactory' or 'sqlSessionTemplate' are required
	at org.springframework.util.Assert.notNull(Assert.java:204) ~[spring-core-6.0.4.jar:6.0.4]
	at org.mybatis.spring.support.SqlSessionDaoSupport.checkDaoConfig(SqlSessionDaoSupport.java:122) ~[mybatis-spring-2.0.5.jar:2.0.5]
	at org.mybatis.spring.mapper.MapperFactoryBean.checkDaoConfig(MapperFactoryBean.java:73) ~[mybatis-spring-2.0.5.jar:2.0.5]
	at org.springframework.dao.support.DaoSupport.afterPropertiesSet(DaoSupport.java:44) ~[spring-tx-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1797) ~[spring-beans-6.0.4.jar:6.0.4]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1747) ~[spring-beans-6.0.4.jar:6.0.4]
	... 16 common frames omitted


进程已结束,退出代码1

```

依赖缺失

gradle

```gradle
    implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.0'
```

‍
