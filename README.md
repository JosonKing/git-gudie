# git --fast-version-control

## 1. 快速上手

### 1.1 安装(Windows)

https://git-scm.com/download/win

### 1.2 git配置

```shell
git config
# ~/.gitconfig 或 ~/.config/git/config 只针对当前用户
git config --global
# .git/config 只针对该仓库
git config --local

git config --global "user.name"
git config --global "user.email"

git config --list

```

### 1.3 获取git仓库

```shell
# 初始化仓库
git init

# 克隆现有仓库
git clone user@server:path/to/repo.git <repoName>
```

### 1.4 仓库的更新

![areas](.\images\areas.png)

![lifecycle](.\images\lifecycle.png)

```shell
# 检查当前文件状态
git status

# 将内容添加到下一次提交中
git add <filename>

# 暂存前后的文件差异
git diff

# 已暂存文件与最后一次提交的文件差异
git diff --staged

# 提交更新
git commit -m 'message'
# 跳过使用暂存区域
# 自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤
git commit -a -m 'message'

# 移除文件
git rm <filename>
git rm -f <filename>
git rm --cached <filename>

# 移动文件
# mv README.md README
# git rm README.md
# git add README
git mv file_from file_to

# 忽略文件
.gitignore
```

### 1.4 查看提交历史

```shell
git log

# 每次提交所引入的差异，并限制显示的日志条目数量
git log -p -n

# 每次提交的简略统计信息
git log --stat

# 仅显示作者匹配指定字符串的提交
git log --author='user.name'

```

> **隐藏合并提交** --no-merges 

### 1.5 撤消操作

```shell
# 不是原位替换旧有提交，旧有的提交将不会存在仓库历史中
git commit --amend -m 'message'

# 取消暂存的文件
git reset HEAD <filename>

# 撤消对文件的修改
git checkout -- <filename>
```

### 1.6 远程仓库的使用

```shell
# 查看远程仓库
git remote
git remote -v

# 添加远程仓库
git remote add <shortname> <url>

# 从远程仓库中抓取与拉取
git fetch <remote>

# 推送到远程仓库
git push <remote> <branch>

# 查看某个远程仓库
git remote show <remote>

# 远程仓库的重命名与移除
git remote rename <newname>
git remote remove <remote>

```

### 1.7 标签

```shell
# 列出标签
git tag

# 创建标签
git tag -a v1.0 -m 'message'
git tag v1.1-lw

# 后期打标签
git tag -a v2.0 SHA-1 -m 'message'

# 共享标签
git push <remote> <tagname>
git push <remote> --tags

# 删除标签
git tag -d <tagname>
git push <remote> --delete <tagname>

# 检出标签
git checkout <tag>
git checkout -b version2 <tag>
```

## 2 分支

### 2.1 分支简介

```shell
git add README test.rb LICENSE
git commit -m 'The initial commit of my project'
```

![commit-and-tree](C:\code\github\git-gudie\images\commit-and-tree.png)

<center>图2.1-1：首次提交对象及其树结构</center>

![commits-and-parents](.\images\commits-and-parents.png)

<center>图2.1-2：提交对象及其父对象</center>



![branch-and-history](.\images\branch-and-history.png)

<center>图2.1-3：分支及其提交历史</center>

```shell
# 分支创建
git branch testing

#查看各个分支当前所指的对象
git log --oneline --decorate
```

![two-branches](.\images\two-branches.png)



<center>图2.1-4：两个指向相同提交历史的分支</center>



![head-to-master](C:\code\github\git-gudie\images\head-to-master.png)

<center>图2.1-5：HEAD 指向当前所在的分支</center>

```shell
# 分支切换
git checkout testing
```

![head-to-testing](.\images\head-to-testing.png)

<center>图2.1-6：HEAD 指向当前所在的分支</center>

```shell
# testing 分支提交
git commit -a -m 'made a change'
```

![advance-testing](.\images\advance-testing.png)

<center>图2.1-7：HEAD 分支随着提交操作自动向前移动</center>

```shell
# 切换到 master 分支
git checkout master
```

![advance-master](.\images\checkout-master.png)

<center>图2.1-7：HEAD 分支随着提交操作自动向前移动</center>



```shell
# master 分支提交
git commit -a -m 'made other changes'
```

![advance-master](.\images\advance-master.png)

<center>图2.1-8：项目分叉历史</center>

```shell
# 提交历史及各个分支分叉历史
git log --oneline --decorate --graph --all

# 创建新分支的同时切换过去，
git checkout -b <newbranchname>
```

## 2.2 分支的新建与合并

实际工作中可能遇到下面类似的工作情况。 步骤如下：

1. 开发某个网站。
2. 为实现某个新的用户需求，创建一个分支。
3. 在这个分支上开展工作。

正在此时，你突然接到一个电话说有个很严重的问题需要紧急修补。 你将按照如下方式来处理：

1. 切换到你的线上分支（production branch）。
2. 为这个紧急任务新建一个分支，并在其中修复它。
3. 在测试通过之后，切换回线上分支，然后合并这个修补分支，最后将改动推送到线上分支。
4. 切换回你最初工作的分支上，继续工作。

#### 2.2.1 新建分支

```shell
git checkout -b iss53
```

```shell
git commit -a -m 'added a new footer [issue 53]'
```