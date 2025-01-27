# Spring学习笔记(7)AOP概述

---
[TOC]

---

# 1.什么是AOP
AOP (Aspect Oriented Programing)
AOP 采用了`横向抽取`机制,取代了`纵向继承`体系中重复性的代码( 性能监视、 事务管理、 安全检查、 缓存)

**Spring AOP 使用纯 Java 实现， 不需要专门的编译过程和类加载器， 在运行期通过代理方式向目标类织入增强代码**

**AspecJ 是一个基于 Java 语言的 AOP 框架， Spring2.0 开始,Spring AOP 引入对 Aspect 的支持， AspectJ扩展了 Java 语言， 提供了一个专门的编译器， 在编译时提供横向代码的织**

# 2.SpringAOP思想

![](../../images/SpringAOP概述SpringAOP思想.jpg)

# 3.AOP 底层原理
就是代理机制

    *  动态代理:(JDK 中使用)
      
       * JDK 的动态代理,对实现了接口的类生成代理.

# 4.Spring的AOP代理

- JDK 动态代理:对实现了接口的类生成代理(缺陷就是这个类必须先实现某个接口)
- CGLib 代理机制:对类生成代理（对没有实现接口的类也可以代理）

# 5.AOP 的术语

- `Joinpoint`(连接点):所谓连接点是指那些被拦截到的点。 在 spring 中,这些点指的是方法,因为 spring 只支持方法类型的连接点.（ 即那些方法可以被拦截）
- `Pointcut`(切入点):所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义.（ 实际拦截的方法）
- `Advice`(通知/增强):所谓通知是指拦截到Joinpoint之后所要做的事情就是通知.通知分为前置通知,后置通知,异常通知,最终通知,环绕通知(切面要完成的功能)
- `Introduction`(引介):引介是一种特殊的通知在不修改类代码的前提下, Introduction 可以在运行期为类动态地添加一些方法或 Field.
- `Target`(目标对象):即代理的目标对象
- `Weaving`(织入):是指把增强应用到目标对象来创建新的代理对象的过程.spring 采用动态代理织入， 而 AspectJ 采用编译期织入和类装载期织入
- `Proxy`(代理):一个类被 AOP 织入增强后， 就产生一个结果代理类
- `Aspect`(切面):是切入点和通知(引介)的结合

![](../../images/SpringAOP概述Spring术语.jpg)

