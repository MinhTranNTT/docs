# docker-compose 安装 redis

```yaml
version: "3.5"

services:
  lcloud-redis:
    image: redis:6.2
    container_name: lcloud-redis
    restart: always
    ports:
      - 6379:6379
```

## 安装运行

```sh
# docker-compose up -d --build
```
