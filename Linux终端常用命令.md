### 终端常用命令

|命令名|描述|栗子|
|:---|:---|:---|
|**目录操作**|||
|mkdir|创建目录|mkdir dirname|
|rmdir|删除目录|rm -r -f dist|
|mvdir|移动或重命名目录|mv dir1 dir2|
|cd|改变当前目录|cd dirname，cd /(返回根目录)|
|pwd|显示当前目录的路径名|pwd|
|ls|显示当前目录内容|ls|
|**文件操作**|||
|vi|创建文件|vi a.js<br/>:wq|
|cat|显示或连接文件|cat filename|
|od|显示非文本文件的内容|od -c filename|
|cp|复制文件或目录|cp file1 file2|
|rm|删除文件|rm filename|
|mv|改变文件名或所在目录|mv file1 file2|
|find|使用匹配表达式查找文件|find . -name "*.c" -print|
|file|显示文件类型|file filename|
|**选择操作**|||
|head|显示文件的最初几行|head -20 filename|
|tail|显示文件的最后几行|tail -15 filename|
|cut|显示文件每行中的某些域|cut -f1,7 -d: /etc/passwd|
|colrm|从标准输入中删除若干列|colrm 8 20 file2|
|diff|比较并显示两个文件的差异|diff file1 file2|
|sort|排序或归并文件|sort -d -f -u file1|
|uniq|去掉文件中的重复行|uniq file1 file2|
|comm|显示两有序文件的公共和非公共行|comm file1 file2|
|wc|统计文件的字符数、词数和行数|wc filename|
|nl|给文件加上行号|nl file1 >file2|
|**时间操作**|||
|date|显示系统的当前日期和时间|date|
|cal|显示日历|cal 8 1996|
|time|统计程序的执行时间|time a.out|
|**网络与通信操作**|||
|telnet|远程登录|telnet hpc.sp.net.edu.cn|
|rlogin|远程登录|rlogin hostname -l username|
|rsh|在远程主机执行指定命令|rsh f01n03 date|
|ftp|在本地主机与远程主机之间传输文件||
|rcp|在本地主机与远程主机 之间复制文件|rcp file1 host1:file2|
|ping|给一个网络主机发送 回应请求|ping www.baidu.com|
|mail|阅读和发送电子邮件|mail|
|**Korn Shell 命令**|||
|history|列出最近执行过的命令|history|
|r|重复执行最近执行过的某条命令|r -2|
|alias|给某个命令定义别名|alias del=rm -i|
|unalias|取消对某个别名的定义|unalias del|
|**其他命令**|||
|uname|显示操作系统的有关信息|uname -a|
|clear|清除屏幕或窗口内容|clear|
|env|显示当前所有设置过的环境变量|env|
|who|列出当前登录的所有用户|who|

----

### Mac OS X 终端命令开启功能(终端输入)

#### Lion下显示资源库
```
// 显示
chflags nohidden ~/Library/

// 隐藏
chflags hidden ~/Library/
```

#### Finder显示隐藏文件
```
// 显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles Yes && killall Finder

// 恢复隐藏文件
defaults write com.apple.finder AppleShowAllFiles No && killall Finder
```

#### 在Finder标题栏显示完整路径
```
defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES && killall Finder
```

#### 端口查询及关闭

```
// 查询当前 8000 端口号是否在运行
lsof -i:8000

// 关闭 8000 端口号，11815 为端口号 PID
kill 11815
```

#### 修改文件夹权限

> 假设需要修改 html 文件夹下的权限

```
// 修改为可读取的权限
chmod -R 777 html

// 修改为只读的权限
chmod 666 html

// 查看权限
cd html
ls -l
```

#### 解压 zip 文件

```
$ unzip xxx.zip
```

#### 查看所有端口执行情况

```
$ netstat -tunpl
// or
$ netstat -lanput
```

#### 查看端口 8072 执行情况

```
$ ps -ef | grep 8090
```

#### 获取端口 8072 执行情况的参数

```
执行
$ netstat -lanput | grep 8072
// 打印
tcp  0  0  0.0.0.0:8070  0.0.0.0:*  LISTEN  29692/java

// 获取 29692/java 参数
$ netstat -lanput | grep 8070 | awk '{print $7}'
// 打印
29692/java

// 如果想获取 java 参数
$ netstat -lanput | grep 8070 | awk '{print $7}' | awk -F'/' '{print $2}'
// 打印
java
```

#### 查看 jar 包运行情况

```
$ jps -l
```

#### 杀进程

```
$ Kill -9 <进程号>
```

#### 运行命令，但是不打印信息

```
公式：<命令> > /dev/null 2>&1

// 如
$ git pull > /dev/null 2>&1
```

#### 查看磁盘使用情况

```
// 查看磁盘
$ df -h

// 当前目录文件大小
$ du -sh

// 显示某个目录或文件的大小
$ du -sh httpd

// 显示当前目录下所有文件的大小
$ du -sh ./*
```

#### 查看服务器内存使用情况

```
$ top
```

#### 禁止 adobe 远程服务

```
// 关闭
launchctl unload -w /Library/LaunchAgents/com.adobe.AdobeCreativeCloud.plist

// 开启
launchctl load -w /Library/LaunchAgents/com.adobe.AdobeCreativeCloud.plist
```
#### 查看隐藏文件大小

```
// cd 到对应目录，如：
$ cd /var/lib/jenkins
$ du -sh .[!.]* * | sort -hr
```

#### 服务器免密登录

```
// 需要先登录 A 服务器，在执行下面命令，其中 125.124.21.221 为B服务器
$ ssh-copy-id -p 22 root@125.124.21.221
```

#### 更新服务器时间

```
// 查看
date -R

// 同步
ntpdate pool.ntp.org
```

#### 查看连接数

```
netstat -nat|grep -i "9090"|wc -l
```

#### 定时脚本

> 基本命令

```
// 运行状态
service crond status

// 启动
service crond start

// 停止
service crond stop

// 编辑定时任务
crontab -e

// 查看定时任务
crontab -l

// 删除所有定时任务
crontab -r
```

> 假设 /file/java 目录下有个 test.sh 脚本，内容如下：

```
#!/bin/sh

echo 123 >> testFile  ## 在 testFile 文件中写入 123
```

> 编辑定时任务

```
# 每分钟执行一次。这样执行定时任务时，test.sh 执行生成的 testFile 文件会在 ~ 目录下，并非在 test.sh 的所在目录。
*/1 * * * * /bin/sh test.sh

# 正确写法，此时生成的 testFile 会在 /file/java 目录下
*/1 * * * * cd /file/java;/bin/sh test.sh
```

#### 修改ssh端口

```
// 编辑配置文件
$ vim /etc/ssh/sshd_config

// 添加 Port3049

// 保存退出后
$ systemctl restart sshd.service
```

#### systemctl 如何启动、关闭、启用/禁用服务

```
启动服务：systemctl start xxx.service
关闭服务：systemctl stop xxx.service
重启服务：systemctl restart xxx.service
显示服务的状态：systemctl status xxx.service
在开机时启用服务：systemctl enable xxx.service
在开机时禁用服务：systemctl disable xxx.service
查看服务是否开机启动：systemctl is-enabled xxx.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed
```

#### 防火墙

```
// 查看状态
systemctl status firewalld.service

// 启动
systemctl start firewalld.service

// 关闭
systemctl stop firewalld.service

// 检查防火墙开放的端口：防火墙必须开启
firewall-cmd --permanent --zone=public --list-ports

// 新增端口：需要重启防火墙
firewall-cmd --zone=public --add-port=8080/tcp --permanent

// 重启防火墙
firewall-cmd --reload

// 验证开放的端口是否生效
firewall-cmd --zone=public --query-port=8080/tcp

// 防火墙取消某一开放端口：需要重启防火墙
firewall-cmd --zone=public --remove-port=9200/tcp --permanent
```

#### 黑名单过滤

```
// 查看状态
systemctl status denyhosts.service

// 关闭
systemctl stop denyhosts.service

// 启用
systemctl start denyhosts.service
```

#### centos统计服务器当前IP连接数

```
netstat -tun | awk '{print $5}' | cut -d: -f1 |sort | uniq -c | sort -n
```

ifcfg-lo：开发机下网卡配置文件

```
DEVICE=lo
IPADDR=127.0.0.1
NETMASK=255.0.0.0
NETWORK=127.0.0.0
# If you're having problems with gated making 127.0.0.0/8 a martian,
# you can change this to something else (255.255.255.255, for example)
BROADCAST=127.255.255.255
ONBOOT=yes
NAME=loopback
```

#### centos查看系统版本号

```
cat /etc/redhat-release
```

#### 打包压缩和解压

```
$ tar -zcvf <文件名>.tar.gz <要打包的目录>

// 如
$ tar -zcvf evidence.tar.gz /file/upload/evidence

// 解压
$ tar xzf evidence.tar.gz
```


#### macOS查看 CPU 信息

```
sudo powermetrics
```
