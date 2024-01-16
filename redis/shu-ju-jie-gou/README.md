# 数据结构

## &#x20;底层数据结构&#x20;

* sds 简单动态字符串。效率高，存储了字符串的长度。二进制安全。
* hashtable
* intlist 整形数组
* ziplist 双向链表改造，减少内存开销
* quicklist 基于ziplist改造，减少ziplist插入带来的性能问题
* skiplist 基于双向链表改造，添加多级索引，提高检索效率



