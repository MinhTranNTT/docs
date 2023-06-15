# CentOS7 安装 Tomcat

‍

**下载地址：**​[https://tomcat.apache.org/download-80.cgi](https://tomcat.apache.org/download-80.cgi)

**配置自启脚本**

```shell
~〉cat /etc/init.d/tomcat                                                                                                                                                                                       03/08/2023 04:48:08 下午
#!/bin/bash
#chkconfig:2345 21 90
#description:apache-tomcat-8
#processname:apache-tomcat-8
CATALANA_HOME=/opt/apache-tomcat-8.5.32		# tomcat地址
export JAVA_HOME=/opt/java/jdk1.8.0_171		# jdk地址
case $1 in
start)
  echo "Starting Tomcat..."
  $CATALANA_HOME/bin/startup.sh
  ;;

stop)
  echo "Stopping Tomcat..."
  $CATALANA_HOME/bin/shutdown.sh
  ;;

restart)
  echo "Stopping Tomcat..."
  $CATALANA_HOME/bin/shutdown.sh
  sleep 2
  echo
  echo "Starting Tomcat..."
  $CATALANA_HOME/bin/startup.sh
  ;;
*)
  echo "Usage: tomcat {start|stop|restart}"
  ;;
esac
~〉 
```

**tomcat 配置系统服务**

```shell
chkconfig --add /etc/init.d/tomcat		# 注册服务 
chmod +x /etc/init.d/tomcat			# 添加权限
```

**启动服务&amp;访问tomcat测试**

```shell
/etc/init.d/tomcat start # 或者 service tomcat start
```

​![image](assets/image-20230308163030-w2cngt4.png)​

‍
