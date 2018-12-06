---
description: 创建、修改以及删除表和它的约束
---

# 管理表（Table）

## 基础知识

### 创建表

基本语法：

```sql
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] <表名> 
[(<字段名> <数据类型> [完整性约束条件][,…])] 
[表的选项];
```

示例：

```sql
-- 创建表，如果它还没存在
CREATE TABLE IF NOT EXISTS Employees(
  E_ID VARCHAR(8) PRIMARY KEY COMMENT '员工编号',
  E_NAME VARCHAR(8) NOT NULL COMMENT '员工姓名',
  SEX VARCHAR(2) DEFAULT '男' COMMENT '性别',
  PROFESSIONAL VARCHAR(6) COMMENT '职称',
  D_ID VARCHAR(5) NOT NULL COMMENT '与Departments表关联',
  CONSTRAINT fk_did FOREIGN KEY(D_ID) REFERENCES Departments(D_ID)
);
```

### 修改表

基本语法：

```sql
ALTER TABLE <表名>
{[ADD <新字段名> <数据类型> [<完整性约束条件>][,…]]
|[ADD INDEX [索引名] (索引字段,...)]
|[MODIFY COLUMN <字段名> <新数据类型> [<完整性约束条件>]]
|[DROP {COLUMN <字段名>| <完整性约束名>}[,…]]
|DROP INDEX <索引名>
|RENAME [AS] <新表名>
};
```

示例：

```sql
-- 添加字段：向student表添加“专业”字段
ALTER TABLE student ADD 专业 CHAR(30);

-- 修改字段的数据类型：修改course表中学分的数据类型
ALTER TABLE course MODIFY 学分 SMALLINT;

-- 删除字段：删除student表中的“专业”字段
ALTER TABLE student DROP COLUMN 专业;

-- 重命名表：重命名student表为stu
ALTER TABLE student RENAME AS stu;
```

### 删除表

语法：

```sql
DROP [TEMPORARY] TABLE [IF EXISTS] <表名> [,<表名>...];
```

示例：

```sql
-- 删除sc表，如果它存在
DROP TABLE IF EXISTS sc;
```

## PRIMARY KEY 约束

 `PRIMARY KEY` 指代主键，主键的值必须唯一且不可为`NULL` 。

### 创建方法

创建 `PRIMARY KEY` 约束有两种方法：

1. 直接在每条字段定义之后声明。这种方式不能指定约束的名称，不能创建联合主键约束（即多个字段共同构成主键）。
2. 使用单独的子句声明。这种方式可以指定约束的姓名，允许创建联合主键。

语法：

```sql
-- 方法二
[CONSTRAINT <约束名>] PRIMARY KEY (字段名[,…])
```

示例：

```sql
-- 方法一：直接追加在字段定义之后
CREATE TABLE student (
  学号 CHAR(9) PRIMARY KEY,
  姓名 VARCHAR(10)
);

-- 方法二：单独创建
CREATE TABLE student (
  学号 CHAR(9),
  姓名 VARCHAR(10),
  CONSTRAINT p_xh PRIMARY KEY (学号)
);

-- 方法二：创建联合主键
CREATE TABLE sc (
  学号 CHAR(9),
  课程号 CHAR(5),
  成绩 DECIMAL(4, 1),
  CONSTRAINT p_xh_kch PRIMARY KEY (学号, 课程号)
);
```

### 删除方法

语法：

```sql
ALTER TABLE <表名> DROP PRIMARY KEY;
```

示例：

```sql
-- 删除student表的主键约束
ALTER TABLE student DROP PRIMARY KEY;
```

## FOREIGN KEY 约束

`FOREIGN KEY` 指代外键，外键的值必须是另一个表中某个字段已有的值，这表示一种参照关系（如“选课表”的学号需要参照“学生表”的学号）。外键的值可以为`NULL` 。

### 创建方法

语法：

```sql
[CONSTRAINT <约束名>]
FOREIGN KEY (字段名[,…])
REFERENCES 引用表名(引用表字段名[,…])
```

示例：

```sql
-- 为“学号”和“课程号”添加外键约束
CREATE TABLE sc (
  学号 CHAR(9),
  课程号 CHAR(5),
  成绩 DECIMAL(4, 1),
  CONSTRAINT f_xh FOREIGN KEY (学号) REFERENCES student(学号),
  CONSTRAINT f_kch FOREIGN KEY (课程号) REFERENCES course(课程号)
);
```

### 删除方法

语法：

```sql
ALTER TABLE <表名> DROP FOREIGN KEY <约束名>;
```

示例：

```sql
ALTER TABLE sc DROP FOREIGN KEY f_xh;
```

##  UNIQUE 约束

`UNIQUE` 指代唯一值。有唯一值约束的字段不允许出现重复值。

### 创建方法

创建 `UNIQUE` 约束有两种方法：

1. 直接在每条字段定义之后声明。这种方式不能指定约束的名称。
2. 使用单独的子句声明。这种方式可以指定约束的姓名。

语法：

```sql
-- 方法二
[CONSTRAINT <约束名>] UNIQUE (字段名[,…])
```

示例：

```sql
-- 方法一：直接追加在字段之后
CREATE TABLE student (
  学号 CHAR(9),
  姓名 VARCHAR(10),
  身份证号 CHAR(18) UNIQUE
);

-- 方法二：单独创建
CREATE TABLE student (
  学号 CHAR(9),
  姓名 VARCHAR(10),
  身份证号 CHAR(18),
  CONSTRAINT u_sfz UNIQUE (身份证号)
);
```

### 删除方法

语法：

```sql
ALTER TABLE <表名> DROP KEY <约束名>;
```

示例：

```sql
ALTER TABLE student DROP KEY u_sfz;
```

##  NOT NULL 约束

`NOT NULL` 指代非空。有非空约束的字段的值不允许为 `NULL` 。

### 创建方法

直接在每条字段定义之后声明。

语法：

```sql
<字段名> <数据类型> [NOT NULL|NULL]
```

示例：

```sql
-- 为“身份证号”添加非空约束
CREATE TABLE student (
  学号 CHAR(9),
  姓名 VARCHAR(10),
  身份证号 CHAR(18) NOT NULL
);
```

### 修改和删除方法

语法：

```sql
ALTER TABLE <表名> MODIFY <字段名> <数据类型> [NOT NULL|NULL];
```

示例：

```sql
-- 设为可空（相当于删除 NOT NULL 约束）
ALTER TABLE student MODIFY 身份证号 CHAR(18) NULL;

-- 设为非空
ALTER TABLE student MODIFY 身份证号 CHAR(18) NOT NULL;
```

> 事实上，创建表时如果字段不声明为 `NOT NULL` （不可空），则会默认声明为 `NULL` （即可空）。

##  DEFAULT 约束

 `DEFAULT` 指代默认值。在插入数据时，如果没有向某个字段指定值，那么 MySQL 就会把这个字段的默认值作为新记录的值。

> 若字段未声明 `DEFAULT` 约束，则表示默认值为 `NULL` 。

### 创建方法

直接在每条字段定义之后声明。

语法：

```sql
<字段名> <数据类型> DEFAULT 默认值表达式
```

示例：

```sql
-- 建表时，为“政治面貌”字段设置默认值
CREATE TABLE student (
  学号 CHAR(9),
  姓名 VARCHAR(10),
  政治面貌 VARCHAR(8) DEFAULT '共青团员'
);
```

### 修改和删除方法

语法：

```sql
ALTER TABLE t_student ALTER COLUMN <字段名> SET DEFAULT 默认表达式;
```

示例：

```sql
-- 修改“政治面貌”字段的默认值
ALTER TABLE t_student ALTER COLUMN 政治面貌 SET DEFAULT '共青团员';

-- 删除默认值，即默认为NULL
ALTER TABLE t_student ALTER COLUMN 政治面貌 SET DEFAULT NULL;
```

## CHECK 约束

很遗憾，MySQL **暂不支持** `CHECK` 约束，直至 `8.1` 版本也都是如此。

在 SQL 中， `CHECK` 指代检查。检查约束用于审核字段的值是否正确。

> 在 MySQL 中请考虑使用 `触发器` 替代 `CHECK` 约束。

### 创建方法（骗人的）

请注意，以下语法不会报错，但其实什么事都不会发生，并不会真的创建了 `CHECK` 约束 😒😒😒 。

语法：

```sql
[CONSTRAINT <约束名>] CHECK (逻辑表达式)
```

示例：

```sql
-- 建表时添加约束
CREATE TABLE sc (
  学号 CHAR(9),
  课程 CHAR(5),
  成绩 DECIMAL(4, 1),
  CONSTRAINT c_score CHECK (成绩 BETWEEN 0 AND 100)
);

-- 建表后添加约束
ALTER TABLE sc
ADD CONSTRAINT c_score
CHECK (成绩 BETWEEN 0 AND 100);
```

### 

