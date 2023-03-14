# nginx学习笔记

## 1. 概述

### 参考资料
http://nginx.org/

### 安装 & 常用命令

```Shell
# homebrew安装nginx
brew install nginx

# 启动
nginx

# 查看帮助
nginx -h

# 关闭
nginx -s stop

# 重新载入
nginx -s reload

# 检查配置是否正确
nginx -t

```

## 反向代理

[谁说前端不需要懂-Nginx反向代理与负载均衡](https://juejin.cn/post/6844903619465068551)


- /usr/local/etc/nginx/s1.conf
```Shell
server {
    listen       80;
    server_name  s1.tiny.net;
    auth_basic off;
    location / {
        proxy_pass    http://localhost:4000;
        proxy_set_header Host $host;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_connect_timeout 60;
        proxy_read_timeout 600;
        proxy_send_timeout 600;
    }
}

```


