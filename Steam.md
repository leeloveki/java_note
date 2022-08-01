# Steam

Java8中有两个最关键的新特性: lambda表达式(语法糖) Stream API(java.util.stream.*)

使用Stream(流)API可以对集合进行操作

Stream的特性:

1. 本身不存储数据, 
2. 不对源数据进行修改, 用新对象存储返回结果
3. 延迟执行操作, 等需要结果时才执行操作

Steam工作步骤:

1. 创建Stream
2. 中间操作

## 函数式接口

java8提供了内置的4种函数式接口, Consumer Function Predicate Supplier

> 这四种函数式接口都位于java.util.function包下

语法糖: 语法糖是指编程语言为了提高编程效率, 提供了使用更方便的编程语法

lambda表达式可以视为实现匿名内部类的语法糖

使用@FunctionalInterface注解可以声明一个接口为函数式接口, 编译器会检查该接口是否满足函数式接口的条件(只有一个抽象方法)

| 名称       | 内置函数接口 | 方法              | 作用                                         |
| ---------- | ------------ | ----------------- | -------------------------------------------- |
| 消费型接口 | Cosumer      | void accept(T t)  | 对传入泛型对象进行操作                       |
| 供给型接口 | Supplier     | T get()           | 返回一个泛型对象                             |
| 函数型接口 | Function     | R apply(T t)      | 对泛型T对象进行操作, 并返回泛型R对象         |
| 断言型接口 | Predicate    | boolean test(T t) | 对传入的泛型对象判断是否满足约束, 返回布尔值 |

> Stream流可以通过使用java8内置的4种函数接口, 实现各种流操作

```java
class Test{
    void test(){
        List<User> userList = Lists.newArrayList();
        //lambda表达式
        userList.stream()
                .filter(user -> user.getAge() > 18)
                .map(User::getName)
                .forEach(System.out::println);
    }
}
```

