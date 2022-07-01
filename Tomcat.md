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
3. 使用web.xml或@WebServlet注解将servlet类映射到对应的url路径中
4. 在前端中调用接口地址

web.xml

```xml
<!--一个类可以对应多个servlet-name-->
<!--一个servlet-name可以对应多个url-->
<servlet>
	<servlet-name>servlet名</servlet-name>
    <servlet-class>servlet类名(全限定名)</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>配置好的servlet名</servlet-name>
    <url-pattern>servlet对应的url路径(注意路径前一般要加/)</url-pattern>
</servlet-mapping>
```

@webServlet注解

```java
@WebServlet(urlPatterns="/register")
//绑定多个url
@WebServlet(name="loginServlet",urlPatterns={"/login01","/login02"})
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

实现一个servlet类由三种方法:

1. 实现Servlet接口, 并实现该接口的所有方法(主要在service方法中处理请求和响应)
2. 继承HttpServlet类, 并实现doPost或doGet方法

> HttpServlet类属于Servlet接口的实现类

HttpServlet

可以重写doPost和doGet方法, 对于浏览器的post请求和get请求

两个方法的形参列表形同, 都包含HttpServletRequest req和HttpServletResponse resp

req对应浏览器发来的请求

resp对应服务器返回给浏览器的响应

HttpServletRequest的实例方法有

| 实例方法             | 值       | 作用                 |
| -------------------- | -------- | -------------------- |
| setCharacterEncoding | "编码集" | 设置解析请求的编码集 |
| getHeader            | "字段"   |                      |
| getHeaderNames       | void     | 返回所有的字段名     |
| getRequestURI        | void     | 返回请求的URI        |
| getRequestURL        | void     | 返回请求的URL        |
| getRemoteAddr        | void     | 返回发送请求的IP地址 |
| getRemotePort        | void     | 返回发送请求的端口   |
| getContextPath       | void     | 返回项目部署的路径   |

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

**jsp**

jsp是一种动态网页技术, 可以视为简化的servlet(可以用于实现servlet的功能)

jsp不能直接被浏览器解析, 必须在tomcat容器中运行, 会被服务器解析为对应的html代码

**applet**

applet是一种过时的java Web技术, applet的代码可以直接被浏览器解析成应用(网页), 相对于一种前端技术

> 目前applet技术已经被淘汰, 主流浏览器中只有IE浏览器还在支持applet
