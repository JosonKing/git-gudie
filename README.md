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

```