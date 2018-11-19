---
title: docker下安装容器监控
date: 2018-10-05 16:30:41
tags:
- docker
- influxdb
- cadvisor
- grafana
categories:
- 笔记
---

----------

# 运行容器
```
docker run -d -p 8083:8083 -p 8086:8086 --name influxsrv tutum/influxdb
```
安装influxdb 保存数据

```
docker run --volume=/:/rootfs:ro --volume=/var/run:/var/run:rw --volume=/sys:/sys:ro --volume=/var/lib/docker/:/var/lib/docker:ro --publish=8080:8080 --detach=true  --name=cadvisor google/cadvisor:latest  -storage_driver=influxdb  -storage_driver_db=cadvisor -storage_driver_host=192.168.9.110:8086  
```
启动容器信息收集器，需要监控的机器都得安装。

```
docker run -d -p 3000:3000 -e INFLUXDB_HOST=localhost -e INFLUXDB_PORT=8086 -e INFLUXDB_NAME=cadvisor -e INFLUXDB_USER=root -e INFLUXDB_PASS=root --link influxsrv:influxsrv --name grafana grafana/grafana
```
启动grafana 界面管理
