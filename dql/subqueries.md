---
description: 了解子查询和相关操作符
---

# 子查询

## 概述 <a id="summary"></a>

子查询又称“嵌套查询”，就是把某次[查询](basic_query.md)的结果当作另一次查询的[数据源](basic_query.md#from)，或者用于构成条件判断表达式。

{% hint style="info" %}
子查询的反复嵌套可以方便解决复杂问题，但嵌套层次过多会急剧增加[查询](basic_query.md)的耗时。为了避免查询时间过长，一般建议，能使用[连接查询](join_query.md)做到的就尽量要不使用子查询。
{% endhint %}

## 语法 <a id="syntax"></a>

要把一段[查询](basic_query.md)语句当作是子查询其实很简单，只需要将它用圆括号包裹起来即可，示例如下：

```sql
(SELECT ...)
```

## 相关操作符 <a id="operator"></a>

下面的操作符都必须构成一个条件表达式。

### ALL

在比较过程中，必须满足子查询结果的所有记录，实际上是对极值进行比较。需要与比较运算搭配使用。

```sql
-- 查询年龄比所有男生都要小的女生
SELECT * FROM student
WHERE 性别='女' AND 出生日期 < ALL (
  SELECT 出生日期 FROM student
  WHERE 性别='男'
);
```

### ANY

在比较过程中，只要满足子查询结果的任意一条记录即可（一旦满足就结束比较）。需要与比较运算搭配使用。

```sql
-- 查询年龄比最小的男生要大的所有女生
SELECT * FROM student
WHERE 性别='女' AND 出生日期 > ANY (
  SELECT 出生日期 FROM student
  WHERE 性别='男'
);
```

### IN

检查某个值是否存在于子查询结果中。

```sql
-- 查询“小明”的所选课程
SELECT * FROM sc
WHERE 学号 IN (
  SELECT 学号 FROM student
  WHERE 姓名='小明'
);
```

### EXISTS

检查子查询结果是否有记录存在，只要存在至少一条记录就返回 `TRUE` ，否则就返回 `FALSE` 。

```sql
-- 查询选修了课程的学生学号和姓名
SELECT 学号, 姓名 FROM student
WHERE EXISTS (
  SELECT * FROM sc
  WHERE student.学号=sc.学号
);
```



