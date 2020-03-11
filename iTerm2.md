> 在 ssh 连接远程服务器时，中文会乱码，这是因为本地 iterm2 和远程服务器的字符集不同，一般远程的 linux 服务器的字符集是 UTF-8。

查看 iterm2 配置，打开终端，输入以下命令：

```
$ locale

// 显示
LANG=
LC_COLLATE="C"
LC_CTYPE="UTF-8"
LC_MESSAGES="C"
LC_MONETARY="C"
LC_NUMERIC="C"
LC_TIME="C"
LC_ALL=
```

添加配置

```
$ vim ~/.zshrc

// 添加，并保存退出
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8

// 生效设置
$ source ~/.zshrc
```
