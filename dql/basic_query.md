---
description: 了解表查询的基本语法和统计函数
---

# 基本查询

## 简单语法 <a id="simple_syntax"></a>

下面的语法经过大量精简，但已能满足基本使用。完整版本请查阅[官方文档](https://dev.mysql.com/doc/refman/8.0/en/select.html)。

```sql
SELECT <字段名> [, ...]
[FROM <表名> [, ...]]
[WHERE 条件表达式]
[GROUP BY <字段名> [, ...]]
[HAVING 条件表达式]
[ORDER BY <字段名> [, ...]]
[LIMIT <最多行数> | <开始行数, 最多行数>]
```

## 入门 <a id="entry"></a>

查看[表](../ddl/table.md)中的数据。

```sql
-- 查询所有字段
SELECT * FROM address;

-- 查询指定字段
SELECT district FROM address;
```

## 查询字段 <a id="query_field"></a>

> 很容易看出，查询结果是若干个数据记录（数据行）的集合，因此它也可以当作是一张临时的[表](../ddl/table.md)来看待。也就是说，查询结果也应该要有字段（表头）。

在这里，查询结果的字段统一成为“查询字段”。

默认情况下，查询字段名等同于它的表达式。比如 `SELECT distinct FROM address` 的查询字段名是“distinct”， `SELECT 1+2` 的查询字段名是“1+2”。

### 别名 <a id="field_another_name"></a>

当为查询字段指定了别名之后，别名就是它的查询字段名。

因为起了别名，所以下面两条语句的查询字段名都是“详细地址”。

```sql
-- 给查询字段起别名
SELECT address AS 详细地址 FROM address;

-- 给查询字段起别名（“AS”可忽略）
SELECT address 详细地址 FROM address;
```

### 计算字段 <a id="calculate_on_field"></a>

你可能已经发现，查询字段其实上是一个表达式，表达式就意味着可计算。

示例如下：

```sql
-- “1+2”作为计算字段，可忽略 FROM 子句
SELECT 1+2;

-- “amount*6.8”作为计算字段，它的别名是“人民币”
SELECT payment_id, amount*6.8 人民币 FROM payment;
```

{% hint style="info" %}
如果全部查询字段都不依赖于任何[表](../ddl/table.md)，那么可以忽略 [`FROM`](basic_query.md#lai-yuan-from) 子句。
{% endhint %}

## 查询子句 <a id="query_clause"></a>

### 来源（FROM） <a id="from"></a>

数据源可以是[表](../ddl/table.md)、[视图](../ddl/view.md)、其它查询结果以及它们的融合（连接）。

```sql
-- 使用其它查询结果作为数据源
SELECT 详细地址 FROM (
  SELECT distict AS 详细地址 FROM address
);
```

数据源也可以指定别名，示例如下：

```sql
-- 为 address 表指定别名为“addr”
SELECT distinct FROM address addr;
SELECT distinct FROM address AS addr;
```

### 过滤（WHERE） <a id="where"></a>

使用 `WHERE` 子句可以对[数据源](basic_query.md#from)进行过滤，只有符合过滤条件的数据记录才能被进一步处理。

示例如下：

```sql
-- 查询所有在1998年1月1日之后出生的女学生ID
SELECT id FROM student
WHERE sex='女' and birthdate>'1998-1-1';
```

### 分组（GROUP BY） <a id="group_by"></a>

_待完成..._

### 分组过滤（HAVING） <a id="having"></a>

_待完成..._

### 排序（ORDER BY） <a id="order_by"></a>

_待完成..._

### 去除重复记录（DISTINCT） <a id="distinct"></a>

使用  `DISTINCT` 关键字可以把多条完全一模一样的记录归并为一条。

```sql
-- 消除重复记录
SELECT DISTINCT district FROM address;
```

### 指定结果范围（LIMIT） <a id="limit"></a>

在查询结果中，每条记录表示为一行数据。

使用 `LIMIT` 子句可以指定你要的查询结果片段，参数为“起始行数”和“片段长度”。

```sql
-- 输出前5条记录
SELECT * FROM address LIMIT 5;

-- 输出在第5条之后的10条记录
SELECT * FROM address LIMIT 5, 10;
```



