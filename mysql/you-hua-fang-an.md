---
description: 基于MySQL 5.7
---

# 优化方案

## SQL语句和索引优化

### 1. 使用正确的索引

给最常使用的查询字段上，添加相应的索引，这样才能提高查询的性能。

避免在 where 查询条件中使用 != 或者 &lt;&gt; 操作符，因为这些操作符会导致查询引擎放弃索引而进行全表扫描。

{% hint style="info" %}
我们应该尽可能的使用主键查询，而非其他索引查询，因为主键查询不会触发回表查询，因此节省了一部分时间，变相的提高了查询的性能。
{% endhint %}

{% hint style="info" %}
在 MySQL 5.0 之前的版本要尽量避免使用 or 查询，可以使用 union 或者子查询来替代，因为早期的 MySQL 版本使用 or 查询可能会导致索引失效，在 MySQL 5.0 之后的版本中引入了索引合并，简单来说就是把多条件查询，比如 or 或 and 查询的结果集进行合并交集或并集的功能，因此就不会导致索引失效的问题了。
{% endhint %}



### 2. 查询具体字段而非全部字段

这个比较好理解，少用 `select * from table_name`



### 3. 优化子查询

尽量使用 Join 语句来替代子查询，因为子查询是嵌套查询，而嵌套查询会新创建一张临时表，而临时表的创建与销毁会占用一定的系统资源以及花费一定的时间，但 Join 语句并不会创建临时表，因此性能会更高。

如：

```sql
SELECT * FROM a WHERE id IN (SELECT id FROM b WHERE b.type = 1);
```

可以替换成：

```sql
SELECT * FROM a
INNER JOIN b ON a.id = b.id 
WHERE b.type = 1;
```



### 4. 注意查询结果集

多表关联查询时，尽量使用数据量小的表驱动数据量大的表进行查询。

假设A表有1000000条数据，B表有10条数据，两表需要关联查询，则：

```sql
SELECT * FROM B LEFT JOIN A ON b.id = a.b_id;
```

或者

```sql
SELECT * FROM A RIGHT JOIN B ON b.id = a.b_id;
```



### 5. 不要在列上进行运算操作



### 6. 表设计时适当增加冗余字段



## 数据库结构优化



## 系统硬件优化





TODO 回表查询

