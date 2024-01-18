# netty

Reactor 模式

* reactor线程。响应io事件，并分发到handler
* handler 处理器。非阻塞处理业务逻辑

Reactor 流程

* channel 通道注册。一个IO事件一定属于一个通道
* selector 查询事件。轮询，查询选择器中的事件。
* reactor事件分发。如果查询到了io事件，就分发给对应的处理器。
* handler。业务处理。

## netty 核心组件

* channel
* eventLoopGroup【reactor】
* pipeline【双向链表】
* selector
