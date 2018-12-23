---
description: 了解交叉连接、内连接和外连接的概念和写法
---

# 连接查询

## 概述 <a id="summary"></a>

“连接”是将多个[数据源](basic_query.md#from)融合到一起的一种方法（另一种方法是集合运算）。

## 演示数据 <a id="demo_data"></a>

你可以先跳过这一节，演示数据仅用于理解各种连接方式的差异。

学生表数据：

| 学号 | 姓名 |
| :--- | :--- |
| 001 | 张三 |
| 002 | 李四 |
| 003 | 王五 |

选课表数据：

| 学号 | 课程号 |
| :--- | :--- |
| 001 | 0000001 |
| 001 | 0000002 |
| 002 | 0000001 |
| 004 | 0000001 |

## 交叉连接 <a id="cross_join"></a>

### 概念 <a id="concept_of_cross_join"></a>

若将数据源 A 和 B 进行交叉连接，即执行了笛卡尔积运算，最终会得到 $$N_{A}N_{B}$$ 行数据。  

按“学号”把[演示数据](join_query.md#yan-shi-shu-ju)两表进行交叉连接，会得到如下结果：

| 学生表.学号 | 姓名 | 课程表.学号 | 课程号 |
| :--- | :--- | :--- | :--- |
| 001 | 张三 | 001 | 0000001 |
| 001 | 张三 | 001 | 0000002 |
| 001 | 张三 | 002 | 0000001 |
| 001 | 张三 | 004 | 0000001 |
| 002 | 李四 | 001 | 0000001 |
| 002 | 李四 | 001 | 0000002 |
| 002 | 李四 | 002 | 0000001 |
| 002 | 李四 | 004 | 0000001 |
| 003 | 王五 | 001 | 0000001 |
| 003 | 王五 | 001 | 0000002 |
| 003 | 王五 | 002 | 0000001 |
| 003 | 王五 | 004 | 0000001 |

### 写法 <a id="usage_cross_join"></a>

在 [`FROM`](basic_query.md#from) 子句中指定多个数据源。示例如下：

```sql
SELECT student.学号, 姓名, 成绩
FROM student, course, sc;
```

## 内连接 <a id="inner_join"></a>

### 概念 <a id="concept_of_inner_join"></a>

内连接需要指定关联字段，用来表示两[数据源](basic_query.md#from)之间的关联。只有两数据源的关联字段的共有部分才会被连接，非共有部分的数据都将被丢弃（类似于一个交集）。

按“学号”把[演示数据](join_query.md#yan-shi-shu-ju)两表进行内连接，将得到如下结果：

| 学号 | 姓名 | 课程号 |
| :--- | :--- | :--- |
| 001 | 张三 | 0000001 |
| 001 | 张三 | 0000002 |
| 002 | 李四 | 0000001 |

> 学号是关联字段，但学号“003”和“004”不是两数据源共有的，因此不会出现在内连接中。

### 写法一 <a id="usage_inner_join_1"></a>

基于[交叉连接](join_query.md#cross_join)改进，在 [`WHERE`](basic_query.md#where) 子句中添加关联条件，示例如下：

```sql
SELECT s.学号, 姓名, 成绩
FROM student s, course c, sc
WHERE s.学号=sc.学号 AND c.课程号=sc.课程号;
```

### 写法二 <a id="usage_inner_join_2"></a>

使用 `INNER JOIN ... ON ...` 语法，示例如下：

```sql
-- ON 的后面是关联条件
SELECT s.学号, 姓名, 成绩
FROM student s INNER JOIN sc ON s.学号=sc.学号
  JOIN course c ON c.课程号=sc.课程号;
```

### 写法三 <a id="usage_inner_join_3"></a>

使用 `INNER JOIN ... USING (...)` 语法，当两[数据源](basic_query.md#from)的关联字段名一致时，可以使用此写法简化关联条件。关联字段名因为必须是一致的，所以可以不用指定数据源，前提是不与其它数据源发生冲突。示例如下：

```sql
-- student、sc 和 course 表的关联字段名都是“学号”
-- 查询字段“学号”不用指定数据源
SELECT 学号, 姓名, 成绩
FROM student s INNER JOIN sc USING(学号)
  JOIN course c USING(课程号);
```

> 在 MySQL 中，连接默认使用内连接，也就是说 `JOIN` 和 `INNER JOIN` 表示的意思是一致的。

## 左外连接 <a id="left_outer_join"></a>

### 概念 <a id="concept_of_left_out_join"></a>

### 写法 <a id="usage_left_out_join"></a>

## 右外连接 <a id="right_outer_join"></a>

### 概念 <a id="concept_of_right_out_join"></a>

### 写法 <a id="usage_right_out_join"></a>

