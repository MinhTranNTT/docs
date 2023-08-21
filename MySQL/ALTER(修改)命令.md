# ALTER(修改)命令

```sql
create table `testalter_tbl` (
	i INT,
	c CHAR(1)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

## 删除 表字段 'i'
```sql
ALTER TABLE testalter_tbl DROP i;
```

## ADD 子句来向数据表中添加列
```sql
ALTER TABLE testalter_tbl ADD i INT;
```

##  FIRST (设定位第一列)
```sql
ALTER TABLE testalter_tbl ADD i INT FIRST;
```

## AFTER 字段名（设定位于某个字段之后）
```sql
ALTER TABLE testalter_tbl ADD i INT AFTER c;
```

## MODIFY（修改字段类型）
```sql
ALTER TABLE testalter_tbl MODIFY c CHAR(10);
```

## CHANGE（ CHANGE 关键字之后，紧跟着的是你要修改的字段名，然后指定新字段名及类型）
```sql
ALTER TABLE testalter_tbl CHANGE i j BIGINT;
ALTER TABLE testalter_tbl CHANGE j j INT;
```

## 指定字段 j 为 NOT NULL 且默认值为100 
```sql
ALTER TABLE testalter_tbl MODIFY j BIGINT NOT NULL DEFAULT 100;
```

## 修改默认值
```sql
ALTER TABLE testalter_tbl ALTER j SET DEFAULT 1000;
```

## DROP子句来删除字段的默认值
```sql
ALTER TABLE testalter_tbl ALTER j DROP DEFAULT;
```

## 修改表类型
```sql
ALTER TABLE testalter_tbl ENGINE = MYISAM;
```

## 查看数据表类型可以使用 SHOW TABLE STATUS 语句
```sql
SHOW TABLE STATUS LIKE 'testalter_tbl';
SHOW TABLE STATUS LIKE 'testalter_tbl'\G;
```

## 修改表名
```sql
ALTER TABLE testalter_tbl RENAME TO alter_tbl;
```

# 显示索引信息
```sql
SHOW INDEX FROM alter_tbl\G;
```
