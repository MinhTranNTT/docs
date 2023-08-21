# Docker 安装 MongoDB

[使用Docker-Compose安装MongoDB - MasonLee - 博客园 (cnblogs.com)](https://www.cnblogs.com/mxnote/articles/16899560.html)

```bash
[root@localhost ~]# docker pull mongo:latest
latest: Pulling from library/mongo
Digest: sha256:5be752bc5f2ac4182252d0f15d74df080923aba39700905cb26d9f70f39e9702
Status: Image is up to date for mongo:latest
docker.io/library/mongo:latest
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
redis        latest    7614ae9453d1   9 months ago    113MB
mysql        latest    3218b38490ce   9 months ago    516MB
mongo        latest    dfda7a2cf273   10 months ago   693MB
[root@localhost ~]# docker run -itd --name mongo -p 27017:27017 mongo --auth
1502dc5dda542495981d04c7efefd304ff63cadcc083c7326c8751e9a9e8be57
[root@localhost ~]# docker exec -it mongo mongo admin
MongoDB shell version v5.0.5
connecting to: mongodb://127.0.0.1:27017/admin?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("6449b90e-c703-4373-bf1a-578394740a88") }
MongoDB server version: 5.0.5
================
Warning: the "mongo" shell has been superseded by "mongosh",
which delivers improved usability and compatibility.The "mongo" shell has been deprecated and will be removed in
an upcoming release.
For installation instructions, see
https://docs.mongodb.com/mongodb-shell/install/
================
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	https://docs.mongodb.com/
Questions? Try the MongoDB Developer Community Forums
	https://community.mongodb.com
> db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});
Successfully added user: {
	"user" : "admin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		},
		"readWriteAnyDatabase"
	]
}
> db.auth('admin', '123456')
1
>
```

‍
