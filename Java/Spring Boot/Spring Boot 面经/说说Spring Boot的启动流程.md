# 说说Spring Boot的启动流程

‍

解题思路

得分点 调用run方法,run方法执行流程 

标准回答 

当Spring Boot项目创建完成后会默认生成一个Application的入口类,这个类中的mn方法可以启动Spring Boot项目,在mn方法中,通过SpringApplication的静态方法,即run方法进行SpringApplication的实例化操作,然后再针对实例化对象调用另外一个run方法来完成整个项目的初始化和启动。

SpringApplication调用的run方法重点做了以下操作： 

- 获取监听器参数配置 

- 打印Banner信息 

- 创建并初始化容器 

- 监听器发送通知 

‍

加分回答 

SpringApplication实例化过程中相对重要的配置： 

- 项目启动类 SpringbootDemoApplication.class 设置为属性存储起来 this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources)); 

- 设置应用类型是 SERVLET 应用还是 REACTIVE 应用 this.webApplicationType = WebApplicationType.deduceFromClasspath(); 

- 设置初始化器(Initializer),最后会调用这些初始化器 

- 所谓的初始化器就是 org.springframework.context.ApplicationContextInitializer 的实现类,在 Spring 上下文被刷新之前进行初始化的操作 setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class)); 

- 设置监听器(Listener) setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class)); 

- 初始化 mnApplicationClass 属性:用于推断并设置项目 mn()方法启动的主程序启动类 this.mnApplicationClass = deduceMnApplicationClass();

‍
