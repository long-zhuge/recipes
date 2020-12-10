## centos软件安装

#### 安装 wget

```
$ yum -y install wget
```

#### 安装 zip/unzip

```
$ yum install -y unzip zip
```

#### 安装 Java

```
// 查看是否有服务
$ jave

// 安装
wget https://linezone-bucket1.oss-cn-hangzhou.aliyuncs.com/jdk1.8.0_191.zip && unzip jdk1.8.0_191.zip -d /usr/local/ && chmod +x /usr/local/jdk1.8.0_191/bin/* && echo 'export JAVA_HOME=/usr/local/jdk1.8.0_191' >> /etc/bashrc  && echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/bashrc && bash && java
```

#### nginx

```
// 安装
& yum install -y epel-release nginx

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
wget https://nodejs.org/download/release/v12.18.4/node-v12.18.4-linux-x64.tar.gz -O /usr/local/node-v12.18.4-linux-x64.tar.gz && tar -xf /usr/local/node-v12.18.4-linux-x64.tar.gz -C /usr/local/ && ln -snf /usr/local/node-v12.18.4-linux-x64/bin/node /usr/bin/ && ln -snf /usr/local/node-v12.18.4-linux-x64/bin/npm /usr/bin/ && ln -snf /usr/local/node-v12.18.4-linux-x64/bin/npx /usr/bin/ && npm install -g pm2 --registry=https://registry.npm.taobao.org && npm install -g cnpm --registry=https://registry.npm.taobao.org && ln -snf /usr/local/node-v12.18.4-linux-x64/bin/pm2 /usr/bin && ln -snf /usr/local/node-v12.18.4-linux-x64/bin/cnpm /usr/bin
```

#### 安装 forever

> node 进程守护

```
npm install forever -g && ln -snf /usr/local/node-v12.18.4-linux-x64/bin/forever /usr/bin
```

#### htop

```
yum -y install epel-release
yum -y install htop
```

```
PID：进行的标识号
USER：运行此进程的用户
PRI：进程的优先级
NI：进程的优先级别值，默认的为0，可以进行调整
VIRT：进程占用的虚拟内存值
RES：进程占用的物理内存值
SHR：进程占用的共享内存值
S：进程的运行状况，R表示正在运行、S表示休眠，等待唤醒、Z表示僵死状态
%CPU：该进程占用的CPU使用率
%MEM：该进程占用的物理内存和总内存的百分比
TIME+：该进程启动后占用的总的CPU时间
COMMAND：进程启动的启动命令名称
```

#### 安装 redis

```
$ wget https://download.redis.io/releases/redis-6.0.8.tar.gz
$ tar xzf redis-6.0.8.tar.gz
$ cd redis-6.0.8
$ make

// 后台启动
$ vim redis.conf

// 修改
daemonize=yes

// 保存后重启启动
$ ./src/redis.server
```
