# 如何判断MySQL中的索引有没有生效 

‍

在select语句前面加上explain就可以了

​`EXPLAIN SELECT surname, first_name FROM a, b WHERE a.id = b.id;`​

执行该语句，可以分析EXPLAIN后面SELECT语句的执行情况，并且能够分析出所查询表的一些特征。

EXPLAIN语句会返回很多列，其中与索引直接相关的列如下。

如果possible_keys行和key行都包含了某个索引，则说明在查询时使用了该索引。

* possible_keys列给出了MySQL在搜索数据记录时可选用的各个索引。
* key列是MySQL实际选用的索引。

‍
