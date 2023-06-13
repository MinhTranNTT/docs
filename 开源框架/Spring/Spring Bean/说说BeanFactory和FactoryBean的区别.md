# 说说BeanFactory和FactoryBean的区别

‍

解题思路

得分点 BeanFactory定义、FactoryBean定义 

标准回答 

它们在拼写上非常相似,一个是Factory,也就是IoC容器或对象工厂,一个是Bean。

在Spring中,所有的Bean都是由BeanFactory（也就是IoC容器）来进行管理的。

但对FactoryBean而言,这个Bean不是简单的Bean,而是一个能产生或者修饰对象生成的工厂Bean,它的实现与设计模式中的工厂模式和修饰器模式类似。

用户使用容器时,可以使用转义符“＆”来得到FactoryBean本身,用来区分获取FactoryBean产生的对象和获取FactoryBean本身。

‍

举例来说,如果 alphaObject 是一个 FactoryBean，那么使用 alphaObject 得到的是 FactoryBean ,而不是 alphaObject 这个 FactoryBean 产生出来的对象。其中, alphaObject 是定义Bean 的时候所指定的名字。

‍

精简回答

BeanFactory 是最基础的IOC容器，给Spring 的容器定义一套规范，给IOC容器提供了一套完整的规范；

FactoryBean 只是SpringIOC容器创建Bean的一种形式；

‍

加分回答 

BeanFactory定义方法： 

* getBean(String name): Spring容器中获取对应Bean对象的方法,如存在,则返回该对象
* contnsBean(String name)：Spring容器中是否存在该对象
* isSingleton(String name)：通过beanName是否为单例对象

* isPrototype(String name)：判断bean对象是否为多例对象

* isTypeMatch(String name, ResolvableType typeToMatch):判断name值获取出来的bean与typeToMath是否匹配

* getType(String name)：获取Bean的Class类型

* getAliases(String name):获取name所对应的所有的别名 FactoryBean方法：

* T getObject()：返回实例

* Class getObjectType();:返回该装饰对象的Bean的类型

* default boolean isSingleton():Bean是否为单例

‍
