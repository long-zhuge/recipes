> 在 ssh 连接远程服务器时，中文会乱码，这是因为本地 iterm2 和远程服务器的字符集不同，一般远程的 linux 服务器的字符集是 UTF-8。

- 首先检查设置是否正确，Preferences => Terminal
  - 看下 Character Encoding 是否为 Unicode(UTF-8)

- 修改 ssh_config 配置
    
    ```
    // 终端输入
    sudo vi /etc/ssh/ssh_config
    
    // 添加代码，并保存退出
    SendEnv LANG LC_ALL=en.US.UTF-8
    ```
