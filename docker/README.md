# Docker笔记

## 概述

https://www.docker.com/
[Docker 是怎么实现的？前端怎么用 Docker 做部署？](https://juejin.cn/post/7137621606469222414)
[写给前端的 docker 使用指南](https://juejin.cn/post/7139724794672447518)

## 快速入门


### 安装Docker
```Shell
brew cask install docker

# 查看版本
docker -v
```

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
