---
title: Centos 下安装docker
date: 2018-10-05 16:30:41
tags:
- docker
categories:
- 笔记
---

----------
# docker 介绍

  >Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大优势，过去需要用数天乃至数周的	任务，在Docker容器的处理下，只需要数秒就能完成。
  - Web 应用的自动化打包和发布。
  - 自动化测试和持续集成、发布。
  - 在服务型环境中部署和调整数据库或其他的后台应用。   


# 安装说明
  Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。
  通过 uname -r 命令查看你当前的内核版本
```
  uname -r
```
# 使用 root 权限登录 Centos。确保 yum 包更新到最新。
```
yum update
```
## 卸载旧版本(如果安装过旧版本的话)
```
 yum remove docker  docker-common docker-selinux docker-engine
```
## 安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```
# 设置yum源
```
 yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
# 安装docker
```
yum install docker-ce
```
# 启动并加入开机启动
```
  systemctl start docker
  systemctl enable docker
```
# 验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
```
docker version
```
