## 1. 概述

- https://www.npmjs.com/
- https://www.npmjs.cn/

npm 由三个独立的部分组成：
- 网站
	- 是开发者查找包（package）、设置参数以及管理 npm 使用体验的主要途径。
- 注册表（registry）
	- 是一个巨大的数据库，保存了每个包（package）的信息。
- 命令行工具 (CLI)
	- 通过命令行或终端运行。开发者通过 CLI 与 npm 打交道。

```Shell
# 查看命令帮助
npm help command
```

## 2. 安装

```sh
# install the latest official, tested version of npm
npm install npm@latest -g

# install a version that will be released in the future
npm install npm@next -g
```

## 3. 常用命令

```sh

# 创建package.json
npm init
# 创建默认package.json
npm init -y

# 安装依赖包
npm install
# 更新依赖包
npm update
```

### 包管理

```
# =>>> 安装包
npm install react

# 安装全局包
npm install -g jshint
# 更新全局包
npm update -g jshint

# =>>> 更新
# 查看需要更新的全局包
npm outdated -g --depth=0
# 更新全部全局包
npm update -g

# =>>> 卸载

# 卸载包
npm uninstall react
# 卸载全局包
npm uninstall -g jshint

# =>>> 查看

# 查看Registry
npm view react

#信息列出react历史版本
npm view react versions --json

# 在浏览器中打开指定包的源码仓库页面
npm repo react

# 搜索
npm search react

```

### 执行脚本

```
# 启动
npm start

# 构建
npm run build

```

## 4. 发布&更新 软件包

https://www.npmjs.cn/getting-started/publishing-npm-packages/

### 准备
```sh

# 注册npm registry账号
npm adduser

# 登录
npm login

# 查看登录账号
npm whoami

# 检查username是否添加到npm registry
open 'https://npmjs.com/~username'

```

### 开发Package

```
# ...
npm init
```

### 发布Package
```

npm publish
```

### 更新Package
```
# 增加主版本号
npm version major
# 增加次版本号
npm version minor
# 增加修正号
npm version patch
```


## Q&A

### 如何发布Package到本地系统？

1.  在本地系统中创建一个新的npm package。可以使用以下命令来创建一个空的npm package：

```sh
mkdir <package-name>
cd <package-name>
npm init
```

2.  在本地系统中进行package的开发，包括编写代码、测试、文档等工作。
3.  在本地系统中使用以下命令将package发布到本地：
```sh
npm pack
```

执行该命令后，npm将会在当前目录下生成一个tarball文件（`.tgz`格式），其中包含了要发布的npm package的所有文件和依赖项。

4.  在另一个项目中，可以使用以下命令将本地的tarball文件安装到项目中：

```sh
npm install <path-to-tarball-file>
```

通过上述步骤，即可将一个npm package发布到本地系统中，并在其他项目中使用。注意，在实际发布npm package时，需要将package发布到npm官方的npm registry中，而不是本地系统中。