---
description: 了解用于查询的统计函数
---

# 统计函数

## 概述 <a id="summary"></a>

统计函数只能在[查询语句](basic_query.md)的[查询字段](basic_query.md#query_field)中使用。因为查询字段是一个表达式，所以它允许使用任何函数（包括统计函数和一般函数）。

统计函数每次处理一个[分组](basic_query.md#group_by)，它将分组中的所有数据进行统计，然后列出这个分组的统计结果。如果查询没有任何分组，那么系统默认把所有查询结果归为为一个分组。

## 函数列举 <a id="functions"></a>

### 计数 - COUNT\(field\) <a id="func_count"></a>

计算指定列的记录条数。如果使用 `*` （表示全体）作为参数则不忽略  `NULL` 值，否则将忽略 `NULL` 值。

```sql
-- 统计填有年龄的学生记录条数（忽略 NULL 值）
SELECT COUNT(age) FROM student;

-- 统计所有学生记录条数（不会忽略 NULL 值）
SELECT COUNT(*) FROM student;
```

### 求和 - SUM\(field\) <a id="func_sum"></a>

计算指定列的记录总和，将忽略 `NULL` 值。

```sql
-- 统计学生年龄的总和
SELECT SUM(age) FROM student;
```

### 平均值 - AVG\(field\) <a id="func_avg"></a>

计算指定列的记录平均值，将忽略 `NULL` 值。

```sql
-- 统计学生年龄的平均值
SELECT AVG(age) FROM student;
```

### 最小值 - MIN\(field\) <a id="func_min"></a>

计算指定列的记录最小值，将忽略 `NULL` 值。

```sql
-- 统计学生中的最小年龄
SELECT MIN(age) FROM student;
```

### 最大值 - MAX\(field\) <a id="func_max"></a>

计算指定列的记录最大值，将忽略 `NULL` 值。

```sql
-- 统计学生中的最大年龄
SELECT MAX(age) FROM student;
```

## 与一般函数的区别 <a id="differentiate"></a>

统计函数都只有一个参数（指定列），每次处理一个[分组](basic_query.md#group_by)，也因此只能在[查询语句](basic_query.md)中使用；一般函数可以有若干个参数，能在任何表达式中使用。

> 个人的理解是，如 `COUNT(field)` 是统计函数，而 `COUNT(a, b, c, ...)` 是一般函数。

