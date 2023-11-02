
### launch.json

```json
{
  // 使用 IntelliSense 了解相关属性。
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Active File",
      "program": "${file}",
      "request": "launch",
      "skipFiles": ["<node_internals>/**"],
      "type": "node"
    },
    {
      "name": "NPM Start",
      "request": "launch",
      "runtimeArgs": ["run-script", "start"],
      "runtimeExecutable": "npm",
      "skipFiles": ["<node_internals>/**"],
      "type": "node"
    },
    {
      "name": "NPM Build:Dev",
      "request": "launch",
      "runtimeArgs": ["run-script", "build:dev"],
      "runtimeExecutable": "npm",
      "skipFiles": ["<node_internals>/**"],
      "type": "node"
    },
    {
      "name": "Test",
      "request": "launch",
      "runtimeArgs": ["run-script", "test"],
      "runtimeExecutable": "npm",
      "skipFiles": ["<node_internals>/**"],
      "type": "node"
    }
  ]
}
```