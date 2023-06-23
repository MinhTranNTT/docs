# ls命令 – 显示指定工作目录下的文件及属性信息

参考资料：

[ls命令 – 显示指定工作目录下的文件及属性信息 – Linux命令大全(手册) (linuxcool.com)](https://www.linuxcool.com/ls)

---

‍

**语法格式：ls** [参数] 文件

‍

**常用参数：**

|参数|解释|
| ------| --------------------------------------------------|
|-a|显示所有文件及目录 (包括以“.”开头的隐藏文件)<br />|
|-l<br />|使用长格式列出文件及目录的详细信息<br />|
|-S|根据文件大小排序<br />|

‍

**参考案例：**

```shell
root@ubuntu:~# ls -l l[s].txt
-rw-r--r-- 1 root root 13957 6月   3 22:46 ls.txt
root@ubuntu:~# ls -l l[ls].txt
-rw-r--r-- 1 root root 13957 6月   3 22:46 ls.txt
root@ubuntu:~# ls -l l[l-s].txt
-rw-r--r-- 1 root root 13957 6月   3 22:46 ls.txt
root@ubuntu:~# ls -l l[!l].txt
-rw-r--r-- 1 root root 13957 6月   3 22:46 ls.txt
```

‍
