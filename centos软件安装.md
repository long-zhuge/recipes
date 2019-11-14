## centos软件安装

#### 安装 Java

```
// 查看是否有服务
$ jave

// 安装
wget https://linezone-bucket1.oss-cn-hangzhou.aliyuncs.com/jdk1.8.0_191.zip && unzip jdk1.8.0_191.zip -d /usr/local/ && chmod +x /usr/local/jdk1.8.0_191/bin/* && echo 'export JAVA_HOME=/usr/local/jdk1.8.0_191' >> /etc/bashrc  && echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/bashrc && bash && java
```

#### Nginx

```
// 安装
& yum install -y epel-release nginx@1.12.2

// 查看 nginx 执行包
$ which nginx

// 启动
$ systemctl enable nginx && systemctl start nginx

// 如果删除了 nginx/html 下的文件，在 start 时会报错。删除 nginx.conf 配置中 server 大段  

// 执行 nginx 操作
$ nginx -s reload
```

#### 安装 Node

```
wget https://nodejs.org/download/release/v8.9.4/node-v8.9.4-linux-x64.tar.gz -O /usr/local/node-v8.9.4-linux-x64.tar.gz && tar -xf /usr/local/node-v8.9.4-linux-x64.tar.gz -C /usr/local/ && ln -snf /usr/local/node-v8.9.4-linux-x64/bin/node /usr/bin/ && ln -snf /usr/local/node-v8.9.4-linux-x64/bin/npm /usr/bin/ && ln -snf /usr/local/node-v8.9.4-linux-x64/bin/npx /usr/bin/ && npm install -g pm2 --registry=https://registry.npm.taobao.org && npm install -g cnpm --registry=https://registry.npm.taobao.org && ln -snf /usr/local/node-v8.9.4-linux-x64/bin/pm2 /usr/bin && ln -snf /usr/local/node-v8.9.4-linux-x64/bin/cnpm /usr/bin
```
