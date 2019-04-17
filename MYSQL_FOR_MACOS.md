## MYSQL FOR MACOS

### 安装

1. 打开终端，查看 brew 是否存在，如果不存在，请安装，如下：
	- ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

2. brew 安装 mysql
	- brew install mysql

### 环境变量设置

- 在 mac 下，环境变量位于 /etc/profile 文件中
- 如何添加环境变量，请看以下事例：

```
// 以添加 mysql 为例

1. 打开终端
2. $ vi ~/.bash_profile
3. export PATH=/usr/local/mysql/bin:$PATH (输入)
4. :wq (保存并退出）
5. $ source ~/.bash_profile (立即生效)
6. $ echo $PATH (查看环境变量)
```

### 启动／停止

```
// 启动
$ mysql.server start

// 停止
$ mysql.server stop
```

### mysql 登录／退出

|命令|简写|描述|
|:--:|:--:|:--:|
|--version|-V|查看版本信息|
|--database=name|-D|打开指定数据库|
|--host=name|-h|服务器名称，默认为本地 127.0.0.1|
|--port=#|-P|端口号，默认3306|
|--user=name|-u|用户名|
|--password=name|-p|密码|

```
// 登录
$ mysql -uroot -p -P3306 -h127.0.0.1
Enter password: *****

// 成功后
mysql>

// 退出有三个命令
mysql> exit;
mysql> quit;
mysql> \q;
```

### 忘记密码

- ps: 只其中比较暴力的方式，其他方式请 google

```
// 打开终端，键入
$ mysqld_safe --skip-grant-tables&

// 新开终端(无需密码)
$ mysql -u -root
mysql>

// 执行下面命令为了能够修改任意的密码
mysql> FLUSH PRIVILEGES;

// 重置密码，你可以将‘111111’替换为自己需要的密码
mysql> SET PASSWORD FOR root@'localhost' = PASSWORD('111111');

// 重新登录，请将原有的终端mysql退出
$ mysql -uroot -p
Enter password: *****
Welcome to the MySQL balabala
mysql>
```

----

## mysql 操作

#### FOREIGN_KEY_CHECKS

> 外键属性，指关联数据表是否允许被删除。A 和 B 是关联表，如果 A 有 B 相关的关联数据，那么 B 不允许被删除。默认禁用外键，允许删除。

```
SET FOREIGN_KEY_CHECKS = 0; // 禁用外键约束，允许删除
SET FOREIGN_KEY_CHECKS = 1; // 启动外键约束，不允许删除
SELECT  @@FOREIGN_KEY_CHECKS; // 查看当前外键约束值
```

#### 显示所有数据库 (SHOW DATABASES)

```
mysql> SHOW DATABASES;
// 此时会显示 mysql自带的几个数据库：
```

#### 创建数据库 (CREATE DATABASE)

- 语法

```
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name

// 解释
{}：中为必选其一
[]：中的表示可选
db_name：为需要命名的数据库名称
charset_name：为编码方式
```

- 创建

```
mysql> CREATE DATABASE db_test;
Query OK, 1 row affected (0.01 sec)

// 查看数据库编码方式
mysql> SHOW CREATE DATABASE db_test;

// 创建指定编码方式的数据库
mysql> CREATE DATABSE IF NOT EXISTS db_test2 CHARACTER SET gbk;

// 修改编码方式
mysql> ALTER DATABASE db_test2 CHARACTER SET utf8;
```

- 打开数据库

```
// 必须是已存在数据库
mysql> USE db_test2
Database changed

// 确认是否打开了数据库
mysql> SELECT DATABASE();
```

#### 删除数据库 (DROP)

- 语法

```
DROP {DATABASE | SCHEMA} [IF EXISTS] db_name
```

- 删除

```
mysql> DROP DATABASE db_test2;
Query OK, 0 rows affected (0.02 sec)
```

#### 数据类型

|类别|数据类型|存储范围|字节|
|:--:|:--|:--|:--:|
|`整型`||||
||TINYINT|0-255|1|
||SMALLINT|0-65535|2|
||MEDIUMINT|0-16777215|3|
||INT|0-4294967295|4|
||BIGINT|0-2E+64-1|8|
|`浮点型`||||
||FLOAT[M,D]|M是总位数，D是小数点后位数||
||DOUBLE[M,D]|||
|`日期`||||
||YEAR|||
||TIME|||
||DATE|||
||DATETIME|||
||TIMESTAMP|||
|`字符型`||||
||CHAR(M)|0<=M<=255|M|
||VARCHAR(M)|L<=M && 0<=M<=65535|L+1|
||TINYTEXT|L<2^8|L+1|
||TEXT|L<2^16|L+2|
||MEDIUMTEXT|L<2^24|L+3|
||ENUM('value1','value2',...)|枚举，65535个|1或2|
||SET('value1','value2',...)|64个成员|1、2、3、4或8|

#### 数据表

- 创建数据表

```
CREATE TABLE [IF NOT EXISTS] table_name(
	column_name data_type,
	...
)

// demo
mysql> CREATE TABLE tb1(
    -> username VARCHAR(20),
    -> age TINYINT UNSIGNED, // UNSIGNED表示不为负数
    -> salary FLOAT(8,2) UNSIGNED
    -> );
```

- 查看数据表

```
// 查看某一个数据库
mysql> SHOW TABLES FROM db_name;

// 查看当前数据库
mysql> SHOW TABLES;

// 查看数据表结构
mysql> SHOW COLUMNS FROM tb1;
```

- 插入数据

```
INSERT [INTO] tbl_name [col_name,...] VALUES(value,...)

// demo, 全部字段，缺少一个会报错
mysql> INSERT tb1 VALUES('ANDY', 30, 8888.88);

// 插入某一个字段
mysql> INSERT tb1(username) VALUES('TOM');
```

- 查看记录

```
SELECT expr,... FROM tb1_name

// demo
mysql> SELECT * FROM tb1;
```

- 空值于非空
	- NULL，字段值可以为空
	- NOT NULL，字段不可空

```
// demo
mysql> CREATE TABLE tb2(
    -> username VARCHAR(20) NOT NULL,
    -> age TINYINT UNSIGNED NULL, // NULL 可以不写，默认为null
    -> );
```

- 自动编号 (AUTO_INCREMENT)
	- 必须和主健组合使用
	- 默认情况下，起始值为1，增量为1
	- PRIMARY KEY
		1. 主键约束
		2. 每张数据表只能存在一个主键
		3. 主键保证记录的唯一性
		4. 主键自动为NOT NULL
	- UNIQUE KEY
		1. 唯一约束
		2. 唯一约束可以保证记录的唯一性
		3. 唯一约束的字段可以为空值（null）
		4. 每张数据表可以存在多个唯一约束
	- DEFAULT
		1. 默认值
		2. 当插入数据时，没有明确为字段赋值，则自动赋值默认值

```
// demo
mysql> CREATE TABLE tb3(
    -> id SMALLINT UNSIGNED AUTO_INCREMENT KEY,
    -> username VARCHAR(30) NOT NULL
    -> );
    
// 插入数据
mysql> INSERT tb3(username) VALUES('ANDY');
mysql> INSERT tb3(username) VALUES('CANDY');
mysql> INSERT tb3(username) VALUES('TOM');

// 查看id
mysql> SELECT * FROM tb3;

// 建表时，key 可以不为 AUTO_INCREMENT，则 key 可以自定义写入，但是不能重复
mysql> CREATE TABLE tb4(
    -> id SMALLINT UNSIGNED KEY,
    -> username VARCHAR(30) NOT NULL
    -> );
mysql> INSERT tb4 VALUES('1', 'ANDY');
mysql> INSERT tb4 VALUES('1', 'TOM');
ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'

// demo [DEFAULT]
mysql> CREATE TABLE tb5(
    -> id SMALLINT UNSIGNED AUTO_INCREMENT KEY,
    -> username VARCHAR(30) NOT NULL,
    -> sex ENUM('man', 'weman', 'renyao') DEFAULT 'weman'
    -> );
    
// 查看
mysql> SHOW COLUMNS FROM tb5;
```

### 数据库用户查询和新增

```
// 查询能操作 mysql 的用户
mysql> select user,host form mysql.user;

+-----------+-----------+
| user      | host      |
+-----------+-----------+
| admin     | %         |
| foodpanel | %         |
| mysql.sys | localhost |
| root      | localhost |
+-----------+-----------+

// 查询 foodpanel 用户能操作的数据库
mysql> show grants for foodpanel;

+-----------------------------------------------------------+
| Grants for foodpanel@%                                    |
+-----------------------------------------------------------+
| GRANT USAGE ON *.* TO 'foodpanel'@'%'                     |
| GRANT ALL PRIVILEGES ON `foodpanel`.* TO 'foodpanel'@'%'  |
| GRANT ALL PRIVILEGES ON `rdassist`.* TO 'foodpanel'@'%'   |
| GRANT ALL PRIVILEGES ON `hanquality`.* TO 'foodpanel'@'%' |
+-----------------------------------------------------------+

// 将 foodpanel 用户添加到 xxx 数据库中
mysql> grant all privileges on <数据库名>.* to 'foodpanel'@'%';
```