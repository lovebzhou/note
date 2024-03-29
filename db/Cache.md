# 数据缓存问题与解决方案笔记

## 1. 常见问题

- [缓存穿透、缓存雪崩、缓存击穿？再也不怕了，你随便问吧！](https://juejin.cn/post/7118890909294395429)

### 缓存穿透

**现象**

- 数据既不在缓存当中，也不在数据库中

**解决**

- 缓存空值（null）或默认值
- 业务逻辑前置校验
- 使用布隆过滤器请求白名单
- 用户黑名单限制

### 缓存雪崩

**现象**

-  大量热点key同时过期；
-   缓存服务故障

**解决**
-   通常的解决方案是将key的过期时间后面加上一个`随机数`（比如随机1-5分钟），让key均匀的失效。
-   考虑用队列或者锁的方式，保证缓存单线程写，但这种方案可能会影响并发量。
-   热点数据可以考虑不失效，后台异步更新缓存，适用于不严格要求缓存一致性的场景。
-   双key策略，主key设置过期时间，备key不设置过期时间，当主key失效时，直接返回备key值。
-   构建缓存高可用集群（针对缓存服务故障情况）。
-   当缓存雪崩发生时，服务熔断、限流、降级等措施保障。

### 缓存击穿

- 单个热点key失效

**解决**
-   使用互斥锁（Mutex Key），只让一个线程构建缓存，其他线程等待构建缓存执行完毕，重新从缓存中获取数据。单机通过synchronized或lock来处理，分布式环境采用分布式锁。
-   热点数据不设置过期时间，后台异步更新缓存，适用于不严格要求缓存一致性的场景。
-   ”提前“使用互斥锁（Mutex Key）：在value内部设置一个比缓存（Redis）过期时间短的过期时间标识，当异步线程发现该值快过期时，马上延长内置的这个时间，并重新从数据库加载数据，设置到缓存中去。

## 2. 解决策略

- 限流
- 降级
- 熔断