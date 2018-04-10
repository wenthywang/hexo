---
title: Centos7安装JDK1.8
date: 2018-04-08 09:30:41
tags: 
- Linux
- JDK1.8
categories: 
- 笔记 
---
安装环境：CentOS7 64位、JDK1.8

----------
# 下载安装包
- 切换到安装目录
`cd /usr/local/`
- 添加安装目录
`mkdir java`
- 切换安装目录
`cd java`
- 下载安装包
`wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.tar.gz"`
- 解压
`tar xzf jdk-8u161-linux-x64.tar.gz`  

# 配置环境变量
- 修改配置文件
 `vim /etc/profile`
   添加以下配置
`JAVA_HOME=/usr/local/java/jdk1.8.0_161`
`JRE_HOME=/usr/local/java/jdk1.8.0_161/jre`
`CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib`
`PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin`
`export JAVA_HOME JRE_HOME CLASS_PATH PATH`
 
- 更新文件
`source /etc/profile`
- 验证
`java -verion`![java](/img/java.png)  
`javac -version`
