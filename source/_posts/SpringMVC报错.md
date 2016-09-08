---
title: SpringMVC报错
date: 2016-01-11 23:03:41
tags: 
- Java
- SpringMVC
- 静态资源
categories: 
- Java 
- SpringMVC
- 问题
---
 ## spring 报错
 ### 贴码
``` bash
  org.springframework.core.convert.ConversionFailedException: Failed to convert from type java.util.ArrayList<?> to type java.util.List<org.springframework.core.io.Resource> for value '[/img/]'; nested exception is org.springframework.core.convert.ConverterNotFoundException: No converter found capable of converting from type java.lang.String to type org.springframework.core.io.Resource
at org.springframework.core.convert.support.ConversionUtils.invokeConverter(ConversionUtils.java:41)
at org.springframework.core.convert.support.GenericConversionService.convert(GenericConversionService.java:169)
at org.springframework.beans.TypeConverterDelegate.convertIfNecessary(TypeConverterDelegate.java:161)
at org.springframework.beans.BeanWrapperImpl.convertIfNecessary(BeanWrapperImpl.java:450)
at org.springframework.beans.BeanWrapperImpl.convertForProperty(BeanWrapperImpl.java:496)
at org.springframework.beans.BeanWrapperImpl.convertForProperty(BeanWrapperImpl.java:490)
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.convertForProperty(AbstractAutowireCapableBeanFactory.java:1437)
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyPropertyValues(AbstractAutowireCapableBeanFactory.java:1396)
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1132)
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:522)
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:461)
at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:295)
at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:223)
at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:292)
at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:194)
at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:607)
at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:932)
at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:479)
at org.springframework.web.servlet.FrameworkServlet.configureAndRefreshWebApplicationContext(FrameworkServlet.java:647)
at org.springframework.web.servlet.FrameworkServlet.createWebApplicationContext(FrameworkServlet.java:598)
at org.springframework.web.servlet.FrameworkServlet.createWebApplicationContext(FrameworkServlet.java:661)
at org.springframework.web.servlet.FrameworkServlet.initWebApplicationContext(FrameworkServlet.java:517)
at org.springframework.web.servlet.FrameworkServlet.initServletBean(FrameworkServlet.java:458)
at org.springframework.web.servlet.HttpServletBean.init(HttpServletBean.java:138)
at javax.servlet.GenericServlet.init(GenericServlet.java:158)
at org.apache.catalina.core.StandardWrapper.initServlet(StandardWrapper.java:1284)
at org.apache.catalina.core.StandardWrapper.loadServlet(StandardWrapper.java:1197)
at org.apache.catalina.core.StandardWrapper.allocate(StandardWrapper.java:864)
at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:134)
at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:122)
at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:505)
at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:170)
at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:103)
at org.apache.catalina.valves.AccessLogValve.invoke(AccessLogValve.java:957)
at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:116)
at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:423)
at org.apache.coyote.http11.AbstractHttp11Processor.process(AbstractHttp11Processor.java:1079)
at org.apache.coyote.AbstractProtocol$AbstractConnectionHandler.process(AbstractProtocol.java:620)
at org.apache.tomcat.util.net.JIoEndpoint$SocketProcessor.run(JIoEndpoint.java:316)
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
at java.lang.Thread.run(Thread.java:745)
Caused by: org.springframework.core.convert.ConverterNotFoundException: No converter found capable of converting from type java.lang.String to type org.springframework.core.io.Resource
at org.springframework.core.convert.support.GenericConversionService.handleConverterNotFound(GenericConversionService.java:276)
at org.springframework.core.convert.support.GenericConversionService.convert(GenericConversionService.java:172)
at org.springframework.core.convert.support.CollectionToCollectionConverter.convert(CollectionToCollectionConverter.java:74)
at org.springframework.core.convert.support.ConversionUtils.invokeConverter(ConversionUtils.java:35)
... 42 more
```


### 总结
我在springmvc.xml配置了mvc：resoure，配置信息如下：
``` bash
<mvc:resources location="/js/" mapping="/js/**"/>
<mvc:resources location="/img/" mapping="/img/**"/>
``` 
如上错误 ，用的是spring3.2的包，程序运行没问题，静态资源可以加载，但是不知道怎么会报这个错，求问怎么解决？？？

网上找不到解决的办法。求好心人能帮忙解答
