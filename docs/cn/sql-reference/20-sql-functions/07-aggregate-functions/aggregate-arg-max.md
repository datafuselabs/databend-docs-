---
title: ARG_MAX
---

计算与最大值 `val` 对应的 `arg` 值。如果有多个 `arg` 值对应相同的最大值 `val`，则返回遇到的第一个 `arg` 值。

## 语法

```sql
ARG_MAX(<arg>, <val>)
```

## 参数

| 参数      | 描述                                                                                       |
|-----------|---------------------------------------------------------------------------------------------------|
| `<arg>`   | [Databend 支持的任何数据类型的参数](../../00-sql-reference/10-data-types/index.md) |
| `<val>`   | [Databend 支持的任何数据类型的值](../../00-sql-reference/10-data-types/index.md)    |

## 返回类型

与最大值 `val` 对应的 `arg` 值。

返回类型与 `arg` 类型匹配。

## 示例

**创建表并插入示例数据**

让我们创建一个名为 "sales" 的表并插入一些示例数据：
```sql
CREATE TABLE sales (
  id INTEGER,
  product VARCHAR(50),
  price FLOAT
);

INSERT INTO sales (id, product, price)
VALUES (1, 'Product A', 10.5),
       (2, 'Product B', 20.75),
       (3, 'Product C', 30.0),
       (4, 'Product D', 15.25),
       (5, 'Product E', 25.5);
```

**查询：使用 ARG_MAX() 函数**

现在，让我们使用 ARG_MAX() 函数来查找价格最高的产品：
```sql
SELECT ARG_MAX(product, price) AS max_price_product
FROM sales;
```

结果应如下所示：
```sql
| max_price_product |
| ----------------- |
| Product C         |
```