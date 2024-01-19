---
cover: >-
  https://images.unsplash.com/photo-1703817843977-11fe5a469e1e?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MDU2NDQyMzd8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# bean生命周期

* 从缓存中获取
* 获取beanFactory工厂
* 处理depends on
* 处理scope ，时候单例模式
* bean 创建
  * 实例化
    * 通过构造器反射方法创建bean
    * 通过属性注入实例化bean
      * byName
      * byType
    * 通过回调机制拓展bean
  * 依赖注入
    * 构造器注入
      * 【强制对象注入】
    * setter 注入
      * 【可选对象注入】
      * <mark style="color:red;">可以解决循环依赖问题</mark>
    * ~~字段注入.不推荐使用。对象无法脱离容器而独立运行。~~
  * 初始化
    * aware 方法处理
    * postprocessor后处理方法
    * aop切面
  * 消费方法处理
