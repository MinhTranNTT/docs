# MySQL 基础指令

参考文档：
- [MySQL官网](https://www.mysql.com/)
- [MySQL:: MySQL 8.0参考手册](https://dev.mysql.com/doc/refman/8.0/en/)
- [数据结构可视化](https://www.cs.usfca.edu/~galles/visualization/about.html)


创建数据库

```sql
CREATE DATABASE <数据库名>;
```

切换数据库

```sql
USE <数据库名>;
```

创建表

```sql
CREATE TABLE IF NOT EXISTS `request_record_info` (
	`id`               BIGINT AUTO_INCREMENT                                                    COMMENT '主键id',
	`request_no`       VARCHAR(128)                           NOT NULL                          COMMENT '请求记录编号',
	`status`    		   INT(2)       DEFAULT 0                 NOT NULL                          COMMENT '请求状态：0-请求中，1-请求成功，2-请求失败',
	`company_name`     VARCHAR(300) DEFAULT ''                NOT NULL                          COMMENT '企业名称',
	`request_message`  TEXT                                   NULL                              COMMENT '请求参数',
  `response_message` TEXT                                   NULL                              COMMENT '返回参数',
  `hidden_display`   INT(2)       DEFAULT 1                 NOT NULL                          COMMENT '隐藏显示：0-隐藏，1-显示',
  `create_name`      VARCHAR(32)                            NULL                              COMMENT '创建人名字',
	`update_name`      VARCHAR(32)                            NULL                              COMMENT '更新人名字',
  `create_id`        BIGINT                                 NULL                              COMMENT '创建人ID',
  `update_id`        BIGINT                                 NULL                              COMMENT '更新人ID',
	`create_time`      DATETIME     DEFAULT CURRENT_TIMESTAMP NULL                              COMMENT '创建时间',
  `update_time`      DATETIME     DEFAULT CURRENT_TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP  COMMENT '更新时间',
	PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8mb4 COMMENT '摸板表';

CREATE INDEX idx_request_no ON request_record_info ( request_no );
```

删除表数据
```sql
delete from <table-name>;
```

删除数据库

```sql
drop database <数据库名>;
```

删除数据表

```sql
DROP TABLE <数据表名> ;
```

清空表数据

```sh
DELETE FROM `sys_login_log`
```

LIKE 子句

```sql
SELECT field1, field2,...fieldN 
FROM table_name
WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'
```

参数
- 你可以在 WHERE 子句中指定任何条件。
- 你可以在 WHERE 子句中使用LIKE子句。
- 你可以使用LIKE子句代替等号 =。
- LIKE 通常与 % 一同使用，类似于一个元字符的搜索。
- 你可以使用 AND 或者 OR 指定一个或多个条件。
- 你可以在 DELETE 或 UPDATE 命令中使用 WHERE...LIKE 子句来指定条件。

UNION 操作符

```sql
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION [ALL | DISTINCT]
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];
```

参数
- expression1, expression2, ... expression_n: 要检索的列。
- tables: 要检索的数据表。
- WHERE conditions: 可选， 检索条件。
- DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。
- ALL: 可选，返回所有结果集，包含重复数据。

排序(ORDER BY)

```sql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
ORDER BY field1 [ASC [DESC][默认 ASC]], [field2...] [ASC [DESC][默认 ASC]]
```

参数

- 你可以使用任何字段来作为排序的条件，从而返回排序后的查询结果。
- 你可以设定多个字段来排序。
- 你可以使用 ASC 或 DESC 关键字来设置查询结果是按升序或降序排列。 默认情况下，它是按升序排列。
- 你可以添加 WHERE...LIKE 子句来设置条件


### 显示列信息
```sql
SHOW COLUMNS FROM <table-name>;
```

### 创建临时表
```sql
CREATE TEMPORARY TABLE SalesSummary (
	product_name VARCHAR(50) NOT NULL, 
	total_sales DECIMAL(12,2) NOT NULL DEFAULT 0.00,
	avg_unit_price DECIMAL(7,2) NOT NULL DEFAULT 0.00, 
	total_units_sold INT UNSIGNED NOT NULL DEFAULT 0
);
```

### 删除临时表
```sql
DROP TABLE SalesSummary;
```


### 复制表
```sql
SHOW CREATE TABLE runoob_tbl\G;
```


### 拷贝数据runoob_tbl表数据到runoobtbl中
```sql
INSERT INTO runoobtbl (runoob_id, runoob_title, runoob_author, submission_date) SELECT runoob_id, runoob_title, runoob_author, submission_date FROM runoob_tbl;
```


### 设置序列的开始值
```sql
CREATE TABLE insect(
	id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (id),
  name VARCHAR(30) NOT NULL, 
  date DATE NOT NULL,
  origin VARCHAR(30) NOT NULL
)engine=innodb auto_increment=100 charset=utf8; # 初始序列100
```


### 初始序列100
```sql
ALTER TABLE t AUTO_INCREMENT = 100;
```


### 统计重复数据
```sql
SELECT COUNT(*) as repetitions, last_name, first_name
	FROM person_tbl
  GROUP BY last_name, first_name
  HAVING repetitions > 1; # HAVING子句设置重复数大于1。
```

  
### DISTINCT（过滤重复数据）
```sql
SELECT DISTINCT last_name, first_nameFROM person_tbl;
```
