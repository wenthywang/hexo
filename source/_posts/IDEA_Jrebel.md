---
title: IDEA 安装 JRebel
date: 2018-07-05 16:30:41
tags:
- IDEA
- JRebel
categories:
- 笔记
---

----------
# 科普JRebel  

  > IDEA上原生是不支持热部署的，一般更新了 Java 文件后要手动重启 Tomcat 服务器，才能生效，浪费不少生命啊。
目前对于idea热部署最好的解决方案就是安装JRebel插件，这样不论是更新 class 类还是更新 Spring 配置文件都能做到立马生效，大大提高开发效率。
进一步说，就是Jrebel让开发更实在，随时修改，随时查看修改结果。目前测试，配置文件和类文件都能实现热部署，强烈推荐。    


# 下载并安装
  插件安装跟Eclipse安装类似，提供了两种安装方法。  

## 在线安装
  打开IDEA,Files->Settings->搜索框填Plugins
  选择**Browser repositories** ,然后搜索框输入Jrebel，界面显示有且只有一个，然后点安装即可。
  ![Jrebel](/img/Jrebel_1.png)  

## 离线安装
  找到最新版本，一般来说，我都会使用最新版本。
>  https://plugins.jetbrains.com/plugin/4441-jrebel-for-intellij

  ![Jrebel](/img/Jrebel_2.png)
  下载下来是一个zip包，然后打开IDEA ,Files->Settins->搜索框填Plugins
  选择**Install Plugin from disk** 选择刚才下载好的zip包

## 生效
   无论是第一种方式还是第二种方式安装，都必须重新启动IDEA才能生效，毕竟这是插件。
  一般来说推荐使用在线安装，但是一般来说在线安装基本上很难，这个跟网络有关。
   所以当在线安装不行的话，就离线安装。

# 激活
打开IDEA,Files->Settings->Jrebel
右面会显示当前是否是已激活状态，现需要激活。  
使用的是lincense Server,点击active now 然后在lincenseServer上添加地址：  
> http://139.199.89.239:1008/88414687-3b91-4286-89ba-2dc813b107ce

邮箱随便填，填完就点active即可。
一般来说都能激活的。不能激活的话，要重新找server了。
![Jrebel](/img/Jrebel_6.png)


# 修改配置
修改IDEA 运行时编译，默认是不选择的。

**Ctrl+Shift+a** 填**Registry** 勾选**complier.\*.app.runninng**

![Jrebel](/img/Jrebel_3.png)
![Jrebel](/img/Jrebel_4.png)
JRebel插件的配置使用默认配置即可

# 运行Springboot程序
  - 运行前需要指定部署的是哪个模块，需要配置运行的模块。
  ![Jrebel](/img/Jrebel5.png)
  - 在main方法右键Run的时候发现有**Run with JRebel**，必然是选择这个了。
  然后其他的配置 看个人使用配置了。
