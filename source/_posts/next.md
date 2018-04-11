---
title: next部署git页面资源加载404
date: 2016-11-18 15:44:41
tags: 
- hexo
categories: 
- 笔记
- bug
---
 ## 资源404
 - 引用一下其他博主的话，这位博主不要生气哦，资源共享嘛
 ** 最近github page更新了，GitHub Pages 过滤掉了 source/vendors 目录的访问，所以next主题下的source下的vendors目录不能够被访问到，所以就出现了本地hexo s能够正常访问，但是deploy到github就是一片空白，按f12，可以看到大量来自source/vendors的css和js提示404 
 **

![404](/img/404.png)  


- 这两天推送git的时候发现，推送hexo成功，但是在github上面打开就报了一堆404找不到资源的情况。所有的配置都已经配置好的，跟以前没有什么区别，然后自己再反复找下是不是hexo的问题，然后就重新装了hexo。发现问题还是存在，于是就度娘了一下，原来最近真的有这样情况，然后next的博主也在git上面解决了这个bug，详情请看[资源加载失败](https://github.com/iissnan/hexo-theme-next/issues/1214 "issues")，原来是有个jekyll的东西改了，导致不能加载这些资源。

- 看到比较快的解决方法，亲测成功，因为我把博主的master的git删掉了，我在想当初我怎么这么手贱啊。同时我也改了挺多博主的东西，免得冲突了。听说博主更新了很多东西，最近实在是太忙了，没有去研究博主的新东西。我还是很喜欢next 这个主题的。更好的方法应该是直接更新博主的next master了。

### 解决方法
- 参考: issue: #1220
- 步骤:
```
.deploy_git 目录, 添加 .nojekyll 空文件
source目录, 添加.nojekyll 空文件
修改 Hexo 上层_config.yml配置文件, 添加
include:
  - .nojekyll
重新部署推送: hexo d -g
```

## 同时出现的问题
- 添加以上内容的时候，还发现win10直接添加.xx的文件是不行。后来想到绝招，使用git bash 添加文件，真是无所不能。git base的命令跟linux的命令几乎一样，所以一下就建好了。新建文件命令
```
touch .nojekyll
```
- 这个bug 就解决到这了。嘻嘻