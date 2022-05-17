# StringBuilder

StringBuilder类是具有缓冲能力的字符串处理类

特点: 长度和内容都是可变的

常用方法:

append

insert

setCharAt

replace

deleteChatAt

delete

capacity

length

reverse

indexOf

lastIndexOf

```java
		StringBuilder sb=new StringBuilder("a1234a");
        //创建一个内容为a1234a的StringBuilder对象
        System.out.println(sb);
        sb.append("a");
        //将另一个字符串,或者基本数据类型加入到原字符串的末尾
        System.out.println(sb);
        sb.insert(1,'b');
        //在index位置插入字符串或基本数据类型数值, 原来位置的字符向后移
        System.out.println(sb);
        sb.setCharAt(2,'c');
        //将index所在位置的字符替换为字符串或基本数据类型
        System.out.println(sb);
        sb.replace(1,3,"d");
        //先删除startIndex到EndIndex-1的内容
        //然后插入子字符串
        System.out.println(sb);
        sb.deleteCharAt(1);
        //删除字符串中index对应字符
        System.out.println(sb);
        sb.delete(1,3);
        //删除startIndex到EndIndex-1的内容
        System.out.println(sb);
        System.out.println(sb.capacity());
        //输出sb底层char[]数组的长度
        System.out.println(sb.length());
        //输出内容字符长度
        System.out.println(sb.reverse());
        //将字符串反转
        System.out.println(sb.indexOf("a"));
        //输出第一个匹配到的字符串的第一个字符所在index
        sb.append('a');
        System.out.println(sb);
        System.out.println(sb.lastIndexOf("aa"));
        //输出第二个匹配到的字符串的第一个字符所在index
```

**注意StringBuffer中也有跟StringBuilder相同功能的同名方法**

**StringBuffer和StringBuilder的方法会操作对象本身, 但是String中的方法不会操作对象, 而是返回一个新的String类对象**

**StringBuffer和StringBuilder两个属性length和capacity, 都是可变的, capacity代表对象的底层存储数组的长度(容量) 通常比length大, 而且该属性由系统自动操作, 程序无须关心**

可以用length()和setLength(int len)方法来获取长度或修改长度

String对象没有capacity属性, 只有length属性

**Java中有三种类来封装字符串: String StringBuffer StringBuilder**

区别: String类是固定的, 一旦创建不可再次改变

StringBuffer和StringBuilder类类似, 两个类都是可变的, 并且两者的方法和构造器基本相同, 但是只有StringBuffer是线程安全

> 线程安全意味着StringBuffer有更好的高并发, 多线程支持

StringBuffer类可以通过toString()方法转换成对应的String对象

StringBuilder的性能较高

创建一个内容可变的字符串对象时, 应该优先考虑使用StringBuilder

>  Java中有CharSequence接口, 该接口被字符串的三种类都实现了, 可以视为Java中的字符串通用接口

Java8中, 三个类都是使用char[]数组来存储字符串, 因此字符串中的每个字符占两个字节大小

Java9改进了三个类, 使用byte[]数组和encoding-flag字段来存储字符, 使得每个字符只占一个字节, 节省了内存空间

但是改进不会影响三个类的功能方法的使用

# String类

```java
String s="123456";
int num=Integer.parseInt(s);
```

java中的8种基本数据类型的包装类都提供了工具方法parseInt(String s)来将字符串转换成对应的包装类对象

String类有大量构造器来创建对象

常用的构造器有:

String() 返回一个0长度的对象

String(String original) 用字符串直接量创建对象

String(StringBuffer buffer) 将StringBuffer类对象转换成String对象

String(StringBuilder bulder) 将StringBuilder类对象转换成String对象

String类的20种方法的使用示例: 

```java
String str="11123";
        String str3="abc";
        String str4="ABC";
//        boolean flag = str3.isEmpty();
//        str.indexOf("23")
        System.out.println(str.indexOf("213"));
        //返回第一个匹配到的字符串的第一个字符所在index
        //不匹配返回-1
        System.out.println(str.charAt(1));
        //返回index对应的字符
        String str2=str.concat("456");
        //进行字符串拼接,等同于+操作
        System.out.println(str2.contains("56"));
        //判断是否包含另一个字符串
        System.out.println(str3.equals(str4));
        //判断字符串内容是否相等
        System.out.println(str3.equalsIgnoreCase(str4));
        //判断字符串内容是否相等, 忽略字母大小写的区别
        System.out.println(str3.startsWith("a"));
        //判断字符串的开头是否是另一个字符串
        //String str3="abc";
        System.out.println(str3.endsWith("a"));
        //判断字符串的末尾是否是另一个字符串
        System.out.println(Arrays.toString(str3.getBytes(StandardCharsets.UTF_8)));
        //以数组形式返回字符串的每个字符在编码集中对应的数值
        System.out.println(Arrays.toString(str3.toCharArray()));
        //返回字符串中的字符对应的char数组
        System.out.println(str3.isEmpty());
        //判断字符串的内容是否为空
        String str5="  aab c   ";
        System.out.println(str5.trim());
        //去除字符串开头和结尾的空格并返回
        //注意不会修改字符串本身, 返回的是另一个新字符串
        System.out.println(Arrays.toString(str5.split(" ")));
        //以参数的字符串为分隔符, 将字符串分割并存储为char类型数组返回
        System.out.println(str5.equals(str5.substring(0)));
        //返回从index开始到结尾的子字符串
        System.out.println(str5.replace("a",""));
        //将字符串中所有的匹配字符串替换为另一个字符串
        System.out.println(str5.lastIndexOf('a'));
        //返回字符串中最后一个匹配的字符所在的index
        str5.toUpperCase(Locale.ROOT);
        //将字符串中的所有小写字母转换为大写
        str5.toLowerCase(Locale.ROOT);
        //将字符串中所有的大写字母转换成小写
        System.out.println(String.valueOf(5));
        //将基本数据类型转换成字符串, 可以用空白字符串+基本数据类型代替
        //注意该方法为类方法, 不是实例方法
        System.out.println(str5.intern());
        //从常量池中取出对应的字符串对象返回, 如果不存在则在常量池中创建
```

```java
String a="1";
String b=a+a+a+a+a+a;
//上述代码将中途产生4个临时对象, 使用StringBuilder或StringBuffer类可以避免产生临时对象
```

# Array(数组)

Arrays类是Java中的数组(Array)工具类

提供了大量static方法来操作数组

> 注意通常是使用Arryas.方法名来调用Arrays中的方法
>
> 不是通过数组实例来调用Arrays中的方法

Array中的常用方法

toString() 将数组转换成字符串

sort() 将数组替换为排序后的数组元素, 会修改数组本身

binarySearch() 用二分查找在数组中查询数据返回index, 二分查找算法要求数组必须是有序的, 否则返回的结果可能不正确

copyOf 将数组复制到另一个指定长度的数组中, 不会修改数组本身

equals: 对比两个数组的长度和数组元素是否一一对应, 不相同则返回false



在Array类里面的static修饰的方法可以直接调用来操作数组

例如:

```java
int binarySearch(int[] nums, int num) 
```

用二分法在nums数组中查找key, 返回其出现过的索引值 由于二分法的要求,nums数组中的元素必须是从小到大排序才能正确查找 如果不包含则返回一个负数

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

将a数组中的元素都赋值为value(注意这里的value需要是type类型的具体值)

```java
void fill(type[] a,int formIndex,int toIndex,type value)
```

与上一方法类似, 但是仅仅将索引范围内的元素赋值为value

```java
void sort(type[] a)
```

对a数组中的数组元素进行排序

```java
void sort(type[] a, int formIndex, int toIndex)
```

与上一方法类似,但是仅对范围内的元素进行操作

```java
String toString(type[] a)
```

将数组的元素按顺序拼接为字符串并返回, 每个元素中间用逗号和空格进行分割

在Java8中, 对Array类的功能进行了增强, 添加了新的工具方法(这些方法支持利用CPU的并发处理性能)

```java
void parallelSort(type[] a)
```

与sort方法类似, 但是增加了对并发运算的支持

> 用parallel开头的方法都表示该方法增加了对并行计算的支持

**sort方法会改变数组本身**

```java
		int[] nums=new int[]{10,2,3,4,5,6,7,8,9,-1};
        Arrays.sort(nums);
        //sort()将数组替换为排序后的数组
        System.out.println(Arrays.toString(nums));
        //toString()将数组打印
        System.out.println(Arrays.binarySearch(nums, 4));
        //用二分查找法搜索数组, 返回匹配到的数组元素所在的index
        int[]nums2=Arrays.copyOf(nums,4);
        //将数组复制成一个长度指定的新数组并返回
        //不会修改原数组本身
        System.out.println(Arrays.toString(nums2));
        nums2=Arrays.copyOfRange(nums,2,4);
        System.out.println(Arrays.toString(nums2));
        //将数组从startIndex复制到endIndex-1的位置
        //不会修改原数组本身
        Arrays.equals(nums,nums2);
        //仅当两数组的长度和数组元素一一相同时才会返回true
        //否则返回false
        Arrays.fill(nums2,2);
        //将数组的元素都赋值为对应类型的变量或直接量
        //将会修改数组本身
        System.out.println(Arrays.toString(nums2));
```

# System类

**System代表当前运行的JVM本身, 程序不能创建System对象**

> System提供了类变量和类方法来给外部调用

**Java提供了System类和Runtime类使程序可以与系统交互**

类成员: 

System.in 标准输出

System.out 标准输出

System.error 异常输出

类方法可以访问环境变量和系统属性

> 这里的标准和异常是指系统的信息状态

**Java中如果需要调用C语言来方法操作系统底层硬件设备可以通过以下步骤实现:**

> 1. 声明一个native修饰的方法, 只有方法签名没有方法实现
> 2. 用Javac编译成class文件
> 3. 用Javah编译成.h文件
> 4. 在C语言的源代码中include .h文件并实现native修饰的方法
> 5. 将C语言的源代码文件编译成动态链接库文件
> 6. 在Java中用System.loadLibrary()或Runtime.loadLibrary()加载第五步的动态链接库文件, 该native方法就可以被调用了

```java
Map<String,String> env=System.getenv();
        //将当前系统中的所有环境变量获取名称
        //Map是java.util中的一个类
        for(String name:env.keySet()){
            System.out.println(name+"--"+env.get(name));
        }
        //遍历所有的环境变量的值
        Properties props =System.getProperties();
        //Properties也是java.util中的一个类
        props.store(new FileOutputStream("props.txt"),"System pro");
        //FileOutPutStream是java.io中的类, 用于将信息存储到文件里
        //默认路径为项目所在的根目录
```

上述代码调用System.getenv方法来获取当前系统的环境变量, 并将获取到的数据存储到了文本文件中

System类中常用的方法有:

1. **获取当前的系统环境变量**

getenv() getProperties() getProperty(String name)

2. **获取当前时间**

currentTimeMills() nanoTime()

> 返回的值为当前时间与1970年1月1日0:00的时间差, 
>
> currentTimeMills()以毫秒为单位, nanoTime()以纳秒为单位
>
> 并且不同操作系统的底层时间粒度不同, 所以导致返回值的精确会有差异
>
> 大部分操作系统以几十毫秒为时间测量单位, 所以很少用到nanoTime()

3. System.exit(0)

   > 关闭程序所在的JVM, 会导致JVM和上面运行的所有程序直接结束运行

4. System.gc()

   > 主动调用垃圾回收

5. setIn() setOut() setErr()

   >改变系统的标准输入 标准输出 标准错误输出流

6. arraycopy()

   > 复制的两个数组都必须已经初始化, 如果index超过数组长度将发生数组索引越界错误

```java
		System.out.println(System.currentTimeMillis());
        //输出当前系统时间与1970年1月1日0:00的时间差, 以毫秒为单位
        System.gc();
        //主动调用垃圾回收
        int[] nums=new int[]{1,2,3,4,5,6};
        int[] nums2=new int[6];
        System.arraycopy(nums,2,nums2,2,2);
        System.out.println(Arrays.toString(nums2));
        //必须是对两个已经初始化的数组进行操作
		System.exit(0);
        //导致整个JVM停止运行
```

# Runtime类

每个Java程序都有一个对应的Runtime实例

Runtime提供了实例方法来操作当前程序的运行时环境

> System类的操作会影响到整个JVM上面的程序

常用的方法:

getRuntime() 获取当前程序对应的Runtime对象

gc()

> 与system类中的方法功能类似

runFinalization()

> 与system类中的方法功能类似

freeMemory()

获取空闲内存量

totalMemory()

获取总的内存量

exec("notepad.exe")

> 运行操作系统中指定名称的程序
>
> 在Java9中提供了ProcessHandle接口和ProcessHandle.Info实现类来获取exec运行的进程信息

# Math

Math是Java中的数学运算工具类, 提供了大量数学运算的方法

常见方法:

random()

> 产生一个0.0到1.0范围的浮点数

abs(int i)	pow(int a,int b)	max(int a,int b)	min(int a,int b)	round(double d) 

```java
		System.out.println(Math.abs(-1));
        //绝对值
        System.out.println(Math.pow(3.0, 2.0));
        //求幂
        System.out.println(Math.max(1, 2));
        //返回最大值
        System.out.println(Math.min(1, 2));
        //最小值
        System.out.println(Math.round(15.5));
        //将double小数四舍五入返回一个int数值
```



# Random类

Random中提供了大量的实例方法用于产生随机数值

使用Random中的实例方法一般要先创建Random类的实例

Random类有两个构造器, 无参构造器用默认的种子(当前系统时间) 有参构造器需要传入一个long类型整数作为种子

```java
Random random=new Random();
//调用无参构造器创建对象
Random random2=new Random(100);
//调用有参构造器创建对象, 如果传入的seed相同, 则创建的随机数可以追溯
random.nextInt(100);
//产生一个0-~100的随机数
```

Java7中提供了Random的增强类 ThreadLocalRandom

> 两个类的功能类似, 但是ThreadLocalRandom提供了对多线程 高并发的支持 有更好的线程安全

# UUID类

UUID: Universally Unique Indentifier (通用唯一标识码)

**UUID用于生成36位的随机值, UUID值的重复概率非常低, 因此可以视为具有唯一性**

可以用于高并发的系统中, 作为数据的唯一索引

> UUID是根据当前系统时间, 网卡MAC地址再加上随机数(盐) 作为种子来产生UUID值, 可以视为具有很高的随机性

# BigDecimal

在Java中进行浮点数(float double)运算时会发生精度丢失,计算的结果不准确

> 原因在于浮点数是将十进制小数转换为二进制存储, 转换过程中会发生数据丢失(精度丢失)

**数据丢失的本质是由于部分十进制小数没有对应的二进制浮点数, 只能存储成无限接近的近似值(类似于分数中无限循环小数的概念)**

为了在十进制小数的存储和使用时避免精度丢失, Java提供了BigDecimal来进行十进制小数的存储和运算

> 在BigDecimal中, 十进制浮点数并非直接转换成二进制浮点数进行存储, 所以可以避免产生数据丢失

BigDeciaml提供了大量构造器来将浮点数存储为对象

BigDecimal(double val)

> 不推荐使用该构造器, 推荐使用BigDecimal.valueOf(double val)类方法来创建对象

BigDecimal(type val)

> 将8种基本数据类型(除了Boolean) 转换成对应的BigDeciaml对象 

**推荐使用BigDeciaml(String str)构造器来将小数对应的字符串转换**

常用方法(都是实例方法, 必须通过对象调用): 加减乘除 幂

add()	subtract()	mutiply()	divide()	pow()

```java
BigDecimal bd=new BigDecimal("10");
BigDecimal bd2=BigDecimal.valueOf(2.2222);
bd.add(bd2);
//将BigDecimal对象与另一个对象相加, 不会修改原对象, 直接返回结果对象
bd.subtract(bd2);
//将两个对象相减并返回结果
bd.multiply(bd2);
//将两个对象相乘并返回结果
bd.divide(bd2);
//相除并返回结果
bd.divide(bd2,2,BigDecimalROUND_DOWN)
//相除, 返回只保留两位小数的结果
bd.pow(2);
//计算幂, 注意必须传入参数必须为int整数
```

**收尾模式**

类常量:

ROUND_DOWN	保留小数位,后面都舍弃

ROUND_UP	判断保留小数位的下一位不等于0则进一

ROUND_HALF_UP	四舍五入

```java
BigDecimal bd2=BigDecimal.valueOf(2.1234567);
bd2.setScale(4,BigDecimal.ROUND_HALF_UP);
//只保留4位小数,四舍五入
```

**setScale() 需要输入保留位数和保留模式的参数**
