---
title: ST_CONTAINS
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新: v1.2.564"/>

如果第二个GEOMETRY对象完全在第一个GEOMETRY对象内部，则返回TRUE。

## 语法

```sql
ST_CONTAINS(<geometry1>, <geometry2>)
```

## 参数

| 参数          | 描述                                                                                           |
|---------------|----------------------------------------------------------------------------------------------|
| `<geometry1>` | 参数必须是类型为GEOMETRY对象的表达式，且不能是GeometryCollection。                             |
| `<geometry2>` | 参数必须是类型为GEOMETRY对象的表达式，且不能是GeometryCollection。                             |

:::note
- 如果两个输入的GEOMETRY对象具有不同的SRID，函数会报告错误。
:::

## 返回类型

布尔值。

## 示例

```sql
SELECT ST_CONTAINS(TO_GEOMETRY('POLYGON((-2 0, 0 2, 2 0, -2 0))'), TO_GEOMETRY('POLYGON((-1 0, 0 1, 1 0, -1 0))')) AS contains

┌──────────┐
│ contains │
├──────────┤
│ true     │
└──────────┘

SELECT ST_CONTAINS(TO_GEOMETRY('POLYGON((-2 0, 0 2, 2 0, -2 0))'), TO_GEOMETRY('LINESTRING(-1 1, 0 2, 1 1)')) AS contains

┌──────────┐
│ contains │
├──────────┤
│ false    │
└──────────┘

SELECT ST_CONTAINS(TO_GEOMETRY('POLYGON((-2 0, 0 2, 2 0, -2 0))'), TO_GEOMETRY('LINESTRING(-2 0, 0 0, 0 1)')) AS contains

┌──────────┐
│ contains │
├──────────┤
│ true     │
└──────────┘

```