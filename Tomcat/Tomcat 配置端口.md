# Tomcat 配置端口

‍

默认协议https，默认端口443

可通过修改tomcat配置文件`/usr/local/tomcat6/conf/server.xml`​来调整协议和端口、

![](assets/Tomcat%20%E9%85%8D%E7%BD%AE%E7%AB%AF%E5%8F%A3/clip_image002-20230208191911-k0s3woo.jpg)​

开启证书认证：`clientAuth="true"`​ ，重启tomcat服务生效

关闭证书认证：`clientAuth="false"`​ ，重启tomcat服务生效

配置第三方证书：

`keystoreFile="/root/sd/123.pfx"`​   根证书位置，所有者root，权限600

`keystoreType="PKCS12"`​   证书类型

`keystorePass="JnOKwX1K"`​   证书密码

‍
