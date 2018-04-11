---
title: maven mirror修改 国内镜像
date: 2016-11-02 20:18:41
tags: 
- maven
- eclipse
categories: 
- 笔记
---
 ## 修改setting.xml
 setting.xml是工程所用的maven的setting.xml,可存在maven安装目录的conf下，或者在.m2下面。
修改 .m2 文件夹中的setting.xml 中的 <mirrors>元素。添加<mirror>子元素如下：
```
<mirrors>  
    <id>alimaven</id>  
    <name>aliyun maven</name>  
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
    <mirrorOf>central</mirrorOf>          
  </mirror>  
</mirrors>
```
阿里云的镜像，亲测很快。以后都用它了。