# refresh流程

* beanFactory相关流程
  * 环境准备。envirment
  * 获取beanFactory
  * 准备beanFactory。$ 支持
  * 子类拓展
  * beanFactory对象解析，支持bean Configuration等注解
  * beanPostProcesser拓展，支持autowired ，resource注解
* applicationContext 相关流程
  * 国际化
  * 发布事件给监听器
  * onfresh子类拓展
  * 接收监听器发布的事件
  * 非延迟单例bean加载
  * 添加lifeCycleProcessor,控制容器内需要生命周期管理的bean.发布事件
