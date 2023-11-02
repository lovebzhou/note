# rollup笔记

## 概述

### 参考资料
- [rollupjs](https://rollupjs.org/)
	- [Tutorial](https://rollupjs.org/tutorial/)
- [[rollup.js 中文文档](https://www.rollupjs.com/)](https://www.rollupjs.com/)
## 配置

```sh
# 解析外部模块 node_modules
npm install -D @rollup/plugin-node-resolve

# 支持 CommonJS 规范导出的包
npm install -D @rollup/plugin-commonjs

```
## 快速开始

```sh
# 安装
npm install rollup

# =>>> rollup常用命令

# 编译为一个在 <script> 标签中可用的自运行函数 ('iife')
rollup main.js --file bundle.js --format iife

# 编译为 CommonJS 模块 ('cjs')
rollup main.js --file bundle.js --format cjs

# 需要为 UMD 格式的包指定一个名称
rollup main.js --file bundle.js --format umd --name "myBundle"

# 以默认配置文件：rollup.config.js打包
rollup -c

# =>>> 混淆压缩

npm install -g tersor

# tersor -c -m -o <output.js> <input.js>
tersor -c -m -o dist/bundle.min.js dist/bundle.js

```

