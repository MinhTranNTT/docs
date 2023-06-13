# 怎么验证 MySQL 的索引是否满足需求？

使用 explain 查看 SQL 是如何执行查询语句的，从而分析你的索引是否满足需求。

explain 语法：`explain select * from table where type=1`​。

‍
