# Mybatis

Mybatis是持久层框架, 支持自定义sql 存储过程 高级映射

> 使用Mybatis框架可以将生成jdbc代码,设置Statement参数, 获取ResultSet的工作都省掉, 程序员只需要专注于编写sql语句, 大大提高了编程效率

为什么要用mybatis代替jdbc:

1. 使用连接池技术, 自动管理数据库连接,提高编程效率
2. sql语句不再放入java代码中, 不需要重复编译

mybatis底层: 使用简单的xml或注解来进行映射, 将接口和POJOs(普通java对象)映射为数据库中的记录

> pojos等同于entity或者bean

mybatis架构图

![image-20220601153901432](Mybatis.assets/image-20220601153901432.png

![image-20220601103119840](Mybatis.assets/image-20220601103119840.png)

Mybatis项目基本结构图

![image-20220601155416148](Mybatis.assets/image-20220601155416148.png)

> mybatis主配置: mybatis-config.xml(不需要记忆)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <typeAliases>
        <!--取别名-->
        <typeAlias alias="ClassBean" type="com.lee.szdx03.bean.ClassBean"></typeAlias>
    </typeAliases>
    <environments default="dev">
        <!--可以包含多个environment标签, default指定使用的数据库环境-->
        <!--不能同时使用多个数据库, 只能用一个-->
        <environment id="dev">
        <!--配置单个数据库环境-->
            <!--使用jdbc类型的事务处理方式-->
            <transactionManager type="JDBC"/>
            <!--dataSource ：指定获取连接的方式
       		type ：
            POOLED：采用传统的javax.sql.DataSource规范中的连接池，mybatis中有针对规范的实现 ​
            UNPOOLED：采用传统的获取连接的方式，虽然也实现Javax.sql.DataSource接口，但是并没有使用池的思想。 ​
            JNDI：采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到DataSource是不一样
            注意：如果不是web或者maven的war工程，JNDI是不能使用的-->
            <dataSource type="POOLED">       
                <!-- 数据库连接的属性信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver"></property>
                <property name="url" value="jdbc:mysql://localhost:3306/woniu"></property>
                <property name="username" value="root"></property>
                <property name="password" value="123456"></property>
            </dataSource>
        </environment>
       
    </environments>
</configuration>
```

> mybatis创建session

```java
class Test{
    void test(){
        try{
             //-- 1.读取mybatis的核心配置文件 里面有连接池和Mapper的配置信息
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            //-- 2.创建SqlSessionFactory的构建者对象.带有Builder的基本是构建者模式
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            //-- 3.利用构建者构建工厂对象
            SqlSessionFactory factory = builder.build(is);
            //-- 4.利用SqlSessionFactory工厂 构建SqlSession
            SqlSession session = factory.openSession();
            System.out.println(session);
            session.close();        
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

配置好mybatis-config.xml后, 使用配置来创建SqlSession对象, 则代表项目已经成功与数据库建立了连接

Mybatis对数据库进行查询操作前需要准备的工作

1. 编写数据库表对应的实体类, 创建实体类对象来存储读取的表信息
2. 编写映射接口 ClassMapper, 该接口用于定义SQL命令对应的抽象方法(需要提供方法名, 返回类型(实体类), SQL命令的传入参数)
3. 编写SQL命令映射文件ClassMapper.xml, 提供SQL命令和对应的抽象方法
4. 在mybatis-config.xml中注册ClassMapper.xml

映射接口ClassMapper

```java
public interface ClassMapper {
    /**
     * 根据ID查询并返回班级对象
     */
    ClassBean getById(long id);
    ClassBean getByName(String classNames);
}
```

**映射文件ClassMapper.xml**

>需要理解和记忆

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace(命名空间)用于绑定接口, 并确保方法的唯一性-->
<mapper namespace="com.lee.szdx03.mapper.ClassMapper">
    <!--resultType: 声明返回值类型,应该填写bean类的全路径-->
    <!--like语句后需要用${}作为字符串中的占位符-->
    <!--用#{}作为占位符, 相当于jdbc的?-->
    <!--parameterType可以设定传入sql语句的参数的类型, 不设置的话会使用绑定的方法的参数列表-->
    <select id="getById" resultType="com.lee.szdx03.bean.ClassBean">
        select id,class_name as className,class_leader as classLeader from class_info where id=#{id}
    </select>
    <!--{arg0}和{arg1}都是属于匿名参数, 匿名参数必须严格按sql语句的顺序进行传参, 匿名参数名不代表参数顺序-->
    <select id="getByIdAndName" resultType="com.lee.szdx03.bean.ClassBean">
        select id,class_name as className,class_leader as classLeader from class_info where id=#{arg0} and class_name=#{arg1}
    </select>
</mapper>
```

将Mapper.xml注册到mybatis-config.xml中

```xml
<mappers>
    <!--注册单个Mapper-->
	<mapper resource="mapper/ClassMapper.xml"></mapper>
    <!--将整个包下面的Mapper.xml都注册-->
    <package name="com.lee.szdx03.mapper"/>
</mapper>
```

Mybatis多参数传给SQL语句的方式有四种:

1. 用封装好的对象传递参数
2. Map集合传参
3. 匿名参数(按顺序传参)
4. @Param注解

1 2 4都要重点理解

> 使用@Param()注解

```java
public Student getByIdAndName(@Param("id")long id,@Param("name")String name);
```

```xml
<select id="getByIdAndName" resultType="com.lee.szdx03.bean.StudentBean">
select id,stu_name as stuName,stu_no as stuNo from t_stu where id=#{id} and stu_name=#{name}
</select>
```

使用Map集合传参时 mybatis会使用Map中元素对应的key作为参数名

使用对象传参时, 使用对象中的属性名来作为参数名

> 注意, 当resultType为Bean类时, mybatis自动将返回结果的字段映射为Bean类中的属性, 将单行的记录值赋给Bean对象的属性

> 底层实现上会先将字段映射为Map中的key值再映射到类的属性名

## 核心api

1. SqlSession
2. SqlSessionFactory
3. SqlSessionFactoryBuilder

SqlSession是一个数据库连接, 以事务为单位执行sql语句, 提供了commit rollback方法来提交或回滚事务

> SqlSession不会提供连接池, 需要手动结束连接

SqlSessionFactory提供了数据库连接池, 负责创建SqlSession对象并管理创建好的对象, SqlSessionFactory的生命周期应该等同于整个MyBatis项目的生命周期, 并且SqlSessionFactory对象应该在整个程序内共享使用(类成员)

SqlSessionFactoryBuilder用于创建SqlSessionFactory对象, 应该在方法作用域中使用(方法成员)

SqlSessionFactory调用openSession方法时可以传入boolean值, 代表是否开启自动提交

# Log4J

Log4J是一种java日志框架

> java中常见的日志框架还有: Log4J Log4J2 Logback

> 记录的日志可以用异常定位, 数据分析, 调试程序
>
> 一般程序开发时会将日志是直接打印到控制台方便开发调试, 程序上线后必须将日志保存到文件或数据库中

Log4J记录的日志通常分为4种等级:

1. DEBUG 调试日志, 开发时用到
2. INFO 正常的输出日志
3. WARN 警告, 说明程序可能有异常
4. ERROR 错误, 说明程序已经出现了异常

> 重要程度(优先级)从低到高

**Log4J可以和Mybatis配合使用**

> mybatis-config.xml

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

> log4j.properties

```java
#全局日志
log4j.rootLogger=DEBUG, stdout
# MyBatis 日志配置
log4j.logger.org.mybatis.example.BlogMapper=TRACE
# 控制台输出
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

## 增删改

Mybatis默认需要手动提交事务, 因此执行sql增删改操作后需要调用commit方法才能将修改提交到数据库中

> insert语句在Mybatis的默认返回类型为int, 代表操作的表受影响的记录行数