# Nginx

nginx是一个网页服务器, 可以用于反向代理 负载均衡 动态静态资源分离

nginx.conf为配置文件 一般在/etc/nginx/conf下面

```bash
#nginx并发进程数
worker_process 1;

events{
	#每个nginx进程的最大用户连接数
	#默认其最大值为1024
	worker_connections 1024;
}
http{
	server{
		
	}
}
```

