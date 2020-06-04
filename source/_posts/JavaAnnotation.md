---
title: JavaAnnotation
date: 2020-06-04 15:05:21
tags: annotation
categories:
- Java
---
# Java 枚举类与注解

## 枚举类使用

- 枚举类的实现
  - `JDK 1.5`之前需要自定义枚举类.
  - `JDK 1.5` 新增的 `enum`关键字用于定义枚举类.
- 若枚举只有`一个`对象, 则可以作为一种单例模式的实现方式.
- 枚举类的属性
  - 枚举类对象的属性`不允许改动`, 所以应该使用 `private fina`修饰.
  - 枚举类的使用 private final 修饰的属性应该在构造器中为其赋值.
  - 若枚举类显式的定义了带参数的构造器, 则在列出枚举值时也必须对应的 传入参数.



## 自定义枚举类

- `私有化`类的构造器，保证不能在类的外部创建其对象.
- 在类的内部创建枚举类的实例。声明为：`public static final.`
- 对象如果有实例变量，应该声明为`private final`，并在构造器中初始化 .

```java
class Season{
    
    private final String SEASONNAME;
    private final String SEASONDESC;

    private Season(String SEASONNAME,String SEASONDESC){
        this.SEASONNAME=SEASONNAME;
        this.SEASONDESC=SEASONDESC;
    }

    public static final Season SPRING=new Season("春天","春暖花开");
    public static final Season SUMMER=new Season("夏天","夏日炎炎");
    public static final Season AUTUMN=new Season("秋天","秋高气爽");
    public static final Season WINTER=new Season("冬天","白雪皑皑");
}
```



## 使用enum定义枚举类

- 使用 `enum` 定义的枚举类默认继承了 `java.lang.Enum`类，因此不能再继承其他类.
- 枚举类的构造器只能使用 `private` 权限修饰符.
- 枚举类的所有实例必须在枚举类中显式列出`(, 分隔 ; 结尾)`。列出的 实例系统会自动添加` public static final` 修饰.
- `必须在枚举类的第一行声明枚举类对象.`
- `JDK1.5` 中可以在 `switch 表达式`中使用`Enum定义的枚举类`的对象 作为表达式,` case `子句可以直接使用枚举值的名字, 无需添加枚举 类作为限定.

```Java
enum SeasonEnum{
    
    SPRING("春天","春暖花开"),
    SUMMER("夏天","夏日炎炎"),
    AUTUMN("秋天","秋高气爽"),
    WINTER("冬天","白雪皑皑");

    private final String seasonName;
    private final String SeasonDesc;

    private SeasonEnum(String seasonName,String seasonDesc){
        this.seasonName=seasonName;
        this.SeasonDesc=seasonDesc;
    }
}
```

![](/img/javaannotation/1.png)



## 枚举类主要方法

- `values()` : 返回枚举类型的对象数组。该方法可以很方便地遍历所有的枚举值.
- `valueOf(String str)` : 可以把一个字符串转为对应的枚举类对象.要求字符串必须是枚举类对象的“名字”.如不是，会有运行时异常：` IllegalArgumentException.`
- `toString()`：返回当前枚举类对象常量的名称.



## 实现接口的枚举类

- 和普通 Java 类一样，枚举类可以实现一个或多个接口.
- 若每个枚举值在调用实现的接口方法呈现相同的行为方式，则只 要统一实现该方法即可.
- 若需要每个枚举值在调用实现的接口方法呈现出不同的行为方式, 则可以让每个枚举值分别来实现该方法.



## 注解Annotation

- 从 `JDK 5.0 `开始, Java 增加了对`元数据(MetaData) `的支持, 也就是 `Annotation(注解)`.
- `Annotation `其实就是代码里的`特殊标记`, 这些标记可以在编译, 类加 载, 运行时被读取, 并执行相应的处理。通过使用` Annotation`, 程序员 可以在不改变原有逻辑的情况下, 在源文件中嵌入一些补充信息.代码分析工具、开发工具和部署工具可以通过这些补充信息进行验证或者进行部署.
- `Annotation` 可以像修饰符一样被使用, 可用于`修饰包,类, 构造器, 方 法, 成员变量, 参数, 局部变量的声明`, 这些信息被保存在 `Annotation 的 “name=value” `对中.
- 在`JavaSE`中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等.在`JavaEE/Android`中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替`JavaEE`旧版中所遗留的繁冗代码和`XML`配置等.
- 未来的开发模式都是基于注解的，`JPA`是基于注解的，`Spring2.5`以上都是基于注解的，`Hibernate3.x`以后也是基于注解的，现在的 `Struts2`有一部分也是基于注解的了，注解是一种趋势，一定程度上 可以说：`框架 = 注解 + 反射 + 设计模式`.
- 使用` Annotation `时要在其前面增加 `@ 符号`, 并把该` Annotation 当成 一个修饰符使用`.用于修饰它支持的程序元素.



## 生成文档相关的注解

- `@author `  标明开发该类模块的作者，多个作者之间使用,分割
- `@version` 标明该类模块的版本
- `@see` 参考转向，也就是相关主题
- `@since` 从哪个版本开始增加的
- `@param` 对方法中某参数的说明，如果没有参数就不能写  `(只用于方法)`  格式 :  `@param 形参名 形参类型 形参说明`
- `@return` 对方法返回值的说明，如果方法的返回值类型是void就不能写 `(只用于方法)` 格式 : `@return 返回值类型 返回值说明`
- `@exception` 对方法可能抛出的异常进行说明 ，如果方法没有用throws显式抛出的异常就不能写`(只用于方法)`  格式 :  `@exception 异常类型 异常说明` . 

```java
/**
* @author shkstart
* @version 1.0
* @see Math.java
*/
public class JavadocTest {
	/**
	* 程序的主方法，程序的入口
	* @param args String[] 命令行参数
	*/
	public static void main(String[] args) {
	
    }
	/**
	* 求圆面积的方法
	* @param radius double 半径值
	* @return double 圆的面积
	*/
	public static double getArea(double radius){
		return Math.PI * radius * radius;
	}
}

```



## 在编译时进行格式检查(JDK内置的三个基本注解)

- `@Override` : 限定重写父类方法, 该注解只能用于方法
- `@Deprecated`: 用于表示所修饰的元素(类, 方法等)`已过时`.通常是因为所修饰的结构危险或存在更好的选择
- `@SuppressWarnings` : 抑制编译器警告

```java
public class AnnotationTest{
	public static void main(String[] args) {
		@SuppressWarnings("unused")
		int a = 10;
	}
	@Deprecated
	public void print(){
		System.out.println("过时的方法");
	}
	@Override
		public String toString() {
		return "重写的toString方法()";
	}
}
```



## 跟踪代码依赖性，实现替代配置文件功能

- `Servlet3.0`提供了注解(annotation),使得不再需要在`web.xml`文件中进行`Servlet`的部署

```java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    
	private static final long serialVersionUID = 1L;
    
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { }
	
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	} 
}
```



```xml
<servlet>
	<servlet-name>LoginServlet</servlet-name>
	<servlet-class>com.servlet.LoginServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>LoginServlet</servlet-name>
	<url-pattern>/login</url-pattern>
</servlet-mapping>
```



## 自定义 Annotation

- 定义新的 `Annotation `类型使用 `@interface`关键字.
- 自定义注解自动继承了`java.lang.annotation.Annotation`接口.
- `Annotation` 的成员变量在 `Annotation` 定义中以无参数方法的形式来声明.其 方法名和返回值定义了该成员的名字和类型.我们称为配置参数.类型只能是`八种基本数据类型`、`String类型`、`Class类型`、`enum类型`、`Annotation类型`、 以上所有类型的数组.
- 可以在定义 `Annotation` 的成员变量时为其指定初始值, 指定成员变量的初始值可使用 `default` 关键字.
- 如果只有一个参数成员，建议使用参数名为`value`.
- 如果定义的注解含有配置参数，那么使用时必须指定参数值，除非它有默认 值。格式是`“参数名 = 参数值”` ，如果只有`一个参数成员`，且名称为`value`， `可以省略“value=”`
- `没有成员`定义的 `Annotation` 称为`标记`; `包含成员`变量的`Annotation` 称为`元数据Annotation`
- 元数据的理解：String name = “kanseer” `修饰数据的数据`

```java
@MyAnnotation(value = "kanseer0001")
public class TestAnnotation {
    public static void main(String[] args) {
        Class clazz=TestAnnotation.class;
        Annotation a = clazz.getAnnotation(MyAnnotation.class);
        MyAnnotation m=(MyAnnotation)a;
        String value = m.value();
        System.out.println(value);
    }
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface MyAnnotation{
    String value() default "Kanseer";
}

```



## JDK中的元注解

- 元注解用来修饰其他注解定义
- `JDK5.0`提供了4个标准的`meta-annotation`类型
  - `Retention`
  - `Target`
  - `Documented`
  - `Inherited`

### @Retention

- 只能用于修饰一个 Annotation 定义, 用于指定该 Annotation 的生命 周期,` @Rentention` 包含一个 `RetentionPolicy` 类型的成员变量, 使用 `@Rentention` 时必须为该 `value` 成员变量指定值.
- `RetentionPolicy.SOURCE`  : 在源文件中有效(即源文件保留),编译器直接丢弃这种策略的注释.
- `RetentionPolicy.CLASS`  : 在class文件中有效(即class保留),当运行 Java 程序时, JVM不会保留注解,这是默认值.
- `RetentionPolicy.RUNTIME`  : 在运行时有效(即运行时保留),当运行 Java 程序时, JVM 会 保留注释,程序可以通过反射获取该注释.

```java
public enum RetentionPolicy{
		SOURCE,
		CLASS,
		RUNTIME
}
```



### @Target

- 用于修饰 Annotation 定义, 用于指定被修饰的 Annotation 能用于修饰哪些程序元素, `@Target` 也包含一个返回值为`ElementType`类型名为 value 的成员变量.
- `ElementType.ANNOTATION_TYPE`  : 注解类型声明
- `ElementType.CONSTRUCTOR`  : 构造函数声明
- `ElementType.FIELD` : 字段声明(包括枚举常数)
- `ElementType.LOCAL_VARIABLE`  : 局部变量声明
- `ElementType.METHOD` : 方法声明
- `ElementType.PACKAGE` : 包装声明
- `ElementType.PARAMENTER` : 正式参数声明
- `ElementType.TYPE` : 类,接口(包括注解类型),或枚举声明
- `ElementType.TYPE_PARAMENTER` : 键入参数声明
- `ElementType.TYPE_USER` 使用类型声明

```java
public enum ElementType {
    /** Class, interface (including annotation type), or enum declaration */
    TYPE,

    /** Field declaration (includes enum constants) */
    FIELD,

    /** Method declaration */
    METHOD,

    /** Formal parameter declaration */
    PARAMETER,

    /** Constructor declaration */
    CONSTRUCTOR,

    /** Local variable declaration */
    LOCAL_VARIABLE,

    /** Annotation type declaration */
    ANNOTATION_TYPE,

    /** Package declaration */
    PACKAGE,

    /**
     * Type parameter declaration
     *
     * @since 1.8
     */
    TYPE_PARAMETER,

    /**
     * Use of a type
     *
     * @since 1.8
     */
    TYPE_USE
}
```



### @Documented与@Inherited

- `@Documented`  : 用于指定被该元 Annotation 修饰的 Annotation 类将被` javadoc` 工具提取成文档.默认情况下，javadoc是不包括注解的.
- `定义为Documented的注解必须设置Retention值为RUNTIME`
- `@Inherited`  : 被它修饰的 Annotation 将具有`继承性`.如果某个类使用了被`@Inherited`修饰的 Annotation, 则其子类将自动具有该注解.
- `实际应用中，使用较少`



## 利用反射获取注解信息

- ``JDK 5.0` 在 `java.lang.reflect` 包下新增了 `AnnotatedElement 接口`, 该接口`代表程序中可以接受注解的程序元素`.
- `当一个 Annotation 类型被定义为运行时 Annotation 后`, 该注解才是运行时 可见, 当class文件被载入时保存在 class 文件中的 Annotation 才会被虚拟机读取.
- 程序可以调用 `AnnotatedElement`对象的如下方法来访问 Annotation 信息

![](/img/javaannotation/2.png)