## 如何在 github 上进行开源项目的 PR

> 本文以 ant-design 项目为例子进行讲解。<br/>
> 本文默认您已经会使用 git，并进行了相关账号的申请和秘钥的绑定。

### Fork

1. 打开 [ant-design](https://github.com/ant-design/ant-design)
2. 点击右上角的 `Fork` 按钮，此时会跳转至自己的主页，并在自己的项目中有了一个 antd 的复制项目。

### clone

1. 将自己 fork 的 antd 项目克隆到本地

	```
	$ git clone git@github.com:<xxxxx>/ant-design.git
	```

2. 查看本地连接的远程仓库，会发现并没有 antd 官方仓库的连接

	```
	$ git remote -v
	
	origin  git@github.com:<xxxxx>/ant-design.git (fetch)
	origin  git@github.com:<xxxxx>/ant-design.git (push)
	```
	
3. 新增官方仓库的连接

	```
	$ git remote add upstream git@github.com:ant-design/ant-design.git
	
	// 再次查看连接
	$ git remote -v
	
	origin  git@github.com:long-zhuge/ant-design.git (fetch)
	origin  git@github.com:long-zhuge/ant-design.git (push)
	upstream  git@github.com:ant-design/ant-design.git (fetch)
	upstream  git@github.com:ant-design/ant-design.git (push)
	```
	
4. 更新代码，在 fork 后我们自己仓库中的项目，是不会同步官方仓库的更新的，需要我们手动进行更新。

	```
	// 更新远程官方仓库代码
	$ git fetch upstream
	
	// 合并到本地 master
	$ git merge upstream/master
	```
	
### 提交代码并 PR

1. 将自己本地修改的代码提交到自己的仓库中
2. 点击 `New pull request`
3. 右边选择自己仓库的分支，左边选择需要合并的官方仓库分支
4. 写好注释以及 changelog 后进行提交