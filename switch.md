# switch

switch可以用于匹配值的条件语句

注意 switch的控制表达式中(匹配值)

只能用四种整数类型进行匹配(byte char short  int )

在Java7中还增加了对枚举类型和java.lang.String的支持

> 注意StringBuffer StringBuilder boolean类型是不支持的

case语句必须在最后加上break语句来确保可以正常跳出

switch语句更适合处理固定值的匹配

if语句更适合对范围进行判断

# for循环

for循环语句可以有4个部分

初始化语句(init_statement): 在整个循环开始前执行的语句, 仅执行一次

循环条件(test_expression): 应当是一个boolean表达式, 决定循环是否结束

循环体(body_statement): 一个代码块, 作为循环的执行主体在每次循环中都会执行

迭代语句(iteration_statement): 执行在循环体结束后, 在循环条件判断前

各语句的执行顺序:

循环执行:

1. 初始化语句(循环开始前执行)

2. 条件语句判断

   **如果不符合条件**: 直接结束循环

   **符合条件**:第三步

3. 执行循环体

4. 执行迭代语句

5. 返回第二步继续

```java
for(;;){}
```

> 上例代码中将for循环语句的四个部分省略不写, 也是符合语法的代码, 但是它本身是个死循环, 会导致程序无法退出结束

**尽量避免在循环体中对循环条件中的变量进行修改**

作为goto的替代, 在Java中可以用continue break (return)关键字来控制循环结构

break: 默认跳出当前所在的循环

continue: 直接进入迭代语句开始新的一轮循环,

**注意使用return关键字会导致整个方法的结束**

# 标签

```java
flag:
```

上例代码中定义了一个标识符为flag的标签

标签可以接在continue和break关键字的后面

> for循环和while循环都是先验循环
>
> 代表他们先验证条件语句再执行循环体

do while语句是后验循环, 先执行一遍循环体, 再验证

> while和for循环的区别
>
> for循环用于已知循环次数的情形
>
> while循环更适合用于未知循环次数, 但是知道退出条件的情况

> 建议循环的嵌套不要超过三层

# 数组

数组的特点:

1. 数组必须在声明的时候确认类型, 并且存入数组的元素都必须是同一类型
2. 数组的空间结构是连续, 有序的, 可以通过索引去访问数组内的元素
3. 数组的长度是固定的, 在初始化时就确定好, 不发生改变

