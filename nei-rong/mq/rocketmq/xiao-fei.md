# 消费

## 消费模式

* 集群模式。负载均衡
* 广播模式。



## 消费方式

* poll
* push&#x20;



## 可靠消费

* 重试-死信机制
* rebalance机制



## 顺序消费

* 发送端。
  * 发送端发送到指定queue
* 消费端。
  * 消费端实现 MessageListenerOrderly
