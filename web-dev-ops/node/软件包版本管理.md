## 1. n

https://github.com/tj/n

### Install
```sh
# 方法1：默认安装到：/usr/local
npm install -g n
brew install n

# 为避免使用n都需要sudo

# make cache folder (if missing) and take ownership
sudo mkdir -p /usr/local/n
sudo chown -R $(whoami) /usr/local/n

# make sure the required folders exist (safe to execute even if they already exist)
sudo mkdir -p /usr/local/bin /usr/local/lib /usr/local/include /usr/local/share
# take ownership of Node.js install destination folders
sudo chown -R $(whoami) /usr/local/bin /usr/local/lib /usr/local/include /usr/local/share
```

### 常用命令

```Shell
n <version>     # 下载某一版本号node    e.g：n v10.16.0
n latest        # 安装最新版本
n stable        # 安装最新稳定版
n 12            # 安装主版本号为12的最新版本
n lts           # 安装最新长期维护版(lts)
n rm <version>  # 删除某个版本   e.g：n  rm 10.16.0
n   // 输入命令后直接使用上下箭头选择版本
    node/12.18.0
    node/14.18.0
    node/16.19.0
  ο node/19.8.1
```

## 2. nvm

### 安装

```sh
# 方式1 浏览器打开下面链接下载
https://github.com/nvm-sh/nvm/blob/v0.39.1/install.sh
# 下载完成后，通过命令安装
sh install.sh

# 方式2 推荐
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

# 方式3
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

### 常用命令

```sh
nvm ls # 查看版本安装所有版本 
nvm ls-remote # 查看远程所有的 Node.js 版本 
nvm install 17.0.0 # 安装指定的 Node.js 版本 
nvm use 17.0.0 # 使用指定的 Node.js 版本 
nvm alias default 17.0.0 # 设置默认 Node.js 版本 
nvm alias dev 17.0.0 # 设置指定版本的别名，如将 17.0.0 版本别名设置为 dev

$ nvm use 16
Now using node v16.9.1 (npm v7.21.1)
$ node -v
v16.9.1
$ nvm use 14
Now using node v14.18.0 (npm v6.14.15)
$ node -v
v14.18.0
$ nvm install 12
Now using node v12.22.6 (npm v6.14.5)
$ node -v
v12.22.6
```


