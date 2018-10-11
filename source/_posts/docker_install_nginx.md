---
title: docker下安装nginx 并挂载博客目录
date: 2018-10-05 16:30:41
tags:
- docker
- nginx
categories:
- 笔记
---

----------
# 执行命令
```
docker run  --restart=always  -p 80:80 -p 443:443 --name nginx
-v /docker/nginx/log/:/var/log/nginx 
-v /docker/nginx/conf/:/etc/nginx/conf.d/
-v /usr/local/wwh/hexo:/usr/local/wwh/hexo
-v /docker/nginx/cert/:/etc/nginx/cert -d nginx
```
## nginx 配置
在/docker/nginx/conf 下新增nginx.conf 配置文件

内容如下：
```
 server {
        listen       80 ;
        server_name  wenthywang.cn;
        rewrite     ^   https://$host$request_uri? permanent;

}

server {
    listen 443;
    server_name wenthywang.cn;
    ssl on;
    root /usr/local/wwh/hexo;
    ssl_certificate   cert/1_wenthywang.cn_bundle.crt;
    ssl_certificate_key  cert/2_wenthywang.cn.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

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
## 配置说明
cert目录为https证书存放目录，使用的腾讯云的证书。80端口配置跳转为443端口，则访问80端口的时候 默认会跳转到443的链接。其他配置为缓存配置，以及一些静态资源配置。
