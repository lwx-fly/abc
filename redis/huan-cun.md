# 缓存

## 缓存雪崩&#x20;

* 多个缓存同时过期，查询数据库导致宕机
* 缓存key 过期时间添加随机数
* 热点数据永不过期

## 缓存穿透&#x20;

* 缓存和数据库都不存在的数据
* 数据校验，不合法直接返回
* 缓存中存不存在的key null值
* bloom 过滤器

## 缓存击穿

* 热点数据，缓存不在，但是数据库存在。
* 设置热点数据永不过期
* 接口限流，熔断，降级
* 加上互斥锁

## 缓存预热方案&#x20;

* 定时任务
* nginx 动静分离
* 客户端本地缓存
