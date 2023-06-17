1. JDK下载

For 64Bit

```shell
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u141-b15/336fa29ff2bb4ef291e347e091f7f4a7/jdk-8u141-linux-x64.tar.gz"
```

For 32Bit

```shell
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u141-b15/336fa29ff2bb4ef291e347e091f7f4a7/jdk-8u141-linux-i586.tar.gz"
```

2. 创建并解压jdk

```shell
mkdir /usr/local/java
tar -zxvf jdk-8u141-linux-x64.tar.gz -C /usr/local/java/
```

3. 设置环境变量

```shell
# vi /etc/profile

export JAVA_HOME=/usr/local/java/jdk1.8.0_141
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar 
```

4. 加载依赖

```shell
source /etc/profile
```

‍
