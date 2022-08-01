# Lambda(闭包)

**Lambda表达式是Java中的函数式编程特性**

> 函数式编程是指将函数作为基本运算单位, 意味着函数可以作为变量来使用(接收函数, 返回函数)

Lambda是Java8的一个重大更新

Lambda表达式相当于创建了一个匿名方法

Lambda只能用来实现单方法接口的实例化(一个接口中只有一个方法)

lambda表达式的有点:

1. 可以简化匿名内部类的代码, 减少生成内部类文件
2. 将函数作为变量传递, 为Java提供了函数式编程的支持

lambda可以用于替代匿名内部类对象

> 只有一个抽象方法的接口被称为函数式接口

lambda表达式允许将代码块作为实参传给一个方法

Lambda表达式有三部分:

1. 形参列表

2. 箭头(->) 

3. 代码块

   **代码块必须包含return语句)

   > 当lambda表达式的代码块只有一条语句时, 会将该语句返回值自动return

```java
interface Hello{
    void hello(int num);
//    void test();
}
class Test15{
    public static void main(String[] args) {
        Hello h=(num)->{
            System.out.println("hello");
        };
    }
    //省略写法, 也是语法正确的
    Hello h=num -> System.out.println("hello");
    //注意当没有形参时, 不可以省略圆括号
}
```

上述代码用lambda实现了一个接口的实例化

**Lambda表达式所创建的对象的目标类型(target type)为函数式接口( functional interface)**

函数式接口: 一个接口只能有一个抽象方法, 但是可以包含多个默认方法 类方法 私有方法

> Java8中为函数式接口提供了注解@FunctionalInterface
>
> 该注解放在接口声明之前, 用于提示编译器检查该接口必须是函数式接口, 如果不是的话会造成编程报错

Lambda表达式的限制条件:

1. 目标必须是函数式接口类型
2. 一个Lambda表达式只能实现一个方法, 只能为函数式接口创建对象
3. Lambda表达式的形参列表必须与该函数

常见使用场景:

1. 将lambda表达式赋值给函数接口类型的变量

2. 将lambda作为函数式接口类型的参数传给一个方法

3. 将lambda表达式进行强制转换为函数接口类型后再使用

   > 如传给Object类型引用变量

Java8在java.util.function包中提供了大量预定义的函数接口

4类典型的接口:

1. XxxFunction
2. XxxConsumer
3. XxxPredicate
4. XxxSupplier

## 方法引用和构造器引用

lambda表达式有更加简洁的写法

| 种类       | 示例           | 对应的表达式                        | 说明 |
| ---------- | -------------- | ----------------------------------- | ---- |
| 类方法     | 类名::类方法   | () -> class.staticMethod()          |      |
| 实例方法   | 对象::实例方法 | () -> 对象.noStaticMethod()         |      |
|            | 类名::实例方法 | (b,....) -> a.noStaticMethod(b,...) |      |
| 引用构造器 | 类名::new      | () -> new Class()                   |      |

```java
interface Hello {
    void hello(int a, int b);

    static void hello() {}
}

interface Hello2{
    void helloHo(Ho a,int b,int c);
}

class Ho {
    public Ho(int i, int i1) {}

    Ho() {}

    void hello(int a, int b) {}

    static void staticHello(int a, int b) {}
}

class Test16 {
    public static void main(String[] args) {
        Ho ho = new Ho();
        Hello h2 = (a, b) -> Ho.staticHello(a, b);
        Hello h6 = Ho::staticHello;
        //类方法
        Hello h3 = (a, b) -> ho.hello(a, b);
        Hello h7 = ho::hello;
        //特定对象的实例方法
        Hello2 h8=(a,b,c)->a.hello(b,c);
        Hello2 h9=Ho::hello;
        //某类对象的实例方法
        Hello h4 = (a, b) -> new Ho(a, b);
        Hello h5 = Ho::new;
        //引用构造器
    }
}
```

lambda和匿名内部类有相同之处:

1. 两者都可以直接访问接口中默认被final修饰的变量
2. 两者对应的实例都可以调用接口中继承的默认方法

两者的区别:

1. 匿名内部类可以为任何接口 抽象类 普通类创建实例, lambda表达式只能创建函数接口的实例
2. 匿名内部类的代码块可以调用接口的默认方法, lambda表达式的代码块不能调用默认方法

在Arrays类中的有些类方法需要Comparator, XxxOperator, XxxFunction等函数接口的实例, 可以用lambda表达式来实现, 使代码更简洁

