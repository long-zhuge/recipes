## redis

> 版本号：5.0.4

### 能外部链接访问

> 由于 reids 默认是不允许外部链接的，只能本地访问

1. 登录远程服务器后，找到 redis 目录
    - 可能在 /usr/local/share 目录下
2. 找到 redis.conf 文件，使用 vim 命令进行编辑
3. 注释 bind 127.0.0.1
4. 修改 protected-mode yes 为 protected-mode no
5. 修改 requirepass password，其中 password 为你要设置的密码
    - 此段配置大概在配置文件的 507 行样子，不同版本的可能不一样
6. 关闭 redis，如果已经启动的话
    - ps -ef | grep redis，获取 pid，假设是 15496
    - kill -9 15496
7. 重新启动
    - 在 redis 目录的 src 目录下
    - 启动命令：`./redis-server ../redis.conf &`
        - redis.conf 配置文件的路径必须要有

### 目录结构

```
- redis-5.0.4
|   - redis.conf
|   - src
|       - redis-server
|       - redis-cli
```
