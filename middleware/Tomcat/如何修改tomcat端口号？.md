# 如何修改tomcat端口号？

tomcat 的`server.xml`​文件

```xml
<!-- apache-tomcat-10.0.6\conf下的 server.xml -->
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

‍
