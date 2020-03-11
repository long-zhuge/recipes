## Maven 私服搭建

- 环境：CentOS7.3 64位
- 操作系统：Linux
- nexus压缩包：nexus-3.13.0-01-unix.tar.gz

### 安装

1. 先将压缩包拷贝(sftp)到服务器的 /opt 目录下，这里的目录无所谓，我一般将 java 相关的东西都放在 opt 下，方便管理
2. 解压到当前目录，也就是 /opt 下
    - tar -zxvf nexus-3.13.0-01-unix.tar.gz

### 启动和关闭

```
// 移动到
$ cd /opt/nexus-3.13.0-01/bin/

// 启动
// 注意，启动后需要过一会才能访问到页面，时间可能较久
$ ./nexus start

// 关闭
$ ./nexus stop
```

### 浏览器访问

> 默认端口号是 8081，请在云主机的安全组中添加 `8081` 端口的出方向规则，或者本机的防火墙。

- http://ip:8081
- 账号：admin
- 密码：admin123

### 将 proxy 修改为阿里云

- 访问 `http://ip:8081/#admin/repository/repositories:maven-central`
- 将 Proxy 的 Remote storage 修改为 `http://maven.aliyun.com/nexus/content/groups/public` 后，点击保存

### 警告解决

#### Detected execution as "root" user.  This is NOT recommended!

- 将启动脚本 nexus 中的 run_as_root=true 改为 run_as_root=false
    - vi /opt/nexus-3.13.0-01/bin/nexus
- 保存文件后退出，并重启 nexus 服务
    - ./nexus stop
    - ./nexus start

#### max file descriptors [4096] for elasticsearch process likely too low, increase to at least [65536]

- [官方解决方案](https://help.sonatype.com/repomanager3/system-requirements#SystemRequirements-Linux)

- 修改 limits.conf，如果没有这个文件，就新建一个
    - vi /etc/security/limits.conf，并添加如下代码
    
    ```
    // 其中“nexus”应替换为用于运行存储库管理器的用户ID
    nexus - nofile 65536
    ```
    - 修改保存后，重启 nexus 服务
