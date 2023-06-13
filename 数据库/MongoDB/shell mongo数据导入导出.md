# shell mongo数据导入导出

```{16,
#!/bin/bash
# mongodb数据导入导出统一json格式

# 定义集合名 14灌机包
SETNAME=()
# echo "${SETNAME[*]}"


# 创建存储目录
mkdir -p /root/admin-mongodb


导出() {
    # 导出集合
    for((i=0;i<${#SETNAME[*]};i++)); do
        /usr/local/las/program/mongodb/bin/mongoexport -d admin -c "${SETNAME[i]}" -o /root/admin-mongodb/"${SETNAME[i]}".json -u root -p mongodb@cl0vdsec.c0m
    done
}

导入() {
    # 导入集合
    for((i=0;i<${#SETNAME[*]};i++)); do
        /usr/local/las/program/mongodb/bin/mongoimport -d admin -c "${SETNAME[i]}" --file /root/admin-mongodb/"${SETNAME[i]}".json -u root -p mongodb@cl0vdsec.c0m
    done
}

case "$1" in
  导出)
        导出
        ;;
  导入)
        导入
        ;;
      *)
			
            echo "bash mongodb_.sh {导出|导入}" >&2
            exit 1
            
    esac
    exit 0
```
