# 监控

## 命令

* top
* jps
* jstat【查看虚拟机运行时信息】  jstat -\<option› I-t] I-h\<lines>] \<vmid> \[\<interval> \[\<count>]]
  * option 参数
    * \-class：显示ClassLoader的相关信息。&#x20;
      * \-compiler：显示JIT编译的相关信息。&#x20;
      * <mark style="color:orange;">-gc：显示与GC相关的堆信息。</mark>
      * &#x20;\-gccapacity：显示各个代的容量及使用情况。
      * &#x20;\-gccause：显示垃圾回收相关信息（同-gcutil），同时显示最后一次或当前正在发生的垃圾回收的诱发原因。&#x20;
      * \-gcnew：显示新生代信息。
      * &#x20;\-gcnewcapacity：显示新生代大小与使用情况。
      * &#x20;\-gcold：显示老年代和永久代的信息。
      * &#x20;\-gcoldcapacity：显示老年代的大小。&#x20;
      * \-gcpermcapacity：显示永久代的大小
      * \-gcutil：显示垃圾回收信息。&#x20;
      * \-printcompilation：输出JIT编译的方法信息。
  * \-h参数可以指定在周期性数据输出时，输出多少行数据后输出一个表头信息。
  * interval参数用于指定输出统计数据的周期，单位为毫秒。&#x20;
  * \-t 参数表示加上时间戳
* jinfo【查看虚拟机参数】
* jmap
  * 导出dump文件
  * 查看内存信息



## 工具

* arthars
