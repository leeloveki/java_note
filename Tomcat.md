# Tomcat

servlet和tomcat处理用户请求的步骤

0. 浏览器对指定的url发送请求(post,get等)
1. tomcat接收请求并解析
2. tomcat根据解析的uri找到处理请求的servlet
3. 创建servlet对象(tomcat统一管理)(可以省略)
4. 将请求交给对应的servlet对象处理

# Tomcat

tomcat配置server.xm

```xml
<--!解决乱码-->
<Connector port="8080" protocol="HTTP/1.1"
        connectionTimeout="20000"
        redirectPort="8443" 
        URIEncoding="UTF-8" ></Connector>
```

# Servlet

如何配置

