---
title: Proxy
date: 2020-08-16 16:00
tags: Design pattern
categories:
- Java
---
# Proxy

## JDK动态代理

- 动态代理的意义在于生成一个占位（又称代理对象）， 来代理真实对象， 从而控制真实
  对象的访问

![proxy1](C:\Users\Lenovo\Desktop\img\proxy\proxy1.png)

- JDK 动态代理是`java.lang.reflect.*`包提供的方式，它必须借助一个接口才能产生代理对
  象,所以先定义接口.

```java
public interface HelloWorld{
    public void sayHelloWorld();
}
```

- 然后提供实现类HelloWordlmpl 来实现接口.

```java
public class HelloWorldimpl implements HelloWorld{
	@Override
	public void sayHelloWorld(){
        System.out.println("Hello World");
    }
}
```

- 在JDK 动态代理中，要实现代理逻辑类必须去实现java.lang.reflect.InvocationHandler
  接口，它里面定义了一个invoke 方法，并提供接口数组用于下挂代理对象.

```java
public class JdkProxyExample<T> implements InvocationHandler{
    
	private T target=null;//真实对象
    
    /**
     * @param target 真实对象
     * @return 代理对象
     */
    public T bind(T target){
        this.target=target;
        return (T)Proxy.newProxyInstance(target.getClass().getClassLoader(),
                target.getClass().getInterfaces(),this);
    }
    
     /**
     * @param proxy 代理对象 bind方法生成对象
     * @param method 当前调度方法
     * @param args 当前方法参数
     * @return 代理结果
     * @throws Throwable 异常
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("进入代理逻辑方法");
        System.out.println("在调度真实对象之前的服务");
        Object obj = method.invoke(target, args); //相当于调用sayHelloWorld方法
        System.out.println("在调度真实对象之后的服务");
        return obj;
    }
}
```

- 测试JDK动态代理.

```java
public class JdkProxyExampleTest {

    @Test
    public void testJdkProxy(){
        HelloWorld proxy=new JdkProxyExample<HelloWorldImpl>().bind(new HelloWorldImpl());
        proxy.sayHelloWorld();
    }
}
```

- 首先通过bind 方法绑定了代理关系，然后在代理对象调度sayHello World 方法时进入
  了代理的逻辑，测试结果如下.

![proxy2](C:\Users\Lenovo\Desktop\img\proxy\proxy2.png)

---



## CGLIB 动态代理

- CGLIB 动态代理,它的优势在于不需要提供接口,只要一个非抽象类就能实现动态代理.
- 定义一个类

```java
public class Hello{
    public void sayHello(String name) {
        System.out.println("Hello "+name);
    }
}
```

- CGLIB 的加强者Enhancer,通过设置超类的方法(setSuperclass),然后通过setCallback 方法设置哪个类为它的代理类.参数为this 就意味着是当前对象，那就要求用this 这个对象实现接口Methodinterceptor 的方法一一intercept, 然后返回代理对象.

```java
public class CglibProxyExample<T> implements MethodInterceptor {

    private T target=null;

    public T getProxy(T target){
        this.target=target;
        //CGLIB enhancer 增强类对象
        Enhancer enhancer=new Enhancer();
        //设置增强类型
        enhancer.setSuperclass(target.getClass());
        //定义代理逻辑对象为当前对象,要求当前对象实现MethodInterceptor接口方法
        enhancer.setCallback(this);
        //创建并返回代理对象
        return (T)enhancer.create();
    }


    /**
     * 代理逻辑方法
     * @param o 代理对象
     * @param method 方法
     * @param objects 方法参数
     * @param methodProxy 方法代理
     * @return 代理结果
     * @throws Throwable 异常
     */
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println("进入代理逻辑方法");
        System.out.println("调用真实对象前");
        Object result = methodProxy.invokeSuper(o, objects);
        System.out.println("调用真实对象后");
        return result;
    }

```

- 测试CGLIB动态代理

```java
public class CglibProxyExampleTest {

    @Test
    public void testCGLIBProxy(){
        Hello proxy= (Hello) new CglibProxyExample<Hello>().getProxy(new Hello());
        proxy.sayHello("张三");
    }
}
```

- 结果如下.

![proxy3](C:\Users\Lenovo\Desktop\img\proxy\proxy3.png)

---

