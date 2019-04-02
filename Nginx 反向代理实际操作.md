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
4. 重启 nginx：/nginx -s reload