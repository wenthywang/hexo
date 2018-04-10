---
title: Centos7安装MySQL5.7
date: 2018-03-27 09:30:41
tags: 
- Linux
- MySQL
categories: 
- 笔记 
---
安装环境：CentOS7 64位、MySQL5.7

----------
# 配置安装环境
## 配置yum源rpm安装包
`wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm`
## 安装MySQL源
`yum localinstall mysql57-community-release-el7-8.noarch.rpm`
## 检查mysql源是否安装成功
`yum repolist enabled | grep "mysql.*-community.*"`
![mysql](/img/mysql_1.png)  
看到上图表示安装成功
## 修改MySQL版本
若默认安装5.7的话就不需要修改,修改源文件 
`vim /etc/yum.repos.d/mysql-community.repo`
改变enabled属性改为1即可 
![mysql](/img/mysql_2.png)  
## 安装MySQL
 `yum install mysql-community-server`
一轮等待后 安装成功
 # 启动MySQL
 `systemctl start mysqld`
 ## 查看MySQL状态
 `systemctl status mysqld`
     ![mysql](/img/mysql_3.png)  
 ## 开机启动
 `systemctl enable mysqld`
 `systemctl daemon-reload`
 # 修改root本地登陆密码
mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码
查看文件
 `grep 'temporary password' /var/log/mysqld.log`
    ![mysql](/img/mysql_4.png)  
 ## 登陆MySQL 修改密码
 `mysql -uroot -p`
 `ALTER USER 'root'@'localhost' IDENTIFIED BY 'test!Wwh';`
 >注意：mysql5.7默认安装了密码安全检查插件（validate_password），默认密码检查策略要求密码必须包含：大小写字母、数字和特殊符号，并且长度不能少于8位。否则会提示ERROR 1819 (HY000): Your password does not satisfy the current policy requirements。 

通过msyql环境变量可以查看密码策略的相关信息：
 `show variables like '%password%';`
![mysql](/img/mysql_5.png)  
>validate_password_policy：密码策略，默认为MEDIUM策略 
 validate_password_dictionary_file：密码策略文件，策略为STRONG才需要 
 validate_password_length：密码最少长度 
 validate_password_mixed_case_count：大小写字符长度，至少1个 
 validate_password_number_count ：数字至少1个 
 validate_password_special_char_count：特殊字符至少1个 
 上述参数是默认策略MEDIUM的密码检查规则。
 共有以下几种密码策略：
 策略  检查规则
 0 or LOW    Length
 1 or MEDIUM Length; numeric, lowercase/uppercase, and special characters
 2 or STRONG Length; numeric, lowercase/uppercase, and special characters; dictionary file
 MySQL官网密码策略详细说明：http://dev.mysql.com/doc/refman/5.7/en/validate-password-options-variables.html#sysvar_validate_password_policy 

## 修改密码策略
在/etc/my.cnf文件添加validate_password_policy配置，指定密码策略
`validate_password_policy=0 `
如果不需要 在my.cnf文件中添加如下配置禁用即可：
`validate_password = off`
重新启动MySQL 配置才会生效
`systemctl restart mysqld`
# 添加远程登录用户
默认只允许root帐户在本地登录，如果要在其它机器上连接mysql，必须修改root允许远程连接，或者添加一个允许远程连接的帐户，为了安全起见，我添加一个新的帐户：
`mysql> GRANT ALL PRIVILEGES ON *.* TO 'wenthywang'@'%' IDENTIFIED BY 'wwhtest123456' WITH GRANT OPTION;`
    添加用户成功后 就可以远程使用mycat等工具连接了。
# 配置默认编码utf8
 `vim /etc/my.cnf`
   添加以下配置
 `character_set_server=utf8 //指定数据库编码为UTF-8`
 `port=33060 //指定端口号`
 
    重新启动mysql服务，查看数据库默认编码如下所示：
   ![mysql](/img/mysql_6.png)  
- END