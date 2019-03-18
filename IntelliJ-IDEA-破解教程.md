## IntelliJ IDEA 破解教程

> 教程环境支持 mac 系统，如果是 windows 系统，在查看相关文件时请自行 google。

- [客户端下载](https://www.jetbrains.com/webstorm/)
- [注册码激活](http://idea.lanyus.com/)

### 修改 hosts 文件

```
##
# WebStorm 激活 hosts 地址
##
0.0.0.0 account.jetbrains.com
```

- 非命令方式
	1. 打开 Finder
	2. 使用 command+shift+g 打开文件搜索，并输入 etc/hosts
	3. 打开定位到的 hosts 文件
		- 提示无权限修改该文件时，将 hosts 文件 copy 到桌面进行修改，完成后复制到 etc 目录下即可，此时需要输入开机密码。

- 终端命令方式
	1. 打开系统自带终端(也可以是 iTerm)
	2. 输入命令（请忽略 $ 符，终端命令前面自带）
		- vim ../../etc/hosts 进行回车
		- 如果需要权限密码，请输入开机密码
	3. 打开 hosts 文件后，键盘敲 i 就可以进行编辑了
	4. 将上面的激活地址写入到 hosts 文件中
	5. 按 esc 退出编辑
	6. :wq 命令进行退出保存

### 激活 IDEA

1. 在上面的 “注册码激活” 网址上获取注册码
2. 启动 IDEA，选择 activation code 后输入注册码即可
