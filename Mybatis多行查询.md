# Mybatis多行查询

**注意, 默认的Mapper函数都会调用SqlSession.selectOne()方法, 该方法只能返回单行结果, 当返回结果大于1行时会报抛出运行时错误TooManyResultsException**

**多行返回结果需要设置resultMa以及将函数的返回类型设置List<Bean类>**

> 返回结果为多行时需要用List(列表)进行接收, 每个元素是一个Bean对象

mybatis多行查询示例

> Mapper.java

```java
List<StudentBean> selectAllStu();
```

> Mapper.xml

**需要注意的地方**

| 标签      | 属性          | 作用                                |
| --------- | ------------- | ----------------------------------- |
| resultMap | autoMapping   | 默认true                            |
|           | type          | 绑定Bean类型                        |
|           | id            | 设置当前resultMap的id               |
| select    | id            | 设置id, 绑定对应的方法名            |
|           | resultMap     | 将返回多个结果映射为对应的resultMap |
|           | parameterType | 传入参数类型                        |
|           | resultType    | 返回结果类型                        |

```xml
<mapper>
    <!--resultMap标签有autoMapping属性, 默认是true-->
    <!--autoMapping属性默认会忽略大小写区别-->
    <resultMap type="com.lee.szdx03.bean.StudentBean" id="stuMap">
            <id column="id" property="id"></id>
            <result column="stu_name" property="stuName"></result>
    </resultMap>
    <select id="selectAllStu"  resultMap="stuMap">
        select * from student_info
    </select>
</mapper>
```

Mapper.xml中有4种statement语句: create update select delete

statemtn语句的id都应该绑定Mapper接口中对应的方法

> statement语句的parameterType和resultType显示指定时必须与绑定的接口方法的形参或返回值类型相同, 不显示指定时会自动绑定

**mybatis中ORM的体现**

1. 一行记录对应一个Bean对象
2. 一个字段对应Bean的一种属性
3. 一张表对应一个Bean类

> 所有持久层框架都遵循ORM设计

**注意Mybatis进行增删改操作后要调用SqlSession的commit方法才能将修改提交到数据库中**

