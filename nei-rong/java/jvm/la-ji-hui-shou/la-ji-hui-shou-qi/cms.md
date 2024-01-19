---
description: CMS
---

# CMS

## 适用

* 小内存
* 响应时间优先



## 回收时机

* 达到回收阈值，即当老年代的空间使用率达到阈值时，会执行一次CMS回收。
* 参数-XX:CMSInitiatingOccupancyFraction，默认是68%



## 漏标方案

* 增量更新
* 黑色对象在自己标记后又指向了白色对象，就将该黑色对象记录下来，在重新标记阶段对其引用进行重新扫描
* 重新扫描黑色对象，导致会稍微浪费点时间



## 回收算法

* 标记清除
* 先标记需要清除的对象，然后清除其内存
* 会产生内存碎片
* UseCMS-CompactAtFullCollection fullGC时会进行内存整理,默认开启
* CMSFullGCsBefore-Compaction 执行几次fullGc会进行内存整理，默认0次





