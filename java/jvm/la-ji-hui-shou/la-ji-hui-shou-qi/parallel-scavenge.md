---
description: ParallelScavge
---

# Parallel SCavenge

## 适用

* 吞吐量优先
* jdk8 默认垃圾回收器



## 并行参数

* ·-XX：+UseParNewGC（考虑到兼容性问题，JDK 9、JDK 10已经删除）：在新生代使用并行回收器。&#x20;
* ·-XX：+UseParallelOldGC：老年代使用并行回收器。
* &#x20;·-XX：ParallelGCThreads：设置用于垃圾回收的线程数。通常情况下可以和CPU数量相等，但在CPU数量比较多的情况下，设置相对较小的数值也是合理的。&#x20;
* ·-XX：MaxGCPauseMillis：设置最大垃圾回收停顿时间。它的值是一个大于0的整数。回收器在工作时，会调整 Java 堆大小或者其他一些参数，尽可能地把停顿时间控制在MaxGCPauseMillis以内。
* &#x20;·<mark style="color:red;">-XX：GCTimeRatio：设置吞吐量大小。它的值是一个 0 到 100 之间的整数。假设GCTimeRatio的值为n ，那么系统将花费不超过1/（1+n ）的时间用于垃圾回收</mark>。&#x20;
* ·<mark style="color:red;">-XX：+UseAdaptiveSizePolicy：打开自适应GC策略。在这种模式下，新生代的大小、eden区和survivior区的比例、晋升老年代的对象年龄等参数会被自动调整，以达到在堆大小、吞吐量和停顿</mark>



