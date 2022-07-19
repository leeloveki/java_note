# Servlet url匹配

servlet的url匹配是有优先级的, 并且每个url只能被优先级最高的那个servlet匹配, 不会被多个servlet匹配

优先级顺序: 全路径匹配(精确匹配) > 目录匹配 > 扩展名匹配(后缀匹配) > 缺省匹配(不属于前三种的所有匹配)

| 匹配类型           | 规则             |    示例 |
| ------------------ | ---------------- | ------: |
| 精确匹配           | /开头, 不能包含* |  /login |
| 路径匹配(目录匹配) | /开头, /*结尾    | /path/* |
| 扩展名匹配         | .*开头           |   *.jsp |
| 缺省匹配(default)  | 只能是/          |       / |

> 路径匹配中有特殊的情况: /* 这种url-pattern会匹配当前网站下所有的路径

# BeanUtils

BeanUtrils是一个第三方类库, 提供了大量的static方法来调用

| 类方法      | 参数列表             | 作用                            |
| ----------- | -------------------- | ------------------------------- |
| getProperty | 对象,"属性名"        | 获取对象对应属性的值            |
| setProperty | 对象,"属性名",属性值 | 设置对象的                      |
| populate    | 对象,map             | 根据map的键值将数据填充到对象中 |

BeanUtrils的方法利用了反射实现了对象的填充和读取功能, 可以使用相同的代码对不同的对象进行操作 

## HttpServlet

> HttpServlet类属于Servlet接口的实现类

在HttpServlet的继承类中可以重写doPost和doGet方法, 对于浏览器的post请求和get请求

> 一般开发中只需要实现doPost和doGet其中一个方法, 另一个方法直接调用复用代码

两个方法的形参列表形同, 都包含HttpServletRequest req和HttpServletResponse resp

req对应浏览器发来的请求

resp对应服务器返回给浏览器的响应

## ServletRequest

tomcat会将接收到的请求封装为一个请求对象(ServletRequest)

**HttpServletRequest**

HttpServletRequest是ServletRequest类的继承类, 用于封装http请求

http请求的数据()请求行, 请求头, 请求体)都在HttpServletRequest里面

HttpServletRequest的实例方法有

| 实例方法             | 值       | 作用                                            |
| -------------------- | -------- | ----------------------------------------------- |
| setCharacterEncoding | "编码集" | 设置解析请求的编码集, 可以解决中文乱码问题      |
| getHeader            | "字段"   | 通过字段获取请求头的值                          |
| getHeaderNames       | 空       | 返回所有的字段名                                |
| getParameter         | 参数名   | 通过参数名获取单个提交里的值                    |
| getParameterValue    | 参数名   | 可以获取多个提交的值                            |
| getParameterNames    | 空       | 获取所有的参数名                                |
| getParameterMap      | 空       | 将所有的参数名和对应的值封装在map里面返回, 键值 |
| getRequestURI        | 空       | 返回请求的URI                                   |
| getRequestURL        | 空       | 返回请求的URL                                   |
| getContextPath       |          |                                                 |
| getMethod            |          |                                                 |
| getCookies           |          |                                                 |
| getSession           |          |                                                 |
| getRemoteAddr        | 空       | 返回发送请求的IP地址                            |
| getRemotePort        | 空       | 返回发送请求的端口                              |
| getContextPath       | 空       | 返回项目部署的路径                              |

HttpServletResponse

| 实例方法        | 值             | 作用                                          |
| --------------- | -------------- | --------------------------------------------- |
| getWriter       | 空             | 返回一个java.io.PrintWriter对象, 用于发送文本 |
| getOutputStream | 空             | 返回一个java.io.PrintWriter对象, 用于发送文本 |
| addCookie       | Cookie对象     | 为当前session的用户添加cookie                 |
| sendRedirect    | location字符串 | 重定向                                        |
| setContentType  |                |                                               |

