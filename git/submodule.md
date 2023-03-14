
[git-submodule](https://git-scm.com/docs/git-submodule)

某个工作中的项目需要包含并使用另一个项目，你想要把它们当做两个独立的项目，同时又想在一个项目中使用另一个。

子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。 它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

## [7.11 Git 工具 - 子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

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


## Mastering Git submodules
[Mastering Git submodules](https://medium.com/@porteneuve/mastering-git-submodules-34c65e940407)

**优点**
- The container and the submodule truly act as independent repos.
-  **the submodule commit referenced by the container is stored using its SHA1**, not a volatile reference (such as a branch name) 
- 创建快

**缺点**
-   Every time you add a submodule, change its remote’s URL, or change the referenced commit for it, you demand **a manual update by every collaborator**.
-   Forgetting this explicit update can result in **silent regressions** of the submodule’s referenced commit.
-   Commands such as _status_ and _diff_ display **precious little info** about submodules by default.
-   Because lifecycles are separate, updating a submodule inside its container project requires **two commits and two pushes**.
-   Submodule heads are generally detached, so any local update requires various **preparatory actions** to avoid creating a lost commit.
-   Removing a submodule requires several commands and tweaks, some of which are **manual and unassisted**.

### Adding a submodule

```Shell
# fatal: transport 'file' not allowed
git submodule add ../plugin vendor/plugins/demo

# 添加-c protocol.file.allow=always
# git config --global protocol.file.allow always
git -c protocol.file.allow=always submodule add ../plugin vendor/plugins/demo

```