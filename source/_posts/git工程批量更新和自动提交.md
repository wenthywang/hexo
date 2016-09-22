---
title: git工程批量更新和自动提交
date: 2016-09-22 15:53:41
tags: 
- Git
- Shell
categories: 
- GIT
---
 ##  git工程批量更新
用gitbash客户端都有一种不爽，更新多个工程需要很多的fetch，rebase，stash等命令，所以无聊就看了下shell脚本，看能不能批量fetch，rebase，搞了下，还是可以的，不过我这个是直接pull，直接合并到当前工程。
``` bash
#/bin/bash
echo '**********选择更新的git项目**********'
echo '0.all'
echo '1.jiaxin_lib_core'
echo '2.jiaxin_lib_dubbox'
echo '3.jiaxin_web_devcenter'
echo '4.jiaxin_web_agent'
echo '5.jiaxin_web_conf'
echo '6.jiaxin_gw_statistics'
echo '7.jiaxin_gw_config'
echo '8.jiaxin_gw_container'
echo '9.jiaxin_gw_order'
read project
               #在控制台输入1 2 3，它们之间用空格隔开。
if test $project -eq 0  ;then
echo '------------------------jiaxin_lib_core-----------------------'
cd jiaxin_lib_core && git pull && cd ..
echo '------------------------jiaxin_lib_dubbox-----------------------'
cd jiaxin_lib_dubbox && git pull && cd ..
echo '------------------------jiaxin_web_devcenter-----------------------'
cd jiaxin_web_devcenter && git pull && cd ..
echo '------------------------jiaxin_web_agent-----------------------'
cd jiaxin_web_agent && git pull && cd ..
echo '------------------------jiaxin_web_conf-----------------------'
cd jiaxin_web_conf && git pull && cd ..
echo '------------------------jiaxin_gw_statistics-----------------------'
cd jiaxin_gw_statistics && git pull && cd ..
echo '------------------------jiaxin_gw_config-----------------------'
cd jiaxin_gw_config && git pull && cd ..
echo '------------------------jiaxin_gw_container-----------------------'
cd jiaxin_gw_container && git checkout *.jar && git pull && cd ..
echo '------------------------jiaxin_gw_order-----------------------'
cd jiaxin_gw_order && git pull && cd ..
fi

if test $project -eq 1 ;then   
echo '-----------------------jiaxin_lib_core-START-----------------------' 

cd jiaxin_lib_core && git pull && cd ..
echo '------------------------jiaxin_lib_core-END-----------------------';
fi
if test $project -eq 2  ;then
echo '------------------------jiaxin_lib_dubbox-START----------------------'
cd jiaxin_lib_dubbox && git pull && cd ..
echo '-----------------------jiaxin_lib_dubbox-END-----------------------';
fi
if test $project -eq 3  ;then
echo '------------------------jiaxin_web_devcenter-START----------------------'
cd jiaxin_web_devcenter && git pull && cd ..
echo '-----------------------jiaxin_web_devcenter-END-----------------------';
fi
if test $project -eq 4  ;then
echo '------------------------jiaxin_web_agent-START----------------------'
cd jiaxin_web_agent && git pull && cd ..
echo '-----------------------jiaxin_web_agent-END-----------------------';
fi
if test $project -eq 5  ;then
echo '------------------------jiaxin_web_conf-START----------------------'
cd jiaxin_web_conf && git pull && cd ..
echo '-----------------------jiaxin_web_conf-END-----------------------';
fi
if test $project -eq 6  ;then
echo '------------------------jiaxin_gw_statistics-START----------------------'
cd jiaxin_gw_statistics && git pull && cd ..
echo '-----------------------jiaxin_gw_statistics-END-----------------------';
fi
if test $project -eq 7  ;then
echo '------------------------jiaxin_gw_config-START----------------------'
cd jiaxin_gw_config && git pull && cd ..
echo '-----------------------jiaxin_gw_config-END-----------------------';
fi
if test $project -eq 8  ;then
echo '------------------------jiaxin_gw_container-START----------------------'
cd jiaxin_gw_container && git checkout *.jar && git pull && cd ..
echo '-----------------------jiaxin_gw_container-END-----------------------';
fi
if test $project -eq 9  ;then
echo '------------------------jiaxin_gw_order-START----------------------'
cd jiaxin_gw_order && git pull && cd ..
echo '-----------------------jiaxin_gw_order-END-----------------------';
fi

```

##  git工程push

``` bash
#/bin/bash
echo '**********选择push的git项目**********'
echo '1.jiaxin_lib_core'
echo '2.jiaxin_lib_dubbox'
echo '3.jiaxin_web_devcenter'
echo '4.jiaxin_web_agent'
echo '5.jiaxin_web_conf'
echo '6.jiaxin_gw_statistics'
echo '7.jiaxin_gw_config'
echo '8.jiaxin_gw_order'

read project
echo '请输入提交参数commit：'
read commit
               #在控制台输入1 2 3，它们之间用空格隔开。
if test $project -eq 1 ;then   
echo '-----------------------jiaxin_lib_core-START-----------------------' 
cd jiaxin_lib_core && git add -A && git commit -m $commit && git push origin HEAD:refs/for/master && cd ..
echo '------------------------jiaxin_lib_core-END-----------------------';
fi
if test $project -eq 2  ;then
echo '------------------------jiaxin_lib_dubbox-START----------------------'
cd jiaxin_lib_dubbox && git add -A && git commit -m $commit && git push origin HEAD:refs/for/master && cd ..
echo '-----------------------jiaxin_lib_dubbox-END-----------------------';
fi
if test $project -eq 3  ;then
echo '------------------------jiaxin_web_devcenter-START----------------------'
cd jiaxin_web_devcenter && git add -A && git commit -m $commit && git push origin HEAD:refs/for/master && cd ..
echo '-----------------------jiaxin_web_devcenter-END-----------------------';
fi
if test $project -eq 4  ;then
echo '------------------------jiaxin_web_agent-START----------------------'
cd jiaxin_web_agent && git add -A && git commit -m $commit && git push origin HEAD:refs/for/master && cd ..
echo '-----------------------jiaxin_web_agent-END-----------------------';
fi
if test $project -eq 5  ;then
echo '------------------------jiaxin_web_conf-START----------------------'
cd jiaxin_web_conf && git add -A && git commit -m $commit && git push origin HEAD:refs/for/master && cd ..
echo '-----------------------jiaxin_web_conf-END-----------------------';
fi
if test $project -eq 6  ;then
echo '------------------------jiaxin_gw_statistics-START----------------------'
cd jiaxin_gw_statistics && git add -A && git commit -m $commit && git push origin HEAD:refs/for/master && cd ..
echo '-----------------------jiaxin_gw_statistics-END-----------------------';
fi
if test $project -eq 7  ;then
echo '------------------------jiaxin_gw_config-START----------------------'
cd jiaxin_gw_config && git add -A && git commit -m $commit && git push origin HEAD:refs/for/master && cd ..
echo '-----------------------jiaxin_gw_config-END-----------------------';
fi
if test $project -eq 8  ;then
echo '------------------------jiaxin_gw_order-START----------------------'
cd jiaxin_gw_order && git add -A && git commit -m $commit && git push origin HEAD:refs/for/master && cd ..
echo '-----------------------jiaxin_gw_order-END-----------------------';
fi  
```   

##  执行脚本  
使用git客户端切换到git的根目录，执行脚本命令即可。
```bash
./update.sh
```  
或
```bash
./push.sh
```  

##  注意事项
- 脚本放到git根目录，其实不放也可以的。你喜欢，[我的脚本位置](/img/git.png)
- 根据个人需要修改相应git命令，以免造成代码混乱，容易产生冲突
- 还有push的时候，也是需要按照个人需要修改的，因为git add的时候是全部的，最好区分一下
- **相关代码已上传到github**          **[链接](https://github.com/wenthywang/pullAndpush)**