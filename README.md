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


# 忽略文件
.gitignore
```