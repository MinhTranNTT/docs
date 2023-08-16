参考：https://zhuanlan.zhihu.com/p/411979704

已知订单有三种属性：订单ID、订单日期（精确到天）、订单完成状态（完成、未完成）。请设计出表示订单的数据库表，为其指定主键，并为以下查询建立索引：
1. 查找某一ID的订单内容；
2. 查找近一周的所有订单；
3. 查找所有未完成的订单；
4. 查找近一个月内所有已完成的订单。

下面是一个表示订单的数据库表的设计，其中包括主键和索引的指定：
```sql
CREATE TABLE orders (
order_id INT PRIMARY KEY,
order_date DATE,
order_status VARCHAR(10) NOT NULL,
customer_id INT,
deleted_at TIMESTAMP NULL,
FOREIGN KEY (order_id) REFERENCES customers(customer_id),
FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
CREATE INDEX order_id_index ON orders(order_id);
CREATE INDEX order_date_index ON orders(order_date);
CREATE INDEX order_status_index ON orders(order_status);
CREATE INDEX customer_id_index ON orders(customer_id);

CREATE TABLE deleted_orders (
order_id INT PRIMARY KEY,
order_date DATE,
order_status VARCHAR(10) NOT NULL,
customer_id INT,
deleted_at TIMESTAMP NOT NULL,
FOREIGN KEY (order_id) REFERENCES orders(order_id),
FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
CREATE INDEX order_id_index ON deleted_orders(order_id);
CREATE INDEX order_date_index ON deleted_orders(order_date);
CREATE INDEX order_status_index ON deleted_orders(order_status);
CREATE INDEX customer_id_index ON deleted_orders(customer_id);
```
基础数据表，一张逻辑删除表

解释：

- CREATE TABLE 语句用于创建一个名为 orders 的表，包含三个列：order_id、order_date 和 order_status。
- order_id 列指定为主键，确保每个订单都有一个唯一的 ID。
- order_date 列存储订单日期，精确到天。
- order_status 列指定订单的完成状态，可以是 完成 或 未完成。
- FOREIGN KEY 语句用于将订单表与客户表关联起来，确保每个订单都属于一个客户。
- CREATE INDEX 语句用于创建索引，以便更快地执行查询操作。
- order_id_index 索引基于 order_id 列创建，以便快速查找特定 ID 的订单。
- order_date_index 索引基于 order_date 列创建，以便快速查找近一周的订单。
- order_status_index 索引基于 order_status 列创建，以便快速查找所有未完成的订单和近一个月内已完成的订单。

通过使用上述索引，可以更快地执行以下查询操作：

1. 查找某一 ID 的订单内容：
    ```sql
    SELECT * FROM orders WHERE order_id =?;  
    ```
2. 查找近一周的所有订单：
    ```sql
    SELECT * FROM orders WHERE order_date >= DATEADD(week, -1, GETDATE()) AND order_date <= DATEADD(week, 0, GETDATE());  
    ```
3. 查找所有未完成的订单：
    ```sql
    SELECT * FROM orders WHERE order_status = '未完成';
    ```
4. 查找近一个月内所有已完成的订单：
    ```sql
    SELECT * FROM orders WHERE order_status = '已完成' AND order_date >= DATEADD(month, -1, GETDATE()) AND order_date <= DATEADD(month, 0, GETDATE());
    ```


```
Input a: 1->2->3->4
Input b:    2->3->4
Output:  1->4->6->8
```

https://play.rust-lang.org/?version=stable&mode=debug&edition=2021