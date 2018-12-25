#### tab键快速创建代码

1. Editor --> Live Templates
2. 如选择 javaScript,右边“＋”，选择“live template”
3. Abbreviation: 输入缩写
4. Description: 输入描述
5. Template text: 输入想要生成的片段
6. Define选择需要的语言

#### 设置代码换行空格

1. Editor --> Code Style
2. 如选择javaScript --> Tabs and Indents

#### 隐藏某个文件，如隐藏 node_modules 文件

1. Editor --> FileTypes
2. 在"lgnore files and folders" 中添加"node_modules;"

#### eslint 手动检测

```
./node_modules/.bin/eslint --ext jsx,js src/{dir}
```

#### eslint 自动检测

常用编辑都支持 ESLint 自动检测代码功能，具体参考：

- VSCode: https://github.com/Microsoft/vscode-eslint
	- 保存时自动修复：配置里增加 `"eslint.autoFixOnSave": true`，详见 https://github.com/Microsoft/vscode-eslint/tree/master/eslint
- WebStorm: https://www.jetbrains.com/help/webstorm/2017.1/eslint.html
	- **注意**将 `ESLint package` 这个路径设置为项目根目录下 `node_modules` 里的 ESLint。
