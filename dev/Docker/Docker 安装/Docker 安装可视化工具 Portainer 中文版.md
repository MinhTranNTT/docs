# Docker 安装可视化工具 Portainer 中文版

参考文档：

[安装中文版 portainer-ce - 简书 (jianshu.com)](https://www.jianshu.com/p/818a00d189b5)

[(21条消息) Docker可视化工具Portainer的安装和使用_琦彦的博客-CSDN博客](https://blog.csdn.net/fly910905/article/details/113415899?ops_request_misc=%7B%22request%5Fid%22%3A%22167198895816800192295094%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=167198895816800192295094&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-3-113415899-null-null.142%5Ev68%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=docker%20volume%20create%20portainer_data&spm=1018.2226.3001.4187)

[安装中文版 portainer-ce - 简书 (jianshu.com)](https://www.jianshu.com/p/818a00d189b5)

[(21条消息) Docker可视化工具Portainer的安装和使用_琦彦的博客-CSDN博客](https://blog.csdn.net/fly910905/article/details/113415899?ops_request_misc=%7B%22request%5Fid%22%3A%22167198895816800192295094%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=167198895816800192295094&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-3-113415899-null-null.142%5Ev68%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=docker%20volume%20create%20portainer_data&spm=1018.2226.3001.4187)

---

### 拉取镜像

```shell
docker pull portainer/portainer-ce
docker pull 6053537/portainer-ce  # 直接用汉化版镜像
```

### 创建数据卷portainer_data

```shell
docker volume create portainer_data # 首先创建一个数据卷portainer_data
```

### 运行容器

```shell
docker run -d -p 7000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/usr/local/program/docker/portainer/data --name lcloud-prtainer 6053537/portainer-ce
```

参数说明：

* -p 7000:9000 \ 9000 为 portainer 默认端口，7000 映射本地端口
* --restart=always \ 重启策略
* -v /var/run/docker.sock:/var/run/docker.sock \ 映射 docker 的管理权限
* -v portainer_data:/usr/local/program/docker/portainer/data \ 映射容器内部数据
* --name lcloud-prtainer \ 设置容器名称
* --net docker_default \ 设置容器使用那个网络
* 6053537/portainer-ce \ 指定容器

### 访问路径

[http://192.168.1.102:7000/#!/2/docker/dashboard](http://192.168.1.102:7000/#!/2/docker/dashboard)

‍
