---
description: 数据记录的插入、更新、删除以及清空操作
---

# 表操作

## 插入数据（Insert） <a id="insert"></a>

基本语法：

```sql
INSERT INTO <表名> [ <字段名> [,…] ]
VALUES (<常量> [,…]) [ (,…) ];
```

示例：

```sql
-- 插入完整数据记录
INSERT INTO student
VALUES ('201507001', '张三', '男', '1996-2-1', '汉族', '共青团员');

-- 插入部分数据记录
INSERT INTO student(id, name, sex)
VALUES ('201507001', '张三', '男');

-- 同时插入多条数据记录
INSERT INTO Departments
VALUES ('A001', '办公室'), 
('A002', '人事处'), 
('A003', '宣传部');
```

## 更新数据（Update） <a id="update"></a>

基本语法：

```sql
UPDATE <表名>
SET <字段名>=<表达式> [,…]
[ WHERE <条件> ];
```

示例：

```sql
-- 更新所有姓名为“张三”的出生日期
UPDATE student
SET 出生日期='1995-02-01'
WHERE 姓名='张三';

-- 使用计算结果更新数据
UPDATE sc
SET 成绩=成绩*0.6;
```

## 删除数据（Delete） <a id="delete"></a>

基本语法：

```sql
DELETE FROM <表名>
[ WHERE <条件> ];
```

示例：

```sql
-- 删除所有姓名为“张三”的数据记录
DELETE FROM student
WHERE 姓名='张三';
```

## 清空数据（Truncate） <a id="truncate"></a>

基本语法：

```sql
TRUNCATE TABLE <表名>;
```

示例：

```sql
-- 清空 student 表
TRUNCATE TABLE student;
```

{% hint style="info" %}
 `TRUNCATE` 操作可以重置自动增长字段的计数器，这是 [`DELETE`](table_manipulation.md#delete) 操作所做不到的。
{% endhint %}

