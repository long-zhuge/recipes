## Nginx 反向代理

- 域名（阿里云）：crm.ryinfotech.com
- 目标ip：125.124.51.95:81 (天翼云)
- 目的：访问域名时，会重定向到目标IP地址，且浏览器地址不会改变成IP地址。

1. 登录阿里云，域名解析，添加 crm.ryinfotech.com 的记录值(47.110.125.126，nginx所在的服务器)
2. ssh 47.110.125.126，修改 nginx.conf
3. 添加代码

	```
	http {
		...
		server {
        	listen 80;
        	server_name crm.ryinfotech.com;
        	index   index.php;
        	location / {
            	proxy_pass http://125.124.51.95:81;
            	proxy_redirect http://125.124.51.95:81 http://$host:$server_port;
        	}
    	}
	}
	```
4. 重启（修改配置后）：/nginx -s reload
5. 停止：nginx -s stop
6. 启动：nginx
7. 查看实时日志
	- 定位日志输出位置，查看 nginx 目录下的 nginx.conf
	
	```
	http {
		access_log  /var/log/nginx/access.log  main;
	}
	```
	
	- 查看实时日志

	```
	// 进入 /var/log/nginx 目录
	tail -f access.log
	```

### 常见命令

查看机器在执行的端口情况

```
// 查看所有端口执行情况
$ netstat -tunpl

// 查看监听的端口号
$ netstat -lanput

// 查看 8072 执行情况
$ ps -ef | grep 8090
```