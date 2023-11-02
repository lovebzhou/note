# Redis学习笔记

## 1. 概述

## 2. 开发环境

### 2.1. 安装

#### Docker安装redis镜像
https://docs.redis.com/latest/rs/installing-upgrading/get-started-docker/

- 下载Docker Desktop：https://www.docker.com/
- 搜索 redis，点击images，选择redis，点击 run
- 填写Container容器信息：
	- Container name: redis-test
	- Ports: 6379:6379
	- Volumes：~/Document/data/redis => /data

> Volumes：挂载本地目录到容器目录。


GUI工具：[medis](https://getmedis.com/)


### 2.2. 交互模式 & 命令行

#### 进入
```sh
# =>>> 在Container Terminal页签下输入redis-cli
redis-cli
127.0.0.1:6379>

# =>>> 系统终端进入

# 查看容器ID
docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                    NAMES
0e60e46494ea   redis:latest   "docker-entrypoint.s…"   37 minutes ago   Up 37 minutes   0.0.0.0:6379->6379/tcp   redis-test

# 进入容器终端：docker exec -it <container-id> /bin/sh
docker exec -it 0e60e46494ea /bin/sh
```

#### 存储
```sh
# set key value
127.0.0.1:6379> set v1 1
OK
127.0.0.1:6379> set v2 2
OK
127.0.0.1:6379> set v3 3
OK
```

#### 读取
```sh
# get key
127.0.0.1:6379> get v1
"1"
127.0.0.1:6379> get v2
"2"
127.0.0.1:6379> get v3
"3"
```

#### 递增
```sh
#incr key
127.0.0.1:6379> incr v1
(integer) 2
127.0.0.1:6379> incr v2
(integer) 3
127.0.0.1:6379> incr v3
(integer) 4
```

#### 查询key
```sh
#keys pattern
127.0.0.1:6379> KEYS "v*"
1) "v2"
2) "v3"
3) "v1"

127.0.0.1:6379> KEYS "*"
1) "c"
2) "a"
3) "v2"
4) "o1"
5) "o2"
6) "v3"
7) "v1"
8) "b"
```

#### List

- LPUSH 添加新元素到头部；RPUSH 添加到尾部
- LPOP 删除并返回从头部删除的元素；RPOP 从尾部
- LLEN 返回列表长度
- LMOVE 从一个列表移动至另一列表
- LTRIM 将列表缩减到指定范围

```sh
# =>>> 添加到头部：lpush key element [element ...]
127.0.0.1:6379> LPUSH l1 111
(integer) 1
127.0.0.1:6379> LPUSH l1 222
(integer) 2
127.0.0.1:6379> LPUSH l1 333
(integer) 3

# =>>> 追加到尾部：rpush key element [element ...]
127.0.0.1:6379> rpush l1 444
(integer) 4
127.0.0.1:6379> rpush l1 555
(integer) 5

# =>>> 列出内容：lrange key start stop
127.0.0.1:6379> lrange l1 0 -1
1) "333"
2) "222"
3) "111"
4) "444"
5) "555"

# =>>> 从头部删除：lpop key [count]
127.0.0.1:6379> lpop l1 2
1) "333"
2) "222"

# =>>> 从尾部删除： rpop key [count]
127.0.0.1:6379> rpop l1
"555"

# =>>> 列表长度
127.0.0.1:6379> LLEN l1
(integer) 2

```

#### Set

set 的特点是无序并且元素不重复。
- SADD：添加
- SREM：删除
- SISMEMBER：test a string for set membership.
- SINTER: 多个集合的交集
- SCARD: returns the size of a set.

```sh
# =>>> 添加：sadd key member [member ...]
127.0.0.1:6379> sadd s1 111
(integer) 1
127.0.0.1:6379> sadd s1 111
(integer) 0
127.0.0.1:6379> sadd s1 222
(integer) 1
127.0.0.1:6379> sadd s1 333 444 555
(integer) 3

# =>>> 删除：srem key member [member ...]
127.0.0.1:6379> SREM s1 333
(integer) 1

# =>>> 是否在集合中：SISMEMBER key member
127.0.0.1:6379> SISMEMBER s1 111
(integer) 1
127.0.0.1:6379> SISMEMBER s1 666
(integer) 0

```

#### ZSet

set 只能去重、判断包含，不能对元素排序。
如果排序、去重的需求，比如排行榜，可以用 sorted set，也就是 zset

```sh
# =>>> 添加： zadd key score member
127.0.0.1:6379> ZADD s2 5 aaa
(integer) 1
127.0.0.1:6379> zadd s2 5 bbb
(integer) 1
127.0.0.1:6379> zadd s2 3 ccc
(integer) 1

# =>>> 列出元素
127.0.0.1:6379> ZRANGE s2 0 -1
1) "ccc"
2) "aaa"
3) "bbb"


```

## 3. 代码演示

### JavaScript演示项目

#### 新建

```sh
mkdir hello-redis
cd hello-redis
npm init -y
npm install redis
```

在package.json中添加
```json
"type": "module"
```

To fix: SyntaxError
```sh
Cannot use import statement outside a module
await is only valid in async functions and the top level bodies of modules
```

#### index.js
```javascript
import { createClient } from 'redis';

const client = createClient({
    socket: {
        host: 'localhost',
        port: 6379
    }
});

client.on('error', err => console.log('Redis Client Error', err));

await client.connect();

const value = await client.keys('*');

console.log(value);

await client.disconnect();
```

#### 运行
```sh
node index.js
```