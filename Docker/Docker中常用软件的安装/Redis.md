
1）下载Redis镜像
```bash
docker pull redis
# Using default tag: latest
# latest: Pulling from library/redis
# 5b5fe70539cd: Pull complete 
# a1bc4b7ad0f0: Pull complete 
# a33d2f01ead9: Pull complete 
# 94b33095e71c: Pull complete 
# d3c2b649b97c: Pull complete 
# 8a5bf7f0acd7: Pull complete 
# Digest: sha256:6ccde0cb87c24c09b1970802b10e702ab4491ac932b79f69682609787379fa42
# Status: Downloaded newer image for redis:latest
# docker.io/library/redis:latest
```
注意：
- 下载的Redis中所有镜像层是不包含配置文件的。因此需要在docker外部作容器卷的挂载，然后在启动参数中指定容器文件系统中配置文件所在位置。


2）挂载环境准备

宿主机环境准备（自定义）：
- `/custom/redis/etc/redis.conf`（配置文件）
- `/custom/redis/data`（数据目录）


下面的配置项一定要修改。
```text
daemonize no
# bind .....
```


3）启动Redis容器实例

```bash
docker run -d \
  --privileged=true \
  -p 6379:6379 \
  -v /custom/redis/etc/redis.conf:/etc/redis/redis.conf \
  -v /custom/redis/data:/var/redis/data \
  redis \
  redis-server /etc/redis/redis.conf 
```

4）查看是否成功
```
docker exec -it redis redis-cli
```

