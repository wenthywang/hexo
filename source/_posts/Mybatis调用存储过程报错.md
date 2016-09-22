---
title: Mybatis调用存储过程报错
date: 2016-09-20 12:13:41
tags: 
- Mybatis
- 存储过程
categories: 
- Mybatis
- 问题
---
 ## Mybatis调用存储过程
 ### 贴码
``` bash
Error querying database.  Cause: java.sql.SQLException: User does not have access to metadata required to determine stored procedure parameter types. If rights can not be granted, configure connection with "noAccessToProcedureBodies=true" to have driver generate parameters that represent INOUT strings irregardless of actual parameter types.
 The error may exist in resources/mapper/AgentStatisDao.xml
 The error may involve com.jiaxincloud.gw.statistics.dao.statistics.AgentStatisDao.callAgentVisitorManualStatisProcedure
 The error occurred while executing a query
 SQL: {CALL PRO_AGENT_VISITOR_MANUAL_STATIS(?,?,?)}
 Cause: java.sql.SQLException: User does not have access to metadata required to determine stored procedure parameter types. If rights can not be granted, configure connection with "noAccessToProcedureBodies=true" to have driver generate parameters that represent INOUT strings irregardless of actual parameter types.
```


### 总结
跟着这个错误在网上找了一下，原来是该用户没有调用存储过程的权限，所以只要赋予proc的权限即可，亲测成功。在MySql中执行如下命令，授予权限。(user@host 是连接数据库的用户名，修改成连接数据库的用户名就行)
``` bash
    grant select on mysql.proc to user@host;
    flush privileges;
``` 
