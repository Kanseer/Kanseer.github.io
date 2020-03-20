---
title: spring-transaction
date: 2020-01-11 21:33:25
tags: springs
---

# Spring-three

## 自动注入

- 在Spring 配置文件中对象名和ref=”id”id 名相同使用自动注入,可以不配置<property/>

  - 在<bean>中通过autowire=”” 配置,只对这个<bean>生效
  - 在<beans>中通过default-autowire=””配置,表当当前文件中所有<bean>都是全局配置内容

- autowire=”” 可取值

  - default: 默认值,根据全局default-autowire=””值.默认全局和局部都没有配置情况下,相当于no

  - no: 不自动注入

  - byName: 通过名称自动注入.在Spring 容器中找类的Id

  - byType: 根据类型注入

    - spring 容器中不可以出现两个相同类型的<bean>

  - constructor: 根据构造方法注入

    - 提供对应参数的构造方法
    - 底层使用byName, 构造方法参数名和其他<bean>的id相同

    

## Spring 中加载properties 文件

- 在spring 配置文件中先引入xmlns:context

  ```xml
  <context:property-placeholder location="classpath:db.properties"/>
  ```

- 在被Spring 管理的类中通过@Value(“${key}”)取出properties 中内容



## scope 属性

- <bean>的属性,控制对象有效范围(单例,多例等),默认单例
- scope 可取值
  - singleton 默认值,单例
  - prototype 多例,每次获取重新实例化
  - request 每次请求重新实例化
  - session 每个会话对象内,对象是单例的
  - application 在application 对象内是单例
  - global session spring 推出的一个对象, 依赖于spring-webmvc-portlet ,类似于session

## 单例设计模式

- 在应用程序有保证最多只能有一个实例
- 提升运行效率,实现数据共享
- 懒汉式(由于添加了锁,所以导致效率低)

```java
public class SingleTon {
	//由于对象需要被静态方法调用,把方法设置为static
	//由于对象是static,必须要设置访问权限修饰符为private,如果是public 可以直接调用对象,不执行访问入口
	private static SingleTon singleton;
	/**
	* 方法名和类名相同
	* 无返回值.
	*
	* 其他类不能实例化这个类对象
	* 对外提供访问入口
	*/
	private SingleTon(){}
	/**
	* 实例方法,实例方法必须通过对象调用
	*
	* 设置方法为静态方法
	*
	* @return
	*/
    public static SingleTon getInstance(){
		//添加逻辑如果实例化过,直接返回
		if(singleton==null){
		/*
		* 多线程访问下,可能出现if 同时成立的情况,添加锁
		*/
			synchronized (SingleTon.class) {
				//双重验证
				if(singleton==null){
					singleton = new SingleTon();
				}
			}
		}
		return singleton;
	}
}
```

- 饿汉式
  - 解决了懒汉式中多线程访问可能出现同一个对象和效率低问题

```java
public class SingleTon {
	//在类加载时进行实例化.
	private static SingleTon singleton=new SingleTon();
	private SingleTon(){}
	public static SingleTon getInstance(){
		return singleton;
	}
}
```



## 声明式事务

- 编程式事务
  - 由程序员编写事务控制代码
- 声明式事务
  - 事务控制代码由spring写好,只需如何进行事务控制
  - 针对ServiceImpl类下方法的
  - 基于通知(advice)的
- 配置

```xml
<!-- 数据源 -->
	<context:property-placeholder location="classpath:db.properties"/>
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="${jdbc.driver}"></property>
		<property name="url" value="${jdbc.url}"></property>
		<property name="username" value="${jdbc.username}"></property>
		<property name="password" value="${jdbc.password}"></property>
	</bean>
	<!-- SqlSessionFactory -->
	<bean id="factory" class="org.mybatis.spring.SqlSessionFactoryBean" autowire="byName">
		<property name="typeAliasesPackage" value="com.whgc.pojo"></property>
	</bean>
	<!-- 扫描器 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="com.whgc.mapper"></property>
		<!-- <property name="sqlSessionFactory" ref="factory"></property> -->
		<property name="sqlSessionFactoryBeanName" value="factory"></property>
	</bean>
        
	<!-- spring jdbc中 -->
	<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	<!-- 配置声明式事务 -->
	<tx:advice id="txAdvice" transaction-manager="txManager">
		<tx:attributes>
			<tx:method name="ins*" />
			<tx:method name="del*"/>
			<tx:method name="upd*"/>
			<tx:method name="*" read-only="true"/>
		</tx:attributes>
	</tx:advice>
	<aop:config>
		<!-- 切点范围设置大些 -->
		<aop:pointcut expression="execution(* com.whgc.service.impl.*.*(..))" id="mypoint"/>
		<aop:advisor advice-ref="txAdvice" pointcut-ref="mypoint"/>
	</aop:config>
```

- 声明式事务属性
  - name="" 哪些方法
  - readonly="Boolean" 是否只读事务 false默认值
  - propagation 控制事务传播行为 
    - 当一个具有事务控制方法被另一个有事务控制方法调用后,需要如何管理事务(新建事务?在事务中执行?把事务挂起?报异常?)
    - REQUIRED(默认值):如果当前有事务,就在事务中执行,如果没有,就新建一个事务
    - SUPPORTS:如果当前有事务,就在事务中执行,如果没有,就在非事务下执行
    - MANDATORY:必须在事务中执行,如果当前有事务,就在事务中执行,如果没有事务,报错
    - REQUIRES_NEW:必须在事务中执行,如果当前没有事务,新建事务,如果有事务,把当前事务挂起
    - NOT_SUPPORTED:必须在非事务下执行,如果没有事务,正常执行,如果有事务,把当前事务挂起
    - NEVER:必须在非事务状态下执行,如果当前没有事务,正常执行,如果当前有事务,报错
    - NESTED:必须在事务状态下执行,如果没有事务,新建事务,如果有事务,创建一个嵌套事务
  - isolation="" 事务隔离级别(在多线程或并发下如何保证访问到的数据具有完整性)
    - 脏读: 一个事务(A)读取到另一个事务(B)中未提交的数据,另一个事务(B)中数据可能进行了改变,此时A事务读取的数据可能和数据库中数据不一致,此时数据是脏数据
    - 不可重复度(某行,针对修改,两次读取在一个事务内)当事务A第一次读取事务,事务B对事务A读取的数据进行修改,事务A中再次读取数据和之前数据不一致,过程不可重复读
    - 幻读(针对新增删除,两次事务结果)事务A按照特定条件查询出结果,事务B新增了一条符合条件的数据,事务A中查询的数据和数据库中
      的数据不一致,事务A好像出现了幻觉,这种情况称为幻读
    - DEFAULT:默认值,由底层自动判断应该使用什么是事务隔离
    - READ_UNCOMMITTED:可以读取未提交数据,可能出现脏读,不重复读,幻读. 效率最高
    - READ_COMMITED:只能读取其他事务已提交事务,可防止脏读,可能出现不可重复读,幻读
    - REPEATABLE_READ:读取的数据被添加锁,防止其他事物修改此数据,可防止不可重复读,可能出现幻读
    - SERIALIZABLE:排队操作对整个表添加锁,一个事务在操作数据时,另一个事务等待事务操作完成后才能操作这个表.最安全,效率最低
  - rollback-for="异常类型全限定路径":当出现什么异常时需要进行回滚
  - no-rollback-for:当出现什么异常不回滚



## spring常用注解

- @Component 创建类对象,相当于<bean/>
- @Service  ServiceImpl类上
- @Repository 写在数据访问层上
- @Controller 控制器类上
- @Resource(不需要写对象get/set)	java注解 默认按照byName注入,如果没有名称按byType
- @Autowired(不需要写对象get/set)	spring注解 默认按照byType注入
- @Value() 获取properties文件中内容
- @Pointcut() 定义切点
- @Aspect() 定义切面类
- @Before() 前置通知
- @After() 后置通知
- @AfterReturning 后置通知,切点必须正确执行
- @AfterThrowing 异常通知
- @Arround 环绕通知