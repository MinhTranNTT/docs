# Linux 导出、导入 MySQL 数据库

​​

### 1、Linux 导出 MySQL 数据库

#### 1.1、导出表结构

```shell
mysqldump -u fort -p -d fort > fort.sql
```

#### 1.2、导出数据和表结构

```shell
# mysql 路径 -u 用户名 -p[密码不要写在明显处] 数据库名 > 数据库名.sql
/usr/local/mysql/bin/ mysqldump -u fort -p fort > fort.sql
Enter password: 
```

### 2、Linux 导入 MySQL 数据库

#### 2.1、创建空数据库

```mysql
create database IF NOT EXISTS fort;
```

#### 2.2、选择数据库

```mysql
use fort;
```

#### 2.3、设置utf8

```mysql
set names utf8;
```

#### 2.4、导入数据

```mysql
source /usr/local/fort.sql;
```

### 3、shell 查询数据导出 xls格式

```shell
echo 'SELECT * FROM employee;' | mysql -uroot -p2wsx3edc fort > /root/liuzonglin/liu.xls
```

‍
