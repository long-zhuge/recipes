## jenkins 前端应用部署操作流程

> 此流程不涉及后端应用，我们假设后端应用已经启用。<br/>
> 大致步骤为：jenkins创建前端应用 --> 配置 Dockerfile --> 配置 nginx 代理

- [jenkins](http://jenkins.abssqr.cn:9000/)
- [abs/config](http://github.abssqr.cn/abs/config)

### 创建和构建

1. 使用 admin 账号登录 jenkins。（ps：密码请询问 @韩飞）
2. 点击左上角 “新建任务” --> 输入任务名称（应用名，必须符合 dockerfile 命名规范）--> 往下拉后，可见一栏 “复制”，输入 formula 项目名，就可复制其配置
3. 配置项修改：修改源码管理的 Git 地址
4. 登录 git 项目仓库，在 Settings -> Members 中添加 “abssqrdeploy” 用户
	- abssqrdeploy 账户是用于整体构建使用的账号
5. 访问 “abs/config” 仓库添加 dockerfile
	1. 假设 jenkins 创建的应用名为 “abssqr”
	2. 创建文件名为 “abssqr-dev-Dockerfile” 的配置文件，且内容如下
	
	```
	FROM js
	ADD dist /usr/share/nginx/html/
	ENV port 89
	EXPOSE $port
	RUN sed -i "s/80/$port/g" /etc/nginx/conf.d/default.conf
	ENTRYPOINT nginx -g "daemon off;"
	```
6. 登录服务器查看哪些端口是可以使用的
	
	```
	$ ssh -p3049 test@47.99.200.24 // 密码请询问 @韩飞
	$ su - // 切换至 root 用户
	$ netstat -tunpl // 查看端口使用情况
	```	
7. 假设 8010 端口可以使用，那么修改 dockerfile 配置的端口号

	```
	...
	ENV port 8010
	...
	```
8. 在新创建的 jenkins 应用中点击 Build with Parameters 按钮进行构建操作
9. 假设构建成功，则可以访问 http://test.pod1.abssqr.cn:8010，此时就能看到前端页面了

### nginx 代理

0. 找 @韩飞 登录阿里云配置一个域名：如 abssqr-dev.abssqr.cn
1. 登录代理服务器：ssh -p3049 juedui@116.62.71.26，密码询问 @韩飞
2. 访问目录：/usr/local/nginx/conf/conf.d
3. 复制已经在使用的的配置文件，如：formula-dev.conf

	```
	命令：cp <目标文件> <生成文件>
	$ cp formula-dev.conf abssqr-dev.conf
	```
4. 修改配置文件：abssqr-dev.conf
	
	```
	$ vim abssqr-dev.conf
	
	// 修改文件内容
	server_name  <阿里云配置的域名>;

	location / {
	    proxy_pass http://test.pod1.abssqr.cn:<前端端口号>;
	}
	location /<约定代理字符> {
	    proxy_pass http://test.pod1.abssqr.cn:<后端端口号>;
	}
	```
5. 重启 nginx
	- 切换到路径：/usr/local/nginx
	- 运行：./sbin/nginx -s reload

### 后端部署遇到的问题

1. 启动类不存在，请检查启动类名称

	```
	根目录下 pom.xml 中 ruiyi.auto.test.version 的版本号未升级
	```

2. dockerfile 和 yml 的端口不一致导致请求 500
