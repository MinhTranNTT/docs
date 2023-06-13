# MySQL 重复数据清理

```sql
use fort;

# 清理 rel_rule_resourcegroup_account 表
delete from rel_rule_resourcegroup_account where id not in (select min_id from (select min(id) as min_id from rel_rule_resourcegroup_account group by resourceGroupID,employeeID,accountName,ruleID,empGroupId )as x);

# 清理rel_rule_resourcegroup 表；先将不重复数据导出到新表
create table x_user2 as (select distinct * from rel_rule_resourcegroup);

# 将 rel_rule_resourcegroup 表数据清空
delete from rel_rule_resourcegroup;

# 将新表数据导回 rel_rule_resourcegroup
insert into rel_rule_resourcegroup select * from x_user2;

# 将新表x_user2 删除
DROP TABLE x_user2;
```

‍

‍

```bash
#!/bin/bash
# path=$(cd `dirname $0`;pwd)

# 数据库更新
# /opt/mysql/server-5.6/bin/mysql -ufort -p2wsx3edc -f <./data_clean.sql

# 目的：清理 debian堡垒机 mysql重复性数据
# 针对操作系统 Debian 6.0.7

#!/bin/bash
basepath=$(
  cd $(dirname $0)
  pwd
)
version=$(awk '{print $3}' /etc/issue)
if [ $(echo $version) = "6.0" ]; then
  echo "system is debian6"
  cd $basepath

  #数据库更新
  /opt/mysql/server-5.6/bin/mysql -ufort -p2wsx3edc -f <./data_clean.sql
  echo 'upgrade complete'
else
  echo "The system is not debian6"
fi

```

‍
