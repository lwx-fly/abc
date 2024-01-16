# rocketmq

## 事务消息

* 半消息

## 顺序消费

* 发送端。
  * 发送端发送到指定queue
* 消费端。
  * 消费端实现 MessageListenerOrderly



## 保证不丢消息

* producer。消息发送时，发送失败重试机制
* broker。消息发送时设置同步刷盘。flushDiskType=SYNC\_FLUSH
* broker主从同步。设置brokerRole=SYNC\_MASTER
* consumer。
