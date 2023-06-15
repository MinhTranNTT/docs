# Redis 持久化有几种方式？

Redis 的持久化有两种方式，或者说有两种策略：

* RDB（Redis Database）：指定的时间间隔能对你的数据进行快照存储。
* AOF（Append Only File）：每一个收到的写命令都通过write函数追加到文件中。

‍
