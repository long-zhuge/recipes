### git命令

#### git init
	在目录中创建git仓库
	
#### git clone
	克隆git仓库到本地
	克隆git仓库某分支到本地：git clone 仓库地址 -b 分支名
	克隆git仓库到本地并重命名：git clone 仓库地址 <文件名>
	
#### git add .
	讲本地文件添加到缓存
	
#### git status
	查看项目的当前状态
	
#### git diff
	1. 显示已经写入缓存的改动
	2. 显示修改当未写入缓存的改动
	
#### git commit
	将git add命令的快照内容添加到仓库中
	
#### git commit --amend

	撤销最近一次的提交

#### git reset：回滚

```
// 回滚上一个版本
git reset --hard HEAD^

// 回退到前 n 次的提交
git reset --hard HEAD~3

// 回退/前进 到指定的 commit id
git reset --hard <commit_id>

// 强推送到远程仓库
git push origin HEAD --force
```

#### git rm
	移除缓存区文件
	如：git rm index.txt(删除文件，目录中)
	
#### git mv
	重命名文件

#### git branch
	查看分支
	创建分支：git branch xxx [创建名称为xxx的分支]

#### git checkout
	切换分支：git checkout master［将分支切换到master］
	创建分支并切换到该分支：git checkout -b xxx
	创建分支并切换到远程分支：git checkout -b AAA origin/分支名
	
#### git push

```
// 远程已有分支并且已经关联本地分支且本地已经切换到该分支
git push

// 远程已有分支但未关联本地分支且本地已经切换到该分支
git push -u origin/<分支>

// 远程没有分支，本地创建新分支并推送远程仓库
git push origin <分支>:<分支>

// demo
git push origin power:power
```
	
#### git branch -r

```
查看远程所有的分支信息
```
    
#### git branch -a

```
查看当前所有远程分支信息，前面带*的为当前分支
```
    
#### delete branch

```
// 删除远程分支
git push origin --delete <分支>

// 删除本地分支
git branch -D <分支>
```

#### git merge

```
合并分支：git merge xxx［将xxx分支合并到master，假设当前分支为master］
```
	
#### git log
	查看提交历史
	查看历史记录到简洁版本：git log --oneline
	查看历史中什么时候出现分支，合并等：git log --graph
	
#### git remote
	查看当前配置有哪些远程仓库
	
#### git pull
	从远程仓库提取数据并合到当前分支

#### git push
	将新分支和数据上传到远程仓库
	如：git push origin master

#### git remote rm
	删除远程仓库：git remote rm xxx

#### git remote add
	添加仓库：git remote add xxx
	
#### fetch
	拉取远程分支代码：git fetch origin master:master
	备注：拉取远程master代码到本地，并命名为master，然后通过checkout切换

#### 常用提交命令
	git add .&&git commit -m "xxx"&&git push
	
#### git stash

- 可用来暂存当前正在进行的工作， 比如想pull 最新代码， 又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作。
- 场景：merge报错，error: Your local changes to the following files would be overwritten by merge: ${xxxxxxx} Please, commit your changes or stash them before you can merge.

```
// 操作命令
git stash
git pull
git pop

// 当多次执行 stash 时，可查看列表
git stash list
```

#### git tag

```
// 打标记
git tag v1.0.0

// 打标记 + 注释
git tag -a v1.0.0 -m "version 1.0.0"

// 查看 tag 列表
git tag

// 删除远程 tag
git push origin :refs/tags/<tagName>

// 删除本地 tag
git tag -d <tagName>

// push tag
git push origin v1.0.0

// push tag list
git push origin --tags

// 拉取 tag 分支
git clone --branch v1.0.0 <url>
```

#### mirror

- 仓库迁移，以镜像方式迁移

```
A 为原始仓库，B为新建仓库，现在需要将 A 仓库文件代码迁移到 B仓库，切保留所有的 commit branch tags 等。

// 在 A 仓库下，执行如下命令
git push --mirror <B仓库的SSH地址>
```

#### config

- 查看文件

```
mac 根目录下执行
vim .gitconfig
```

- 文件区分大小写：false = 区分大小写，true
	- false：区分大小写
	- true：不区分大小写
	
	```
	git config core.ignorecase false
	```