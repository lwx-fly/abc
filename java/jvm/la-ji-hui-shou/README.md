# 垃圾回收

## 垃圾识别

* GC Root【可达性判断】
* 引用计数法
  * 不能解决循环引用问题

## 分代收集

* 组成
  * 新生代
    * Eden
      * s1
      * s0
  * 老年代
* 跨带收集【新生代引用老年代对象】
  * 卡表

## 分区收集