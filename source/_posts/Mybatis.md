---
title: Mybatis
date: 2020-05-06 10:30:25
tags: Mybatis
categories:
- Java Frame
---

## pom.xml

```
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis</artifactId>
   <version>3.5.2</version>
</dependency>
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
   <version>5.1.47</version>
</dependency>
```

## mybatisUtil

```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {

   private static SqlSessionFactory sqlSessionFactory;

   static {
       try {
           String resource = "mybatis-config.xml";
           InputStream inputStream = Resources.getResourceAsStream(resource);
           sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
      } catch (IOException e) {
           e.printStackTrace();
      }
  }

   //获取SqlSession连接
   public static SqlSession getSession(){
       return sqlSessionFactory.openSession();
  }

}
```

## mybatis-config.xml

```xml
-properties（属性）
    --property
-settings（全局配置参数）
    --setting
-typeAliases（类型别名）
    --typeAliase
    --package
-typeHandlers（类型处理器）
-objectFactory（对象工厂）
-plugins（插件）
-environments（环境集合属性对象）
    --environment（环境子属性对象）
        ---transactionManager（事务管理）
        ---dataSource（数据源）
-mappers（映射器）
    --mapper
    --package

<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration
                PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="db.properties"/>
    <!-- 延迟加载-->
    <settings>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>

    <typeAliases>
        <package name="com.kang.pojo"/>
    </typeAliases>
    
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <!--<mapper resource=" "/> 使用相对于类路径的资源-->

        //此种方法要求 mapper 接口名称和 mapper 映射文件名称相同，且放在同一个目录中
        <mapper class="com.kang.dao.IAccountDao"/>
        
        <!--<package name=""/>
        此种方法要求 mapper 接口名称和 mapper 映射文件名称相同，且放在同一个目录中-->
    </mappers>
</configuration>
```
---

## mybatis-mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace 对应Dao接口位置-->
<mapper namespace="com.kang.dao.IUserDao">
    <!-- id对应Dao接口方法名 resultype返回值类型 parameterType参数类型-->
    <select id="findById" resultType="com.kang.pojo.User" parameterType="int">
        select * from t_user where id=#{id}
    </select>

    <insert id="saveUser" parameterType="com.kang.pojo.User">
        insert into t_user(id,username,birthday,sex,address)
        values(#{id},#{username},#{birthday},#{sex},#{address})
    </insert>
</mapper>
```

## mybatis-log4j

```
# 主要配置
log4j.rootLogger=DEBUG,Console
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.Console.layout=org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n

## log4j.rootLogger ##
# 作用：控制日志输出的级别、输出的位置。
# 参数：级别、输出的位置
# 级别： DEBUG < INFO < WARN < ERROR (除了这些，还有其他级别)
# 级别作用：以选DEBUG级别为例，大于DEBUG级别的信息（INFO/WARN/ERROR）也会输出
# 选用DEBUG级别的原因：是跟Mybatis的源码有关（你会发现最低的级别是DEBUG）。

## log4j.appender.Console ##
# 作用：真正控制，日志输出到什么地方，取决于参数用了什么类
# log4j.appender.名称  名称可以自定义，而 log4j.rootLogger中的位置就要写这个名称

## log4j.appender.Console.layout ##
# 作用：以什么布局方式输出日志，这里是自定义的布局

## log4j.appender.Console.layout.ConversionPattern ##
# 作用：就是你自定义的布局
# %d 是产生日志的时间。 %t 是产生这个日志所处的线程的名称。
# %p 是输出日志的级别，%-5p 5,就是输出5位字符,不足补空格 -,补的空格在右边
# %c 是输出的这个日志所在的类的全名。  %m 是你附加的信息。  %n 是换行

## log4j.logger.org.apache 对 org.apache ##
# 作用：的输出级别进行设置（也可以换成其他包）
```
---

## 缓存

>mybatis 一级缓存
- 一级缓存是 SqlSession 级别的缓存，只要 SqlSession 没有 flush 或 close，它就存在。
>mybatis 二级缓存
- 二级缓存是 mapper 映射级别的缓存，多个 SqlSession 去操作同一个 Mapper 映射的 sql 语句，多个
SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的
>开启二级缓存
- 1.配置mybatis-config.xml

```
<settings>
<!-- 开启二级缓存的支持 -->
<setting name="cacheEnabled" value="true"/>
</settings>
<!--因为 cacheEnabled 的取值默认就为 true，所以这一步可以省略不配置。为 true 代表开启二级缓存；为-->
<!--false 代表不开启二级缓存-->
```

- 2.配置相关的 Mapper 映射文件

```
<!--<cache>标签表示当前这个 mapper 映射将使用二级缓存，区分的标准就看 mapper 的 namespace 值-->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test.dao.IUserDao">
    <!-- 开启二级缓存的支持 -->
    <cache></cache>
</mapper>
```

- 3.配置 statement 上面的 useCache 属性

```
<!-- 根据 id 查询 -->
<select id="findById" resultType="user" parameterType="int" useCache="true">
    select * from user where id = #{uid}
</select>
<!--将 UserDao.xml 映射文件中的<select>标签中设置 useCache=”true”代表当前这个 statement 要使用二级缓存，如果不使用二级缓存可以设置为 false-->
```

>mybatis注解开发
- 常用注解
```
@Insert:实现新增
@Update:实现更新
@Delete:实现删除
@Select:实现查询
@Result:实现结果集封装
@Results:可以与@Result 一起使用，封装多个结果集
@ResultMap:实现引用@Results 定义的封装
@One:实现一对一结果集封装
@Many:实现一对多结果集封装
@SelectProvider: 实现动态 SQL 映射
@CacheNamespace:实现注解二级缓存的使用
```

## 静态资源过滤

```xml
<resources>
   <resource>
       <directory>src/main/java</directory>
       <includes>
           <include>**/*.properties</include>
           <include>**/*.xml</include>
       </includes>
       <filtering>false</filtering>
   </resource>
   <resource>
       <directory>src/main/resources</directory>
       <includes>
           <include>**/*.properties</include>
           <include>**/*.xml</include>
       </includes>
       <filtering>false</filtering>
   </resource>
</resources>
```