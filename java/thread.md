---
description: 多线程
---

# thread

## volatile&#x20;

* 有序性(内存屏障)
* 可见性(JIT)

## 线程池参数

* coreSize
* maxSize
* keepAliveTime
* TimeUnit
* ThreadFactory
* reject policy
* work queue

## 有新任务添加到线程池

* 判断当前线程数量是否达到了coreSize，如没有则+1
* 判断工作队列是否已满，未满则+1
* 判断当前线程数量是否达到了maxSize，如没有则+1。存活keepAliveTime
* 工作队列已满且线程数达到最大线程数，则执行拒绝策略。

## 线程池拒绝策略&#x20;

* abort 丢弃且报异常。默认策略
* caller 由调用的线程执行
* discard 直接丢弃
* discardOldest 丢弃最老的

## synchronized&#x20;

* 字节码
  * monitorEnter(尝试获取锁,锁计数器+1)
  * monitorExit(锁计数器-1)
* 释放锁前总会阻塞后面的线程
  * 等待锁的线程不能中断等待
  * 等待锁的线程不能超时退出
  * 不能让已获取到锁的强制释放锁
* 执行完或有异常会自动释放锁

## wait 和sleep&#x20;

* sleep 不会释放锁，wait 会释放锁
* Thread.sleep必须传时间，wait可传可不传，如果不传则一直阻塞

## Object.wait()和LockSupport.park()的区别&#x20;

* Object.wait()方法需要在synchronized块中执行；
* LockSupport.park()可以在任意地方执行；
* Object.wait()方法声明抛出了中断异常，调用者需要捕获或者再抛出；
* LockSupport.park()不需要捕获中断异常；
* Object.wait()不带超时的，需要另一个线程执行notify()来唤醒，但不一定继续执行后续内容；
* LockSupport.park()不带超时的，需要另一个线程执行unpark()来唤醒，一定会继续执行后续内容；

## aqs node结构&#x20;

* waitstatus
* pre
* next
* thread
* nextWaiter

