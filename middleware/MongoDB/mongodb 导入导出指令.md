# mongodb 导入导出指令

```sh
# 导出mongodb数据库
/usr/local/las/program/mongodb/bin/mongodump -d admin -u root -p "密码" -o /root/sd 

# 导入mongodb数据库
/usr/local/las/program/mongodb/bin/mongorestore -d admin /root/sd/admin -u root -p "密码" --drop

# 导出mongodb单个集合
/usr/local/las/program/mongodb/bin/mongoexport -d admin -c role -o /root/role.json -u root -p "密码"

# 导入mongodb单个集合
/usr/local/las/program/mongodb/bin/mongoimport -u root -p "密码" -d admin  -c role --type=json --file /root/role.json 
```
