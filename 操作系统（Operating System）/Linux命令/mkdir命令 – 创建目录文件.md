# mkdir命令 – 创建目录文件

参考资料：

[mkdir命令 – 创建目录文件 – Linux命令大全(手册) (linuxcool.com)](https://www.linuxcool.com/mkdir)

---

‍

**语法格式：**mkdir [参数] 目录  

‍

**常用参数：**

|参数|解释|
| ------| ------------------------------|
|-p|递归创建多级目录|
|-m|建立目录的同时设置目录的权限|

‍

**参考案例：**

```shell
mkdir -p ./root/{mydir,datadir,conf,source}		# 指定目录下创建多个文件夹
mkdir -pm 700 ./root/{mydir,datadir,conf,source}		# 指定目录下创建多个文件夹 并 授权 700
```

‍
