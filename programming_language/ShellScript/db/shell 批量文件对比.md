# shell 批量文件对比

```bash
#!/bin/bash

# 基于 CentOS 7.5 编写
# 作用：校验文件是否一致
# --------------------------------------------------------------------
# 7d7fe3fbc1bce90a2882b4a06c9439e5  /etc/init.d/arb
# 7d7fe3fbc1bce90a2882b4a06c9439e5
# --------------------------------------------------------------------

# 标准文件
H5=H5_fort_file_md5

# 执行路径
GETPATH=(
	"/usr/local/sbin/"
	"/usr/local/las/config/"
)

# 过略参数
OFF=(
	-not -name '*.log'
	-not -name '*.out'
)

# 生成标准版本
标准() {
	> $H5
    for((i=0;i<${#GETPATH[*]};i++)); do
		find "${GETPATH[i]}" -type f "${OFF[@]}" | xargs md5sum >> $H5
    done
}

# 其他版本对比
对比() {
	> $H5.log;
	IN_ALL=$(wc -l $H5 | awk {'print $1'})	# 定义总行数

	# 如果不操过总行数
	for((i=1;i<=IN_ALL;i++)); do
		MD5=$(head -$i $H5 | tail -1 | awk {'print $1'})	# 标准文件的MD5
		path=$(head -$i $H5 | tail -1 | awk {'print $2'})	# 标准文件路径
		NOW=$(md5sum $path | awk {'print $1'})			    # 现在文件的MD5
		if [[ "$MD5" != "$NOW" ]]; then
			echo "$path" >> $H5.log;
		fi
	done
}

备份() {
	lu=$(pwd)
	IN_ALL1=$(wc -l $H5.log | awk {'print $1'})
	for((i=1;i<=IN_ALL1;i++)); do
		path=$(head -$i $H5.log | tail -1)	# 获得每一行的路径
		cp --path $path $lu/
	done
}


case "$1" in
  标准)
        标准
        ;;
  对比)
        对比
        ;;
  备份)
		备份
		;;
      *)
            echo "bash H5_fort_file_md5.sh {标准|对比|备份}" >&2
            exit 1
    esac
    exit 0
```
