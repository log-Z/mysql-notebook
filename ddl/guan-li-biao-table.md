---
description: 创建、修改以及删除表
---

# 管理表（Table）

## 创建表（Create）

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

## 修改表（Alter）

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

## 删除表（Drop）

语法：

```sql
DROP [TEMPORARY] TABLE [IF EXISTS] <表名> [,<表名>...];
```

示例：

```sql
-- 删除sc表，如果它存在
DROP TABLE IF EXISTS sc;
```



