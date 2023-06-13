# 说说Bean的作用域,以及默认的作用域

‍

解题思路

得分点 singleton、prototype、request、session、globalSession 

标准回答 

在默认情况下，Bean 在 Spring IoC 容器中是 **singleton(单例模式) ​**的，但我们可以通过 @Scope 注解来修改 Bean 的作用域。

‍

这个注解有五个不同的取值，代表了 Bean 的五种不同类型作用域

* **singleton(单例模式) ：**在 Spring IoC 容器中仅存在一个实例，即 Bean 以单例的形式存在

* **prototype(原型模式) ：**每次调用 getBean() 时,都会执行 new 操作，返回一个新Bean 的实例

* **request(请求模式) ：**每次 HTTP 请求都会创建一个新的 Bean 的实例

* **session(会话模式) ：**同一个 HTTP Session 共享一个 Bean，不同的 HTTP Session 使用不同的 Bean 的实例

* **globalSession(全局会话模式)：**同一个全局的 Session 共享一个 Bean 的实例

‍

加分回答 

自定义作用域： 

1.实现自定义Scope类。 要将自定义scope集成到Spring容器当中就必须要实现 `org.springframework.beans.factory.config.Scope`​ 这个接口。接口中有两个常用的方法,分别用于底层存储机制获取和删除这个对象。

2.注册新的作用域 在实现一个或多个自定义Scope并测试通过之后,接下来便是如何让Spring容器来识别新的作用域。registerScope方法就是在Spring容器中用来注册新的作用域,这个方法有两个参数：第一个参数是与作用域相关的全局唯一的名称,第二个参数是准备实现的作用域的实例,就是实现Scope接口的实例。 `void registerScope(String scopeName, Scope scope);`​

3.使用自定义的Scope 要使用自定义的Scope,开发者不仅可以通过编程的方式来实现自定义bean作用域,也可以通过在xml中配置和使用自定义作用域,假设声明作用域名称为controller,那就可以在xml文件下做如下定义 

​`<bean class="&#46;&#46;&#46;" id="&#46;&#46;&#46;" scope="controller"></bean>`​

‍

**「注意：」** 使用 prototype 作用域需要慎重的思考，因为频繁创建和销毁 bean 会带来很大的性能开销。

‍
