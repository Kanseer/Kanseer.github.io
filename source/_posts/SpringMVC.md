---
layout: SpringMVC
title: SpringMVC
date: 2020-03-27 13:31:49
tags: SpringMVC
categories:
- Java Frame
---


# Spring MVC

## 三层架构
>
> -  我们的开发架构一般都是基于两种形式，一种是 C/S 架构，也就是客户端/服务器，另一种是 B/S 架构，也就 是浏览器服务器。在 JavaEE 开发中，几乎全都是基于 B/S 架构的开发。那么在 B/S 架构中，系统标准的三层架构 包括：表现层、业务层、持久层。三层架构在我们的实际开发中使用的非常多，所以我们课程中的案例也都是基于 三层架构设计的我们的开发架构一般都是基于两种形式，一种是 C/S 架构，也就是客户端/服务器，另一种是 B/S 架构，也就 是浏览器服务器。在 JavaEE 开发中，几乎全都是基于 B/S 架构的开发。那么在 B/S 架构中，系统标准的三层架构 包括：表现层、业务层、持久层。三层架构在我们的实际开发中使用的非常多.
> - 表现层
>   - 也就是我们常说的web层.它负责接收客户端请求，向客户端响应结果，通常客户端使用http协议请求 web 层，web 需要接收 http 请求，完成 http 响应
>   - 表现层包括展示层和控制层：控制层负责接收请求，展示层负责结果的展示
>   - 表现层依赖业务层，接收到客户端请求一般会调用业务层进行业务处理，并将处理结果响应给客户端
>   - 表现层的设计一般都使用 MVC 模型.（MVC 是表现层的设计模型，和其他层没有关系）
> - 业务层
>   - 也就是我们常说的 service 层.它负责业务逻辑处理，和我们开发项目的需求息息相关.web 层依赖业 务层，但是业务层不依赖 web 层.
>   - 业务层在业务处理时可能会依赖持久层，如果要对数据持久化需要保证事务一致性。（也就是我们说的， 事务应该放到业务层来控制）
> - 持久层
>   - 也就是我们是常说的 dao 层.负责数据持久化，包括数据层即数据库和数据访问层，数据库是对数据进 行持久化的载体，数据访问层是业务层和持久层交互的接口，业务层需要通过数据访问层将数据持久化到数据库 中.通俗的讲，持久层就是和数据库交互，对数据库表进行曾删改查的

---

## MVC模型
>
> -  MVC 全名是 Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写， 是一种用于设计创建 Web 应用程序表现层的模式。MVC 中每个部分各司其职
>
> -  Model
>   - 通常指的就是我们的数据模型。作用一般情况下用于封装数据
> - View
>   - 通常指的就是我们的 jsp 或者 html。作用一般就是展示数据的
> - Controller
>   - 是应用程序中处理用户交互的部分。作用一般就是处理程序逻辑的

---

## SpringMVC
>
> -  SpringMVC 是一种基于 Java 的实现 MVC 设计模型的请求驱动类型的轻量级 Web 框架，属于 Spring FrameWork 的后续产品，已经融合在 Spring Web Flow 里面。Spring 框架提供了构建 Web 应用程序的全功 能 MVC 模块。使用 Spring 可插入的 MVC 架构，从而在使用 Spring 进行 WEB 开发时，可以选择使用 Spring 的 Spring MVC 框架或集成其他 MVC 开发框架，如 Struts1(现在一般不用)，Struts2 等。 SpringMVC 已经成为目前最主流的 MVC 框架之一，并且随着 Spring3.0 的发布，全面超越 Struts2，成 为最优秀的 MVC 框架。 它通过一套注解，让一个简单的 Java 类成为处理请求的控制器，而无须实现任何接口。同时它还支持 RESTful 编程风格的请求

![](/img/SpringMVC.png)

---

## SpringMVC优势
>
> -  清晰的角色划分
>   - 前端控制器（DispatcherServlet）
>   - 请求到处理器映射（HandlerMapping）
>   - 处理器适配器（HandlerAdapter）
>   - 视图解析器（ViewResolver）
>   - 处理器或页面控制器（Controller）
>   - 验证器（ Validator）
>   - 命令对象（Command 请求参数绑定到的对象就叫命令对象）
>   - 表单对象（Form Object 提供给表单展示和提交到的对象就叫表单对象）
> - 分工明确，而且扩展点相当灵活，可以很容易扩展，虽然几乎不需要.
> - 由于命令对象就是一个 POJO，无需继承框架特定 API，可以使用命令对象直接作为业务对象.
> - 和 Spring 其他框架无缝集成，是其它 Web 框架所不具备的.
> - 可适配，通过 HandlerAdapter 可以支持任意的类作为处理器.
> - 可定制性，HandlerMapping、ViewResolver 等能够非常简单的定制.
> - 功能强大的数据验证、格式化、绑定机制.
> - 利用 Spring 提供的 Mock 对象能够非常简单的进行 Web 层单元测试.
> - 本地化、主题的解析的支持，使我们更容易进行国际化和主题的切换.
> - 强大的 JSP 标签库，使 JSP 编写更容易.
> - RESTful风格的支持、简单的文件上传、约定大于配置的契约式编程支持、基于注解的零配 置支持等等.

---

## SpringMVC和Structs2优劣分析
>
> -  共同点
>   - 它们都是表现层框架，都是基于 MVC 模型编写的
>   - 它们的底层都离不开原始 ServletAPI
>   - 它们处理请求的机制都是一个核心控制器
> - 区别
>   - Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter 
>   - Spring MVC 是基于方法设计的，而 Struts2 是基于类，Struts2 每次执行都会创建一个动作类.所 以 Spring MVC 会稍微比 Struts2 快些.
>   - Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便(JSR303 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注 解加在我们 JavaBean 的属性上面，就可以在需要校验的时候进行校验了)
>   - Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些，但执行效率并没有比 JSTL 提 升，尤其是 struts2 的表单标签，远没有 html 执行效率高.

---

## SpringMVC入门
>
> - web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://java.sun.com/xml/ns/javaee"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
id="WebApp_ID" version="2.5">
	<!-- 配置 spring mvc 的核心控制器 -->
	<servlet>
		<servlet-name>SpringMVCDispatcherServlet</servlet-name>
		<servlet-class>
			org.springframework.web.servlet.DispatcherServlet
		</servlet-class>
		<!-- 配置初始化参数，用于读取 SpringMVC 的配置文件 -->
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:SpringMVC.xml</param-value>
		</init-param>
		<!-- 配置 servlet 的对象的创建时间点：应用加载时创建。取值只能是非 0 正整数，表示启动顺序 -->
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>SpringMVCDispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
</web-app>
```

> - SpringMVC.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:mvc="http://www.springframework.org/schema/mvc"
 xmlns:context="http://www.springframework.org/schema/context"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
 http://www.springframework.org/schema/mvc
 http://www.springframework.org/schema/mvc/spring-mvc.xsd
 http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">
 	<!-- 配置创建 spring 容器要扫描的包 -->
 	<context:component-scan base-package="com.test"></context:component-scan>
 	<!-- 配置视图解析器 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
 		<property name="prefix" value="/WEB-INF/pages/"></property>
 		<property name="suffix" value=".jsp"></property>
 	</bean>
    
    <!-- 配置spring开启注解mvc的支持-->
	<mvc:annotation-driven></mvc:annotation-driven>
</beans>
```

> - 编写控制器

```java
@Controller("helloController")
public class HelloController {
	@RequestMapping("/hello")
	public String sayHello() {
		System.out.println("HelloController 的 sayHello 方法执行了。。。。");
		return "success";
	}
}
```

## SpringMVC请求响应流程

![](/img/SpringMVC1.png)

---

## SpringMVC组件
>
> - DispatcherServlet：前端控制器
>   - 用户请求到达前端控制器，它就相当于 mvc 模式中的 c，dispatcherServlet 是整个流程控制的中心，由 它调用其它组件处理用户的请求，dispatcherServlet 的存在降低了组件之间的耦合性
> - HandlerMapping：处理器映射器
>   - HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的 映射方式，例如：配置文件方式，实现接口方式，注解方式等
> - Handler：处理器
>   - 它就是我们开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由 Handler 对具体的用户请求进行处理
> - HandlAdapter：处理器适配器
>   - 通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理 器进行执行
> - View Resolver：视图解析器
>   - View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名 即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户
> - View：视图
>   - SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView 等。我们最常用的视图就是 jsp.
> - <mvc:annotation-driven>说明

```xml
<!--在SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为SpringMVC的三大组件
使用<mvc:annotation-driven> 自动加载RequestMappingHandlerMapping(处理映射器)和
RequestMappingHandlerAdapter(处理适配器)-->
<!-- 上面的标签相当于 如下配置-->
<!-- Begin -->
<!-- HandlerMapping -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean>
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>

<!-- HandlerAdapter -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean>
<bean class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"></bean>
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>

<!-- HadnlerExceptionResolvers -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExceptionResolver"></bean>
<bean class="org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolver"></bean>
<bean class="org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver"></bean>
<!-- End -->
```

## 请求参数绑定
>
> -  绑定机制
>   - 表单中请求参数都是基于 key=value
>   - SpringMVC 绑定请求参数的过程是通过把表单提交请求参数，作为控制器中方法参数进行绑定的
> - 支持数据类型
>   - 基本类型参数
>     - 要求我们的参数名称必须和控制器中方法的形参名称保持一致。(严格区分大小写)
>   - POJO 类型参数
>     - 要求表单中参数名称和 POJO 类的属性名称保持一致。并且控制器方法的参数类型是 POJO 类型
>   - 数组和集合类型参数
>     - 1.要求集合类型的请求参数必须在 POJO 中。在表单中请求参数名称要和 POJO 中集合属性名称相同。 给 List 集合中的元素赋值，使用下标
>     - 2.接收的请求参数是 json 格式数据。需要借助一个注解实现
> -  请求参数中文乱码的解决(web,xml)

```xml
	<!-- 配置过滤器，解决中文乱码的问题 -->
	<filter>
		<filter-name>characterEncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filterclass>
			<!-- 指定字符集 -->
			<init-param>
				<param-name>encoding</param-name>
				<param-value>UTF-8</param-value>
			</init-param>
	</filter>
	<filter-mapping>
		<filter-name>characterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```

## 自定义类型转换器实现Converter的接口
>
> -  自定义类型转换器

```java
/**
* 把字符串转换成日期的转换器
* @author rt
*/
public class StringToDateConverter implements Converter<String, Date>{
	public Date convert(String source) {
		// 判断
		if(source == null) {
			throw new RuntimeException("参数不能为空");
		}
        try {
			DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
			// 解析字符串
			Date date = df.parse(source);
			return date;
		} catch (Exception e) {
			throw new RuntimeException("类型转换错误");
		}
    }
}
```

> -  注册自定义类型转换器，在springmvc.xml配置文件中编写配置

```xml
	<!-- 注册自定义类型转换器 -->
	<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
		<property name="converters">
			<set>
				<bean class="cn.itcast.utils.StringToDateConverter"/>
			</set>
		</property>
	</bean>
	<!-- 开启Spring对MVC注解的支持 -->
	<mvc:annotation-driven conversion-service="conversionService"/>
```

## 常用注解
>
> -  RequestParam注解
>   -  作用：把请求中的指定名称的参数传递给控制器中的形参赋值
>   - 属性
>     -  value：请求参数中的名称
>     -  required：请求参数中是否必须提供此参数，默认值是true，必须提供

```java
	/**
	* 接收请求
	* @return
	*/
	@RequestMapping(path="/hello")
	public String sayHello(@RequestParam(value="username",required=false)String name) {
		System.out.println("aaaa");
		System.out.println(name);
		return "success";
	}
```

> - RequestBody注解
>   - 作用：用于获取请求体的内容（注意：get方法不可以）
>   - 属性
>     -  required：是否必须有请求体，默认值是true

```java
	/**
	* 接收请求
	* @return
	*/
	@RequestMapping(path="/hello")
	public String sayHello(@RequestBody String body) {
		System.out.println("aaaa");
		System.out.println(body);
		return "success";
	}
```

> - PathVariable注解
>   - 作用：拥有绑定url中的占位符的。例如：url中有/delete/{id}，{id}就是占位符
>   - 属性
>     - value：指定url中的占位符名称
>   -  Restful风格的URL
>     - 请求路径一样，可以根据不同的请求方式去执行后台的不同方法
>     -  restful风格的URL优点
>       -  结构清晰 , 符合标准 ,易于理解 ,扩展方便

```java
	<a href="user/hello/1">入门案例</a>
	/**
	* 接收请求
	* @return
	*/
	@RequestMapping(path="/hello/{id}")
	public String sayHello(@PathVariable(value="id") String id) {
		System.out.println(id);
		return "success";
	}
```

> - RequestHeader注解
>   - 作用：获取指定请求头的值
>   - 属性
>     -  value：请求头的名称

```java
	@RequestMapping(path="/hello")
	public String sayHello(@RequestHeader(value="Accept") String header) {
		System.out.println(header);
		return "success";
	}
```

> -  CookieValue注解
>   - 作用：用于获取指定cookie的名称的值
>   - 属性
>     - value：cookie的名称

```java
	@RequestMapping(path="/hello")
	public String sayHello(@CookieValue(value="JSESSIONID") String cookieValue) {
		System.out.println(cookieValue);
		return "success";
	}
```

> -  ModelAttribute注解
>   - 作用
>     - 出现在方法上：表示当前方法会在控制器方法执行前线执行
>     -  出现在参数上：获取指定的数据给参数赋值
>   - 应用场景
>     - 当提交表单数据不是完整的实体数据时，保证没有提交的字段使用数据库原来的数据

- 修饰的方法有返回值

```java
	/**
	* 作用在方法，先执行
	* @param name
	* @return
	*/
	@ModelAttribute
	public User showUser(String name) {
		System.out.println("showUser执行了...");
		// 模拟从数据库中查询对象
		User user = new User();
		user.setName("哈哈");
		user.setPassword("123");
		user.setMoney(100d);
		return user;
	}

	/**
	* 修改用户的方法
	* @param cookieValue
	* @return
	*/
	@RequestMapping(path="/updateUser")
	public String updateUser(User user) {
		System.out.println(user);
		return "success";
	}

```

-  修饰的方法没有返回值

```java
	/**
	* 作用在方法，先执行
	* @param name
	* @return
	*/
	@ModelAttribute
	public void showUser(String name,Map<String, User> map) {
		System.out.println("showUser执行了...");
		// 模拟从数据库中查询对象
		User user = new User();
		user.setName("哈哈");
		user.setPassword("123");
		user.setMoney(100d);
		map.put("abc", user);
	}
	/**
	* 修改用户的方法
	* @param cookieValue
	* @return
	*/
	@RequestMapping(path="/updateUser")
	public String updateUser(@ModelAttribute(value="abc") User user) {
		System.out.println(user);
		return "success";
	}
```

> -  SessionAttributes注解
>   - 作用：用于多次执行控制器方法间的参数共享
>   - 属性
>     - value：指定存入属性的名称

```java
@Controller
@RequestMapping(path="/user")
@SessionAttributes(value= {"username","password","age"},types={String.class,Integer.class}) // 把数据存入到session域对象中
public class HelloController {
	/**
	* 向session中存入值
	* @return
	*/
	@RequestMapping(path="/save")
	public String save(Model model) {
		System.out.println("向session域中保存数据");
		model.addAttribute("username", "root");
		model.addAttribute("password", "123");
		model.addAttribute("age", 20);
		return "success";
	}
	/**
	* 从session中获取值
	* @return
	*/
	@RequestMapping(path="/find")
	public String find(ModelMap modelMap) {
		String username = (String) modelMap.get("username");
		String password = (String) modelMap.get("password");
		Integer age = (Integer) modelMap.get("age");
		System.out.println(username + " : "+password +" : "+age);
		return "success";
	}
	/**
	* 清除值
	* @return
	*/
	@RequestMapping(path="/delete")
    public String delete(SessionStatus status) {
        status.setComplete();
		return "success";
	}
}
```



