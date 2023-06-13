# SpringMVC 中如何开启注解处理器与适配器

需要在 springmvc.xml 文件中通过`<mvc:annotation-driven></mvc:annotation-driven>`​

```xml
<!-- 配置包扫描 -->
<context:component-scan base-package="cn.java.controller.*,cn.java.service.impl"></context:component-scan>

<!-- 开启mvc注解驱动 -->
<mvc:annotation-driven></mvc:annotation-driven>
```

‍
