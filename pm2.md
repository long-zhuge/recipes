## pm2

> 基于 nodejs 的进程管理，包括守护进程、监控、日志。

### 安装

```
$ npm install pm2 -g
```

### 常用命令

```
// 启动 app.js 应用
pm2 start app.js

// 查看启动列表
pm2 list

// 显示应用程序的信息
pm2 show [app-name]

// 日志
pm2 logs

// 查看某应用的日志
pm2 logs [app-name]

// 显示程序的 CPU 和 内存使用情况
pm2 monit

// 停止所有程序
pm2 stop all

// 停止编号为 0 的程序
pm2 stop 0

// 删除全部
pm2 delete all

// 删除编号为 0 的程序
pm2 delete 0

// mallpc 的启动程序
pm2 start npm --name="buyer-pc" -- run start
```

