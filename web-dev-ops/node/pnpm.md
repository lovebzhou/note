## 1. 概述

- https://pnpm.io/
- https://www.pnpm.cn/

## 2. 快速入门

### 2.1. 安装

#### 通过 npm 安装 pnpm[​](https://www.pnpm.cn/installation#%E9%80%9A%E8%BF%87-npm-%E5%AE%89%E8%A3%85-pnpm "标题的直接链接")

```
npm install -g pnpm
```

#### 通过 Homebrew 安装 pnpm[​](https://www.pnpm.cn/installation#%E9%80%9A%E8%BF%87-homebrew-%E5%AE%89%E8%A3%85-pnpm "标题的直接链接")

如果你已经安装了 Homebrew 软件包管理器，则可以使用如下命令赖安装 pnpm：

```
brew install pnpm
```


### 2.2. 初始化一个新项目
```sh
pnpm init
```

### 2.3. 添加依赖包
```sh
pnpm add [package]
pnpm add [package]@[version]
pnpm add [package]@[tag]
```

### 将依赖项添加到不同依赖项类别中

分别添加到 `devDependencies`、`peerDependencies` 和 `optionalDependencies` 类别中：

```sh
pnpm add [package] --dev
pnpm add [package] --peer
pnpm add [package] --optional
```

### 升级依赖包

```sh
pnpm upgrade [package]
pnpm upgrade [package]@[version]
pnpm upgrade [package]@[tag]
```

### 移除依赖包

```sh
pnpm remove [package]
```

### 安装项目的全部依赖

```sh
pnpm
```

或者

```sh
pnpm install
```
### 调试本地package


```sh
# 安装package到本地
pnpm link --global

# 使用方：链接本地package
pnpm link --global your-loader

```


## 特性 Features

### 工作区 Workspace

pnpm 内置了对单一存储库（也称为多包存储库、多项目存储库或单体存储库）的支持， 你可以创建一个 workspace 以将多个项目合并到一个仓库中。

一个 workspace 的根目录下必须有 [`pnpm-workspace.yaml`](https://pnpm.io/zh/pnpm-workspace_yaml) 文件， 也可能会有 [`.npmrc`](https://pnpm.io/zh/npmrc) 文件。