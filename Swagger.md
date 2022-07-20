# Swagger

> 使用maven打包SpringBoot程序

```java
mvn package
```

打包会生成jar文件, 可以直接被java解释器运行

```bash
#运行打包的jar程序
java -jar 工程.jar
#使用命令行临时参数启动程序
java -jar 工程.jar --server.port=80
#命令行的临时参数相当于main函数中的形参String[] args
```

> 注意命令行临时参数优先级比程序的配置文件中的参数优先级高, 优先级高的参数会覆盖低优先级的参数

swagger是一个用于自动生成web api文档的框架

会生成url 请求内容和响应内容的示例

注解

| 注解                               | 作用 | 位置                 |
| ---------------------------------- | ---- | -------------------- |
| @Api("解释类的作用")               |      | 需要生成文档的工具类 |
| @EnableSwagger2                    |      | 配置类               |
| @ApiOperation(Value="all" note="") |      |                      |
|                                    |      |                      |



