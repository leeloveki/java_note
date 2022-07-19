# Tomcat

tomcat是开源的java web服务器, 实现了Servlet JSP Java WebScoket等web技术

Tomcat目录解析:

| 目录    | 作用                  |
| ------- | --------------------- |
| lib     | 存放java的jar包       |
| bin     | 二进制可执行文件      |
| conf    | 存放配置              |
| logs    | 存放日志              |
| webapps | 存放所有部署的web项目 |

当前常用的Tomcat版本为8.0对应Servlet3.1

tomcat配置server.xml解决乱码

```xml
<--!解决乱码-->
<Connector port="8080" protocol="HTTP/1.1"
        connectionTimeout="20000"
        redirectPort="8443" 
        URIEncoding="UTF-8" ></Connector>
```

当web程序部署到tomcat时, web文件夹会作为项目的根目录, src下面的资源会被编译为字节码后放到web-INF的class目录下

# Servlet

servlet和tomcat处理用户请求的步骤

0. 浏览器对指定的url发送请求(post,get等)到特定的服务器

1. 服务器(tomcat)接收请求并解析

2. 服务器(tomcat)根据uri将请求转发给对应的servlet

   > 如果对应的servlet对象还不存在将创建servlet对象, 运行的servlet对象都将由tomcat统一管理

3. servlet对象处理请求并生成响应

4. 服务器(tomcat)将响应转发给对应的用户

创建一个简单servlet程序的步骤:

1. 创建servlet类需要继承HttpServlet类并重写其doGet和doPost方法
2. 在doGet或doPost方法中处理请求(req)并生成响应(sesp)
3. 使用web.xml或@WebServlet注解将servlet类映射到对应的url路径中(触发关系)
4. 在前端中调用接口地址

web.xml

```xml
<!--一个类可以对应多个servlet-name-->
<!--一个servlet-name可以对应多个url-->
<servlet>
	<servlet-name>servlet名</servlet-name>
    <servlet-class>servlet类名(全限定名)</servlet-class>
    <!--配置servlet中使用的参数-->
    <!--通过servletconfig来获取-->
    <init-param>
    <param-name>username</param-name>
    <param-value>admin</param-value>
    </init-param>
</servlet>
<servlet-mapping>
	<servlet-name>配置好的servlet名</servlet-name>
    <url-pattern>servlet对应的url路径(注意路径前一般要加/)</url-pattern>
</servlet-mapping>
<!--配置全局参数-->
<context-param>
    <param-name>encoding</param-name>
    <param-value>utf-8</param-value>
</context-param>
```

> init-param只能在单个servlet中配置和使用
>
> context-param可以在整个web程序中配置和使用

全局参数一般用来配置编码或加载初始化文件

@webServlet注解

```java
@WebServlet(urlPatterns="/register")
//绑定多个url
@WebServlet(name="loginServlet",urlPatterns={"/login01","/login02"})
//注意urlPatterns的路径可以使用*通配符进行范围匹配对应的url
"/*/abc"注意这个url会匹配所有的url，实际上等同于"/*"
@WebServlet(urlPatterns="/register02",
           initParams={
               @WebInitParam(name="username",value="admin")
           })
```

获取配置中的参数

```java
ServletConfig servletConfig=getServletConfig();
String username=servletConfig.getInitParameter("username");
```

## 生命周期

**servlet是单实例多线程的对象**

在tomcat中一个servlet只能对应一个实例, 但是该实例可以被多个线程同时访问(一个servlet对象同时服务多个请求)

> 意味着tomcat中只需要创建一个servlet就同时可以处理多个请求, 不需要创建重复的对象

servlet的生命周期:

编译 加载 实例化 服务化 销毁

由于servlet单实例多线程的特性, 因此编译 加载 实例化都只需要进行一次, 而服务化在一个生命周期中可以重复进行多次直到销毁

## 具体使用

在java中servlet是一个接口, javax.servlet, 需要从外部导入使用(不属于JDK自带的类库)

实现一个servlet类有三种方法:

1. 实现Servlet接口, 并实现该接口的所有方法
2. 继承HttpServlet类, 并实现doPost或doGet方法
3. 继承GenericServlet类, 实现service()方法

继承关系: GenericServlet类实现了Servlet接口, HttpServlet继承了GenericServlet类

**Servlet接口的**

Servlet接口需要实现5个方法

| 方法名           | 作用                                                         |
| ---------------- | ------------------------------------------------------------ |
| init             | 初始化时执行, 对应生命周期的实例化, 每个生命周期内只会执行一次 |
| service          | 对应生命周期的服务化, 每次处理请求都会调用                   |
| destory          | 对应生命周期的销毁,在tomcat关闭时被自动调用                  |
| getServletConfig | 获取servlet的配置                                            |
| getServletInfo   | 获取servlet对象的状态信息                                    |

> 实现Servlet的service方法并调用自定义的doPost或doGet方法

```java
//service方法的参数均由tomcat传入
@Override
public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
    HttpServletRequest req=(HttpServletRequest)servletRequest;
    String method = req.getMethod();
    if(method.equals("GET")){
        doGet();
    }else if(method.equals("POST")){
        doPost();
    }
}
```

面试题

jsp和servlet的区别

单独使用jsp或者servlet都可以编写一个网页应用

jsp和servlet都可以包含html代码和java代码

| 区别              | servlet                         | jsp                                                  |
| ----------------- | ------------------------------- | ---------------------------------------------------- |
|                   | 后端                            | 前端                                                 |
| 本质              | java后端api, 可以在java中写html | 一种动态网页, 本质是在html代码中插入java代码 servlet |
| mvc模型           | 属于controller控制器            | 属于view视图                                         |
| 重写service()方法 | 可以                            | 不可以                                               |
| 接受的请求协议    | 所有协议都支持                  | http请求                                             |
| 会话管理          | 默认开启                        | 默认关闭                                             |
| 修改后如何生效    | 需要重新编译和重启服务器        | 直接刷新页面                                         |
| 隐式对象          | 没有                            | 有                                                   |
| 导包              | 作为库导入                      | 将包导入jsp代码中                                    |

## ServletContext

ServletContext是由Tomcat提供的上下文对象, 每个运行的servlet web程序都有一个对应的ServletContext对象

该对象的生命周期与对应web程序的生命周期相同

ServletContext对象是tomcat和web程序通信的唯一方法, 可以用于数据通信, servlet间的数据共享, 全局配置

有4种对象都提供了相同的方法来获取ServletContext

getServletContext()方法

1. ServletConfig对象
2. HttpServlet对象
3. HttpSession对象
4. ServletContextEven对象

```java
//在HttpServlet中可以调用父类的getServletContext()方法;
getServletContext();
//通过config获取
getServletConfig.getServletContext();
//通过session获取
req.getSession().getServletContext();
```

Java Web四大域对象:

1. ServletContext
2. PageContext
3. ServletRequest
4. HttpSession

这些域对象使用map来提供数据存储功能

servletContext有各种实例方法

| 方法名          | 值       | 作用                  |
| --------------- | -------- | --------------------- |
| setAttribute    | 键,值    | 存储context属性       |
| getAttribute    | 键       | 读取                  |
| removeAttribute | 键       | 删除对应的context属性 |
| getRealPath     | 相对路径 | 获取绝对路径          |

> getRealPath通常用于读取项目中的资源(调用字节流读取时需要提供绝对路径)

```java
//这里的相对路径中 /代表classes目录
//  不加/或者./ 代表当前所在package的目录
servlet类.class.getResourceAsStream("相对路径");
//这里的相对路径 /代表web目录
servletContext.getResourceAsStream("相对路径");
String path=servletContext对象.getRealPath("相对路径");
InutStream(path);
```

**重定向和转发**

```java
//转发请求
requestDispatcher.forward(req,resp);
//重定向过程
//手动重定向
//设置302响应码
resp.setStatus(302);
resp.setHeader("Location","url地址");
//使用resp自带的重定向方法
resp.sendRedirect("url地址")
```



**jsp**

jsp是一种动态网页技术, 可以视为简化的servlet(可以用于实现servlet的功能)

jsp不能直接被浏览器解析, 必须在tomcat容器中运行, 会被服务器解析为对应的html代码

**applet**

applet是一种过时的java Web技术, applet的代码可以直接被浏览器解析成应用(网页), 相对于一种前端技术

> 目前applet技术已经被淘汰, 主流浏览器中只有IE浏览器还在支持applet

**通过反射将map封装为对象**

```java
static User mapToBean(Map<String,Object> map,Class<User> clazz) throws IllegalAccessException {
    Field[] fields = clazz.getDeclaredFields();
    User user = new User();
    //通过反射遍历属性名
    for (Field field : fields) {
        //关闭安全检查
        field.setAccessible(true);
        String key = field.getName();
        Class<?> type = field.getType();
        Object o = map.get(key);
        //对user对象的特定属性进行赋值
        field.set(user,o);
    }
    return user;
}
```

