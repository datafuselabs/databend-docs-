---
title: COUNT_DISTINCT
title_includes: uniq
---

聚合函数。

`count(distinct ...)` 函数用于计算一组值的唯一值数量。

为了在内存和时间有限的情况下从大数据集中获得估计结果，可以考虑使用 [APPROX_COUNT_DISTINCT](aggregate-approx-count-distinct.md)。

:::caution
 NULL 值不会被计数。
:::

## 语法

```sql
COUNT(distinct <expr> ...)
UNIQ(<expr>)
```

## 参数

| 参数      | 描述                                      |
|-----------|--------------------------------------------------|
| `<expr>`  | 任何表达式，参数的数量范围为 [1, 32] |

## 返回类型

UInt64

## 示例

**创建表并插入示例数据**
```sql
CREATE TABLE products (
  id INT,
  name VARCHAR,
  category VARCHAR,
  price FLOAT
);

INSERT INTO products (id, name, category, price)
VALUES (1, 'Laptop', 'Electronics', 1000),
       (2, 'Smartphone', 'Electronics', 800),
       (3, 'Tablet', 'Electronics', 600),
       (4, 'Chair', 'Furniture', 150),
       (5, 'Table', 'Furniture', 300);
```

**查询示例：统计不同类别的数量**

```sql
SELECT COUNT(DISTINCT category) AS unique_categories
FROM products;
```

**结果**
```sql
| unique_categories |
|-------------------|
|         2         |
```