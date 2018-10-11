---
title: docker下安装redis
date: 2018-10-05 16:30:41
tags:
- docker
- redis
categories:
- 笔记
---

----------
# 执行命令即可
```
docker run -d  --restart=always  -p 6379:6379
-v /docker/redis/conf/redis.conf:/etc/redis/redis.conf 
-v /docker/redis/data:/data
--name redis redis:latest redis-server /etc/redis/redis.conf
--appendonly yes --requirepass "12345678"
```
声明为需要密码访问的redis 配置requirepass参数
