---
title: MAP_FILTER
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新版本：v1.2.652"/>

使用 [lambda 表达式](../../00-sql-reference/42-lambda-expressions.md) 定义条件，从 map 中过滤键值对。

## 语法

```sql
MAP_FILTER(<map>, (<key>, <value>) -> <condition>)
```

## 返回类型

返回一个仅包含满足 lambda 表达式指定条件的键值对的 map。

## 示例

此示例返回一个仅包含库存量低于 10 的产品的 map：

```sql
SELECT MAP_FILTER({101:15, 102:8, 103:12, 104:5}, (product_id, stock) -> (stock < 10)) AS low_stock_products;

┌────────────────────┐
│ low_stock_products │
├────────────────────┤
│ {102:8,104:5}      │
└────────────────────┘
```