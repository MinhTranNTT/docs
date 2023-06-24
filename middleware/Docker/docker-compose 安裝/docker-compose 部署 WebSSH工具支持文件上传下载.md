# docker-compose 部署 WebSSH工具支持文件上传下载

参考文档：

[Docker-搭建一个网页版的WebSSH工具-支持文件上传下载 (ywsj.cf)](https://www.ywsj.cf/archives/docker--da-jian-yi-ge-wang-ye-ban-de-webssh-gong-ju---zhi-chi-wen-jian-shang-chuan-xia-zai)

[Jrohy/webssh: 简易在线终端和sftp工具 (github.com)](https://github.com/Jrohy/webssh)

[Docker-搭建一个网页版的WebSSH工具-支持文件上传下载_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Zt4y1N7ub/?spm_id_from=333.788&vd_source=9bfc54d2ed901f1eab04708cc346c2f5)

---

‍

### 创建docker-compose.yml文件

创建 `docker-compose.yml`文件

```yaml
version: "3.5"

services:
  lcloud-webssh: #服务名，可以自定义
    image: jrohy/webssh #镜像名不要改
    container_name: lcloud-webssh #容器名，可以自定义
    restart: always #开启开机自启动
    environment:
            - PUID=0 # 稍后在终端输入id可以查看当前用户的id
            - PGID=0 # 同上
            - TZ=Asia/Shanghai #时区，可以自定义
    ports:
      - 2222:5032 # 2222可以改成任意vps上未使用过的端口，5032不要改
```

### 部署运行

```shell
docker-compose up -d
```

### 登录webSSH页面 [http://127.0.0.1/](http://127.0.0.1:2222/)

![1](assets/docker-compose%20%E9%83%A8%E7%BD%B2%20WebSSH%E5%B7%A5%E5%85%B7%E6%94%AF%E6%8C%81%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E4%B8%8B%E8%BD%BD/1-20230113232542-pjmnycm.png)​

![2](assets/docker-compose%20%E9%83%A8%E7%BD%B2%20WebSSH%E5%B7%A5%E5%85%B7%E6%94%AF%E6%8C%81%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E4%B8%8B%E8%BD%BD/2-20230113232542-0cmjgxb.png)

‍
