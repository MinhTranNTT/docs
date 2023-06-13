# shell 批量文件对比MD5

```bash
#!/bin/bash

# 基于 CentOS 7.5 编写
# 作用：校验文件是否一致
# --------------------------------------------------------------------
# 7d7fe3fbc1bce90a2882b4a06c9439e5  /etc/init.d/arb
# 7d7fe3fbc1bce90a2882b4a06c9439e5
# /etc/init.d/arb
# --------------------------------------------------------------------
# 清空日志文件
H5=H5_fort_file_md5
> $H5.log;
# 定义总行数
IN_ALL=$(wc -l $H5 | awk {'print $1'})
# 如果不操过总行数
for((i=1;i<=IN_ALL;i++)); do
    # 标准文件的MD5
    MD5=$(head -$i $H5 | tail -1 | awk {'print $1'})
    # 标准文件路径
    path=$(head -$i $H5 | tail -1 | awk {'print $2'})
    # 现在文件的MD5
    NOW=$(md5sum $path | awk {'print $1'} || echo "$path 这个文件不存在")
    if [[ "$MD5" != "$NOW" ]]; then
        echo "文件与标准版不符：$path" >> $H5.log;
    fi
done
```
