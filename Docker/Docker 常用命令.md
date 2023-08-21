## docker 常用指令

| 指令                                    | 解释                                                              |
| --------------------------------------- | ----------------------------------------------------------------- |
| docker version                          | 显示 docker 版本信息                                              |
| docker info                             | 显示 docker 系统信息                                              |
| docker images                           | 查看所有镜像id                                                    |
| docker images -aq                       | 查看所有镜像id                                                    |
| docker [命令] --help                    | 帮助指令                                                          |
| docker ps -a                            | 查看运行容器                                                      |
| docker rm -f $(docker ps -qa)           | 清理所有运行容器                                                  |
| docker search centos                    | 查找镜像                                                          |
| docker pull centos                      | 下载镜像                                                          |
| docker rmi -f feb5d9fea6a5              | 删除指定id 镜像                                                   |
| docker rm feb5d9fea6a5                  | 删除容器                                                          |
| docker {start\|kill} feb5d9fea6a5       | 启动                                                              |
| docker attach 3913ce5ec9fd              | 进入当前正在运行的容器                                            |
| docker exec -it 3913ce5ec9fd /bin/bash  | 进入一个新的终端                                                  |
| docker inspect 3913ce5ec9fd             | 查看容器元数据                                                    |
| docker run -it centos /bin/bash<br />   | -i # 交互式操作</br> -t # 终端</br> /bin/bash # 交互式 Shell</br> |
| docker logs -ft --tail 10  9970a5ec9fb5 | 查看容器运行日志                                                  |


## 参考文档
- [官网 docker 指令帮助文档](https://docs.docker.com/engine/reference/commandline/docker/ "官网 docker 指令帮助文档")
- [Docker-compose 常用命令_docker-compose 命令](https://blog.csdn.net/qq_42267173/article/details/124687804)
- [Overview of docker compose CLI](https://docs.docker.com/compose/reference/)
- [在 docker 中查看容器日志](https://www.cnblogs.com/xwgli/p/13674414.html)
- [docker cp: 从容器中复制文件到本地_docker cp 到外侧](https://blog.csdn.net/qq_39378657/article/details/108995290)