---
title: spring aop 初体验
date: 2017-06-28 10:53:41
tags: 
- spring
- aop
categories: 
- spring
---

 ## AOP
AOP（Aspect Oriented Programming），即面向切面编程，可以说是OOP（Object Oriented Programming，面向对象编程）的补充和完善。OOP引入封装、继承、多态等概念来建立一种对象层次结构，用于模拟公共行为的一个集合。不过OOP允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能。日志代码往往横向地散布在所有对象层次中，而与它对应的对象的核心功能毫无关系对于其他类型的代码，如安全性、异常处理和透明的持续性也都是如此，这种散布在各处的无关的代码被称为横切（cross cutting），在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。

AOP技术恰恰相反，它利用一种称为"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。

使用"横切"技术，AOP把软件系统分为两个部分：核心关注点和横切关注点。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如权限认证、日志、事物。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。

 ## AOP核心概念
1、横切关注点

对哪些方法进行拦截，拦截后怎么处理，这些关注点称之为横切关注点

2、切面（aspect）

类是对物体特征的抽象，切面就是对横切关注点的抽象

3、连接点（joinpoint）

被拦截到的点，因为Spring只支持方法类型的连接点，所以在Spring中连接点指的就是被拦截到的方法，实际上连接点还可以是字段或者构造器

4、切入点（pointcut）

对连接点进行拦截的定义

5、通知（advice）

所谓通知指的就是指拦截到连接点之后要执行的代码，通知分为前置、后置、异常、最终、环绕通知五类

6、目标对象

代理的目标对象

7、织入（weave）

将切面应用到目标对象并导致代理对象创建的过程

8、引入（introduction）

在不修改代码的前提下，引入可以在运行期为类动态地添加一些方法或字段

## Spring对AOP的支持

Spring中AOP代理由Spring的IOC容器负责生成、管理，其依赖关系也由IOC容器负责管理。因此，AOP代理可以直接使用容器中的其它bean实例作为目标，这种关系可由IOC容器的依赖注入提供。Spring创建代理的规则为：

1、默认使用Java动态代理来创建AOP代理，这样就可以为任何接口实例创建代理了

2、当需要代理的类不是代理接口的时候，Spring会切换为使用CGLIB代理，也可强制使用CGLIB

AOP的实现也不是难事，配置下文件（可用注解），可分3步。

1、定义普通业务组件

2、定义切入点，一个切入点可能横切多个业务组件

3、定义增强处理，增强处理就是在AOP框架为普通业务组件织入的处理动作

所以进行AOP编程的关键就是定义切入点和定义增强处理，一旦定义了合适的切入点和增强处理，AOP框架将自动生成AOP代理，即：代理对象的方法=增强处理+被代理对象的方法。


eg:aop.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
            
</beans>
```

## 基于Spring的AOP简单实现
需要额外下载以下两个jar

1、aopalliance.jar

2、aspectjweaver.jar

xml实现方式，注解方式就不多说了。
- 声明接口
```
public interface HelloWorld
{
    void printHelloWorld();
    void doPrint();
}
```
- 添加接口实现
```
public class HelloWorldImpl implements HelloWorld
{
    public void printHelloWorld()
    {
        System.out.println("Enter HelloWorldImpl.printHelloWorld()");
    }
    
    public void doPrint()
    {
        System.out.println("Enter HelloWorldImpl.doPrint()");
        return ;
    }
}
```
- 横切关注点，这里是打印时间：
```
public class TimeHandler
{
    public void printTime()
    {
        System.out.println("CurrentTime = " + System.currentTimeMillis());
    }
}
```

-aop.xml配置
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
        
        <bean id="helloWorldImpl" class="com.xrq.aop.HelloWorldImpl" />
     
        <bean id="timeHandler" class="com.xrq.aop.TimeHandler" />
        
        <aop:config>
            <aop:aspect id="time" ref="timeHandler">
                <aop:pointcut id="addAllMethod" expression="execution(* com.xrq.aop.HelloWorld.*(..))" />
                <aop:before method="printTime" pointcut-ref="addAllMethod" />
                <aop:after method="printTime" pointcut-ref="addAllMethod" />
            </aop:aspect>
        </aop:config>
</beans>
```

- main 方法
```
public static void main(String[] args)
{
    ApplicationContext ctx = 
            new ClassPathXmlApplicationContext("aop.xml");
        
    HelloWorld hw1 = (HelloWorld)ctx.getBean("helloWorldImpl");
   
    hw1.printHelloWorld();
    System.out.println();
    hw1.doPrint();
 
}
```
- 结果
```
CurrentTime = 1446129611993
Enter HelloWorldImpl.printHelloWorld()
CurrentTime = 1446129611993

CurrentTime = 1446129611994
Enter HelloWorldImpl.doPrint()
CurrentTime = 1446129611994
```

- 总结
   很不好意思地从度娘那拷过来的，暂时对aop的理解还没有这么深刻，但是使用过之后已经有了进一步的了解，然后再通过这篇博文发现对aop更加了解。以后对于一些能用aop方式解决的问题，还是用aop。
   总得来讲，现在公司的所有模块都几乎没有用aop，这是有很大的优化空间啊，因为看到一堆重复的代码，真叫人费解。