
- https://nodejs.org/api/debugger.html
- https://github.com/node-inspector/node-inspector



- https://juejin.cn/post/7148816702917050399

## VSCode

### React-Scripts
```json
{
  // 使用 IntelliSense 了解相关属性。
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Chrome",
      "request": "launch",
      "type": "chrome",
      // "runtimeExecutable": "canary",
      "runtimeArgs": [
        // 自动打开Chrome DevTools
        "--auto-open-devtools-for-tabs",
        // 无痕模式启动，也就是不加载插件，没有登录状态
        "--incognito"
      ],
      // "userDataDir": false,
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}"
    },
    {
      // 调试模式启动Chrome：/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --user-data-dir=/Users/zhoubo/Documents/Chrome
      // https://juejin.cn/book/7070324244772716556/section/7071920248835801126
      "name": "Attach to Chrome",
      "port": 9222,
      "request": "attach",
      "type": "chrome",
      "webRoot": "${workspaceFolder}"
    },
    // 调试：npm start
    {
      "name": "Debug Start",
      "request": "launch",
      "runtimeArgs": ["run-script", "start"],
      "runtimeExecutable": "npm",
      "skipFiles": ["<node_internals>/**"],
      "type": "node",
      "env": {
		    "PORT": "4000"
		}
    },
    // 调试：npm run build
    {
      "name": "Debug Build",
      "request": "launch",
      "runtimeArgs": ["run-script", "build"],
      "runtimeExecutable": "npm",
      "skipFiles": ["<node_internals>/**"],
      "type": "node"
    }
  ]
}
```


`skipFiles`置空就可以看到全部源码了