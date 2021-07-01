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

// 停止
$  nginx -s stop
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

// 修改，使其可以远程登录
daemonize=yes

// 修改密码
requirepass 123456

// 保存后重启
$ ./src/redis.server &
```

- 报错：DENIED Redis is running in protected mode because protected mode is enabled

```
// 修改 redis.conf 的第88行
protected-mode yes 改为 no
```

- 报错：server.c:5166:39: error: ‘struct redisServer’ has no member named ‘maxmemory’
    - 原因：gcc 版本太低
    
```
// 查看版本
$ gcc -v

// 升级至6.0+
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
 
scl enable devtoolset-9 bash
```

#### 安装 minio

[文档](http://docs.minio.org.cn/docs/)

安装

```
// 下载
$ cd /opt
$ wget http://dl.minio.org.cn/server/minio/release/linux-amd64/minio
$ chmod +x minio

// 设置后台启动
$ vim minio.sh

// 写入，保存。假设存储到 /file 目录下
export MINIO_ROOT_USER=admin
export MINIO_ROOT_PASSWORD=password
nohup /opt/minio server /file/minio > minio.log 2>&1 &

// 修改脚本权限
$ chmod +x minio.sh

// 启动
$ ./minio.sh

// 访问，需要开放防火墙和安全组，默认端口号 9000
http://localhost:9000
```

设置永久访问链接

> 由于 minio 的 share 只有 7天 有效时间，想要设置永久的，需要下载 mc 进行设置

```
// 下载
$ cd /opt
$ wget http://dl.minio.org.cn/client/mc/release/linux-amd64/mc

// 修改脚本权限
$ chmod +x mc

/*
 * 添加配置，命令中的参数解释如下：
 *   minio：当前 minio 安装的路径，如果 mc 和 minio 在同路径下，这如下命令即可
 *   http://localhost:9000：表示公网访问地址
 *   admin：minio 账号
 *   password：minio 密码
 */ 
$ ./mc config host add minio http://localhost:9000 admin password --api S3v4

// 设置指定桶的访问权限
$ ./mc policy set download minio/cdn

// 在 minio 客户端的 cdn 桶下上传 helloword.png 后，可使用链接访问：
http://localhost:9000/cdn/helloword.png
```

创建新用户并赋予权限

```
// 需要cd到 mc 所在的目录，执行以下命令后，输入 “用户名、密码”，比如我们创建了一个 dev 的用户
$ ./mc admin user add minio

// 创建配置文件，如：dev.json，内容如下
{
  "Version": "2012-10-17", // 该字段固定，请不要修改
  "Statement": [
    {
      "Action": [
        "s3:ListBucket",
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::cdn/*" // 表示拥有 /cdn 目录下的增删改权限
      ],
      "Sid": ""
    }
  ]
}

// 对 minio 写入上面配置文件并生成规则，如下生成了一个 devrule 的规则
$ ./mc admin policy add minio devrule dev.json

// 将权限规则赋予用户
$ ./mc admin policy set minio devrule user=dev
```
