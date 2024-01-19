# 设计

## 前缀索引

> 索引的选择性是指，不重复的索引值（也称为基数，cardinality）和数据表的记录总数（＃T）的比值，范围从1/＃T到1之间。索引的选择性越高则查询效率越高，因为选择性高的索引可以让MySQL在查找时过滤掉更多的行。唯一索引的选择性是1.

* 优点
  * 提升索引性能
  * 减少索引空间
* 缺点
  * 无法进行order by 排序
  * 无法进行分组
* 长度的确定
  * 前缀的选择性能够接近0.031

```sql
 SELECT COUNT(DISTINCT LEFT(city, 3))/COUNT(*) AS sel3,
 COUNT(DISTINCT LEFT(city, 4))/COUNT(*) AS sel4,
 COUNT(DISTINCT LEFT(city, 5))/COUNT(*) AS sel5,
 COUNT(DISTINCT LEFT(city, 6))/COUNT(*) AS sel6,
 COUNT(DISTINCT LEFT(city, 7))/COUNT(*) AS sel7
 FROM sakila. city_demo;

```



## 联合索引

> <mark style="color:red;">在多列上独立地创建多个单列索引，在大部分情况下并不能提高MySQL的查询性能。</mark>MySQL引入了一种叫“索引合并”（index merge）的策略，它在一定程度上可以使用表中的多个单列索引来定位指定的行。在这种情况下，查询能够同时使用两个单列索引进行扫描，并将结果进行合并

## 合适的索引顺序【 查询 排序 分组】&#x20;

> 有a b c 列组成的联合索引

* a and b 或者 b and a 可以走到索引
* 单独通过b 或c 查询 走不到索引



## 索引排序规则

* 只有当索引的顺序和ORDER BY子句的顺序完全一致，并且所有列的排序方向（倒序或正序）都一样时，MySQL才能使用索引来对结果做排序
* 如果前导列为常量的时候，ORDER BY子句中的列也可以不满足索引的最左前缀的要求。

## 不能通过索引排序

> 有表结构如下
>
> ```sql
> CREATE TABLE rental (
> PRIMARY KEY (rental_id),
> UNIQUE KEY rental
> _date (rental_date, inventory_id, customer_id),
> KEY idx_fk_inventory_id (inventory_id),
> KEY idx_fk_customer_id (customer_id),
> KEY idx_fk_staff_id (staff.
> _id）
> ```

* order by 字段升降序不同
* order by 有字段不在索引中
* where 和order 字段无法组成最左前缀
* 查询在索引列的第一列上是范围条件，所以MySQL无法使用索引的其余列：
* 查询在inventory\_id列上有多个等于条件。对于排序来说，这也是一种范围查询&#x20;





