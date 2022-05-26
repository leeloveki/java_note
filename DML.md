# 数据库对象

| 关键字     | 名称     | 描述                                                 |
| ---------- | -------- | ---------------------------------------------------- |
| table      | 表       | 存储数据的逻辑单元                                   |
|            | 数据字典 | 系统表, 存储数据库的信息                             |
| constraint | 约束     | 进行数据校验, 确保符合限制                           |
| view       | 视图     | 表中数据的逻辑展示, 本身不存储数据                   |
| index      | 索引     | 提高数据库的查询性能                                 |
| function   | 函数     | 对数据库中的数据进行计算, 具有一个返回值             |
| procedure  | 存储过程 | 进行完整的业务处理, 没有返回值, 但是可以有传出参数   |
| tigger     | 触发器   | 相当于事件监听器, 在数据库发生事件时触发执行逻辑处理 |

> 数据的列被称为字段, 行称为记录

数据库对象都可以通过create语句+关键字来创建

drop语句和alter语句也是可以操作多种数据库对象

# 约束

mysql支持4种约束

| 约束名      | 作用 |
| ----------- | ---- |
| not null    | 非空 |
| unique      | 唯一 |
| primary key | 主键 |
| foreign key | 外键 |

约束也分为单列约束和多列约束

# DML

DML:  数据库操作语言(Data Manipulation language)

> 用于对表进行增删改 CUD create update delete 

> DQL用于查数据

> 插入数据

```sql
#往test2中插入两行数据
insert into test2(id,age)values(1,2),(2,3);
#省略式写法插入数据
#不推荐使用
insert into test2 values(1,2,"小白"),(2,3,"小白2");
```

> 修改数据

```sql
#将id=3的那行数据修改
update test2 set age=9,name="小黑" where id=3;
```

> 删除数据

```sql
#将id=4那行数据删除
delete from test2 where id=4;
#将id为1或2的数据都删除
delete from test2 where id=2 or id=1;
#将id为1或2的数据都删除
delete from test2 where id in (1,2);
#将id不等于3的行都删除
delete from test2 where id!=3;
#将id不等于3的行都删除
delete from test2 where not id=3;
#将test2表的所有数据删除
delete from test2;
#将整个表重置为初始化状态
truncate table test2;
#从test3中导出数据并去重后创建新表
create table test4 as (select distinct * from test3);
```

**delete from是逐行将整个表的数据删除, 有日志可以恢复**

**truncate table是直接将整个表重置, 速度最快, 但是操作不可恢复**

注意where语句后面是接boolean表达式, 可以使用逻辑判断符

> 注意*只能用于select语句后面匹配任意列
>
> %只能用于where like后面用于匹配单元格

=和:=

> 在SQL中单等号用于比较是否相等
>
> :=冒号等号用于赋值运算

查询语句中可以嵌套子查询语句

```sql
#嵌套子查询语句
select * from (select * from test4)t where t.age=1;
```

查询语句的结果是一个集合, 可以进行数学的并(union)运算

```sql
select ... from ... minus select ... from ...;
```

> mysql不支持对select的结果集进行交(intersect)  差(minus) 运算

并运算需要满足:

1. 两个结果集的列数量相等
2. 两个结果集的数据类型必须一一相等

**限制查询**

限制查询也被称为分页查询, 在mysql中通过limit关键字实现

```sql
#从查询结果的0号位(第一条数据)开始返回3条数据
select * from test4 where id%2=0 order by age limit 0,3;
```

上述SQL语句底层执行顺序和原理

**总体上可以视为从左向右执行**

> 注意select筛选字段语句是紧跟在where之后执行即可

1.读取整个表 2.where语句 3.select语句(筛选段) 4. order语句 5.limit语句

1. 读取test4表, 完整复制到内存中
2. 执行where语句, 在上一步的内存表中筛选数据到新的内存表
3. 执行selet语句, 从上一步的表中筛选对应的列到新的内存表
4. 执行order语句, 对上一步的表进行排序, 排序好后存到新的内存表
5. 执行limit语句, 对上一步的表进行分页后存到新的表打印给用户

where语句可以用的运算符

| 运算符              | 功能                                         |
| ------------------- | -------------------------------------------- |
| <> <= >= = !=       | 关系运算符                                   |
| + - * %             | 算数运算符                                   |
| and                 | 并且, 需要两个条件都满足才执行               |
| or                  | 或者, 只需要满足一个条件才执行               |
| not                 | 取否(等同于java中的叹号)                     |
| between ... and ... | 小于等于                                     |
| in (,)              | 在括号内的范围进行匹配                       |
| like '%'            | 等同于通配符* 匹配多个任意字符               |
| like '_'            | 等同于通配符. 匹配单个任意字符               |
| is null             | 匹配空值                                     |
| is not null         | 匹配非空值                                   |
| limit  a,b          | 从查询结果的a号位(第一条数据)开始返回b行数据 |

# DQL

DQL: 数据库查询语言(Data Query Language)

> DQL语句都是select ... from语句

公司里80%的业务都是使用select语句查数据

**注意 select语句可以用算数运算符**

**where语句可以用算数运算符和逻辑运算符还有and or关键字**

**where语句不能作为单独的语句, 必须作为条件语句与修改语句或者查找语句搭配使用**

```sql
#将表中的所有数据打印出来
select * from student_info;
#将名字为小白的行的id加上1, stu_name打印
select id+1,stu_name from student_info where stu_name='小白';
#按照age升序(从小到大)
select * from test4 order by age;
#按照age升序(从小到大)
select * from test4 order by age asc;
#按照age降序(从大到小)
select * from test4 order by age desc;
#复合排序, 按照age降序, age相同时按照id降序
select * from test4 order by age desc,id desc;
```

## SQL语句

| 语句                                  | 结果对象                                       |
| ------------------------------------- | ---------------------------------------------- |
| 创建语句                              |                                                |
| create database ...                   | database                                       |
| create table ...                      | table                                          |
| 修改表(对列进行操作)语句              |                                                |
| alter ... add ...                     | column                                         |
| alter ... add constraint ...          | 约束                                           |
| 修改行操作语句                        |                                                |
| update ... set ...                    | all row                                        |
| update ... set ... where ...          | row                                            |
| 插入语句                              |                                                |
| insert into ... values ...            | row                                            |
| 删除语句                              |                                                |
| delete from ...                       | all row                                        |
| delete from ... where ...             | row                                            |
| 查找语句                              |                                                |
| select ... from ...                   | 打印                                           |
| select ... from ... where ...         |                                                |
| select ... from ... order by ...      | 指定一列作为(默认)升序排列                     |
| select ... from ... order by ... asc  | 指定一列作为(默认)升序排列                     |
| select ... from ... order by ... desc | 指定一列作为(默认)降序排列                     |
| select distinct ... from ...          | 查找结果去重复行后再打印(不会修改数据库源数据) |
| 匹配语句(不能单独使用)                |                                                |
| where                                 | 筛选条件                                       |
| where ... like ...(通配符%_)          | 筛选条件                                       |
| 分组语句(不能单独使用)                |                                                |
| group by ...                          |                                                |
| group by ... having ...               |                                                |

**sql中的字符串值必须用''单引号括起来**

通配符%_既可以单独使用也可以在字符串中搭配使用

注意sql语句中如果一次用到多个值, 一般使用逗号,作为分割

mysql中还可以使用反斜线作为转义字符,可以使通配符作为普通字符进行匹配

```sql
#匹配name等于_\%小白的行
select * from test where name like '\_\\\%小白'
```

# mysql函数

> mysql中的数据类型分为数值类型 字符类型 日期类型
>
> 所以mysql中提供了对应的转换函数来完成类型转换

SQL中的函数分为单行函数和多行函数(聚合函数)

> 分别处理单行数据和多行数据

mysql中的函数按照功能又能分为5种:

转换函数	位函数	流程控制函数	加密解密函数	 信息函数

# 多行函数(聚合函数)

| 聚合函数 | 作用           | 操作对象   |
| -------- | -------------- | ---------- |
| count()  | 统计多少条数据 | 单列或多列 |
| sum()    | 求和           | 单列       |
| avg()    | 求平均值       | 单列       |
| max()    | 求最大值       | 单列       |
| min()    | 求最小值       | 单列       |

**聚合函数只能统计单列数据, 不能统计多列数据, 所以不能使用*通配符**

> 除了count()函数可以用

**聚合函数不会统计列中为null数据**

> SQL中一列数据被称为一个字段

可以使用ifull(列,默认值)函数给列中的null重新赋值

```sql
select avg(ifnull(age,0)) as 平均年龄
```

> 多行函数测试

```sql
#统计test4中总共有多少条数据, 注意limit语句是最后执行不会影响到count()函数
select COUNT(*) as total from test4 limit 0,3;
#统计年龄大于20岁的人数
select count(*) as total_20_age from stu_info where age>=20;
#统计表中有多少行数据
select count(*) from test;
#统计age字段有多少行非null数据
select count(age) from test;
```

> SQL不允许有整行都是null的数据

# 分组查询

**注意group by和having语句都必须与聚合函数搭配使用**

分组查询的语句为group by

```sql
#按照性别分组再统计每个性别的人数
select gender,count(*) from student_info group by gender;
```

mysql5.7前, select只能跟字段或聚合函数

mysql5.7后, select还可以跟group_concat()函数

> group_concat()函数的对象是字段

```sql

```

having语句用于对聚合函数的结果进行筛选

```sql
#统计各班级的人数, 并筛选出人数大于3的班级
select class_name,count(*) from test group by class_name having count(*)>3;
```

# SQL总结

最简SQL语句:

```sql
select 字段或(函数加字段) from 表名; 
```

完整SQL语句

```sql
select 字段或(函数加字段) from 表名 where 查询条件 group by 字段 having 过滤条件 order by 字段 limit (a,b)
```

**执行顺序从左向右**

> 除了select字段语句是在where后执行, 但是实际上不影响最终结果







> mysql5.7以上同时使用order by和limit语句时, 如果order by的列存在重复值, 会造成多页面数据重复

```sql
create table student_info(
id int,
stu_no varchar(20),
stu_name varchar(20),
age int,
birthday date,
telphone int,
deucation_level varchar(20),
fk_class_id int
);

create table class_info(
id int,
class_name varchar(20),
class_leader varchar(20)
);

create database szdx character set 'utf8mb4'

insert into class_info(id,class_name,class_leader) values(1,'1班','大白'),(2,'2班','大白2'),(3,'3班','大白3');
insert into student_info(id,stu_name,age,fk_class_id,gender) values(1,"小白",13,1,'男'),(2,"小白2",14,2,'男'),(3,"小白3",15,3,'男'),(4,"小白4",16,1,'男');
delete from student_info where id=3 and gender='男';

update student_info set gender='男';
update student_info set gender='女',age=18 where id%2=0;
alter table student_info add gender CHAR(1);
alter table class_info add constraint primary key(id);
alter table student_info add constraint primary key(id);
alter table student_info add constraint foreign key(fk_class_id) references class_info(id);
```

