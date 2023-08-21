数据库导入

```sql
source /home/abc/abc.sql  # 导入备份数据库

create database abc;      # 创建数据库
use abc;                  # 使用已创建的数据库 
set names utf8;           # 设置编码
source /home/abc/abc.sql  # 导入备份数据库
```