# git命令笔记

## 1. 子模块

[7.11 Git 工具 - 子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

某个工作中的项目需要包含并使用另一个项目，你想要把它们当做两个独立的项目，同时又想在一个项目中使用另一个。

子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。 它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

### 添加子模块
```Shell

# =>>> 添加子模块
git submodule add https://github.com/chaconinc/DbConnector
```

### 克隆含有子模块的项目

```Shell
# 1. 默认会包含该子模块目录，但其中还没有任何子模块文件
git clone https://github.com/chaconinc/MainProject

# 2. 克隆子模块
# 初始化本地配置文件
git submodule init
# 从该项目中抓取所有数据并检出父项目中列出的【合适的】提交
git submodule update

# 将 `git submodule init` 和 `git submodule update` 合并成一步
git submodule update --init
# 初始化、抓取并检出任何嵌套的子模块
git submodule update --init --recursive

# 1&2： 自动初始化并更新仓库中的每一个子模块， 包括可能存在的嵌套子模块。
git clone --recurse-submodules https://github.com/chaconinc/MainProject
```

### 在包含子模块的项目上工作
如果想要在子模块中查看新工作，可以进入到目录中运行 `git fetch` 与 `git merge`，合并上游分支来更新本地代码。

```Shell
# 更新【所有】子模块
git submodule update --remote

# 设置为想要的其他分支。不用 `-f .gitmodules` 选项，那么它只会为你做修改
git config -f .gitmodules submodule.DbConnector.branch stable
```
如果你现在返回到主项目并运行 `git diff --submodule`，就会看到子模块被更新的同时获得了一个包含新添加提交的列表。
```Shell
# 配置选项 `status.submodulesummary`，Git 也会显示你的子模块的更改摘要
git config status.submodulesummary 1

# 直接看到将要提交到子模块中的提交日志
git log -p --submodule
```

如果在此时提交，那么你会将子模块【锁定】为其他人更新时的新代码。

### 从项目远端拉取上游更改

```Shell
# 递归地抓取子模块的更改，然而，它不会【更新】子模块，可通过 `git status` 命令看到
git pull

# 完成子模块更新
git submodule update --init --recursive

# 抓取子模块更新&check到本地
git pull --recurse-submodules
```

`.gitmodules` 文件中记录的子模块的 URL 发生了改变，补救措施
```Shell
# 将新的 URL 复制到本地配置中
$ git submodule sync --recursive
# 从新 URL 更新子模块
$ git submodule update --init --recursive
```

### 合并本地、上游更新
```Shell
git submodule update --remote --rebase
```
如果你忘记 `--rebase` 或 `--merge`，Git 会将子模块更新为服务器上的状态。并且会将项目重置为一个游离的 HEAD 状态——即：合并是一个明确的动作，update仅仅是切换到上游最新代码。


### 发布子模块改动

```Shell
# 如果任何提交的子模块改动没有推送那么 “check” 选项会直接使 `push` 操作失败。
git push --recurse-submodules=check
```

### 子模块的技巧

```Shell
# 保存所有子模块的工作进度
git submodule foreach 'git stash'

# 创建一个新分支，并将所有子模块都切换过去
git submodule foreach 'git checkout -b featureA'

# 生成一个主项目与所有子项目的改动的统一差异
git diff; git submodule foreach 'git diff'
```

### 有用的别名
```Shell
git config alias.sdiff '!'"git diff && git submodule foreach 'git diff'"
git config alias.spush 'push --recurse-submodules=on-demand'
git config alias.supdate 'submodule update --remote --merge'

# 更新子模块
git supdate

# 检查子模块依赖后推送。
git spush
```
### 子模块的问题
- 切换分支
- 从子目录切换到子模块


## 2. subtree

[git subtree教程](https://segmentfault.com/a/1190000012002151)
[Git subtree 工作流](https://zhuanlan.zhihu.com/p/179366650)

## 3. Fork工作流

[Forking工作流](https://github.com/oldratlee/translations/blob/master/git-workflows-and-tutorials/workflow-forking.md)
[Gitflow工作流](https://github.com/oldratlee/translations/blob/master/git-workflows-and-tutorials/workflow-gitflow.md)
[功能分支工作流](https://github.com/oldratlee/translations/blob/master/git-workflows-and-tutorials/workflow-feature-branch.md)

[5.2 分布式 Git - 向一个项目贡献](https://git-scm.com/book/zh/v2/%E5%88%86%E5%B8%83%E5%BC%8F-Git-%E5%90%91%E4%B8%80%E4%B8%AA%E9%A1%B9%E7%9B%AE%E8%B4%A1%E7%8C%AE)
[GitHub 的 Fork 是什么意思？](https://www.zhihu.com/question/20431718)
[Git 实际使用场景之 Fork 工作流（一）](https://www.bilibili.com/read/cv13888114/)

`Forking`工作流要先有一个公开的正式仓库存储在服务器上。 但一个新的开发者想要在项目上工作时，不是直接从正式仓库克隆，而是`fork`正式项目在服务器上创建一个拷贝。

`Forking`工作流的一个主要优势是，贡献的代码可以被集成，而不需要所有人都能`push`代码到仅有的中央仓库中。

### 服务端fork正式仓库

可以用`git clone`命令[用`SSH`协议连通到服务器](https://confluence.atlassian.com/display/BITBUCKET/Set+up+SSH+for+Git)， 拷贝仓库到服务器另一个位置 —— 是的，`fork`操作基本上就只是一个服务端的克隆。 `GitHub`、`Bitbucket`和`Stash`上可以点一下按钮就让开发者完成仓库的`fork`操作。

### 克隆fork仓库到本地

用熟悉的`git clone`命令，克隆自己的公开仓库。
```Shell
git clone https://user@bitbucket.org/user/repo.git
```

`Forking`工作流需要2个远程别名 —— 一个指向正式仓库upstream，另一个指向开发者自己的服务端仓库origin。
```Shell
# 常见的约定是使用`origin`作为远程克隆的仓库的别名
# git remote add origin https://user@bitbucket.org/user/repo.git

# upstream（上游）作为正式仓库的别名
git remote add upstream https://bitbucket.org/maintainer/repo.git
```

### 发布自己的功能
```Shell
# 首先，通过push将代码推送到自己的公开仓库中
git push origin feature-branch

# 第二件事，开发者要通知项目维护者，想要合并他的新功能到正式库中。
# Bitbucket和Stash提供了Pull Request按钮，弹出表单让你指定哪个分支要合并到正式仓库。
# 一般你会想集成你的功能分支到上游远程仓库的master分支中。

```

### 项目维护者集成开发者的功能

```Shell
# 维护者需要从开发者的服务端仓库中fetch功能分支
git fetch https://bitbucket.org/user/repo feature-branch
# 查看变更
git checkout master
# 解决冲突，合并到本地master分支
git merge FETCH_HEAD
# push变更到服务器上的正式仓库
git push
```

### 开发者和正式仓库做同步
```Shell
git pull upstream master
```
