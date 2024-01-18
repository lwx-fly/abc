# 线程池

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

## 线程池拒绝策略

* abort 丢弃且报异常。默认策略
* caller 由调用的线程执行
* discard 直接丢弃
* discardOldest 丢弃最老的
