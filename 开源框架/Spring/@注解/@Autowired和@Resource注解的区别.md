# @Autowired和@Resource注解的区别

‍

解题思路

得分点 注解来源、注入方式 

标准回答 

1. @Autowired是Spring提供的注解,@Resource是JDK提供的注解。 

2. @Autowired是只能按类型注入,@Resource默认按名称注入,也支持按类型注入。 

3. @Autowired按类型装配依赖对象,默认情况下它要求依赖对象必须存在,如果允许null值,可以设置它required属性为false,如果我们想使用按名称装配,可以结合@Qualifier注解一起使用。

@Resource有两个中重要的属性：name和type。name属性指定byName,如果没有指定name属性,当注解标注在字段上,即默认取字段的名称作为bean名称寻找依赖对象,当注解标注在属性的setter方法上,即默认取属性名作为bean名称寻找依赖对象。 

‍

加分回答 

@Resource装配顺序 

1. 如果同时指定了name和type,则从Spring上下文中找到唯一匹配的bean进行装配,找不到则抛出异常 

2. 如果指定了name,则从上下文中查找名称（id）匹配的bean进行装配,找不到则抛出异常 

3. 如果指定了type,则从上下文中找到类型匹配的唯一bean进行装配,找不到或者找到多个,都会抛出异常 

4. 如果既没有指定name,又没有指定type,则自动按照byName方式进行装配；如果没有匹配,则回退为一个原始类型进行匹配,如果匹配则自动装配；

‍
