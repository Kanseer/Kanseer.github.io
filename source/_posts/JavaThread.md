---
title: JavaThread
date: 2020-04-23 17:59:55
tags: thread
categories:
- Java
---
# Java 多线程

## 基本概念

- **程序**
  - 是为完成特定任务、用某种语言编写的一组指令的集合。即指一 段`静态`的代码，静态对象
- **进程**
  - 是程序的一次执行过程，或是正在运行的一个程序。是一个`动态`的过程：有它自身的`产生`、`存在`和`消亡的过程`.——生命周期
- **线程**
  - 进程可进一步细化为线程，是一个程序内部的一条执行路径
- **单核与多核**
  - 单核CPU，其实是一种假的多线程，因为在一个时间单元内，也只能执行一个线程 的任务
  - 如果是多核的话，才能更好的发挥多线程的效率
  - 一个Java应用程序java.exe，其实至少有三个线程：`main()`主线程，`gc()` 垃圾回收线程，`异常处理`线程.当然如果发生异常，会影响主线程
- **并行与并发**
  - **并行**:`多个`CPU同时执行`多个`任务
  - **并发**:`一个`CPU(采用时间片)`同时执行多个`任务
- **使用多线程的优点**
  - 提高应用程序的响应。对图形化界面更有意义，可增强用户体验
  - 提高计算机系统CPU的利用率
  - 改善程序结构。将既长又复杂的进程分为多个线程，独立运行，利于理解和修改
- **何时需要多线程**
  - 程序需要同时执行两个或多个任务
  - 程序需要实现一些需要等待的任务时，如用户输入、文件读写 操作、网络操作、搜索等
  - 需要一些后台运行的程序时

---

## 线程的创建和使用

### Thread类

- `Thread()`：创建新的Thread对象
- `Thread(String threadname)`：创建线程并指定线程实例名
- `Thread(Runnable target)`：指定创建线程的目标对象，它实现了Runnable接 口中的run方法
- `Thread(Runnable target, String name)`：创建新的Thread对象
- **特性**
  - 每个线程都是通过某个特定`Thread`对象的`run()`方法来完成操作的，经常 把run()方法的主体称为`线程体`
  - 通过该`Thread`对象的`start()`方法来启动这个线程，而非直接调用run()
- **创建线程方式**(JDK1.5之前)
  - 继承`Thread类`的方式
  - 实现`Runnable接口`的方式
- **区别**
  - `继承Thread`：线程代码存放Thread子类run方法中
  - `实现Runnable`：线程代码存在接口的子类的run方法
- **实现好处**
  - 避免了单继承的局限性
  - 多个线程可以共享同一个接口实现类的对象，非常适合多个相同线 程来处理同一份资源
- **Thread类方法**
  - `start()` :启动线程，并执行对象的run()方法
  - `run()` :线程在被调度时执行的操作
  - `String getName()` : 返回线程的名称
  - `setName(String name)` :设置该线程名称
  - `static Thread currentThread()` : 返回当前线程
    - 在Thread子类中就 是this，通常用于主线程和Runnable实现类
  - `static void yield()`：线程让步
    - 暂停当前正在执行的线程，把执行机会让给优先级相同或更高的线程
    - 若队列中没有同优先级的线程，忽略此方法
  - `join()` : 当某个程序执行流中调用其他线程的 join() 方法时，调用线程将 被阻塞，直到 join() 方法加入的 join 线程执行完为止
    - 低优先级的线程也可以获得执行
  - `static void sleep(long millis)`: (指定时间:毫秒)
    - 令当前活动线程在指定时间段内放弃对CPU控制,使其他线程有机会被执行,时间到后 重排队
    - 抛出`InterruptedException`异常
  - `stop()` : 强制线程生命期结束，不推荐使用
  - `boolean isAlive()` : 返回boolean，判断线程是否还活着

---

### 线程调度

- **调度策略**
  - 时间片
  - 抢占式：高优先级的线程抢占CPU
- **调度方法**
  - 同优先级线程组成先进先出队列（先到先服务），使用时间片策略
  - 对高优先级，使用优先调度的抢占式策略
- **线程的优先级等级**
  - `MAX_PRIORITY`：10 
  - `MIN _PRIORITY`：1 
  - `NORM_PRIORITY`：5 
- **方法**
  - `getPriority()` ：返回线程优先值
  - `setPriority(int newPriority)` ：改变线程的优先级
- **说明**
  - 线程创建时继承父线程的优先级
  - 低优先级只是获得调度的概率低，并非一定是在高优先级线程之后才被调用
- **线程分类**
  - 一种是守护线程，一种是用户线程
  - 它们在几乎每个方面都是相同的，唯一的区别是判断JVM何时离开
  - 守护线程是用来服务用户线程的，通过在start()方法前调用 `thread.setDaemon(true)`可以把一个用户线程变成一个守护线程
  - Java垃圾回收就是一个典型的守护线程
  - 若JVM中都是守护线程，当前JVM将退出
  - 形象理解：`兔死狗烹，鸟尽弓藏`

---

### 线程生命周期

- `Thread.State类`定义了线程的几种状态
- `新建`：当一个Thread类或其子类的对象被声明并创建时，新生的线程对象处于新建状态
- `就绪`：处于新建状态的线程被start()后，将进入线程队列等待CPU时间片，此时它已具备了运行的条件，只是没分配到CPU资源
- `运行 `: 当就绪的线程被调度并获得CPU资源时,便进入运行状态， run()方法定义了线程的操作和功能
- `阻塞` : 在某种特殊情况下，被人为挂起或执行输入输出操作时，让出 CPU 并临时中止自己的执行，进入阻塞状态
- `死亡 `: 线程完成了它的全部工作或线程被提前强制性地中止或出现异常导致结束

![](/img/javathread/1.png)

![](/img/javathread/2.png)

---

### 线程同步

- **问题的提出**
  - 多个线程执行的不确定性引起执行结果的不稳定
  - 多个线程对账本的共享，会造成操作的不完整性，会破坏数据

![](/img/javathread/3.png)

- **同步机制**

  - 同步代码块

  ```java
  synchronized(对象){
  	//需要被同步的代码;
  }
  ```

  - 同步方法

  ```java
  public synchronized void show(String name){
  	...
  }
  ```

- **锁**

  - 同步锁机制 : 对于并发工作，你需要某种方式来防 止两个任务访问相同的资源（其实就是共享资源竞争）。 防止这种冲突的方法 就是当资源被一个任务使用时，在其上加锁。第一个访问某项资源的任务必须锁定这项资源，使其他任务在其被解锁之前，就无法访问它了，而在其被解锁之时，另一个任务就可以锁定并使用它了
  - synchronized的锁
    - 任意对象都可以作为同步锁。所有对象都自动含有单一的锁（监视器）
    - 同步方法的锁：静态方法（类名.class）、非静态方法（this）
    - 同步代码块：自己指定，很多时候也是指定为this或类名.class
  - 注意
    - 必须确保使用同一个资源的多个线程共用一把锁，这个非常重要，否则就 无法保证共享资源的安全
    - 一个线程类中的所有静态方法共用同一把锁（类名.class），所有非静态方 法共用同一把锁（this），同步代码块（指定需谨慎）

- **释放锁的操作**

  - 当前线程的同步方法、同步代码块执行结束
  - 当前线程在同步代码块、同步方法中遇到break、return终止了该代码块、 该方法的继续执行
  - 当前线程在同步代码块、同步方法中出现了未处理的Error或Exception，导 致异常结束
  - 当前线程在同步代码块、同步方法中执行了线程对象的wait()方法，当前线 程暂停，并释放锁

- **不会释放锁的操作**

  - 线程执行同步代码块或同步方法时，程序调用`Thread.sleep()`、 `Thread.yield()`方法暂停当前线程的执行
  - 线程执行同步代码块时，其他线程调用了该线程的`suspend()`方法将该线程 挂起，该线程不会释放锁（同步监视器）

- **单例设计模式之懒汉式**

```java
class Singleton{
 	private static Singleton instance = null;
    private Singleton(){}
    public static Singleton getInstance(){
    	if(instance==null){
            synchronized(Singleton.class){
        		if(instance==null){
                	instance=new Singleton();
                }
            }
           	return instance;
        }   
    }
}
```

- **线程死锁**
  - 不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃 自己需要的同步资源，就形成了线程的死锁
  - 出现死锁后，不会出现异常，不会出现提示，只是所有的线程都处于 阻塞状态，无法继续
  - 解决方法
    - 专门的算法、原则
    - 尽量减少同步资源的定义
    - 尽量避免嵌套同步

### Lock(锁)

- 从JDK 5.0开始，Java提供了更强大的线程同步机制——通过显式定义同 步锁对象来实现同步。同步锁使用Lock对象充当
- `java.util.concurrent.locks.Lock`接口是控制多个线程对共享资源进行访问的 工具。锁提供了对共享资源的独占访问，每次只能有一个线程对Lock对象 加锁，线程开始访问共享资源之前应先获得Lock对象
- `ReentrantLock 类`实现了 Lock ，它拥有与 synchronized 相同的并发性和 内存语义，在实现线程安全的控制中，比较常用的是`ReentrantLock`，可以 显式加锁、释放锁

```java
class A{
 	private final ReentrantLock lock = new RennTrantLock();
    public void m{
    	lock.lock();
        try{
            //保证线程安全的代码;
        }
        finally{
        	lock.unlock();
        }
    }
    
}
```

- **synchronized 与 Lock 的对比**
  - Lock是显式锁（手动开启和关闭锁，别忘记关闭锁），synchronized是 隐式锁，出了作用域自动释放
  - Lock只有代码块锁，synchronized有代码块锁和方法锁
  - 使用Lock锁，JVM将花费较少的时间来调度线程，性能更好。并且具有 更好的扩展性（提供更多的子类）

---

### 线程通信

- `wait() 与 notify() 和 notifyAll()`
  - `wait()`：令当前线程挂起并放弃CPU、同步资源并等待，使别的线程可访问并修改共享资源，而当 前线程排队等候其他线程调用`notify()`或`notifyAll()`方法唤醒，唤醒后等待重新获得对监视器的所有 权后才能继续执行
  - `notify()`：唤醒正在排队等待同步资源的线程中优先级最高者结束等待
  - `notifyAll ()`：唤醒正在排队等待资源的所有线程结束等待
- 这三个方法只有在`synchronized方法`或`synchronized代码块`中才能使用，否则会报 `java.lang.IllegalMonitorStateException`异常
- 因为这三个方法必须有锁对象调用，而任意对象都可以作为synchronized的同步锁， 因此这三个方法只能在Object类中声明
- **经典问题：生产者/消费者问题**

---

### JDK5.0新增线程创建方式

- **实现Callable接口**
  - 相比run()方法，可以有返回值
  - 方法可以抛出异常
  - 支持泛型的返回值
  - 需要借助FutureTask类，比如获取返回结果
- **Future接口**
  - 可以对具体Runnable、Callable任务的执行结果进行取消、查询是 否完成、获取结果等
  - FutrueTask是Futrue接口的唯一的实现类
  - FutureTask 同时实现了Runnable, Future接口。它既可以作为 Runnable被线程执行，又可以作为Future得到Callable的返回值
- **线程池**
  - `背景`：经常创建和销毁、使用量特别大的资源，比如并发情况下的线程， 对性能影响很大
  - `思路`：提前创建好多个线程，放入线程池中，使用时直接获取，使用完 放回池中。可以避免频繁创建销毁、实现重复利用。类似生活中的公共交 通工具
  - 提高响应速度（减少了创建新线程的时间）
  - 降低资源消耗（重复利用线程池中线程，不需要每次都创建）
  - 便于线程管理
    - `corePoolSize`：核心池的大小
    - `maximumPoolSize`：最大线程数
    - `keepAliveTime`：线程没有任务时最多保持多长时间后会终止
- **线程池API**
  - `ExecutorService`：真正的线程池接口。常见子类`ThreadPoolExecutor`
    - `void execute(Runnable command)` ：执行任务/命令，没有返回值，一般用来执行 Runnable
    - `<T> Future<T> submit(Callable<T> task)`：执行任务，有返回值，一般又来执行 Callable
    - `void shutdown()` : 关闭连接池
  - `Executors`：工具类、线程池的工厂类，用于创建并返回不同类型的线程池
    - `Executors.newCachedThreadPool()`：创建一个可根据需要创建新线程的线程池
    - `Executors.newFixedThreadPool(n)` : 创建一个可重用固定线程数的线程池
    - `Executors.newSingleThreadExecutor()` : 创建一个只有一个线程的线程池
    - `Executors.newScheduledThreadPool(n)` : 创建一个线程池，它可安排在给定延迟后运 行命令或者定期地执行

---