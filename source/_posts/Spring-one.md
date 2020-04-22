---
layout: Spring
title: Spring-synopsis
date: 2020-01-11 21:33:25
tags: Spring
categories:
- Java Frame
---
## 核心功能

- IOC/DI 控制反转/依赖注入

- AOP 面向切面编程

- 声明式事务

## Spring runtime

- test: spring提供测试功能

- Core Container: 核心容器 spring启动最基本的条件

- Beans: spring负责创建类对象并管理对象

  -  Core: 核心类
  -  Context: 上下文参数.获取外部资源或者管理注解等
  - SpEl: 表达式语言

- AOP: 实现aop功能需要依赖
- Aspects: 切面AOP依赖的包
- Data Access/Integration: spring封装数据访问层相关内容
  - JDBC: spring对JDBC封装的代码
  - ORM: 封装了持久层框架的代码.(Hibernate)
  - transaction: spring-tx.jar,声明事务使用

- WEB: spring完成web相关功能是需要

​		由Tomcat加载配置文件时需要spring-web.jar

​	![](/img/spring-runtime.jpg)

## IOC(控制反转)是什么

​	IoC完成new实例化对象事情,控制指控制类对象,反转指转交给spring负责

## 环境搭建

- 四个核心包一个日志包(commons-logging)

- src下新建applicationContext.xml(文件名称路径可自定义)

- 记住spring容器ApplicationContext,applicationContext.xml配置信息最终存储到了ApplicationContext容器中

- spring配置文件基于schema(.xsd),每次引入一个xsd文件是一个 namespace(xmlns)

- 配置文件只需引入基本schema,通过<bean/>创建对象,默认配置文件被加载时创建对象

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans
  xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instancexsi:schemaLocation="http://www.springframework.org/shema/beans
  http://www.springframework.org/schema/beans/spring-bans.xsd">
  <!-- id 表示获取到对象标识   class创建哪个类的对象
  -->
  <bean id="peo" class="com.bjsxt.pojo.People"/>
  </beans>
  ```

  测试方法

  ```java
  ApplicationContext ac = new
  ClassPathXmlApplicationContext("applicationContext.xm
  l");
  People people = ac.getBean("peo",People.class);
  System.out.println(people);
  ```

## Spring创建对象三种方式

- 通过构造方法

  1. 无参构造(默认)
  2. 有参构造(需配置)
     - 需在类提供有参构造方法
     - 在<construction-arg>标签配置(条件匹配多个构造执行最后一个)
     - 参数 
       - index:参数索引(0开始)
       - name: 参数名
       - type: 类型(区分int和Integer)

  ```xml
  <bean id="peo" class="com.bjsxt.pojo.People">
  <!-- ref 引用另一个 bean value 基本数据类型或String 等 -->
  <constructor-arg index="0" name="id" type="int"value="123"></constructor-arg>
  <constructor-arg index="1" name="name"type="java.lang.String" value="张三"></constructor-arg>
  </bean>
  ```

  

- 通过实例工厂

  ​	- 工厂设计模式:帮助创建类对象,一个工厂可以生产多个对象

  ```java
  public class PeopleFactory {
  	public People newInstance(){
  		return new People(1,"测试");
  	}
  }
  ```

  ​	- 在applicationContext.xml配置

  ```xml
  <bean id="factory" class="com.bjsxt.pojo.PeopleFactory"></bean>
  <bean id="peo1" factory-bean="factory" factory-method="newInstance"></bean>
  ```

- 静态工厂

  ​	- static 方法

  ```java
  public class PeopleFactory {
  	public static People newInstance(){
  		return new People(1,"测试");
  	}
  }
  ```

  ​	- 在applicationContext.xml配置

  ```xml
  <bean id="peo2" class="com.bjsxt.pojo.PeopleFactory" factory-method="newInstance"></bean>
  ```

## 给Bean属性赋值(注入)

- 通过构造方法

- 通过注入(set方法)

  - 如果属性是基本数据类型或String

  ```xml
  <bean id="peo" class="com.bjsxt.pojo.People">
  	<property name="id" value="222"></property>
  	<property name="name" value="张三"></property>
  </bean>
  ```

  - 如果是set,map,list,array...

  ```xml
  <property name="sets">
  	<set>
  		<value>1</value>
          <value>2</value>
  		<value>3</value>	
  	</set>
  </property>5.DI
  ```

## DI(依赖注入)是什么

​		当一个类(A)需要依赖另一个类(B)对象时,把B赋值给A的过程

```xml
<bean id="peo" class="com.bjsxt.pojo.People">
	<property name="desk" ref="desk"></property>
</bean>
<bean id="desk" class="com.bjsxt.pojo.Desk">
	<property name="id" value="1"></property>
	<property name="price" value="12"></property>
</bean>
```



​	