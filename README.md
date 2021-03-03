# git --fast-version-control

[toc]

## 1. 快速上手

1. 安装

   Windows版下载地址：https://git-scm.com/download/win

2. git配置

   ```shell
   git config --global user.name "user.name"
   git config --global user.email "user.email"
   ```

3. 获取git仓库

   ```shell
   # 克隆现有仓库
   git clone http://server/path/repo.git
   ```
   
4. 仓库的更新

   ```shell
   # 本地修改添加到暂存区
   git add .
   # 提交到本地仓库
git commit -m 'message'
   # 拉取远程最新数据与本地代码合并
   git pull
   # 推送至远程
   git push
   ```
   

## 2. git教程

### 2.1 git配置

```shell
git config
# ~/.gitconfig 或 ~/.config/git/config 只针对当前用户
git config --global
# .git/config 只针对该仓库
git config --local

git config --global user.name "user.name"
git config --global user.email "user.email"

git config --list

```

### 2.2 获取git仓库

> ## 生成 SSH 公钥
>
> ```shell
> # ~/.ssh
> ssh-keygen -o
> ```

```shell
# 初始化仓库
git init

# 克隆现有仓库
git clone user@server:path/to/repo.git <repoName>
```

### 2.3 仓库的更新

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

### 2.4 查看提交历史

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

### 2.5 撤消操作

```shell
# 不是原位替换旧有提交，旧有的提交将不会存在仓库历史中
git commit --amend -m 'message'

# 取消暂存的文件
git reset HEAD <filename>

# 撤消对文件的修改
git checkout -- <filename>

# 拉取服务器指定版本提交
git reset --hard SHA-1

# 拉取服务器最近一次提交（倒数第二次）
git reset --hard HEAD^
```

### 2.6 远程仓库的使用

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

### 2.7 标签

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

### 2.8 分支

#### 2.8.1 分支简介

```shell
git add README test.rb LICENSE
git commit -m 'The initial commit of my project'
```

![commit-and-tree](.\images\commit-and-tree.png)

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



![head-to-master](.\images\head-to-master.png)

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

#### 2.8.2 分支的新建与合并

实际工作中可能遇到下面类似的工作情况。 步骤如下：

1. 开发某个网站。
2. 为实现某个新的用户需求，创建一个分支。
3. 在这个分支上开展工作。

正在此时，你突然接到一个电话说有个很严重的问题需要紧急修补。 你将按照如下方式来处理：

1. 切换到你的线上分支（production branch）。
2. 为这个紧急任务新建一个分支，并在其中修复它。
3. 在测试通过之后，切换回线上分支，然后合并这个修补分支，最后将改动推送到线上分支。
4. 切换回你最初工作的分支上，继续工作。

##### 2.8.2.1 新建分支

![basic-branching-1](.\images\basic-branching-1.png)

<center>图2.8.2.1-1：一个简单提交历史</center>

```shell
git checkout -b iss53
```

![basic-branching-2](.\images\basic-branching-2.png)

<center>图2.8.2.1-2：创建一个新分支指针</center>

```shell
git commit -a -m 'added a new footer [issue 53]'
```



![basic-branching-3](.\images\basic-branching-3.png)

<center>图2.8.2.1-3：iss53 分支随着工作的进展向前推进</center>

```shell
git checkout master
git checkout -b hotfix
git commit -a -m 'fixed the broken email address'
```



![basic-branching-4](.\images\basic-branching-4.png)

<center>图2.8.2.1-4：基于 master 分支的紧急问题分支 hotfix branch</center>

```shell
git checkout master

# 将 hotfix 分支合并回你的 master 分支
# 快进（fast-forward）
git merge hotfix
```

![basic-branching-5](.\images\basic-branching-5.png)

<center>图2.8.2.1-5：master 被快进到 hotfix</center>

```shell
git checkout iss53
git commit -a -m 'finished the new footer [issue 53]'
```

![basic-branching-6](.\images\basic-branching-6.png)

<center>图2.8.2.1-6：继续在 iss53 分支上的工作</center>

##### 2.8.2.2 分支的合并

```shell
git checkout master
git merge iss53
```

![basic-merging-1](.\images\basic-merging-1.png)

<center>图2.8.2.2-1：合并中所用到的三个快照</center>

![basic-merging-2](.\images\basic-merging-2.png)

<center>图2.8.2.2-2：一个合并提交</center>

```html
# 遇到冲突时的分支合并
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

#### 2.8.3 分支管理

```shell
# 查看所有分支
git branch

# 查看每一个分支的最后一次提交
git branch -v

# 查看哪些分支已经合并到当前分支
git branch --merged

# 查看所有包含未合并工作的分支
git branch --no-merged

# 查看其它分支的合并状态
git checkout testing
git branch --no-merged master
```

> 这些分支全部都存于本地。 当你新建和合并分支的时候，所有这一切都只发生在你本地的 Git 版本库中 —— 没有与服务器发生交互。

#### 2.8.4 远程分支

```shell
# 查看远程分支
git ls-remote <remote>
```

![lr-branches-2](.\images\remote-branches-1.png)

<center>图2.8.4-1：克隆之后的服务器与本地仓库</center>

![lr-branches-2](.\images\remote-branches-2.png)

<center>图2.8.4-2：本地与远程的工作可以分叉</center>



```shell
# 远程仓库同步数据
git fetch <remote>
```

![lr-branches-2](.\images\remote-branches-3.png)

<center>图2.8.4-3：git fetch 更新你的远程跟踪分支</center>

```shell
# 添加一个新的远程仓库引用到当前的项目
git remote add
```

![lr-branches-2](.\images\remote-branches-4.png)

<center>图2.8.4-4：添加另一个远程仓库</center>

```shell
# 抓取远程仓库 teamone 有而本地没有的数据
git fetch teamone
```

![lr-branches-2](.\images\remote-branches-5.png)

<center>图2.8.4-5：远程跟踪分支 teamone/master</center>

```shell
# 推送到有写入权限的远程仓库上
git push <remote> <branch>
git fetch <remote>
git merge <remote> <branch>
git checkout -b <branch> <remote>/<branch>
```

> ### 跟踪分支
>
> 从一个远程跟踪分支检出一个本地分支会自动创建所谓的“跟踪分支”（它跟踪的分支叫做“上游分支”）。

```shell
git checkout --track <remote>/<branch>

# <branch> 本地不存在且远程分支唯一匹配 
git checkout <branch>

# 本地分支别名
git checkout -b <newbranch> <remote>/<branch>

# 本地分支重命名
git branch -u <remote>/<branch>

# 查看设置的所有跟踪分支
git branch -vv
git fetch --all;git branch -vv

# 拉取
git fetch
git merge

# 查找当前分支所跟踪的服务器与分支， 从服务器上抓取数据然后尝试合并入那个远程分支
git pull

# 删除远程分支
git push origin --delete <branch>
```

### 2.9. 子模块

#### 2.9.1 添加子模块

```shell
# URL: 想要跟踪的项目的相对或绝对路径
git submodule add URL <submodulename>

git status
git diff --cached --submodule
git commit -am 'message'
git push <remote> <branch>
```

> 虽然 `submodule` 是工作目录中的一个子目录，但 Git 还是会将它视作一个子模块。当你不在那个目录中时，Git 并不会跟踪它的内容， 而是将它看作子模块仓库中的某个具体的提交

#### 2.9.2 克隆含有子模块的项目

```shell
# 克隆一个含有子模块的项目 #1
git clone url <repoName>

# 初始化本地配置文件 #2
git submodule init
# 从该项目中抓取所有数据并检出父项目中列出的合适的提交 #3
git submodule update

# 效果同#2#3
git submodule update --init
git submodule update --init --recursive

# 效果同#1#2#3
git clone url <repoName> --recurse-submodules
```

#### 2.9.3 在包含子模块的项目上工作

```shell
# 从子模块的远端拉取上游修改
git fetch
git merge
git diff --submodule

# 效果同 git fetch;git merge
git submodule update --remote <submodulename>

# 修改子模块跟踪仓库的其它分支，-f:在仓库中保留跟踪信息
git config -f .gitmodules submodule.submodulename.branch branchname
```

```shell
# 从项目远端拉取上游更改， --init:新子模块 --recursive:子模块有嵌套的子模块
git submodule update --init --recursive

# 将新的 URL 复制到本地配置中
git submodule sync --recursive
```

```shell
# 在子模块上工作
# /project/submodulename/
git checkout -b branchname
git commit -am 'message'
# /project/
git submodule update --remote --merge
```

```shell
# 发布子模块改动
# /project/submodulename/
git commit -am 'message'
git push
# /project/
git commit -am 'message'
git push

git push --recurse-submodules=check
git push --recurse-submodules=on-demand
```

#### 2.9.4 子模的块技巧

```shell
# 子模块遍历
git submodule foreach 'git stash'
git submodule foreach 'git checkout -b branchname'
git diff; git submodule foreach 'git diff

# 别名配置
git config alias.sdiff '!'"git diff && git submodule foreach 'git diff'"
git config alias.spush 'push --recurse-submodules=on-demand'
git config alias.supdate 'submodule update --remote --merge'
```

## 3. 项目开发规范

### 3.1 分支开发工作流

![branch-work](.\images\branch-work.png)

<center>图3.1-3：git 多分支流程图</center>

### 3.2 多流并行开发

**参考资料：**

- [git官方文档 https://git-scm.com/book/en/v2](https://git-scm.com/book/en/v2)