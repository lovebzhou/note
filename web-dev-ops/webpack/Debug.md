# webpack调试笔记

## 参考
- [webpack bits: Learn and Debug webpack with Chrome Dev Tools!](https://medium.com/webpack/webpack-bits-learn-and-debug-webpack-with-chrome-dev-tools-da1c5b19554)
- 

## 新建演示项目
```sh
npx create-react-app hello-react-eject

cd hello-react-eject
npm run eject
```

## 在VSCode中调试

添加调试配置：通过 npm "debug" 脚本启动 node 程序

```json
{
	// 使用 IntelliSense 了解相关属性。
	// 悬停以查看现有属性的描述。
	// 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
	"version": "0.2.0",
	"configurations": [
		{
			"name": "Launch via NPM",
			"request": "launch",
			"runtimeArgs": [
				"run-script",
				"start"
			],
			"runtimeExecutable": "npm",
			"skipFiles": [
				"<node_internals>/**"
			],
			"console": "integratedTerminal",
			"type": "node"
		}
	]
}
```


> 为方便选择启动端口，设置 启动调试目标的位置：
> "console": "`integratedTerminal`": VS Code 的集成终端

## 在浏览器中调试

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