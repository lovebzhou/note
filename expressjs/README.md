
https://github.com/expressjs/express

### Install

```Shell

mkdir hello
cd hello

# create a `package.json`
npm init

# 安装
npm install express
```

### HelloWorld
```JavaScript
// app.js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```
运行
```Shell
node app.js

# 
nodemon app.js
```

### Express application generator
通过express-generator快速创建express应用骨架
```Shell
npm install -g express-generator

# 创建hello_pug
express --view=pug hello_pug
cd hello_pug

# install dependencies
npm install

# run the app
DEBUG=hello-pug:* npm start
```

### More Examples

http://expressjs.com/en/starter/examples.html