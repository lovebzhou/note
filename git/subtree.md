## 1. 概述
 
Subtree允许你以子目录的形式将子项目引入到主项目中(可包括子项目的整个历史记录)。

Git不提供原生subtree命令，这与submodule是不同的。与其说subtree是一个特性，不如说它是一个概念，一种使用 Git管理嵌入式代码的方法。他们主要依赖于使用原有的git命令（主要是merge、cherry-pick和read-tree）来实现的。

主项目与普通项目repo(本地库)的主要区别是：主项目repo的remote除origin外，还包括子项目的remote——并且以子目录形式连嵌入到主项目中。

无论**手动**也好，还是社区贡献的**subtree**命令也好，它们完成如下预期行为：
- 主项目中所有对代码的增、删、改操作，除像普通项目一样commit、push到主项目的remote外，同时还期待能将其中那些仅针对子项目的操作提取出来，commit、push到子项目的remote中。
- 主项目fetch、merge子项目remote更新时，不会污染主项目历史记录。

主项目的克隆版本与普通项目是一样的——我们感知不到子项目的存在——对子项目（子目录）的修改会直接commit、push到主项目remote中，可能会忽略提取并反合到子项目remote的过程——这可能并非是预期的结果——submodule则不会出现这种情况。

与submodule相比：
- subtree对子项目的pull、push等类似操作是非常难记的。
- subtree透明引入子项目——克隆repo不涉及子项目git相关remote地址及操作权限——适用于**只读**引入。

#### 名词解释

**repo**：本地代码仓库

**remote**：远程代码仓库

**子项目**：第三方项目repo

**主项目**：添加了子项目remote的repo，通常在这里合并或者反合子项目remote。克隆后的主项目会成为普通项目，可通过前面步骤转变为主项目。


**backporting**
Backporting is the action of taking parts from a newer version of a software system or software component and porting them to an older version of the same software. It forms part of the maintenance step in a software development process, and it is commonly used for fixing security issues in older versions of the software and also for providing new features to older versions.
> Backporting是从较新版本的软件系统或软件组件中取出部分并将它们移植到同一软件的较旧版本中的操作。它构成了软件开发过程中维护步骤的一部分，通常用于修复旧版本软件中的安全问题以及为旧版本提供新功能。


### 参考资料
[Mastering Git subtrees](https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec)
[git subtree教程](https://segmentfault.com/a/1190000012002151)
[Git subtree 工作流](https://zhuanlan.zhihu.com/p/179366650)

## 2. 文摘：掌握Git subtree

### 2.1. 三种方式

#### Manually

```Shell
git merge

git cherry-pick

git read-tree
```
#### The _git subtree_ contrib script

```Shell
# =>>> 查看帮助
# Usage
git subtree
git subtree -h

# 查看帮助手册
# Darwin: git version 2.37.1 (Apple Git-137.1)无帮助手册
# Linux: git version 1.8.3.1
git subtree --help

# 添加
git subtree add --prefix=<prefix> <commit>

git subtree add --prefix=<prefix> <repository> <ref>

# 合并
git subtree merge --prefix=<prefix> <commit>

# 拆分目录
git subtree split --prefix=<prefix> [<commit>]

# local <= remote
git subtree pull --prefix=<prefix> <repository> <ref>

# local => remote
git subtree push --prefix=<prefix> <repository> <refspec>
```

从1.7.11开始纳入官方发行版的第三方脚本。

#### git-subrepo

### 2.2. 示例简述

[Download the example repos](http://drive.delicious-insights.com/assets/git-subs-demo.zip)

You’ll find three directories in there:

-   _main_ acts as the **container** repo, local to the first collaborator,
-   _plugin_ acts as the **central maintenance repo** for the module, and
-   _remotes_ contains the filesystem remotes for the two previous repos.

### 2.3. 添加子项目

#### Manually

```Shell
# 添加subtree远程仓库
git remote add plugin ../remotes/plugin

# 拉取代码
git fetch plugin

# 此时：nothing to commit, working tree clean
git status

# 将subtree绑定到工作目录
git read-tree --prefix=vendor/plugins/demo -u plugin/master

# 此时：提示plugin/master有更新
git status

# 提交更新
git commit -m "Added demo plugin subtree in vendor/plugins/demo"

```

#### With git subtree

```Shell
# 添加subtree远程仓库
git remote add plugin ../remotes/plugin/

# 将subtree绑定到工作目录
# Working tree has modifications.  Cannot add.
git subtree add --prefix=vendor/plugins/demo plugin master

# 拉取代码
git fetch plugin master

# 在绑定一次？？？
git subtree add --prefix=vendor/plugins/demo plugin master


# subtree提交日志合并到了container
git log --oneline --graph --decorate


#
# 清除subtree日志污染
#

# 重置container
git reset --hard @{u}

# 使用--squash参数：压缩subtree提交日志
git subtree add --prefix=vendor/plugins/demo --squash plugin master

# 
git log --oneline --graph --decorate
```


### 2.4. 获取、更新包含子项目的主项目代码库

Grabbing/updating a repo that uses subtrees
与普通仓库是一样

```Shell
# push到远程库
manually/main %git push
subtree/main %git push

# 分别克隆manually、subtree
cd ..
git clone remotes/main colleague
cd colleague
tree vendor
```


### 2.5. 合并子项目remote代码库更新
Getting an update from the subtree’s remote
这里指的是在包含子项目remote的主项目中进行子项目的更新、合并——主项目通过命令合并子项目remote更新

```Shell
cd ../plugin
git log --oneline
date > fake-work
git add fake-work
git commit -m "Pseudo-commit #1"
date >> fake-work
git commit -am "Pseudo-commit #2"
git push
cd ../main
```

#### Manually

```Shell
git fetch plugin

git merge -s subtree --squash plugin/master --allow-unrelated-histories

git status

git commit -m "Updated the plugin"
```


#### With git subtree
```Shell
git subtree pull --prefix=vendor/plugins/demo --squash plugin master
git log --oneline --graph --decorate
```

> Oh GAWD… And remember: your container only has this one _master_ branch right now. Clearly reflected by this graph, right? Tsk tsk.


### 2.6.  在主项目中更新子项目remote代码库
Updating a subtree in-place in the container

```Shell
git push  

echo '// Now super fast' >> vendor/plugins/demo/lib/index.js  
git ci -am "[To backport] Faster plugin"  
date >> main-file-1  
git ci -am "Container-only work"  
date >> vendor/plugins/demo/fake-work  
date >> main-file-2  
git ci -am "[To backport] Timestamping (requires container tweaks)"  
echo '// Container-specific' >> vendor/plugins/demo/lib/index.js  
git ci -am "Container-specific plugin update"

# 查看以上更新日志
git log --oneline --decorate --stat -5
```

#### Manually
```Shell
# 创建本地backport-plugin分支 
git checkout -b backport-plugin plugin/master

git cherry-pick -x master~3

git cherry-pick -x --strategy=subtree master^

git log --oneline --decorate --stat -2

# 推送到plugin远程库
git push plugin HEAD:master

# 切回本地主分支
git checkout master

# 合并子项目更改
git merge -s subtree --squash backport-plugin --allow-unrelated-histories

# 提交变更到主项目
git commit -m "merge backport-plugin"

# 推送到主项目远程库
git push
```
#### With git subtree
```Shell
# it backports every single commit that touched the subtree
git subtree push -P vendor/plugins/demo plugin master

# Note the latest (top) backport, that we don’t want here
git log --oneline --decorate -4 plugin/master

```

### 删除子项目

```Shell
git rm -r vendor/plugins/demo
git commit -m "Removing demo subtree"
```

### 将子目录转换为子项目

Turning a directory into a subtree

新建local remote仓库
```Shell
cd ..  
mkdir remotes/myown  
cd remotes/myown  
git init --bare  
cd ../../main
```
新增重用演示文件夹
```Shell
mkdir -p lib/plugins/myown/lib  
echo '// Yo!' > lib/plugins/myown/lib/index.js  
git add lib/plugins/myown  
git ci -m "Plugin sez: Yo, dawg."  
date >> main-file-1  
git ci -am "Container-only work"  
echo '// Now super fast' > lib/plugins/myown/lib/index.js  
date >> main-file-2  
git ci -am "Faster plugin (requires container tweaks)"  
git push
```
#### Manually

```Shell

```
#### With git subtree
```Shell
git subtree split -P lib/plugins/myown -b split-plugin

git checkout split-plugin

git remote add myown ../remotes/myown

git push -u myown split-plugin:master

cd ..

# warning: remote HEAD refers to nonexistent ref, unable to checkout.
git clone remotes/myown myown

cd main
git checkout master

# Can't squash-merge: 'lib/plugins/myown' was never added.
git subtree pull --squash -P lib/plugins/myown myown master

#
#
#

git rm -r lib/plugins/myown
git commit -m "Removing lib/plugins/myown for subtree replacement"
git subtree add -P lib/plugins/myown --squash myown master

git subtree pull -P lib/plugins/myown --squash myown master

```