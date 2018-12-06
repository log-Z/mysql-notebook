---
description: 创建、修改以及删除数据库
---

# 管理数据库（Database）

## 创建数据库

基本语法：

```sql
CREATE {DATABASE|SCHEMA} [IF NOT EXISTS] 数据库名 
[DEFAULT] CHARACTER SET [=] 字符集 
[DEFAULT] COLLATE [=] 校对规则名;
```

示例：

```sql
-- 创建数据库，如果它还没存在
CREATE DATABASE IF NOT EXISTS database_test_01;
```

## 修改数据库

基本语法：

```sql
ALTER DATABASE 数据库名 [[DEFAULT] attr_name_1 value_name_1 ...];
```

示例：

```sql
-- 修改数据库字符集
ALTER DATABASE database_test_01 CHARACTER SET gbk;

-- 修改数据库排序方式
ALTER DATABASE database_test_01 COLLATE gbk_chinese_ci;
```

## 删除数据库

语法：

```sql
DROP DATABASE [IF EXISTS] 数据库名;
```

示例：

```sql
-- 删除数据库，如果它存在
DROP DATABASE IF EXISTS database_test_01;
```



