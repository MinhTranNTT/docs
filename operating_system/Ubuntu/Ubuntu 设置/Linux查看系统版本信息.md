# Linux查看系统版本信息

参考资料：

[Linux常用命令分类汇总](https://blog.51cto.com/longlei/1970770#:~:text=Linux%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4%E5%88%86%E7%B1%BB%E6%B1%87%E6%80%BB%EF%BC%881%EF%BC%89%201%201%E3%80%81%E6%96%87%E4%BB%B6%E7%AE%A1%E7%90%86%E5%92%8C%E5%AD%97%E7%AC%A6%E5%A4%84%E7%90%86%E5%91%BD%E4%BB%A4%202%202%E3%80%81%E6%96%87%E6%9C%AC%E7%BC%96%E8%BE%91%E5%99%A8vim%E7%9A%84%E4%BD%BF%E7%94%A8%203,3%E3%80%81%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86%E5%91%BD%E4%BB%A4%204%204%E3%80%81%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86%E5%91%BD%E4%BB%A4%205%205%E3%80%81%E6%9F%A5%E6%89%BE%E5%91%BD%E4%BB%A4%206%206%E3%80%81%E5%B8%AE%E5%8A%A9%E5%91%BD%E4%BB%A4)

[查看Linux系统版本信息的几种方法](https://www.cnblogs.com/opensmarty/p/10936315.html)

---

```lua
uname -a		# 查看Linux内核版本命令
cat /proc/version 	# 查看Linux内核版本命令
cat /etc/issue		# 此命令也适用于所有的Linux发行版。
lsb_release -a		# 即可列出所有版本信息：
```

‍

‍
