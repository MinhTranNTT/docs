# 什么是 Spring Bean？Spring Bean 的生命周期？在 Spring 中如何注入一个集合

Spring Bean: `<bean id="smallDog" class="cn.java.entity.Dog" scope="prototype" lazy="true/false/default">`​

Bean 的生命周期：

​`<bean id="smallDog" class="cn.java.entity.Dog" init-method="init" destroy-method="destroy">`​

1. 在 applicationContext.xml 文件中指定何种类对象被 spring 容器管理
2. spring 容器帮助我们创建一个 Dog 对象，底层还是调用构造方法
3. 执行 init 初始化方法
4. 执行 Dog 对象中的其他方法
5. 当销毁 Dog 对象时，执行 Dog 对象中的 destroy

Spring 注入集合：通过 list、map、set、props 这些标签来实现的

```xml
<!-- 通过set修改器实现di -->
<bean id="dS" class="cn.java.di_set6.DataSource">
	<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="port" value="3306"></property>
  
    <!-- 绘list绘赋值 -->
    <property name="list">
    	<list>
        	<value>liuzonglin</value>
            <value>18</value>
            <ref bean="oldMa"/>
        </list>
    </property>
    <!-- 绘set绘赋值 -->
    <property name="set">
    	<set>
        	<value>liuzonglin</value>
            <value>18</value>
            <ref bean="oldMa"/>
        </set>
    </property>
    <!-- 绘map绘赋值 -->
    <property name="set">
    	<map>
        	<entry key="username" value="小红"></entry>
            <entry key-ref="oldMa" value-ref="oldMa"></entry>                               
        </map>
    </property>
    <!-- 绘Properties类型属性赋值 -->
    <property name="set">
    	<props>
        	<prop key="username">老马</prop>
            <prop key="uName">哈希值</prop>
            <prop key="usName">liuzonglin</prop>
        </props>
    </property>
</bean>
```

‍
