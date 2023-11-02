### Run Node.js scripts from the command line
- run
```Shell
node app.js
```
- app.js
```Shell
#!/usr/bin/node
```
or
```Shell

#!/usr/bin/env node

// your code

```
- 执行权限
```Shell
chmod u+x app.js
```
### Restart the application automatically

```Shell
# 安装nodemon
npm i -g nodemon

# 运行
nodemon app.js
```

### environment variables

```Shell
USER_ID=239482 USER_KEY=foobar node app.js
```