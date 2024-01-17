---
description: 集合
---

# collections

## hashmap 树化条件&#x20;

* 链表长度>8
* 数组长度>64

## hashmap 退化树条件

* 扩容时,左右两边红黑树节点小于等于临界值6个
* 移除元素 remove( ) 时，在removeTreeNode( ) 方\*\*\*检查红黑树是否满足退化条件，与结点数无关。如果红黑树根 root 为空，或者 root 的左子树/右子树为空，root.left.left 根的左子树的左子树为空，都会发生红黑树退化成链表。

## concurrent 1.7 put 流程&#x20;

* 计算二次hash值
* 计算segment 小数组下标。segment 容量为capacity / clevel
* 计算segment 下标。如segment 容量为32 ，那么就是二次hash值的高五位。

## concurrentHashMap 扩容时写请求&#x20;

* put的元素在扩容的链表前，可以正常执行
* put的元素正好是扩容的链表，需要阻塞
* put的元素是forwarded node 已经迁移完成的链表，可以帮助扩容

## copyOnWriteList 添加数据流程

* 加可重入锁
* 获取原数组的引用和长度
* 根据原数组复制一个新的数组，长度为原来的+1
* 将新添加的元素放在数组的最后一个位置上
* 修改array 数组引用，指向新数组
* 可重入锁解锁

## copyOnWriteList 结构&#x20;

* 可重入锁
* transient volalile Objet\[] array

