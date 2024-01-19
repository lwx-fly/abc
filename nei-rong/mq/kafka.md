# kafka

## 为什么这么快

* 磁盘顺序读取
* 0拷贝
* 批处理
* 压缩技术

## 保证消息不丢失

* producer ,
  * 采用有回调的方法，避免由于网络抖动导致发送失败
  * ack
* broker
  * 在消息还在pageCache中，kafka没有同步刷盘策略，导致可能会丢失消息
  * min.insync.replicas 设置大于1
  * unclean.leader.election.enable设置为false，避免follower 和leader 差的很多情况下被选举成leader
  * 设置副本数量为大于等于3
* consumer
  * 关闭自动提交offset

## 消费模式

* 指定分区消费
* 指定offset消费
* 指定时间消费



