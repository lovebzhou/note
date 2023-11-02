# `webpack`简介
## 1. 概述

本质上，**webpack** 是一个用于现代 JavaScript 应用程序的 _静态模块打包工具_。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，然后将你项目中所需的每一个模块组合成一个或多个 _bundles_，它们均为静态资源，用于展示你的内容。
[更多...](https://webpack.js.org/concepts/)

### 为什么选择`webpack`？

在浏览器中运行 JavaScript 有两种方法。第一种方式，引用一些脚本来存放每个功能；此解决方案很难扩展，因为加载太多脚本会导致网络瓶颈。第二种方式，使用一个包含所有项目代码的大型 `.js` 文件，但是这会导致作用域、文件大小、可读性和可维护性方面的问题。

[更多...](https://webpack.docschina.org/concepts/why-webpack/)

### 内部原理

https://webpack.docschina.org/concepts/under-the-hood/

## 2. 核心概念

### Compiler

`Compiler` 模块是 `webpack` 的主要引擎，它通过 [CLI](https://webpack.docschina.org/api/cli) 或者 [Node API](https://webpack.docschina.org/api/node) 传递的所有选项创建出一个 compilation 实例。 它扩展（extends）自 `Tapable` 类，用来注册和调用插件。 大多数面向用户的插件会首先在 `Compiler` 上注册。

在为 webpack 开发插件时，你可能需要知道每个钩子函数是在哪里调用的。想要了解这些内容，请在 webpack 源码中搜索 `hooks.<hook name>.call`。


`Compiler` 支持可以监控文件系统的 [监听(watching)](https://webpack.docschina.org/api/node/#watching) 机制，并且在文件修改时重新编译。 当处于监听模式(watch mode)时， compiler 会触发诸如 `watchRun`, `watchClose` 和 `invalid` 等额外的事件。 通常在 [开发环境](https://webpack.docschina.org/guides/development) 中使用， 也常常会在 `webpack-dev-server` 这些工具的底层调用， 由此开发人员无须每次都使用手动方式重新编译。 还可以通过 [CLI](https://webpack.docschina.org/api/cli/#watch-options) 进入监听模式。


### Compilation
`Compilation` 模块会被 `Compiler` 用来创建新的 compilation 对象（或新的 build 对象）。 `compilation` 实例能够访问所有的模块和它们的依赖（大部分是循环依赖）。 它会对应用程序的依赖图中所有模块， 进行字面上的编译(literal compilation)。 在编译阶段，模块会被加载(load)、封存(seal)、优化(optimize)、 分块(chunk)、哈希(hash)和重新创建(restore)。

- MainTemplate
- ChunkTemplate
- RuntimeTemplate

### NormalModule

### ContextModuleFactory
`Compiler` 使用 `ContextModuleFactory` 模块从 webpack 独特的 [require.context](https://webpack.docschina.org/api/module-methods/#requirecontext) API 生成依赖关系。它会解析请求的目录，为每个文件生成请求，并依据传递来的 regExp 进行过滤。最后匹配成功的依赖关系将被传入 [NormalModuleFactory](https://webpack.docschina.org/api/normalmodulefactory-hooks)。

`ContextModuleFactory` 类扩展了 `Tapable` 并提供了以下的生命周期钩子。 你可以像使用编译器钩子一样使用它们：

```js
ContextModuleFactory.hooks.someHook.tap(/* ... */);
```

与 `compiler` 一样，`tapAsync` 和 `tapPromise` 是否可用 取决于钩子的类型。
### NormalModuleFactory
`Compiler` 使用 `NormalModuleFactory` 模块生成各类模块。从入口点开始，此模块会分解每个请求，解析文件内容以查找进一步的请求，然后通过分解所有请求以及解析新的文件来爬取全部文件。在最后阶段，每个依赖项都会成为一个模块实例。
