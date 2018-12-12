---
description: 创建、修改、删除以及查看索引
---

# 索引结构（Index）

在一定程度上 `索引` 可以提高 [`表`](table.md) 的查询速度，但它会也占用额外的存储空间。

## 创建索引 <a id="create_index"></a>

基本语法：

```sql
-- CREATE 方法
CREATE [ UNIQUE | FULLTEXT | SPATIAL ] INDEX <索引名称>
[ USING index_type ]
ON <表名> ( 索引字段 [ ASC | DESC ] [,...] );
```

示例：

```sql
-- 使用 Create 方法创建
-- 为 student 表的“学号”字段创建唯一性索引，使用降序排序
CREATE UNIQUE INDEX istudent
ON student(学号 DESC);

-- 建表时创建
-- 为 book 创建全文索引
CREATE TABLE book (
  isbn CHAR(13) PRIMARY KEY,
  书名 CHAR(100) NOT NULL,
  出版日期 DATE NOT NULL,
  FULLTEXT INDEX ibook (内容摘要)
);
```

## 添加索引 <a id="add_index"></a>

基本语法：

```sql
ALTER TABLE <表名> ADD INDEX <索引名> (索引字段 [ ASC | DESC ] [,...]);
```

示例：

```sql
-- 为 sc 表的“学号”和“课程号”添加复合索引
ALTER TABLE sc ADD INDEX isc (学号,课程号);
```

## 删除索引 <a id="drop_index"></a>

基本语法：

```sql
-- Drop 方法
DROP INDEX <索引名> ON <表名>;

-- Alter 方法
ALTER TABLE <表名> DROP INDEX <索引名>;
```

示例：

```sql
-- 使用 Drop 方法删除
-- 删除 student 表中的 istudent 索引
DROP INDEX istudent ON student;

-- 使用 Alter 方法删除
-- 删除 sc 表中的 isc 索引
ALTER TABLE sc DROP INDEX isc;
```

## 查看索引 <a id="show_index"></a>

语法：

```sql
SHOW CREATE TABLE <表名>;
```

{% hint style="info" %}
可参考 `SHOW` 命令。
{% endhint %}

