---
title: 使用Hexo+Git+Nodejs搭建个人博客
date: 2016-09-11 17:34:41
tags: 
- Hexo
categories: 
- 笔记 
---
&emsp;&emsp;处于好奇，看到别人搞自己的博客，自己也想尝试一番，没想到，弄着弄着就喜欢上了，有时候真是挡也挡不住。遇到的问题也是甚多。刚才发现md的语法原来没有两个空格的，网上有说法使用`&emsp;&emsp;`反正我是这么用了，因为我用不了输入法的全角的两个空格，我的输入法是win10自带的输入法，所以如果某f有输入法可以尝试一下哦。好了，废话不说了。上教程。
## Git
- 首先也是必须要注册一个[Git](https://github.com/ "github")，注册流程就不多说了，反正都是一样了，然后就新增一个Repository，**名字必须是XXX.github.io,也必须是master主干，xxx是你的git的用户名**![参考](/img/git1.png)  创建完成后，需要git客户端，客户端下载就不说了，百度都有。

## NodeJs
&emsp;&emsp;首先要安装[nodejs](https://nodejs.org/download/)挺多版本的 我的npm是1.4.28版本 作用是生成一些静态的html，安装成功后 可输入命令  
`mpn -v`  
查看当前版本 如果查看不了 证明安装失败，那就要重新安装了。
## Hexo
- 正式安装Hexo 建立文件夹hexo，切换到当前文件夹下，输入命令  
`npm install -g hexo`   
安装hexo，速度的快慢要看你的网速了。  

- 执行初始化hexo,命令：  
`hexo init`  
- 启动本地服务命令：  
`hexo server`（hexo s也可以）  
- 浏览器输入http://localhost:4000  

**浏览器有出现hexo的主题页面**，证明安装成功了，若没出现，可以看下哪里配置出问题，一般是没有问题的。


## 配置Github
- 找到hexo的配置文件_config.yml,这个文件在hexo的根目录下，打开配置文件。进行如下配置：
```deploy:
    type: git  
    repository: git@github.com:wenthywang/wenthywang.github.io.git  
    branch: master
```
- 执行命令：
`
npm install hexo-deployer-git --save
`
***网上会有很多说法，有的type是github, 还有repository最后面的后缀也不一样，是github.com.git，我也踩了很多坑，我现在的版本是hexo: 3.2.2，执行命令hexo -vsersion就出来了,貌似3.0后全部改成我上面这种格式了。***

- 执行配置命令：
`
hexo deploy(hexo d)
`
- 浏览器中输入```http://wenthywang.github.io/```就行了，我的github的账户叫wenthywang,把这个改成你github的账户名就行了

## 部署步骤

三步走：

- hexo clean

- hexo generate(hexo g)

- hexo deploy(hexo d)

## 常用命令

- hexo new"postName" #新建文章

- hexo new page"pageName" #新建页面

- hexo generate #生成静态页面至public目录

- hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）

- hexo deploy #将.deploy目录部署到GitHub

- hexo help # 查看帮助

- hexo version #查看Hexo的版本
