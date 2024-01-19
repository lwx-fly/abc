# 并发

## volatile

* 有序性(内存屏障)
* 可见性(JIT)



## synchronized

* 字节码
  * monitorEnter(尝试获取锁,锁计数器+1)
  * monitorExit(锁计数器-1)
* 释放锁前总会阻塞后面的线程
  * 等待锁的线程不能中断等待
  * 等待锁的线程不能超时退出
  * 不能让已获取到锁的强制释放锁
* 执行完或有异常会自动释放锁

## wait 和sleep

* sleep 不会释放锁，wait 会释放锁
* Thread.sleep必须传时间，wait可传可不传，如果不传则一直阻塞

## aqs node结构

* waitstatus
* pre
* next
* thread
* nextWaiter

## aqs &#x20;

* state
* 阻塞队列&#x20;
  * 双向链表
* 支持阻塞和唤醒
