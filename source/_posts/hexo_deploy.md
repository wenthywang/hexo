---
title: 我的hexo部署到腾讯云服务器
date: 2018-03-07 18:59:41
tags: 
- Hexo
- Linux
categories: 
- 笔记 
---
&emsp;&emsp;最近发现腾讯云搞活动，然后就心血来潮买了个服务器，感觉是挺便宜的（肯定是腾讯的套路），还续费了2年，希望自己能玩好这台服务器。昨天刚买完之后，一直在想不知道用来干嘛，想到现在还是不清楚要搞些撒东西出来，所以索性就先把博客迁移到这台服务器吧。


# 服务器配置
 ## 安装Git
`yum -y update`
`yum install -y git nginx`
安装git是因为hexo部署静态资源的时候需要有个git仓库
安装nginx用来部署hexo静态资源文件  

> git安装后需要配置公钥，打开以下文件,把需要部署的客户端的公钥复制到这个文件中就行，这个操作在部署hexo的时候可以跳过openssh的密码验证。（可以不操作，操作更好）
 
 - 在服务器上操作
`vim ~/.ssh/authorized_keys`  

 - 在hexo客户端中操作
 
打开GIT GUI 找到HELP找到SHOW SSH KEY
```
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAyPyy6mKAQrVQXVWCJ/2SeIDxF6a5FA8exlTTBtuAJZawpgRrnTCma+JFIvdViPH0fNIDLU0IwXZMExK5Gw2u90g3z0O2kJqF1pyAduyKUqd2oSK/aBGgcKCMej5OnS6xfDYqZn+zawsfU58ohUJHWNsXvTtK8XMGKi/N7nBsPxBqgwrTBMwHKIzhWjhv6SquQnnGaNXbddaidImixNyIHsbpJiPAQZtT4RH3WfCasBOtYF5Xl1ZMs07MiGEe+grX3MjrenMe1MJyWEziZTKREaV8jOgVbJi0EjpGTYqDb/oC6paqL4MbpQxQiFD70n2gOjSAyyVWEPfIHKaVl4z+cQ== Administrator@LQN-PC

```
复制key到authorized_keys 保存即可。
PS:如果服务端重装系统了 则需要清除本地公钥缓存 使用以下命令
`ssh-keygen -R 111.230.24.31`
    
 1. 新建git仓库
`cd /usr/local/wwh`
`mkdir GitLibrary`
`chmod -R 755 /data/GitLibrary`

 2. 初始化
`git init --bare hexo.git`
`vim /usr/local/wwh/GitLibrary/hexo.git/hooks/post-receive`

 添加以下代码
```
#!/bin/bash
git --work-tree=/usr/local/wwh/hexo --git-dir=/usr/local/wwh/GitLibrary/hexo.git checkout -f
```
 保存并退出

 3. 给文件添加可执行权限
`chmod +x /usr/local/wwh/GitLibrary/hexo.git/hooks/post-receive`
> 至此 git仓库配置完成。

## nginx配置

> nginx安装目录在/etc/nginx,配置测试性配置来检查是否安装成功了。

 1. 在/usr/local/wwh下新建文件夹 `mkdir hexo `
 2. `cd hexo` 新建index.html 文件 `vim index.html`
文件内容如下：

```
<!DOCTYPE html>
<html>
  <head>
    <title></title>
    <meta charset="UTF-8">
  </head>
  <body>
    <p>Nginx running</p>
  </body>
</html>
```
3.配置 Nginx 服务器
`vim /etc/nginx/nginx.conf`    添加以下配置

```
server {
      listen       80 default_server;
      listen       [::]:80 default_server;
      server_name   localhost
      root         /usr/local/wwh/hexo;//地址为刚才新建hexo文件夹
  }
```

4.启动Nginx 测试是否启动成功
输入命令： `systemctl start nginx.service` 
在没有配置系统启动Nginx的时候只能使用这种方式

5.访问IP
在浏览器输入IP地址：http://111.230.24.31/ 
会显示`Nginx running` 
表明Nginx启动成功。

# 部署Hexo
>  hexo 客户端安装 配置等不说明,在另外一篇文章中已经提及过了

1.修改nginx.conf 配置文件 `vim  /etc/nginx/nginx.conf `

添加以下代码
```
 server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  localhost;
        root         /usr/local/wwh/hexo;
      
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|ico)$ {
      expires 30d;
      access_log off;
      }

    location ~ .*\.(eot|ttf|otf|woff|svg)$ {
    expires 30d;
    access_log off;
    }

    location ~ .*\.(js|css)?$ {
    expires 7d;
    access_log off;
    }

    error_page 404 /404.html;
            location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
            location = /50x.html {
     }


}
```

> 代码中location 可不添加，location的配置主要是用来做页面缓存,提高访问性能,避免页面加载太慢。

重新启动nginx 
` systemctl restart nginx.service ` 

> nginx 配置完成

2.hexo客户端配置

 - 修改hexo中_config.yml

  修改deploy 参数,修改为以下代码
  ```
  deploy:
    type: git
    repo: 
       github: git@github.com:wenthywang/wenthywang.github.io.git //部署到github
       wwh: root@111.230.24.31:/usr/local/wwh/GitLibrary/hexo //部署到腾讯云服务器
    branch: master
  ```
  

 - 执行hexo部署命令

`hexo clean` //清除本地资源
`hexo g -d` //生成静态文件 并部署到配置文件中git地址


> 至此,hexo博客部署完毕。
 

# 为Nginx添加系统启动配置 

 - 在/etc/init.d/目录下编写脚本，名为nginx
 ```
 #!/bin/sh 
# 
# nginx - this script starts and stops the nginx daemon 
# 
# chkconfig:   - 85 15 
# description: Nginx is an HTTP(S) server, HTTP(S) reverse \ 
#               proxy and IMAP/POP3 proxy server 
# processname: nginx 
# config:      /etc/nginx/nginx.conf 
# config:      /etc/sysconfig/nginx 
# pidfile:     /var/run/nginx.pid 

# Source function library. 
. /etc/rc.d/init.d/functions 

# Source networking configuration. 
. /etc/sysconfig/network 

# Check that networking is up. 
[ "$NETWORKING" = "no" ] && exit 0 

nginx="/usr/local/nginx/sbin/nginx" 
prog=$(basename $nginx) 

NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf" 

[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx 

lockfile=/var/lock/subsys/nginx 

start() { 
    [ -x $nginx ] || exit 5 
    [ -f $NGINX_CONF_FILE ] || exit 6 
    echo -n $"Starting $prog: " 
    daemon $nginx -c $NGINX_CONF_FILE 
    retval=$? 
    echo 
    [ $retval -eq 0 ] && touch $lockfile 
    return $retval 
} 

stop() { 
    echo -n $"Stopping $prog: " 
    killproc $prog -QUIT 
    retval=$? 
    echo 
    [ $retval -eq 0 ] && rm -f $lockfile 
    return $retval 
killall -9 nginx 
} 

restart() { 
    configtest || return $? 
    stop 
    sleep 1 
    start 
} 

reload() { 
    configtest || return $? 
    echo -n $"Reloading $prog: " 
    killproc $nginx -HUP 
RETVAL=$? 
    echo 
} 

force_reload() { 
    restart 
} 

configtest() { 
$nginx -t -c $NGINX_CONF_FILE 
} 

rh_status() { 
    status $prog 
} 

rh_status_q() { 
    rh_status >/dev/null 2>&1 
} 

case "$1" in 
    start) 
        rh_status_q && exit 0 
    $1 
        ;; 
    stop) 
        rh_status_q || exit 0 
        $1 
        ;; 
    restart|configtest) 
        $1 
        ;; 
    reload) 
        rh_status_q || exit 7 
        $1 
        ;; 
    force-reload) 
        force_reload 
        ;; 
    status) 
        rh_status 
        ;; 
    condrestart|try-restart) 
        rh_status_q || exit 0 
            ;; 
    *)    
      echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}" 
        exit 2 

esac
 ```

 - 开启nginx服务
`chmod 755 /etc/init.d/nginx`
`chkconfig --add nginx`

 - nginx启动，停止
`service nginx start`//启动nginx
`service nginx stop`//停止nginx
`service nginx restart`//重启nginx
`service nginx reload`//修改配置后立马生效

通过系统配置后,nginx可以使用service nginx *的方式来启动了。

# 定时清除nginx日志 

1.修改nginx日志地址
`vim /etc/nginx/nginx.conf`
 修改以下内容
`error_log  /etc/nginx/logs/error.log warn;`
`access_log  /etc/nginx/logs/access.log  main;`

2.重新启动nginx 
因为重新加载配置不生效，需要重新启动nginx。
`service nginx restart`

3.添加定时任务（5天前的日志删除）
`cd /etc/nginx`
`mkdir sh`
`cd sh`
`vim delete_nginx_logs.sh`

添加以下代码
```
#set the path to nginx log files
log_files_path="/etc/nginx/logs/"
save_days=5
#delete ? days ago nginx log files
find $log_files_path -mtime +$save_days -exec rm -rf {} \;
``` 

修改定时任务
`crontab -e`
添加以下代码 保存
`00 00 * * * /bin/sh /etc/nginx/sh/delete_nginx_logs.sh`
每天0点0分执行脚本