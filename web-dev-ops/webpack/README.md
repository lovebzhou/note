# Webpack学习笔记

## 1. 概述

- https://webpack.js.org/
- https://www.webpackjs.com/
- https://astexplorer.net/
- [minipack](https://github.com/ronami/minipack)
- [webpack，Babel，babel-loader的关系](https://juejin.cn/post/6844904098303574023)
- [Webpack 深入浅出之公司级分享总结](https://juejin.cn/post/6844904022567043080)
- [干货！撸一个webpack插件(内含tapable详解+webpack流程)](https://juejin.cn/post/6844903713312604173)
- [webpack原理](https://juejin.cn/post/6844903838420303886)
- [探究前端项目打包构建的用户故事](https://juejin.cn/post/7216153056282411045)



## 2. 开发环境

### 2.1. 调试webpace.config.js

#### 2.1.0. 演示环境

```sh
# 新建项目
npx create-react-app hello-react-eject

cd hello-react-eject

# 安装依赖
npm install

npm run eject
```

#### 2.1.1. 在浏览器中调试

##### 在package.json中添加如下代码：
```json
"scripts": {
	"start:debug": "node --inspect-brk scripts/start.js",
},
```
该命令表示调试启动webpack并停留在`scripts/start.js`的第一行。

##### 执行终端命令
```sh
npm run start:debug
```

##### 打开浏览器中开发者工具

![[devtool4node.png]]
点击开发者工具左上角webpack调试按钮，启动webpack调试窗口。此时，脚本执行停留在第一行。webpack配置调试完成后，继续执行会执行正常的App启动命令。

#### 2.1.2. 在VSCode中调试

##### 在package.json中添加命令：

```json
"scripts": {
	"start:debug:vscode": "node --inspect-brk=8229 scripts/start.js",
},
```

##### 添加调试配置
https://code.visualstudio.com/docs/editor/debugging
```json
{
	// 使用 IntelliSense 了解相关属性。
	// 悬停以查看现有属性的描述。	
	// 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
	"version": "0.2.0",
	"configurations": [
		{
			"type": "node",	
			"request": "launch",
			"name": "debug-webpack",
			"skipFiles": ["<node_internals>/**"],
			"runtimeExecutable": "npm",
			"runtimeArgs": ["run", "start:debug:vscode"],
			"attachSimplePort": 8229
		}
	]
}
```

##### 添加断点&启动调试

- 在`scripts/start.js`中点击断点
- 点击`debug-webpack`启动调试

## 配置

### 多入口

```js
module.exports = {
	entry: {
		main: './src/main.js',
		login: './src/Login.js',
	},
	output: {
		path: path.resolve(__dirname, 'dist'),
		//filename: '[name].js',
		filename: '[name].[contenthash].js',
	},
	plugins: [
		new HtmlWebpackPlugin({
			template: path.join(__dirname, 'public', 'index.html'),
			filename: './index.html',
			chunks: ['main'],
		}),
		new HtmlWebpackPlugin({
			template: path.join(__dirname, 'public', 'index.html'),
			filename: './login.html',
			chunks: ['login'],
		}),
	]
}
```

## 代码分离- Code Splitting

- [指南-代码分离](https://www.webpackjs.com/guides/code-splitting/)
- [SplitChunksPlugin](https://www.webpackjs.com/plugins/split-chunks-plugin/)
- [webpack分包](https://juejin.cn/post/7142797454490992653)


## Tree Shaking

_tree shaking_ 是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。它依赖于 ES2015 模块语法的 [静态结构](http://exploringjs.com/es6/ch_modules.html#static-module-structure) 特性，例如 [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) 和 [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)。