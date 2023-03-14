
## 概述

### 参考资料
[Gitflow工作流](https://github.com/oldratlee/translations/blob/master/git-workflows-and-tutorials/workflow-gitflow.md)
[功能分支工作流](https://github.com/oldratlee/translations/blob/master/git-workflows-and-tutorials/workflow-feature-branch.md)

[5.2 分布式 Git - 向一个项目贡献](https://git-scm.com/book/zh/v2/%E5%88%86%E5%B8%83%E5%BC%8F-Git-%E5%90%91%E4%B8%80%E4%B8%AA%E9%A1%B9%E7%9B%AE%E8%B4%A1%E7%8C%AE)
[GitHub 的 Fork 是什么意思？](https://www.zhihu.com/question/20431718)
[Git 实际使用场景之 Fork 工作流（一）](https://www.bilibili.com/read/cv13888114/)

### Why
有改动第三方库的需求：
- bug修复
- 依赖库更新
- 性能优化
- 功能定制

但因各种原因不能或等不及官方更新：
- 官方维护者没有时间
- 没有权限修改官方库
- 有权限修改官方库，但集成周期过长

### How

不是直接从官方仓库克隆，而是从在服务器上通过fork官方仓库创建自己的私有仓库来克隆。

> 在`Forking`工作流中，『官方』仓库的叫法只是一个约定，理解这点很重要。 从技术上来看，各个开发者仓库和正式仓库在`Git`看来没有任何区别。
> 
> `Forking`工作流的一个主要优势是，贡献的代码可以被集成，而不需要所有人都能`push`代码到仅有的中央仓库中。
> 
> `Forking`工作流需要2个远程别名 —— 一个指向正式仓库（upstream)，另一个指向开发者自己的服务端仓库（origin）

要提交本地修改时，`push`提交到自己公开仓库中 —— 而不是正式仓库中。 然后，给正式仓库发起一个`pull request`，让项目维护者知道有更新已经准备好可以集成了。 对于贡献的代码，`pull request`也可以很方便地作为一个讨论的地方。


## 文摘：Forking工作流

[Forking工作流](https://github.com/oldratlee/translations/blob/master/git-workflows-and-tutorials/workflow-forking.md)

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
