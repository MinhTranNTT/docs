# SpringMVC 框架与 Struts2 框架有什么区别?

SpringMVC 的核心类是 DispatcherServlet(是一个 Servlet);Struts2 的核心类是 StrutsPrepareExcuteFilter(是一个过滤器)

SpringMVC 获取参数直接在 controller 方法中定义形参接收；Struts2 获取参数时通过 Get 阅读器方法/模型驱动来获取

SpringMVC 是通过返回值为 String 类型来实现重定向与转发；Struts2 需要在 struts.xml 文件中配置

‍
