---
title: spring-AOP
date: 2020-01-11 21:33:25
tags: springs
---
# Spring-two

## AOP

- AOP[Aspect Oriented Programming]:面向切面编程
- 正常程序执行流程都是纵向执行流程
  - 又叫面向编程,在原有纵向执行流程添加横切面(前置,后置通知)
  - 不需要修改原有程序代码(体现出程序高扩展,释放部分逻辑)
- 面向切面编程是什么?
  - 在程序原有纵向执行流程中,针对某一个或某一些方法添加通知,形成横面切面过程就叫做面向切面编程
- 常用概念
  - 原有功能: 切点, pointcut
  - 前置通知: 在切点之前执行的功能. before advice
  - 后置通知: 在切点之后执行的功能,after advice
  - 异常通知: 如果切点执行过程中出现异常,会触发异常通知.throws advice
  - 切面: 所有功能总称
  - 织入: 把切面嵌入到原有功能的过程叫做织入

## Schema-based实现AOP

- 每个通知都需要实现接口或类
- 配置spring 配置文件时在<aop:config>配置
- 实现步骤
  - 新建前置通知类
    - arg0: 切点方法对象Method 对象
    - arg1: 切点方法参数
    - arg2:切点在哪个对象中

- ```JAVA
  public class MyBeforeAdvice implements MethodBeforeAdvice {
  	@Override
  	public void before(Method arg0, Object[] arg1, Object arg2) throws Throwable {
  		System.out.println("执行前置通知");
  	}
  }
  ```

  - 新建后置通知类
    - arg0: 切点方法返回值
    - arg1:切点方法对象
    - arg2:切点方法参数
    - arg3:切点方法所在类的对象

- ```JAVA
  public class MyAfterAdvice implements AfterReturningAdvice {
  	@Override
  	public void afterReturning(Object arg0, Method arg1,Object[] arg2,Object arg3) throws Throwable {
  		System.out.println("执行后置通知");
  	}
  }
  ```

  - 配置spring 配置文件
    - 引入aop 命名空间
    - 配置通知类的<bean>
    - 配置切面
    - *通配符,匹配任意方法名,任意类名,任意一级包名
    - 如果希望匹配任意方法参数(..)

- ```XML
  <beans>
  <!-- 配置通知类对象,在切面中引入-->
  <bean id="mybefore" class="com.bjsxt.advice.MyBeforeAdvice"></bean>
  <bean id="myafter" class="com.bjsxt.advice.MyAfterAdvice"></bean>
  <!-- 配置切面-->
  <aop:config>
  <!-- 配置切点-->
  <aop:pointcut expression="execution(* com.bjsxt.test.Demo.demo2())" id="mypoint"/>
  <!-- 通知-->
  	<aop:advisor advice-ref="mybefore" pointcut-ref="mypoint"/>
  	<aop:advisor advice-ref="myafter" pointcut-ref="mypoint"/>
  </aop:config>
  <!-- 配置Demo 类,测试使用-->
  <bean id="demo" class="com.bjsxt.test.Demo"></bean>
  </beans>
  ```

  - 异常通知
    - 新建一个类实现throwsAdvice 接口,必须自己写方法,且必须叫afterThrowing,有两种参数方式,必须是1 个或4 个,异常类型要与切点报的异常类型一致

- ```java
  public class MyThrow implements ThrowsAdvice{
     /*
   	public void afterThrowing(Method m, Object[] args,Object target, Exception ex) {
  			 System.out.println("执行异常通知");
   }*/
  	public void afterThrowing(Exception ex) throws Throwable {
  		System.out.println("执行异常通过-schema-base 方式");
  	}
  }
  ```

- ```xml
  <bean id="mythrow" class="com.bjsxt.advice.MyThrow"></bean>
  <aop:config>
  	<aop:pointcut expression="execution(* com.bjsxt.test.Demo.demo1())" id="mypoint"/>
  	<aop:advisor advice-ref="mythrow" pointcut-ref="mypoint" />
  </aop:config>
  <bean id="demo" class="com.bjsxt.test.Demo"></bean>
  ```

  - 环绕通知
    - 把前置通知和后置通知都写到一个通知中,组成了环绕通知
    - 新建一个类实现MethodInterceptor接口

- ```java
  public class MyArround implements MethodInterceptor {
  	@Override
  	public Object invoke(MethodInvocation arg0) throws Throwable {
  		System.out.println("环绕-前置");
  		Object result = arg0.proceed();//放行,调用切点方式
  		System.out.println("环绕-后置");
  		return result;
  	}
  }
  ```

- ```xml
  <bean id="myarround" class="com.bjsxt.advice.MyArround"></bean>
  <aop:config>
  	<aop:pointcut expression="execution(* com.bjsxt.test.Demo.demo1())" id="mypoint"/>
  	<aop:advisor advice-ref="myarround" pointcut-ref="mypoint" />
  </aop:config>
  <bean id="demo" class="com.bjsxt.test.Demo"></bean>
  ```



## AspectJ实现AOP

- 新建类,不用实现,类中方法名任意

- ```java
  public class MyAdvice {
  	public void mybefore(String name1,int age1){
  		System.out.println("前置"+name1 );
  	}
  	public void myaftering(){
  		System.out.println("后置2");
  	}
  	public void myafter(){
  		System.out.println("后置1");
  	}
  	public void mythrow(){
  		System.out.println("异常");
  	}
  	public Object myarround(ProceedingJoinPoint p) throws Throwable{
  		System.out.println("执行环绕");
  		System.out.println("环绕-前置");
  		Object result = p.proceed();
  		System.out.println("环绕后置");
  		return result;
  	}
  }
  	
  ```

- 配置spring 配置文件

  - <aop: xxxx/> 表示什么通知

  - <aop:after/> 后置通知,是否出现异常都执行
  - <aop:after-returing/> 后置通知,只有当切点正确执行时执行
  - <aop:after/> 和<aop:after-returing/> 和<aop:after-throwing/>执行顺序和配置顺序有关
  - execution() 括号不能扩上args
  - 中间使用and 不能使用&& 由spring 把and 解析成&&
  - args(名称) 名称自定义的.顺序和demo1(参数,参数)对应
  - <aop:before/> arg-names=” 名称” 名称来源于expression=”” 中args(),名称必须一样
  - args() 有几个参数,arg-names 里面必须有几个参数
  - arg-names=”” 里面名称必须和通知方法参数名对应

- ```xml
  <aop:config>
  	<aop:aspect ref="myadvice">
  	<aop:pointcut expression="execution(* com.bjsxt.test.Demo.demo1(String,int)) and args(name1,age1)" id="mypoint"/>
  	<aop:pointcut expression="execution(* com.bjsxt.test.Demo.demo1(String)) and args(name1)" id="mypoint1"/>
       <aop:before method="mybefore" pointcut-ref="mypoint" arg names="name1,age1"/>
  	<aop:after method="myafter" pointcut-ref="mypoint"/>
  	<aop:after-returning method="myaftering" pointcut-ref="mypoint"/>
  	<aop:after-throwing method="mythrow" pointcut-ref="mypoint"/>
  	<aop:around method="myarround" pointcut-ref="mypoint"/>-->
  	</aop:aspect>
  </aop:config>
  ```



## 使用注解(基于Aspect)

- spring 不会自动去寻找注解,必须告诉spring 哪些包下的类中可能有注解

- ```xml
  <context:component-scan base-package="com.bjsxt.advice"></context:component-scan>
  ```

- @Component相当于<bean/>,如果没有参数,把类名首字母变小写,相当于<bean id=""/>

- 在方法上添加@Pointcut(“ ”) 定义切点

- ```java
  @Component
  public class Demo {
  	@Pointcut("execution(* com.bjsxt.test.Demo.demo1())")
  	public void demo1() throws Exception{
  	// int i = 5/0;
  	System.out.println("demo1");
  	}
  }
  ```

- 在通知类中配置,@Component 类被spring 管理,@Aspect 相当于<aop:aspect/>表示通知方法在当前类中

- ```java
  @Component
  @Aspect
  public class MyAdvice {
  	@Before("com.bjsxt.test.Demo.demo1()")
  	public void mybefore(){
  		System.out.println("前置");
  	}
  	@After("com.bjsxt.test.Demo.demo1()")
  	public void myafter(){
  		System.out.println("后置通知");
  	}
  	@AfterThrowing("com.bjsxt.test.Demo.demo1()")
  	public void mythrow(){
  		System.out.println("异常通知");
  	}
  	@Around("com.bjsxt.test.Demo.demo1()")
  	public Object myarround(ProceedingJoinPoint p) throws Throwable{
  		System.out.println("环绕-前置");
  		Object result = p.proceed();
  		System.out.println("环绕-后置");
  		return result;
  	}
  }
  ```



## 代理设计模式

- 优点

  - 保护真实对象
  - 让真实对象职责更明确
  - 扩展

- 静态代理

  - 由代理对象代理所有真实对象的功能,自己编写代理类,每个代理的功能需要单独编写
  - 缺点当代理功能比较多时,代理类中方法需要写很多

- 动态代理

  - 为了解决静态代理频繁编写代理功能缺点
  - JDK 提供的
  - cglib 动态代理

- JDK 动态代理

  - 和cglib 动态代理对比
    - 优点: jdk 自带,不需要额外导入jar
    - 缺点: 真实对象必须实现接口,利用反射机制.效率不高
    - 使用JDK 动态代理时可能出现异常(出现原因:希望把接口对象转换为具体真实对象)

- cglib 动态代理

  - 优点: 基于字节码,生成真实对象的子类,运行效率高于JDK 动态代理,不需要实现接口

  - 缺点: 非JDK 功能,需要额外导入jar

  - 使用spring aop 时,只要出现Proxy 和真实对象转换异常,设置为true 使用cglib,设置为false 使用jdk(默认值)

  - ```xml
    <aop:aspectj-autoproxy proxy-target-class="true"></aop:aspectj-autoproxy>
    ```

    