# 解决 cargo 堵塞 blocking 问题

如果在运行cargo的时候，出现：<u>Blocking waiting for file lock on package
cache</u>

请产生.cargo文件夹下面的.package_cache文件：

```text
rm ~/.cargo/.package-cache
```

‍
