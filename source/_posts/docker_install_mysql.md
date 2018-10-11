---
title: docker下安装mysql8.0
date: 2018-10-05 16:30:41
tags:
- docker
- mysql
categories:
- 笔记
---

----------

# docker mysql 运行命令
```
docker run  -d -p 3306:3306  
--restart=always  
-v /docker/mysql/log/:/var/log/mysqld.log
--privileged=true  
-v /docker/mysql/conf/:/etc/mysql/conf.d
-v /docker/mysql/data/:/var/lib/mysql/
-e MYSQL_ROOT_PASSWORD=123456
--name mysql mysql
```
# 配置mysql配置文件
在/docker/mysql/conf下新增my.cnf文件 添加以下配置
```
[mysqld]
default-authentication-plugin = mysql_native_password
skip-name-resolve  
```
## 配置说明：
- default-authentication-plugin   
 由于mysql8.0默认的密码加密方式是 caching_sha2_password，而目前大多数人使用的navicat版本是不支持的，因此需要在docker启动mysql的时候指定挂载服务器主机的my.cnf配置文件，在[mysqld] 下需要添加以下配置：
default-authentication-plugin = mysql_native_password
网上看有人直接进到mysql容器修改user表下root用户的plugin（加密方式），这方法是行不通的，服务器会拒绝连接。


- skip-name-resolve  
mysql日志显示[Warning] IP address 'xxxx' could not be resolved: Name or service not known，那是因为mysql默认会反向解析DNS，对于访问者Mysql不会判断是hosts还是ip都会进行dns反向解析，频繁地查询数据库和权限检查，这大大增加了数据库的压力，导致数据库连接缓慢，严重的时候甚至死机，出现“连接数据库时出错”等字样。
解决办法：禁用dns反查即可
进入/etc 找到mysql的配置文件my.cnf（linux环境下）或者my.ini(windows环境下)进行编辑加入如下一行即可：
```
[mysqld]
skip-name-resolve
```
