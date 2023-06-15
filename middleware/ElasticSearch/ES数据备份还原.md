# ES数据备份还原

## 1. 创建备份目录并更改权限

```sh
执行以下命令创建备份文件存储的路径
mkdir -p /mount/backups/my_backup
更改权限以及属性
chmod 775 /mount/backups/my_backup
chown elasticsearch:elasticsearch /mount/backups/my_backup/
```

## 2. 更改 elasticsearch.yml 文件

```sh
文件末尾增加
path.repo: ["/mount/backups/my_backup/"]
```

## 3. 重启 ES 并注册repository

```sh
重启命令:
/etc/init.d/elasticsearch restart
注册repository
curl -XPUT 'http://localhost:9200/_snapshot/backup' -d 
'{
	"type": "fs",
	"settings": {
		"location": "/mount/backups/my_backup",
		"compress": true
	}
}'
```

## 4. 查看是否注册成功

```sh
curl -XGET 'http://127.0.0.1:9200/_snapshot/backup'
返回结果：
{
	"backup": {
		"type": "fs",
		"settings": {
			"compress": "true",
			"location": "/usr/local/las/data/backup/es"
		}
	}
}
```

## 5. 执行备份所有索引信息

```sh
curl -XPUT 'http://127.0.0.1:9200/_snapshot/backup/test1?wait_for_completion=true'
注释：此次备份名称为：test1,等待命令执行完毕，执行过程会消耗一段时间（需多等待一会）
```

## 6. 备份指定索引信息

```sh
curl -XPUT 'http://127.0.0.1:9200/_snapshot/backup/esback' -d
'{
	"indices": "las-e-2017-06-21,las-e-2016-07-21",
	"ignore_unavailable": true,
	"include_global_state": false,
	"wait_for_completion": true
}'
```

## 7. 查看备份状态

```sh
curl -XGET 'http://localhost:9200/_snapshot/backup/esback?pretty'
```

## 8. 删除指定备份信息

```sh
curl -XDELETE 'http://localhost:9200/_snapshot/backup/esback'
```

## 9. 还原所有备份信息

```sh
curl -XPOST 'http://localhost:9200/_snapshot/backup/bak/_restore'
```

## 10. 还指定索引信息

```bash
curl -XPOST 'http://localhost:9200/_snapshot/backup/bak/_restore' -d 
'{ 
	"indices":"las-e2018-05-01,
	las-e-2018-05-02",
	"ignore_unvailable":true
}'
```
