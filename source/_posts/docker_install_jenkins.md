---
title: docker下安装jenkins
date: 2018-11-18 16:30:41
tags:
- docker
- jenkins
categories:
- 笔记
---

----------


# 赋予文件夹权限  
如果没有权限，则jenkins在构建时会出错，ssh 是方便jenkins使用ssh连接其他机器。  

```  
chown -R 1000 /docker/jenkins/jenkins_home
chown -R 1000 /data/maven_rept
chown -R 1000 /data/apache-maven-3.5.2
chown -R 1000 ~/.ssh
```  

  
# 执行命令  
```  
docker run  --restart=always --name jenkins -p 8080:8080 -p 50000:50000 \
    -v /docker/jenkins/jenkins_home:/var/jenkins_home \
    -v /etc/localtime:/etc/localtime \
    -v /etc/timezone:/etc/timezone \
    -v /data/apache-maven-3.5.2:/data/apache-maven-3.5.2 \
    -v /data/maven_rept:/data/maven_rept \
    -v ~/.ssh:/var/jenkins_home/.ssh \
    -d  docker.io/jenkins/jenkins
```  
