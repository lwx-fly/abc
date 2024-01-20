---
cover: >-
  https://images.unsplash.com/photo-1702255489644-392758161f1f?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MDU2NDQyMzd8&ixlib=rb-4.0.3&q=85
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 简介

解析一次接口请求

## 前置处理

* dns 域名解析
* tcp 三次握手
* nginx 反向代理
* https 加密处理
* 网关
  * 权限校验【header 判断】
  * 限流
* 注册中心
* 服务器负载均衡

## 业务逻辑处理

* 参数反序列化
* 修改
  * 分布式锁
  * synchronized
* 、查询
  * 数据库查询
  * 缓存
* rpc调用
  * 熔断
  * 降级
  * completableFuture 合并请求
  * 分布式事务
*   异步逻辑处理

    * mq 消息
    * async 注解

