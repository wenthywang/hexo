---
title: jenkins maven构建时报错
date: 2018-11-18 16:30:41
tags:
- docker
- jenkins
- maven
categories:
- 笔记
---

----------

# maven构建错误提示  

```  

 Thin Pool has 0 free data blocks which is less than minimum required 163840 free data blocks. Create more free space in thin pool or use dm.min_free_space option to change behavior.
See ‘/usr/bin/docker-current run --help‘.
```


运行下面三个命令：  

 <p style="color:red">注意，以下三个命令执行时可能出错是正常的。  
</p>
- 清理exited进程：   

```  
docker rm $(docker ps -q -f status=exited)
```

- 清理dangling volumes：  

```
docker volume rm $(docker volume ls -qf dangling=true)
```

- 清理dangling image：  

```
docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
```


&nbsp;&nbsp;&nbsp;&nbsp;以上命令，可能是dockerhost 磁盘满了，可使用docker info 查看docker 使用情况，如果不是dockerhost 可能是registry满了，都必须检查，不然会一直构建失败，相关联的docker机器都得检查下。
