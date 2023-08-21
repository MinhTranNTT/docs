# docker-compose 部署 NginxProxyManager 中文版安装教程

参考资料：

[NginxProxyManager中文版安装教程 (ywsj.cf)](https://www.ywsj.cf/archives/nginxproxymanager-zhong-wen-ban-an-zhuang-jiao-cheng)

[(21条消息) 反向代理神器——Nginx Proxy Manager_蒟蒻颖的博客-CSDN博客_nginx proxy manager](https://blog.csdn.net/zy440458/article/details/122090513)

[nginx开源可视化代理管理器nginx-proxy-manager - 简书 (jianshu.com)](https://www.jianshu.com/p/07267b741b54)

---

### docker-compose.yml

```yaml
version: "3.5"

services:
  lcloud-nginx:
    image: 'chishin/nginx-proxy-manager-zh:latest'
    container_name: lcloud-nginx
    restart: always
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

### 部署运行

```yaml
docker-compose up -d
```

‍
