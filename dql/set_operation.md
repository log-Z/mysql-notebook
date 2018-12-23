---
description: 了解查询结果之间的集合运算
---

# 集合运算

## 概述 <a id="summary"></a>

集合运算是针对与[查询](basic_query.md)结果的，可以把两个查询结果融合成一个新的查询结果（另一种融合方法是[连接](join_query.md)）。

常见的集合运算有交集、并集和差集，但有点遗憾， MySQL中只支持并集运算。

{% hint style="info" %}
一般来说，如果两查询结果需要进行集合运算，那么它们的字段应该是一致的（数量、名称和顺序）。
{% endhint %}

## 并集 <a id="union"></a>

| 关键字 | 意义 |
| :--- | :--- |
| UNION ALL |  把两个查询结果合并到一起。 |
| UNION |  把两个查询结果合并到一起，并去除重复数据记录。 |

语法：

```sql
SELECT ...
UNION | UNION ALL
SELECT ...
```

示例：

```sql
-- 查询女生或大于22岁的所有学生，并去除重复记录
SELECT * FROM student WHERE 性别='女'
UNION
SELECT * FROM student WHERE YEAR(NOW())-YEAR(出生日期)<=22;
```

