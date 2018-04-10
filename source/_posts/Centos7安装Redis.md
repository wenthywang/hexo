---
title: Centos7安装Redis
date: 2018-04-08 16:30:41
tags: 
- Linux
- Redis
categories: 
- 笔记 
---
安装环境：CentOS7 64位、Redis-4.0.8

----------
# 下载安装包
- 切换到安装目录
`cd /usr/local/`
- 添加安装目录
`mkdir redis`
- 切换安装目录
`cd redis`
- 下载安装包
`wget http://download.redis.io/releases/redis-4.0.8.tar.gz`
- 解压
`tar xzf redis-4.0.8.tar.gz`  

# 安装gcc
如果不安装gcc 编译redis会报错![error](/img/error.png)  
`yum install gcc`
# 编译安装
`cd redis/redis-4.0.8/`
`make MALLOC=libc` 
Redis并没有自己实现内存池，没有在标准的系统内存分配器上再加上自己的东西。所以需要配置内存分配。
不执行以上命令编译会报下面的错![error](/img/error1.png)  
编译redis
`cd src && make install`![redis](/img/redis-1.png)  

# 启动redis
`./redis-server`
![redis](/img/redis-2.png)  
如上图：redis启动成功，但是这种启动方式需要一直打开窗口，不能进行其他操作，不太方便。
## 以后台进程方式启动redis
 - 修改redis.conf文件
`vim /usr/local/redis/redis-4.0.8/redis.conf`
将`daemonize no` 改为`daemonize yes`
 - 指定redis.conf启动
 `./redis-server /usr/local/redis/redis-4.0.8/redis.conf`
### 设置redis 开机启动
 - 在/etc目录下新建redis目录
  `mkdir redis`
 - 复制redis 配置文件
  `cp /usr/local/redis/redis-4.0.8/redis.conf /etc/redis/6379.conf`
 - 复制redis 脚本到启动脚本目录中
  `cp /usr/local/redis/redis-4.0.8/utils/redis_init_script /etc/init.d/redisd`
 - 切换到自启目录
 `cd /etc/init.d`
 `chkconfig redisd on`
 会显示`service redisd does not support chkconfig　`
看结果是redisd不支持chkconfig  

解决方法：
使用vim编辑redisd文件，在第一行加入如下两行注释，保存退出
`# chkconfig:   2345 90 10`
`# description:  Redis is a persistent key-value database`
注释的意思是，redis服务必须在运行级2，3，4，5下被启动或关闭，启动的优先级是90，关闭的优先级是10。
再次执行开机自启命令，成功
`chkconfig redisd on`
# 服务启动
现在可以直接已服务的形式启动和关闭redis了
启动：
`service redisd start`
![redis](/img/redis-3.png)  
关闭：
`service redisd stop`
![redis](/img/redis-4.png) 

# 过程中的问题
## service redisd stop出现以下问题
>Stopping ...
(error) NOAUTH Authentication required.  

解决方法：因为我在配置redis 时候 配置了密码连接 所以 出现上面的问题,如果不配置 则不需要以下配置。修改启动脚本
`vim /etc/init.d/redisd`
增加密码设置变量,密码是你设置的redis密码
![redis](/img/redis-5.png)  

## service启动或者关闭时出现以下问题
> /var/run/redis_6379.pid exists, process is already running or crashed  

解决方法：出现这个问题是因为redis 关闭不正常，导致文件没有被删除掉。
执行以下命令即可删除
`rm -rf /var/run/redis_6379.pid`
 