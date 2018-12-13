---
description: 了解表查询的基本语法和统计函数
---

# 基本查询

## 简单语法

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

## 入门

查看[表](../ddl/table.md)中的数据。

```sql
-- 查询所有字段
SELECT * FROM address;

-- 查询指定字段
SELECT district FROM address;
```

## 字段别名

每个查询字段都可以拥有自己的别名。

```sql
-- 给查询字段起别名
SELECT address AS 详细地址 FROM address;

-- 给查询字段起别名（“AS”可忽略）
SELECT address 详细地址 FROM address;
```

## 计算字段

在这里，计算字段指：使用一个表达式表示的查询字段。

如果全部计算字段都不依赖于任何[表](../ddl/table.md)，那么可以忽略 `FROM` 子句。

```sql
-- “1+2”作为计算字段，可忽略 FROM 子句
SELECT 1+2;

-- “amount*6.8”作为计算字段，它的别名是“人民币”
SELECT payment_id, amount*6.8 人民币 FROM payment;
```

## 去除重复记录

使用  `DISTINCT` 关键字可以把多条完全一模一样的记录归并为一条。

```sql
-- 消除重复记录
SELECT DISTINCT district FROM address;
```

## 指定结果范围

在查询结果中，每条记录表示为一行数据。

使用 `LIMIT` 关键字可以指定你要的查询结果片段，参数为“起始行数”和“片段长度”。

```sql
-- 输出前5条记录
SELECT * FROM address LIMIT 5;

-- 输出在第5条之后的10条记录
SELECT * FROM address LIMIT 5, 10;
```



