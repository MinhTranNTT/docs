# Linux 下转换 Windows 文件格式为 unix

sed 指令清理文件中的隐藏符号

```shell
sed -i 's/\r//' <filename>
```

安装 dos2unix 应用

```shell
rpm -ivh dos2unix-6.0.3-7.el7.x86_64.rpm
yum install -y dos2unix
dos2unix <filename>			# 清理文件中的隐藏符号 ^M
```

‍
