所有的类都是引用类型

一个对象可以有多个引用, 当对象不存在引用时, 会被GC(垃圾回收器)回收销毁其内存空间

将一个数组变量指向另一个数组变量时, 需要两个数组的类型相兼容

Java中没有从底层实现真正的多维数组, 

例如:在Java里面的二维数组本质还是一维数组, 其数组元素储存引用变量, 引用变量指向另外的一维数组

如何定义多维数组: 

```java
type[][] arrName;
```

初始化

```java
arrName = new type[length][]
```

上述语句实际上相当于初始化了一个一维数组, 该一维数组的长度为length, 其数组元素为引用类型, 被系统自动初始化赋值为null

![image-20220427102048009](.\image-20220427102048009.png)

> 注意上图中定义了一个元素为对象的数组, 其数值在内存的存储方式如图所示

```java
a = new int[4][];
```



![image-20220427102324663](.\image-20220427102324663.png)

> 上图中该二维数组进行了(一维)初始化, 其堆内存存储方式跟一维数组非常类似

![image-20220427102454276](.\image-20220427102454276.png)

> 上图显示了二维数组对其数组元素进行了(二维)初始化

```java
b = new int[3][4];
```
![plot](./image-20220427103350502.png)
![image-20220427103350502](.\image-20220427103350502.png)

> Java里面的三维数组也是一维数组, 其数组元素是二维数组, 二维数组里面的数组元素是一维数组, 所以Java多维数组的本质都是一维数组



# Array(数组)

Array是Java8里面提供的增强工具类

在Array类里面的static修饰的方法可以直接调用来操作数组

例如:

```java
int binarySearch(type[] a, type key) 
```

可以用二分法在a数组中查找key, 返回其出现过的索引值 由于二分法的要求,a数组中的元素必须是从小到大排序才能正确查找 如果不包含则返回一个负数

```java
int binarySearch(type[] a, int formIndex, int toIndex, type key)
```

与前一个方法类似, 但是搜索范围限制在 formIndex到toIndex里面

```java
type[] copyOf(type[] originalArray, int length)
```

将originalArray数组复制为length长度的新数组

```java
type[] copyOfRange(type[]originalArryay, int form, int to)
```

与前方法类似, 但是将复制范围限制在form到to的索引范围内

```java
boolean equals(type[] a,type[] a2)
```

如果两个数组的长度和数组元素一一对应相同,则返回true 否则返回false

```java
void fill(type[] a,type value)
```

将a数组中的元素都赋值为vlaue(注意这里的value需要是type类型的具体值)

```java
void fill(type[] a,int formIndex,int toIndex,type value)
```

与上一方法类似

