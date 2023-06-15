# CentOS7 安装 zookeeper

**安装JDK 环境：**CentOS7 安装 JDK

**下载地址：**​[Index of /dist/zookeeper/zookeeper-3.4.11 (apache.org)](https://archive.apache.org/dist/zookeeper/zookeeper-3.4.11/)

```shell
wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.11/zookeeper-3.4.11.tar.gz
```

**配置启动文件**

```shell
~〉cat /etc/rc.d/init.d/zookeeper                                                                                                                                                                                           03/08/2023 04:48:08 下午
#!/bin/bash
#chkconfig:2345 20 90
#description:zookeeper
#processname:zookeeper
ZK_PATH=/usr/local/zookeeper            		# zookeeper存放位置
export JAVA_HOME=/usr/local/java/jdk1.8.0_171		# JDK 路径
case $1 in
         start) sh  $ZK_PATH/bin/zkServer.sh start;;
         stop)  sh  $ZK_PATH/bin/zkServer.sh stop;;
         status) sh  $ZK_PATH/bin/zkServer.sh status;;
         restart) sh $ZK_PATH/bin/zkServer.sh restart;;
         *)  echo "require start|stop|status|restart"  ;;
esac
~〉 
```

**把脚本注册为Service**

```shell
chkconfig -add zookeeper
chkconfig --list
```

​![image](assets/image-20230308162730-fa3fb0h.png)​

**增加权限**

```shell
chmod +x /etc/init.d/zookeeper
```

配置zookeeper

初始化zookeeper配置文件

```shell
cp /usr/local/zookeeper/conf/zoo_sample.cfg zoo.cfg	# 拷贝文件
```

启动zookeeper

```shell
/etc/init.d/zookeeper start # 或者 service zookeeper start
```

‍
