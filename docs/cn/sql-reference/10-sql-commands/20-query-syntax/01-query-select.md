---
title: SELECT
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新版本：v1.2.435"/>

import DetailsWrap from '@site/src/components/DetailsWrap';

从表中检索数据。

-- Databend 认为 NULL 值大于任何非 NULL 值。
-- 在以下按升序排序结果的示例中，NULL 值出现在最后：

SELECT number FROM t_null order by number ASC;
+--------+
| number |
+--------+
|      1 |
|      2 |
|      3 |
|   NULL |
|   NULL |
+--------+

-- 要使 NULL 值在前面的示例中首先出现，请使用 NULLS FIRST 选项：

SELECT number FROM t_null order by number ASC nulls first;
+--------+
| number |
+--------+
|   NULL |
|   NULL |
|      1 |
|      2 |
|      3 |
+--------+

-- 使用 NULLS LAST 选项使 NULL 值在降序排序中最后出现：

SELECT number FROM t_null order by number DESC nulls last;
+--------+
| number |
+--------+
|      3 |
|      2 |
|      1 |
|   NULL |
|   NULL |
+--------+
```

## LIMIT 子句

```sql
SELECT number FROM numbers(1000000000) LIMIT 1;
+--------+
| number |
+--------+
|      0 |
+--------+

SELECT number FROM numbers(100000) ORDER BY number LIMIT 2 OFFSET 10;
+--------+
| number |
+--------+
|     10 |
|     11 |
+--------+
```

为了优化具有大结果集的查询性能，Databend 默认启用了 `lazy_read_threshold` 选项，默认值为 1,000。此选项专门设计用于涉及 LIMIT 子句的查询。当 `lazy_read_threshold` 启用时，优化会在查询中指定的 LIMIT 数小于或等于您设置的阈值时激活。要禁用此选项，请将其设置为 0。

<DetailsWrap>

<details>
  <summary>工作原理</summary>
    <div>此优化提高了带有 ORDER BY 子句和 LIMIT 子句的查询性能。当启用且查询中的 LIMIT 数小于指定阈值时，仅检索并排序涉及 ORDER BY 子句的列，而不是整个结果集。</div><br/><div>系统检索并排序涉及 ORDER BY 子句的列后，应用 LIMIT 约束从排序后的结果集中选择所需数量的行。然后，系统返回有限的行作为查询结果。这种方法通过仅获取和排序必要的列来减少资源使用，并通过将处理的行限制为所需的子集进一步优化查询执行。</div>
</details>

</DetailsWrap>

```sql
SELECT * FROM hits WHERE URL LIKE '%google%' ORDER BY EventTime LIMIT 10 ignore_result;
Empty set (0.300 sec)

set lazy_read_threshold=0;
Query OK, 0 rows affected (0.004 sec)

SELECT * FROM hits WHERE URL LIKE '%google%' ORDER BY EventTime LIMIT 10 ignore_result;
Empty set (0.897 sec)
```

## OFFSET 子句

```sql
SELECT number FROM numbers(5) ORDER BY number OFFSET 2;
+--------+
| number |
+--------+
|      2 |
|      3 |
|      4 |
+--------+
```

## IGNORE_RESULT

不输出结果集。

```sql
SELECT number FROM numbers(2);
+--------+
| number |
+--------+
|      0 |
|      1 |
+--------+

SELECT number FROM numbers(2) IGNORE_RESULT;
-- 空集
```

## 嵌套子查询

SELECT 语句可以在查询中嵌套。

```
SELECT ... [SELECT ...[SELECT [...]]]
```

```sql
SELECT MIN(number) FROM (SELECT number%3 AS number FROM numbers(10)) GROUP BY number%2;
+-------------+
| min(number) |
+-------------+
|           1 |
|           0 |
+-------------+
```