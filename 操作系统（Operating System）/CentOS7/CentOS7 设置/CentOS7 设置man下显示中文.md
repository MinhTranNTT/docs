# CentOS7 设置man下显示中文

参考资料：

[centos7设置man下显示中文](https://blog.csdn.net/caikeng/article/details/120580374#:~:text=centos7%E8%AE%BE%E7%BD%AE%E4%B8%AD%E6%96%87man%201%E3%80%81%E6%9F%A5%E6%89%BEman%E4%B8%AD%E6%96%87%E5%AE%89%E8%A3%85%E5%8C%85%20yum%20list%20%7Cgrep%20man.%2Azh,1%20%E7%94%B1%E6%AD%A4%E5%8F%AF%E4%BB%A5%E6%89%BE%E5%88%B0%E4%BB%A5%E4%B8%8A%E5%AE%89%E8%A3%85%E5%8C%85%EF%BC%8C%E5%A6%82%E6%9E%9C%E6%89%BE%E4%B8%8D%E5%88%B0%EF%BC%8C%E6%89%A7%E8%A1%8C%20yum%20-y%20update%20%E6%9B%B4%E6%96%B0%E5%AE%89%E8%A3%85%E5%8C%85%E3%80%82)

---

```java
yum list |grep man.*zh
yum install man-pages-zh-CN.noarch
```

‍
