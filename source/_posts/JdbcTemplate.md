---
layout: spring
title: JdbcTemplate
date: 2020-03-25 09:13:38
tags: Spring
categories:
- Java Frame
---
>JdbcTemplate 概述
-  它是 spring 框架中提供的一个对象，是对原始 Jdbc API 对象的简单封装。spring 框架为我们提供了很多
的操作模板类
    -  操作关系型数据
        - JdbcTemplate
        - HibernateTemplate
    -  操作nosql数据库
        - RedisTemplate
    -  操作消息队列
        - JmsTemplate  
		
---

>JdbcTemplate对象的创建

```Java
    public JdbcTemplate() {
    }
    public JdbcTemplate(DataSource dataSource) {
        setDataSource(dataSource);
        afterPropertiesSet();
    }
    public JdbcTemplate(DataSource dataSource, boolean lazyInit) {
        setDataSource(dataSource);
        setLazyInit(lazyInit);
        afterPropertiesSet();
}

```
-  spring 配置文件中配置 JdbcTemplate

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 配置一个数据库的操作模板：JdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 配置数据源 -->
    <bean id="dataSource"class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql:///spring_day02"></property>
        <property name="username" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
</beans>
```
-  基本使用

```Java
public class JdbcTemplateDemo2 {
    public static void main(String[] args) {
        //1.获取 Spring 容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.根据 id 获取 bean 对象
        JdbcTemplate jt = (JdbcTemplate) ac.getBean("jdbcTemplate");
        //3.执行操作
        jt.execute("insert into account(name,money)values('eee',500)");
    }
}
```

>在Dao使用JdbcTemplate ==(两种方式)==

-  在 dao 中定义 JdbcTemplate
-  让 dao 继承 JdbcDaoSupport

```Java
//JdbcDaoSupport 是 spring 框架为我们提供的一个类，该类中定义了一个 JdbcTemplate 对象，我们可以直接获取使用，但是要想创建该对象，需要为其提供一个数据源具体源码如下
public abstract class JdbcDaoSupport extends DaoSupport {
    //定义对象
    private JdbcTemplate jdbcTemplate;
    //set 方法注入数据源，判断是否注入了，注入了就创建 JdbcTemplate
    public final void setDataSource(DataSource dataSource) {
        if (this.jdbcTemplate == null || dataSource != this.jdbcTemplate.getDataSource())
        { //如果提供了数据源就创建 JdbcTemplate
            this.jdbcTemplate = createJdbcTemplate(dataSource);
            initTemplateConfig();
        }
    }
    //使用数据源创建 JdcbTemplate
    protected JdbcTemplate createJdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
    //当然，我们也可以通过注入 JdbcTemplate 对象
    public final void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
        initTemplateConfig();
    }
    //使用 getJdbcTmeplate 方法获取操作模板对象
    public final JdbcTemplate getJdbcTemplate() {
        return this.jdbcTemplate;
    }

```

-  区别
    -  第一种在Dao类中定义JdbcTemplate的方式,适用于所有配置方式(xml 和注解都可以).
    -  第二种让Dao继承JdbcDaoSupport的方式,只能用于基于 XML 的方式,注解用不了.