---
description: 订单系统设计
---

# order

## 表结构设计

* 订单表 【订单号分表】
* 订单明细表 【订单号分表】
* 支付单表
* 地址表
* 冻结库存表
* 发货信息表



## 流程设计

* 通过订单状态定义状态机
