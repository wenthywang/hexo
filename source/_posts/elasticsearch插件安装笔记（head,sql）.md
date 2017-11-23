---
title: linux下elasticsearch插件安装笔记(head,sql)
date: 2017-05-04 15:43:41
tags: 
- elasticsearch插件
- linux
categories: 
- elasticsearch
- linux
---

 ## 说明
 这里的版本是es5.1.2，不适用5.x以下，5.x以下是用插件的形式安装在es上面，具体看git。
 安装的目录，和解压的目录都可以随便选的，但是为了统一管理，最好全部插件安装在es的根目录下（个人建议），因为我这里此前有人安装过相关的插件，分散在不同的目录，搞起来让我很懵逼。

 ## elasticsearch-head
github链接 **[elasticsearch-head](https://github.com/mobz/elasticsearch-head)**
 head 插件在window安装跟linux安装操作步骤基本相似，这里就介绍linux下安装即可。
1.首先需要git clone head 插件到本地，在linux上使用git命令也行，下载到本地再上传到linux服务器也行，哪方便行。
2.安装npm：
使用国外镜像会比较慢，所以使用淘宝镜像，很快的。
```
wget https://npm.taobao.org/mirrors/node/latest-v4.x/node-v4.4.7-linux-x64.tar.gz
```
3.解压：
```
tar -zxvf node-v4.4.7-linux-x64.tar.gz
```
4.设置环境变量：<strong>这个步骤很重要<strong>,直接影响到npm安装是否正确
```
export PATH=$PATH:/opt/node-v4.4.7-linux-x64/bin
```
  这个opt是刚才解压的目录，这个一定不要错。
5.查看npm版本：

```
npm -v
```

 看下有没有版本号，有的话就没问题了。没有的话应该是环境变量出问题了，先看下当前环境变量

 ```
 echo $PATH
 ```

 有上一步的安装目录的话，就没有问题了，这时候需要你重新设置环境变量

 ```
 export PATH=把刚才path的值拷贝过来就可以了
 ```

然后重新npm -v 看下，如果还是不行，建议重新安装npm。  
  
另外还有一个很奇葩的东西，不知道是不是linux自带了node，会导致环境变量有问题，如果发现自带的node，请务必要删掉，然后重新设置PATH。  

另外装完npm之后可以装n插件，这个插件可以更新最新版本的node，具体详情可以bing一下。

6.在head插件根目录下安装npm和grunt插件

```
 npm install  cnpm --registry=https://registry.npm.taobao.org
 npm install  grunt–cli
```

7.修改Elasticsearch配置文件

编辑elasticsearch-5.1.1/config/elasticsearch.yml,加入以下内容：

```
http.cors.enabled: true
http.cors.allow-origin: "*"
```

8.修改Gruntfile.js
打开elasticsearch-head-master/Gruntfile.js，找到下面connect属性，新增hostname: ‘0.0.0.0’,同时里面的port参数是对应的端口号，访问的时候就是这个端口号了，按需配置。

```
connect: {
        server: {
            options: {
                hostname: '0.0.0.0',
                port: 9100,
                base: '.',
                keepalive: true
            }
        }
    }   
```

9.启动elasticsearch-head
在elasticsearch-head-master/目录下，运行启动命令:

``` 
grunt server  

```

这个操作可能会出现异常，提示插件找不到，所以要找到elasticsearch-head-master/目录下的package.json里面最后的"devDependencies"参数，安装需要的插件包。

```
npm install 插件名@版本号
```

安装完再启动就好了。


10.后台启动elasticsearch-head

```
nohup grunt server &exit
```

如果想关闭head插件，使用Linux查找进程命令：

```
ps aux|grep head
```

结束进程：

```
kill 进程号
```

至此，head插件已成功安装。
![head](/img/head.bmp)  

 ## elasticsearch-sql
 github链接 **[elasticsearch-sql](https://github.com/NLPchina/elasticsearch-sql)**
由于上面已经安装好整个环境了，就不需要再安装了，如果是反过来装的话，也要先装npm才行。
>1.下载es-sql-site压缩包[https://github.com/NLPchina/elasticsearch-sql/releases/download/5.0.1/es-sql-site-standalone.zip]并解压到相应目录。
>2.切换到解压目录，并安装express插件，同时部署node服务。
```
cd site-server
npm install express --save
node node-server.js 
```
>至此sql插件已成功安装,同样如果需要后台运行，需要使用nohup命令执行。
另外sql端口号的配置在es-sql-site\site-server目录下的site_configuration.json文件中配置。
![sql](/img/sql.bmp)  

### 总结
   插件安装可能比较繁琐，特别是npm的安装，需要耐心。另外配置链接的地址可以写死在js里面
![link1](/img/link1.bmp)  
>这个的链接配置在head-master目录下elasticsearch-head-master\_site\app.js 
```
line 4327 "http://localhost:9200" ->"http://172.16.54.74:19200"
```
配置成启动的es的ip和端口即可

![link2](/img/link2.bmp)  
>这个的链接配置在head-master目录下es-sql-site\_site\controller.js

```
line 387    url = "http://localhost:9200"->"http://172.16.54.74:19200"
line 390    url = location.protocol+'//' + location.hostname + (location.port ? ':'+location.port : '')->"http://172.16.54.74:19200"
```  
都改成启动的es的ip和端口即可，这样子就不需要每次打开插件都填写对应的ip和port了。