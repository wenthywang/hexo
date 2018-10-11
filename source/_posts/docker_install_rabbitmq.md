---
title: docker 下部署rabbitmq
date: 2018-10-05 16:30:41
tags:
- docker
- rabbitmq
categories:
- 笔记
---

----------
# 执行命令即可
```
docker run  --restart=always -d  -p 5671:5671 -p 5672:5672  
-p 15672:15672 -p 15671:15671  -p 25672:25672  
-v /docker/rabbitmq-data/:/var/rabbitmq/lib  
--name rabbitmq   rabbitmq:management
```  

- 访问15672即可看到mq的管理界面
