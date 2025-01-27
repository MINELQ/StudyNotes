﻿# springmvc学习笔记(1)-框架原理和入门配置

---

[TOC]

---

本文主要介绍springmvc的框架原理，并通过一个入门程序展示环境搭建，配置以及部署调试。


springmvc是spring框架的一个模块，springmvc和spring无需通过中间整合层进行整合。


## springmvc框架原理

Springmvc是一个web层mvc框架。什么是mvc？model view controller

### 什么是mvc？
mvc是一个架构设计模式。
![](../../images/SpringMVC/mvc架构模式.jpg)

### Springmvc框架原理
![](../../imaegs/../images/SpringMVC/springmvc框架原理.jpg)

组件及其作用

- 前端控制器(DispatcherServlet)：接收请求，响应结果，相当于转发器，中央处理器。减少了其他组件之间的耦合度
- 处理器映射器(HandlerMapping)：根据请求的url查找Handler
- **Handler处理器**：按照HandlerAdapter的要求编写
- 处理器适配器(HandlerAdapter)：按照特定规则(HandlerAdapter要求的规则)执行Handler。
- 视图解析器(ViewResolver)：进行视图解析，根据逻辑视图解析成真正的视图(View)
- **视图(View)**：View是一个接口实现类试吃不同的View类型（jsp,pdf等等）

*注：其中加粗的为需要程序员开发的，没加粗的为不需要程序员开发的*

步骤：

- 1.发起请求到前端控制器(`DispatcherServlet`)
- 2.前端控制器请求处理器映射器(`HandlerMapping`)查找`Handler`(可根据xml配置、注解进行查找)
- 3.处理器映射器(`HandlerMapping`)向前端控制器返回`Handler`
- 4.前端控制器调用处理器适配器(`HandlerAdapter`)执行`Handler`
- 5.处理器适配器(HandlerAdapter)去执行Handler
- 6.Handler执行完，给适配器返回ModelAndView(Springmvc框架的一个底层对象)
- 7.处理器适配器(`HandlerAdapter`)向前端控制器返回`ModelAndView`
- 8.前端控制器(`DispatcherServlet`)请求视图解析器(`ViewResolver`)进行视图解析，根据逻辑视图名解析成真正的视图(jsp)
- 9.视图解析器(ViewResolver)向前端控制器(`DispatcherServlet`)返回View
- 10.前端控制器进行视图渲染，即将模型数据(在`ModelAndView`对象中)填充到request域
- 11.前端控制器向用户响应结果

### Springmvc和spring什么关系？
![](../../images/SpringMVC/springmvc与spring的关系.jpg)
Springmvc是spring一部分

## SpringMVC入门程序

![](../../images/SpringMVC/springmvc入门程序的项目结构.jpg)

### 1.所需的包
- pom.xml
```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
    <!-- spring版本号 -->
    <spring.version>5.1.5.RELEASE</spring.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <!-- Spring依赖 -->
    <!-- 1.Spring核心依赖 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-expression</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <!-- 2.Spring dao依赖 -->
    <!-- spring-jdbc包括了一些如jdbcTemplate的工具类 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!-- 3.Spring web依赖 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!-- 4.Spring test依赖：方便做单元测试和集成测试 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>

  </dependencies>
```

### 2.配置SpringMVC的前端控制器
```xml
  <!--配置springmvc的前端控制器-->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--springmvc默认加载WEB-INF:servletname-servlet.xml-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
```

### 3.springmvc配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--开启扫描注解-->
    <context:component-scan base-package="club.krislin"/>

    <!--配置试图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/JSPS/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>

    <!--开启SpringMVC注解的支持-->
    <mvc:annotation-driven/>
</beans>
```

### 4.编写Controller
```java
@Controller
public class MyController{
    @RequestMapping(path = "/hello")
    public ModelAndView hello(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("message","Hello SpringMVC!");
        modelAndView.setViewName("success");
        return modelAndView;
    }
}
```

### 5.编写success.jsp
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>入门程序</title>
</head>
<body>
    <h1>入门成功</h1>
</body>
</html>
```

## SpringMVC入门程序的流程
![](../../images/SpringMVC/springmvc入门程序的执行流程.jpg)