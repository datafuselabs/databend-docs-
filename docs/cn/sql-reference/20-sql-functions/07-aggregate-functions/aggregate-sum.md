---
title: SUM
---

聚合函数。

SUM() 函数计算一组值的总和。

:::caution
NULL 值不计入。
:::

## 语法

```
SUM(<expr>)
```

## 参数

| 参数      | 描述               |
|-----------|--------------------|
| `<expr>`  | 任何数值表达式     |

## 返回类型

如果输入类型是 double，则返回 double，否则返回整数。

## 示例

**创建表并插入示例数据**
```sql
CREATE TABLE sales_data (
  id INT,
  product_id INT,
  quantity INT
);

INSERT INTO sales_data (id, product_id, quantity)
VALUES (1, 1, 10),
       (2, 2, 5),
       (3, 3, 8),
       (4, 4, 3),
       (5, 5, 15);
```

**查询示例：计算售出的产品总数**
```sql
SELECT SUM(quantity) AS total_quantity_sold
FROM sales_data;
```

**结果**
```sql
| total_quantity_sold |
|---------------------|
|         41          |
```