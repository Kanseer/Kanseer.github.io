---
title: JavaClass
date: 2020-04-24 20:05:26
tags: Class
categories:
- Java
---
# Java常用类

## String

- String是一个`final类`，代表不可变的字符序列
- 字符串是常量，用双引号引起来表示.它们的值在创建之后不能更改
- String对象的字符内容是存储在一个字符数组`value[]`中的

![](/img/javaclass/1.png)

- 字符串常量存储在 字符串常量池，目的是共享
- 字符串非常量对象 存储在堆中
- 常量与常量的拼接结果在常量池。且常量池中不会存在相同内容的常量
- 只要其中有一个是变量，结果就在堆中
- 如果拼接的结果调用`intern()方法`，返回值就在常量池中

![](/img/javaclass/2.png)



## String常用方法

- `int length()`：返回字符串的长度
- `char charAt(int index)`：返回某索引处的字符
- `boolean isEmpty()`：判断是否是空字符串
- `String toLowerCase()`：使用默认语言环境，将 String 中的所有字符转换为小写
- `String toUpperCase()`：使用默认语言环境，将 String 中的所有字符转换为大写
- `String trim()`：返回字符串的副本，忽略前导空白和尾部空白
- `boolean equals(Object obj)`：比较字符串的内容是否相同
- `boolean equalsIgnoreCase(String anotherString)`：与equals方法类似，忽略大 小写
- `String concat(String str)`：将指定字符串连接到此字符串的结尾。 等价于用“+”
- `int compareTo(String anotherString)`：比较两个字符串的大小
- `String substring(int beginIndex)`：返回一个新的字符串，它是此字符串的从 beginIndex开始截取到最后的一个子字符串
- `String substring(int beginIndex, int endIndex)` ：返回一个新字符串，它是此字 符串从beginIndex开始截取到endIndex(不包含)的一个子字符串
- `boolean endsWith(String suffix)`：测试此字符串是否以指定的后缀结束
- `boolean startsWith(String prefix)`：测试此字符串是否以指定的前缀开始
- `boolean startsWith(String prefix, int toffset)`：测试此字符串从指定索引开始的 子字符串是否以指定前缀开始
- `boolean contains(CharSequence s)`：`charSequence是接口`，表示char值的一个可读序列
- `int indexOf(String str)`：返回指定子字符串在此字符串中第一次出现处的索引
- `int indexOf(String str, int fromIndex)`：返回指定子字符串在此字符串中第一次出 现处的索引，从指定的索引开始
- `int lastIndexOf(String str)`：返回指定子字符串在此字符串中最右边出现处的索引
- `int lastIndexOf(String str, int fromIndex)`：返回指定子字符串在此字符串中最后 一次出现处的索引，从指定的索引开始反向搜索
- `String replace(char oldChar, char newChar)` : 在字符串中用newChar字符替代oldChar字符，返回一个新的字符串
- `String replaceAll(String regex,String replacement)` : 使用给定的 replacement 字符串替换此字符串匹配给定的正则表达式的每个子字符串
- `boolean matches(String regex)` : 方法用于检测字符串是否匹配给定的正则表达式
- `String[] split(String regex, int limit)` : 根据匹配给定的正则表达式来拆分字符串
- **字符串 -> 基本数据类型、包装类**
  - Integer包装类的`public static int parseInt(String s)`：可以将由“数字”字 符组成的字符串转换为整型
  - 使用java.lang包中的Byte、Short、Long、Float、Double类调相应 的类方法可以将由“数字”字符组成的字符串，转化为相应的基本数据类型
- **基本数据类型、包装类 -> 字符串**
  - 调用String类的`public String valueOf(int n)`可将int型转换为字符串
  - 相应的`valueOf(byte b)、valueOf(long l)、valueOf(float f)、valueOf(double d)、valueOf(boolean b)`可由参数的相应类型到字符串的转换
- **字符数组 -> 字符串**
  - String 类的构造器：`String(char[])` 和 `String(char[]，int offset，int length)` 分别用字符数组中的全部字符和部分字符创建字符串对象
- **字符串 -> 字符数组**
  - `public char[] toCharArray()`：将字符串中的全部字符存放在一个字符数组 中的方法
  - `public void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)`：提供了将指定索引范围内的字符串存放到数组中的方法
- **字节数组 -> 字符串**
  - `String(byte[])`：通过使用平台的默认字符集解码指定的 byte 数组，构 造一个新的 String
  - `String(byte[]，int offset，int length)` ：用指定的字节数组的一部分， 即从数组起始位置offset开始取length个字节构造一个字符串对象
- **字符串 -> 字节数组**
  - `public byte[] getBytes()` ：使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中
  - `public byte[] getBytes(String charsetName)` ：使用指定的字符集将 此 String 编码到 byte 序列，并将结果存储到新的 byte 数组



## StringBuffer

-  `java.lang.StringBuffer`代表可变的字符序列，`JDK1.0`中声明，可以对字符 串内容进行增删，此时不会产生新的对象
- 作为参数传递时，方法内部可以改变值

![](/img/javaclass/3.png)

- `StringBuffer`类不同于String，其对象必须使用构造器生成
  - `StringBuffer()`：初始容量为16的字符串缓冲区
  - `StringBuffer(int size)`：构造指定容量的字符串缓冲区
  - `StringBuffer(String str)`：将内容初始化为指定字符串内容
- `StringBuffer`是线程安全的,所有方法都有`synchronized`

## StringBuffer常用方法

- `StringBuffer append(xxx)`：提供了很多的append()方法，用于进行字符串拼接
- `StringBuffer delete(int start,int end)`：删除指定位置的内容
- `StringBuffer replace(int start, int end, String str)`：把[start,end)位置替换为str
- `StringBuffer insert(int offset, xxx)`：在指定位置插入xxx , 长度不够,可扩容



## StringBuilder

- `StringBuilder` 和 `StringBuffer` 非常类似，均代表可变的字符序列，而且 提供相关功能的方法也一样
- `StringBuilder(JDK 5.0)`：可变字符序列、`效率高、线程不安全`
- `StringBuffer(JDK1.0)`：可变字符序列、`效率低、线程安全`
- `String(JDK1.0)`：不可变字符序列



## java.lang.System

- 系统的很多属性和控制方法都放置在该类的内部
- System类内部包含`in`、`out`和`err`三个成员变量，分别代表标准输入流 (键盘输入)，标准输出流(显示器)和标准错误输出流(显示器)
- `public static long currentTimeMillis()` :  用来返回当前时 间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差
- `void exit(int status)`：该方法的作用是退出程序。其中status的值为0代表正常退出，非零代表 异常退出
- `void gc()`：该方法的作用是请求系统进行垃圾回收。至于系统是否立刻回收，则 取决于系统中垃圾回收算法的实现以及系统执行时的情况
- `String getProperty(String key)`：该方法的作用是获得系统中属性名为key的属性对应的值。系统中常见 的属性名以及属性的作用如下表所示

![](/img/javaclass/4.png)



## java.util.Date

- 表示特定的瞬间，精确到毫秒
- `Date()`：使用无参构造器创建的对象可以获取本地当前时间
- `Date(long date)`
- `getTime(`):返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象 表示的毫秒数
- `toString()`:把此 Date 对象转换为以下形式的 String： dow mon dd hh:mm:ss zzz yyyy 其中： dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)，zzz是时间标准



## java.text.SimpleDateFormat

- **格式化**
  - `SimpleDateFormat()` ：默认的模式和语言环境创建对象
  - `public SimpleDateFormat(String pattern)`：该构造方法可以用参数pattern 指定的格式创建一个对象，该对象调用：
  - `public String format(Date date)`：方法格式化时间对象date
- **解析**
  - `public Date parse(String source)`：从给定字符串的开始解析文本，以生成 一个日期

![](/img/javaclass/5.png)



## java.util.Calender

- `Calendar是一个抽象基类`，主用用于完成日期字段之间相互操作的功能
- 获取Calendar实例的方法
  - 使用`Calendar.getInstance()`方法
  - 调用它的`子类GregorianCalendar`的构造器
- 一个Calendar的实例是系统时间的抽象表示，通过`get(int field)`方法来取得想 要的时间信息。比如YEAR、MONTH、DAY_OF_WEEK、HOUR_OF_DAY 、 MINUTE、SECOND
  - `public void set(int field,int value)`
  - `public void add(int field,int amount)`
  -  `public final Date getTime()`
  - `public final void setTime(Date date)`



## java.time(Java 8)

- 中包含了所有关于本地日期（`LocalDate`）、本地时间 （`LocalTime`）、本地日期时间（`LocalDateTime`）、时区（`ZonedDateTime`） 和持续时间（`Duration`）的类

![](/img/javaclass/6.png)

- `LocalDate`、`LocalTime`、`LocalDateTime` 类是其中较重要的几个类，它们的实例是`不可变的对象`，分别表示使用 ISO-8601日历系统的日期、时间、日期和时间
  - `LocalDate`代表IOS格式（yyyy-MM-dd）的日期,可以存储 生日、纪念日等日期
  - `LocalTime`表示一个时间，而不是日期
  - `LocalDateTime`是用来表示日期和时间的，这是一个最常用的类之一

![](/img/javaclass/7.png)

- **瞬时(Instant)**
  - `Instant`：时间线上的一个瞬时点. 这可能被用来记录应用程序中的事件时间戳
  - `java.time`包通过值类型Instant提供机器视图，不提供处理人类意义上的时间单位

![](/img/javaclass/8.png)

- **格式化与解析**
  - `java.time.format.DateTimeFormatter 类`：该类提供了三种格式化方法
  - `ISO_LOCAL_DATE_TIME`;`ISO_LOCAL_DATE`;`ISO_LOCAL_TIME`
  - `ofLocalizedDateTime(FormatStyle.LONG)` : 本地化相关的格式
  - `ofPattern(“yyyy-MM-dd hh:mm:ss”)` : 自定义的格式

![](/img/javaclass/9.png)

- **其他API**
  - `ZoneId`：该类中包含了所有的时区信息，一个时区的ID，如 Europe/Paris
  - `ZonedDateTime`：一个在ISO-8601日历系统时区的日期时间，如 2007-12- 03T10:15:30+01:00 Europe/Paris
  - `Clock`：使用时区提供对当前即时、日期和时间的访问的时钟
  - `Duration` : 用于计算两个“时间”间隔
  - `Period` : 用于计算两个“日期”间隔
  - `TemporalAdjuster` : 时间校正器。有时我们可能需要获取例如：将日期调整 到“下一个工作日”等操作
  - `TemporalAdjusters` : 该类通过静态方法 `(firstDayOfXxx()/lastDayOfXxx()/nextXxx())`提供了大量的常用 TemporalAdjuster 的实现

![](/img/javaclass/10.png)



## java.lang.Comparable

- `Comparable接口`强行对实现它的每个类的对象进行整体排序.这种排序被称 为类的自然排序
- 实现 Comparable 的类必须实现 `compareTo(Object obj)` 方法，两个对象即 通过 compareTo(Object obj) 方法的返回值来比较大小
- 实现Comparable接口的对象列表（和数组）可以通过 `Collections.sort` 或 `Arrays.sort`进行自动排序.实现此接口的对象可以用作有序映射中的键或有 序集合中的元素，无需指定比较器
- 对于类 C 的每一个 e1 和 e2 来说，当且仅当 `e1.compareTo(e2) == 0` 与 `e1.equals(e2)` 具有相同的 boolean 值时，类 C 的自然排序才叫做与 equals 一致。建议（虽然不是必需的）最好使自然排序与 equals 一致
- `Comparable` 的典型实现：(默认都是从小到大排列的)
  - `String`：按照字符串中字符的Unicode值进行比较
  - `Character`：按照字符的Unicode值来进行比较
  - 数值类型对应的包装类以及`BigInteger`、`BigDecimal`：按照它们对应的数值 大小进行比较
  - `Boolean`：true 对应的包装类实例大于 false 对应的包装类实例
  - `Date`、`Time`等：后面的日期时间比前面的日期时间大



## java.util.Comparator

- 当元素的类型没有实现`java.lang.Comparable接口`而又不方便修改代码， 或者实现了java.lang.Comparable接口的排序规则不适合当前的操作，那 么可以考虑使用 `Comparator 的对象`来排序，强行对多个对象进行整体排 序的比较
- 重写`compare(Object o1,Object o2)方法`，比较o1和o2的大小：如果方法返 回正整数，则表示o1大于o2；如果返回0，表示相等；返回负整数，表示 o1小于o2
- 可以将 Comparator 传递给 sort 方法（如 `Collections.sort` 或 `Arrays.sort`）， 从而允许在排序顺序上实现精确控制
- 还可以使用 Comparator 来控制某些数据结构（如有序 set或有序映射）的 顺序，或者为那些没有自然顺序的对象 collection 提供排序



## Math

- `java.lang.Math`提供了一系列静态方法用于科学计算。其方法的参数和返回 值类型一般为double型

![](/img/javaclass/11.png)



## BigInteger

- `Integer`类作为int的包装类，能存储的最大整型值为`2^31 - 1`，Long类也是有限的， 最大为`2^63 - 1`
- `java.math`包的`BigInteger`可以表示不可变的任意精度的整数。BigInteger 提供 所有 Java 的基本整数操作符的对应物，并提供 java.lang.Math 的所有相关方法。 另外，BigInteger 还提供以下运算：模算术、GCD 计算、质数测试、素数生成、 位操作以及一些其他操作
- `BigInteger(String val)`：根据字符串构建BigInteger对象

![](/img/javaclass/12.png)



## BigDecimal

- 一般的Float类和Double类可以用来做科学计算或工程计算，但在商业计算中， 要求数字精度比较高，故用到`java.math.BigDecimal`类
- `BigDecimal`类支持不可变的、任意精度的有符号十进制定点数
- 构造器
  - `public BigDecimal(double val)`
  - `public BigDecimal(String val)`
- 常用方法
  - `public BigDecimal add(BigDecimal augend)`
  - `public BigDecimal subtract(BigDecimal subtrahend)`
  - `public BigDecimal multiply(BigDecimal multiplicand)`
  - `public BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)`

