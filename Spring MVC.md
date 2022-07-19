# SpringMVC

SpringMVC是Spring内置的web开发框架

工作流程:

1. 客户端发送请求, DispatcherServlet(主控制器)接收请求

2. DispatcherServlet调用HandlerMapping处理请求

3. 根据HandlerMapping存储的请求和Controller关系, 将请求转发给对应的Controller

4. Controller根据业务逻辑对请求进行处理(如果需要访问数据库会调用到DAO组件)

5. Controller处理完请求后返回ModelAndView对象

   > ModelAndView相当于响应的原始信息, 在该对象中封装了模型数据和视图资源标识

6. Servlet控制器调用ViewResolver组件, 将ModelAndView对象解析为响应信息返回给客户端

Spring MVC要重点掌握的知识:

1. SpringMVC的架构和各组成部分的作用
2. 如何处理请求和响应
3. REST设计模式
4. SSM整合
5. 拦截器

REST是一种url的设计风格

```html
<!--传统风格-->
http://127.0.0.1/user/getById?id=1
<!--REST风格-->
http://127.0.0.1/user/1
<!--传统风格-->
http://127.0.0.1/user/saveUser
<!--REST风格-->
http://127.0.0.1/user
```

REST风格需要将url进行简化, 隐藏资源行为(降低可读性)(防止通过url地址知道url背后的服务操作)

REST风格的url示例

```html
get : http://localhost/users 
查询全部用户信息
get : http://localhost/users/1
查询指定用户信息
post : http://localhost/users 
添加用户信息
put : http://localhost/users 
修改用户信息
delete : http://localhost/users/1 
删除指定用户信息
```

# Spring SSM注解

| 注解                                      | 作用                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| @Configuration                            | 声明该类为配置类                                             |
| @ComponentScan("包或类路径")              | 设置初始化容器时扫描的类的路径, 比如servlet类包, mapper类包, service(dao)类包 |
| @PropertySource("classpath:mapper/*.xml") | 配置需要扫描的配置, 需要放置在Resource资源下                 |
| @Import({MybatisConfig.class})            | 加载指定的配置类写的注解                                     |
| @Bean                                     | 将指定的类对象添加到IOC容器中管理                            |
| @MapperScanner("com.lee.mapper")          | 指定mybatis的mapper类的扫描路径                              |
| @RestController                           | 等同于@Controller+@ResponseBody                              |
| @Controller                               | 声明该类为servlet类, 可以绑定url接收处理请求                 |
| @RequestMapping("url")                    | 将类或方法绑定对应url, 接收请求                              |
| 自动序列化注解                            |                                                              |
| @ResponseBody                             | 使用插件将返回值序列化为对应的json字符串                     |
| @EnableWebMvc                             | 启用自动序列化插件, 一般在spring配置类中添加该注解           |
| 异常处理类注解                            |                                                              |
| @RestControllerAdvice                     | 统一处理异常                                                 |
| @ExceptionHandler                         | 处理指定的异常                                               |

## 异常抛出

SpringMVC 按层异常抛出处理流程

1. controller层
2. service层
3. dao层

> 注意是自底向上的结构

SpringMvc启动类

REST风格的要求:

(一)同一条url, 根据客户端发送http请求的方法不同而提供不同的功能:

1. GET 查询
2. POST 添加
3. PUT 修改
4. DELETE 删除

(二)servlet响应统一格式: code msg data

> code:状态码 msg:信息 data:数据

**SSM项目**

SSM项目是指同时整合了三大框架(Spring+Spring MVC+MyBatis)的项目, 可以

**初始化容器**

1. ServletContainer创建后加载springmvcConfig
2. 根据@ComponentScan注解的路径扫描有@Controller注解的类, 并通过@RequestMapping将类或方法和url绑定
3. 根据getServletMappings配置当前项目总的拦截方法



