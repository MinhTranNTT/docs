# SpringMVC 中常用的注解有哪些?

@Controller 注解：如果某个类被 Controller 修饰后，将此类对象的创建过程交给 spring 容器来管理；此类的功能类似与 Servlet

@RequestMapping 注解：为 controller 中方法配置虚拟访问路径

@Scope 注解：指定 spring 容器创建对象的方式，取值 prototype 为多例、取值 singleton 为单例

@responseBody 注解：将 JavaBean/POJO/Map/List 等变成 json 数据

@requestBody 注解：将前端/app 等传递过来的 JSon 数据封装到 Map、JavaBean 中去

@requestParam 注解：当 Jsp 页面等中的字段名与 Controller 方法名不一致时，中间连接作用；当获取的字段不存在时，可以给一个默认值

@ControllerAdvice 注解：配置全局异常时和@ExceptionHandler 注解一起使用

@Lazy 注解：spring 创建对象时是否以懒加载的方式启动

‍
