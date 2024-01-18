# concurrentHashmap

##

## &#x20;扩容时写请求

* put的元素在扩容的链表前，可以正常执行
* put的元素正好是扩容的链表，需要阻塞
* put的元素是forwarded node 已经迁移完成的链表，可以帮助扩容
