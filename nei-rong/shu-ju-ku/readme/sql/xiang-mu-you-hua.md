# 项目优化

#### sql

现有导购会员关系表w，其中导购和会员对应关系为多对多

* 去重查询导购所在门店下客户列表
  1. 使用视图。定义关系去重表视图，并将查询条件尽可能追加到视图后面进行查询。
  2. 设置一个最大阈值，例如查询前1w条数据。然后再将取出的数据在内存中去重，分页。【代码简单，改动小，但是会有数据准确性的问题】
  3. 引入es ，在es 中进行复杂查询。
* 新增关联会员统计表，包括会员消费时间，消费金额等字段，要支持字段搜索、列表展示以及排序。
  * 使用explain 观察sql 执行情况，避免file sort 等情况
  * 搜索和排序字段添加索引
  * 更改表连接方式。无查询条件和排序条件时使用left join 获取信息展示，其他通过inner join进行数据筛选。

#### 优化器

* 通过标签筛选符合的用户。标签支持多选。多个标签组为或的关系
  * 先通过标签查询符合对应的客户，将对应的子查询代入到客户关系表中进行筛选
  * 当子查询数量查过5个时，页面上会出现多条重复的数据
  * 关闭mysql8 优化器semi-join。