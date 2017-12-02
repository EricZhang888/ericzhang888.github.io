---
title: Spring 核心之IOC
date: 2017-12-2 17:30:09
categories:
- Spring
comments: true
---

不得不说Spring框架非常强大而复杂；要想全面掌握Spring框架的方方面面需要大量的学习时间以及项目实践。而理解Spring核心原理是真正精通Spring必不可少的一环。利用本篇文章带领大家复习一下Spring的核心之一IOC容器，即控制反转或依赖注入。

# Spring Framework
笔者最初接触Spring框架只是使用Spring framework作为bean的管理容器，使用Struts作为web MVC框架，使用JDBC原生Dao类访问数据库来开发Web项目。我想2012年以前参加工作的Java工程师应该都有类似的经验。而今天谈论Spring框架我们就应该说清楚讨论的是```Spring core```? ```Spring web Mvc```? ```Spring cloud```? ```Spring Data```? ```Spring security```? 还是```Spring boot```?

越来越多的分支项目被加入Spring框架，理解好Spring核心分支(Spring Framework)对于掌握好Spring其他项目的重要性不言而喻。下面这幅图来源于Spring.io官网，很好的展示了Spring Framework讨论的范畴，希望能将这幅图闹记于心。
![](/assets/img/spring/spring-overview.png)

## Framework Modules（框架模块）
根据上图能够清晰的看到Spring Framework主要包含6个方面的内容：
1. 核心容器(Core Container) -- 核心中的核心
2. 切面编程(AOP)
3. 消息(Messaging)
4. 数据访问(Data Access)
5. Web编程(MVC)
6. 测试(Test)

本章主要讨论第一点内容核心容器(IOC)。

# 核心容器(Core Container)
相信所有工程师都知道Spring框架最原始最基本最核心的功能是帮助我们管理Bean，那么将Spring的核心称之为一个存储Bean的容器是不是不难理解？    
所以，Spring的核心是一个容器，这个容器实现了控制反转(IoC)和依赖注入(DI)的特性，又称为IoC容器。

## IoC容器如何创建Bean？
![](/assets/img/spring/container-magic.png)
简单的回答就是IoC容器根据配置（XML或注解）来创建POJOs，Bean在创建后被维护在Spring容器中随时可被使用，同时Spring容器负责维护Bean的生命周期。

复杂点讲，要了解容器是如果创建，维护，销毁Bean需要深入容器机制的细节，如```Bean生命周期```, ```DI实现机制```,```集合值配置```,```集合值合并```等。

XML配置样例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

## 如何编写XML配置
事实上本章不应该讨论这个话题，应为文章关注的是Spring Framework的核心机制，而非具体操作。值得提及的有几点内容：

1. 结构化你的XML配置文件

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

2. p-namespace    
常规写法：

```xml
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- results in a setDriverClassName(String) call -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="masterkaoli"/>
</bean>
```

利用p-namespace简化xml结构，增强可读性。需要注意的是增加```xmlns:p="http://www.springframework.org/schema/p``` 命名空间

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="masterkaoli"/>
</beans>
```

## 使用XMl Or 注解
随着```Spring boot```的出现，构建基于Spring的项目变得更为简单，大量注解被使用，不再看到xml的身影；甚至是配置文件也从properties变成了yml。

## 理解Spring容器上下文(Context)
