## webpack 打包发布一个 npm 包

#### 检查 npm 是否已经安装

```
$ npm --version

// 如果没有安装，请先安装 node，会自带安装 npm
```

#### 新建目录

```
$ mkdir npmDemo
$ cd npmDemo
$ npm init
```
- 执行后会出现一堆需要确认的东西，包括 “name、version” 等等，都回车就ok了

#### 安装 webpack
- 注意，我们这里安装的是 webpack v4 版本，需要再安装 webpack-cli，我们一起安装即可。

```
// 这里可以使用 npm / cnpm(淘宝镜像)
$ npm install --sava-dev webpack webpack-cli
```

#### webpack.config.js

- 根目录下创建，并进行配置

```
const path = require('path');

module.exports = {
  entry:  './src/index.js', // webpack v4 中默认执行 src/index.js 目录，可以不写 entry
  output: {
    libraryTarget: "umd",  // 按照 AMD or CommonJS 规范进行打包
    filename: 'index.js',	// npm 需要在根目录下暴露一个 index.js 文件
    path: path.resolve(__dirname) // 在根目录下创建
  },
};
```

#### 创建 src/index.js

```
const demo = {
  version: '1.0.0',
  text: '这是一个测试包',
};

module.exports = demo;
```

#### 此时目录结构为

```
- npmDemo
  - src
  	- index.js
  - package.json
  - webpack.config.js
```

#### 打包

```
// 命令行运行
$ webpack

// 成功后根目录下会出现一个 index.js 文件
```

----

#### npm 账号注册

- [账号注册](https://www.npmjs.com/)
	- Full Name： 全称
	- Public Email： 账号激活时需要使用，请填写正确的邮箱地址
	- Username：账号
	- Password：密码
	- 订阅周刊：可选
	- 协议：必选

- 邮箱激活
	- 登录邮箱，点击链接就 ok 了

#### 发布前检查

- 由于 npm 包名不允许重复，所以可以先上 npm 官网查看下当前包名称是否已存在
	- 存在：修改 package.json --> name

#### 本机添加 npm 账号

```
$ npm adduser

// 根据提示输入 “账号、密码、邮箱”
```

#### 发布

```
$ npm publish

// 可能会出现以下错误
ERR! no_perms Private mode enable, only admin can publish this module: npmDemo

// 此问题可能出现的原因是，npm 资源地址不对，我们改回 npm 路径
$ npm config set registry http://registry.npmjs.org
$ npm publish
+ <package>@1.0.0

// 此时已经成功发布了一个 npm 包，刷新自己的 npm 页面，会有相应的包存在
```

#### 其他项目引入

```
$ npm install --sava-dev <package>

import demo form '<package>';
console.log(demo);
```

#### 删除 npm 包

- 创建的包默认在 30分钟 内允许删除，之后将不允许删除。该情况发生在 “npm 删包事件” 之后，情况较恶劣。具体情况请自行 google

```
$ npm unpublish
```

#### 淘宝镜像设置

```
$ npm config set registry https://registry.npm.taobao.org/.

// 之后可以使用 cnpm
```