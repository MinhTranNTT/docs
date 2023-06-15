# rsync命令 – 远程数据同步工具

参考资料：

[rsync命令 – 远程数据同步工具 – Linux命令大全(手册) (linuxcool.com)](https://www.linuxcool.com/rsync)

---

**语法格式：ls** [参数] 文件

‍

**常用参数：**

|参数|解释|
| ------| ----------------|
|-t|相当于去重了<br />|
|-a<br />|<br />|
|-u|<br />|
|-v|详细模式输出<br />|
|-z|压缩文件|
|-e||

‍

**参考案例：**

```shell
rsync -auvzte 'ssh -p 220' /home/db_log ftp_log rdp_log sftp_log ssh_log telnet_log root@10.96.128.133:/home/ 		# -t 相当于去重了
```

‍

‍
