---
description: 创建、修改以及删除视图结构
---

# 视图结构（View）

视图属于外模式，是若干张 [`表`](table.md) 的聚合表示，因此它需要依附于 [`表`](table.md) 而存在。

视图可以当作 [`表`](table.md) 来使用，可以进行 `DML` （增删改） 和 `DQL` （查询）操作。

## 创建视图 <a id="create_view"></a>

基本语法：

```sql
CREATE VIEW <视图名> AS
SELECT ...
```

示例：

```sql
-- 创建选课详细视图
CREATE VIEW v_sc AS
SELECT 学号, 姓名, 课程号, 课程名称, 成绩
FROM student s JOIN sc USING(学号) JOIN course c USING (课程号);
```

## 修改视图 <a id="alter_view"></a>

基本语法：

```sql
-- 方法一：直接修改视图
ALTER VIEW <视图名> AS
SELECT ...

-- 方法二：创建新视图，并覆盖原有视图，使用“OR REPLACE”修饰
CREATE OR REPLACE VIEW <视图名> AS
SELECT ...
```

示例：

```sql
-- 方法一：直接修改视图
ALTER VIEW v_sc AS
SELECT 学号, 姓名, 性别, 课程号, 课程名称, 成绩
FROM student s JOIN sc USING(学号) JOIN course c USING (课程号);

-- 方法二：覆盖原有视图，使用  OR REPLACE  修饰
CREATE OR REPLACE VIEW v_sc AS
SELECT 学号, 姓名, 性别, 课程号, 课程名称, 成绩
FROM student s JOIN sc USING(学号) JOIN course c USING (课程号);
```

## 删除视图 <a id="drop_view"></a>

基本语法：

```sql
DROP VIEW <视图名>;
```

示例：

```sql
-- 删除v_sc视图
DROP VIEW v_sc;
```



