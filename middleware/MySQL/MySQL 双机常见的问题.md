# MySQL 双机常见的问题

参考文档：

[MySQL报错ERROR1872(HY000):Slave failed to initialize怎么解决 - MySQL数据库 - 亿速云 (yisu.com)](https://www.yisu.com/zixun/30701.html)

‍

### A机数据同步B机正常，B机同步A机失败？

#### 正常

1. 登陆B机数据库：`mysql –u root –p 2wsx3edc`​
2. 查看同步是否正常：`show slave status\G;`​

    * A 如果出现：
  
    ![image](assets/MySQL%20%E5%8F%8C%E6%9C%BA%E5%B8%B8%E8%A7%81%E7%9A%84%E9%97%AE%E9%A2%98/image-20230208184130-vbv8uyj.png)​

    没有添加同步信息，双机没有组合成功。

    * B 如果出现：

    ![image](assets/MySQL%20%E5%8F%8C%E6%9C%BA%E5%B8%B8%E8%A7%81%E7%9A%84%E9%97%AE%E9%A2%98/image-20230208184500-xse6en0.png)​

    `Slave_IO_Running`​ 与 `Slave_SQL_Running`​ 状态都要为 `Yes`​

#### 不正常

1. 如果从库的 Slave 未启动，`Slave_IO_Running`​ 为 NO 。  
    可能是主库是的master的信息有变化，  
    查看主库 `show master status;`​

    ![image](assets/MySQL%20%E5%8F%8C%E6%9C%BA%E5%B8%B8%E8%A7%81%E7%9A%84%E9%97%AE%E9%A2%98/image-20230208184304-z2oghv9.png)​

    记录下`File, Position`​字段，假设为‘mysql-bin.000004’,98;  

    在从库执行：
    ```sql
    mysql> stop slave;
    mysql> change master to master_log_file='binlog.000004',master_log_pos=98;
    mysql> start slave;
    ```
2. 如果从库的 `slave_sql_running`​ 为 NO。  
    Err文件中记录：  
    ​`Slave:Error "Duplicate entry '1' for key 1" on query.....`​  
    可能是 master 未向 slave 同步成功，但 slave 中已经有了记录。造成的冲突可以在从库上执行 `set global sql_slave_skip_counter=n;`​（n代表同步不成功数据条数）  
    跳过几步。再 `restart slave`​ 就可以了。


### TO 语句连接 master

```sql
change master to master_host = '10.10.1.2', master_port = 3306, master_user = 'fort', master_password = '2wsx3edc', master_log_file = 'binlog.000002', master_log_pos = 154;
```

‍
