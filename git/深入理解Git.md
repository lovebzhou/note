## 概述

![[framework.png]]
![[fetch.png]]
## 原理

[Git原理浅析](https://juejin.cn/post/7106353721298124808)

我们的项目一般由文件夹和文件组成，在git的术语中，文件夹称为 **“tree”** ，文件称为 **“blob”** ，顶层文件夹称为 **“top-level tree”** 。

Git中还有一个很重要的概念叫“分支”，术语叫 **“reference”** ，它其实就是一个指向commit的指针。

.git内容

-   HEAD 文件：指向当前所在分支。
-   config文件：包含了一些配置。
-   description文件：只有在GitWeb项目中才会用到，所以不用关注这个文件。
-   hooks文件夹：包含了一些钩子脚本。
-   info文件夹：包含了`.gitignore` 文件中的信息。
-   objects文件夹：存放object的数据库，存放整个项目的所有数据。
-   refs文件夹：存放了指向objects的指针（如branches，tags，remotes等）。


```Shell
# 获取hashId指向的object内容
git cat-file -p <hashId>

# 获取hashId指向的object类型
git cat-file -t <hashId>

# 查看暂存区(.git/index)数据文件
git ls-files --stage
```