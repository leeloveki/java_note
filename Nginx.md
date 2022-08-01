# Nginx

nginx是一个静态网页服务器, 可以用于反向代理 负载均衡 动态静态资源分离

nginx特点: 专门为静态资源优化, 高性能 高效率, 可以用于高并发高负载的场景

nginx应用场景:

1. 静态资源服务器
2. 代理服务器
   1. 正向代理: 负责代理客户端对外的流量
   2. 反向代理: 负责代理服务器对外的流量
      1. 负载均衡: 通过反向代理实现将客户端的请求分配给多个服务器
      2. 动静分离: 将动态资源请求和静态资源请求分配给不同的服务器

nginx命令:

1. nginx stop 关闭nginx
2. nginx start
3. nginx restart
4. nginx -s load nginx热重载配置, 会关闭正在连接的worker线程
5. nginx -v

nginx.conf为配置文件 一般在/etc/nginx/conf下面

```bash
#nginx并发进程数
worker_processes 1;

#动态均衡
upstream myserver{
		#weight是权重, 默认为1
        server 127.0.0.1:6060 weight=2;
        server 127.0.0.1:6061;
}
events{
	#每个nginx进程的最大用户连接数
	#默认其最大值为1024
	worker_connections 1024;
}
http{
	server{
		location / {
				//设置要转发给的upstream
                proxy_pass http://myserver;
        }
	}
}
```

nginx提供了4种负载均衡的方式:

| 负载均衡方式       | 作用                                                         |
| ------------------ | ------------------------------------------------------------ |
| 轮询（默认）       | 默认， 按照请求时间顺序来轮询, 会自动剔除失效的服务器        |
| 权重轮询（weight） | 根据权重来分配访问比例， 权重一般对应服务器的性能            |
| ip_hash轮询        | 根据ip的hash值来轮询对应的服务器， 可以解决session问题       |
| fair               | 第三方智能负载均衡模块， 根据服务器响应时间， 页面大小等因素 |

nginx.conf组成部分

1. 全局块: 
2. events块: 
3. http块: 
   1. server块
   2. location块
      1. 设置不同的url匹配

location后的url匹配语法:

```bash
location [=|~|~*|^~] url{}
```

| 匹配符 | 作用                                      |
| ------ | ----------------------------------------- |
| =      | url匹配不使用正则匹配, 使用字符串严格匹配 |
| ^~     | url匹配不使用正则匹配, 使用字符串模糊匹配 |
| ~      | 使用正则匹配, 区分大小写                  |
| ~*     | 使用正则匹配, 不区分大小写                |

