# Swagger

swagger是一个用于自动生成web api文档的框架

会生成url 请求内容和响应内容的示例

注解

| 注解                 | 作用 | 位置                 |
| -------------------- | ---- | -------------------- |
| @Api("解释类的作用") |      | 需要生成文档的工具类 |
| @EnableSwagger2      |      | 配置类               |
|                      |      |                      |
|                      |      |                      |



```java
//Api一般用于注释类


@ApiOperation(Value="all" note="")

//配置类里

```

