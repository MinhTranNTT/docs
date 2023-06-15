# Windows 安装 Maven

官网下载地址：[Maven – Download Apache Maven](https://maven.apache.org/download.cgi)

---

### 变量：JAVAHOME

变量值：

```shell
D:\.github\.m2\settings.xml
```

### 变量：MAVEN_HOME

变量值：

```shell
D:\program\apache-maven\apache-maven-3.9.0
```

### 变量：Path

```shell
%MAVEN_HOME%\bin
```

### 测试

```shell
mvn -v
```

### Maven 核心配置文件

文件路径：`apache-maven/conf/settings.xml`​

#### 配置依赖仓库

```xml
  <localRepository>D:\.github\.m2\repository</localRepository>
```

#### 阿里云公共仓库是central仓和jcenter仓的聚合仓

```xml
<mirrors>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云公共仓库</name>
		<url>https://maven.aliyun.com/repository/public</url>
	</mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云谷歌仓库</name>
		<url>https://maven.aliyun.com/repository/google</url>
	</mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云阿帕奇仓库</name>
		<url>https://maven.aliyun.com/repository/apache-snapshots</url>
	</mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云spring仓库</name>
		<url>https://maven.aliyun.com/repository/spring</url>
	</mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云spring插件仓库</name>
		<url>https://maven.aliyun.com/repository/spring-plugin</url>
	</mirror>
	<mirror>
		<id>maven</id>
		<mirrorOf>*</mirrorOf>
		<name>maven仓库</name>
		<url>https://mvnrepository.com/</url>
	</mirror>
	<mirror>
		<id>nexus-aliyun</id>
		<mirrorOf>central</mirrorOf>
		<name>Nexus aliyun</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror>
</mirrors>
```

#### 配置 Maven 工程的基础 JDK1.8 版本编译项目

```xml
<profiles>
	<profile>
		<id>JDK-1.8</id>
		<activation>
			<activeByDefault>true</activeByDefault>
			<jdk>1.8</jdk>
		</activation>
		<properties>
			<maven.compiler.source>1.8</maven.compiler.source>
			<maven.compiler.target>1.8</maven.compiler.target>
			<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
		</properties>
	</profile>
</profiles>
```

### 变量：JAVAHOME

变量值：

```shell
D:\.github\.m2\settings.xml
```

### 变量：MAVEN_HOME

变量值：

```shell
D:\program\apache-maven\apache-maven-3.8.6
```

### 变量：Path

```shell
%MAVEN_HOME%\bin
```

### 测试

```shell
mvn -v
```

​​

‍
