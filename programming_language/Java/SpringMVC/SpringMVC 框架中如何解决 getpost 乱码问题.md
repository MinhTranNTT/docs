# SpringMVC 框架中如何解决 getpost 乱码问题

在 SpringMVC 框架中为我们提供了一个解决乱码问题的过滤器(CharacterEncodingFilter)，需要在 web.xml文件中进行配置。

```xml
<!-- 配置乱码的过滤器 -->
<filter>
    <filter-name>CharaterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
    	<param-name>encoding</param-name>
	<!--当前工程下所有的文件采用UTF-8的编码-->
        <param-value>UTF-8</param-value>	
    </init-param>
</filter>
```

```java
public class EncodingFilter implements Filter {		// 自定义处理乱码的工具类
  
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
        // 1.进行强转
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse request = (HttpServletResponse) resp;
        // 2.修改request、response的编码--默认为ISO-8859-1
        request.setCharacterEncoding("UTF-8");
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        // 3.放行
        chain.doFilter(request,response);
    }
}
```

‍
