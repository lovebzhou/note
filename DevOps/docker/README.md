## 概述

https://www.docker.com/
[Docker 是怎么实现的？前端怎么用 Docker 做部署？](https://juejin.cn/post/7137621606469222414)
[写给前端的 docker 使用指南](https://juejin.cn/post/7139724794672447518)


- [Nest.js 的微服务，写起来也太简单了吧！](https://juejin.cn/post/7207637337571901495)
- [一文学会用 Docker 和 Docker Compose 部署 Node.js 微服务](https://juejin.cn/post/7208384641190723644)
- [掌握这 5 个技巧，让你的 Dockerfile 像个大师！](https://juejin.cn/post/7248145094600900669)



## 安装
```Shell
brew cask install docker

# 查看版本
docker -v
```

## 快速入门

### 创建React应用程序
```sh
npx create-react-app my-app

cd my-app

npm run build
```

### 创建Dockerfile
```Dockerfile
# 使用官方的Node.js v14镜像作为基础镜像
FROM node:16

# 设置工作目录
WORKDIR /app

# 将应用程序源代码复制到工作目录
COPY . .

# 安装依赖项
RUN npm install --production

# 构建应用程序
RUN npm run build

# 暴露端口80
EXPOSE 80

# 启动应用程序
CMD ["npm", "run", "start"]
```

### 构建Docker镜像
在终端中切换到应用程序目录，并使用以下命令构建Docker镜像
```sh
docker build -t my-app-image .
```

### 运行容器
```sh
docker run -it -p 80:80 my-app-image
```

## React前端Docker

### 安装配置nginx镜像

```Shell
# 拉取nginx镜像
docker pull nginx

# 配置nginx
# touch default.conf
server {
	listen 80;
	server_name localhost;
	
	#charset koi8-r;
	access_log /var/log/nginx/host.access.log main;
	error_log /var/log/nginx/error.log error;
	
	location / {
		root /usr/share/nginx/html;
		index index.html index.htm;
	}
	
	error_page 500 502 503 504 /50x.html;
	location = /50x.html {
		root /usr/share/nginx/html;
	}
}
```

### 安装配置前端App镜像

#### 前端App：构建
```Shell
# 1. 以hello-react为例，构建App
cd ~/demo/react/hello
npm install
npm run build
```

#### 前端App：nginx配置
```Shell
# 在根目录创建default.conf，内容如下：
server {
	listen 80;
	server_name localhost;
	
	#charset koi8-r;
	access_log /var/log/nginx/host.access.log main;
	error_log /var/log/nginx/error.log error;
	
	location / {
		root /usr/share/nginx/html;
		index index.html index.htm;
	}
	
	error_page 500 502 503 504 /50x.html;
	location = /50x.html {
		root /usr/share/nginx/html;
	}
}
```
#### 前端App：Docker配置
```Shell
# 在根目录创建Dockerfile，内容如下：
FROM nginx
COPY build/ /usr/share/nginx/html/
COPY default.conf /etc/nginx/conf.d/default.conf
```


#### 前端App：镜像构建

```Shell
docker build -t hello-react .
```
#### 前端App：容器运行

```Shell
docker run -d -p 3000:80 --name docker-hello hello-react
```
