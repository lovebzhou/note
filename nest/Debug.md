### 调试模式
```sh
# --inspect 是调试模式运行，而 --inspect-brk 还会在首行断住。
node --inspect-brk index.js
```

打开 [chrome://inspect/](https://link.juejin.cn/?target=)，可以看到可以调试的目标：

### VSCode调试

```json
{
  // 使用 IntelliSense 了解相关属性。
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug",
      "request": "launch",
      "runtimeArgs": ["run-script", "start:dev"],
      "runtimeExecutable": "npm",
      "skipFiles": ["<node_internals>/**"],
      "type": "node",
      "console": "integratedTerminal"
    },
    {
      "name": "Attach",
      "port": 9229,
      "request": "attach",
      "skipFiles": ["<node_internals>/**"],
      "type": "node"
    }
  ]
}

```