---
title: MAP_CAT
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新于：v1.2.459"/>

返回两个 MAP 的拼接结果。

## 语法

```sql
MAP_CAT( <map1>, <map2> )
```

## 参数

| 参数      | 描述                     |
|-----------|---------------------------------|
| `<map1>`  | 源 MAP。                 |
| `<map2>`  | 要追加到 map1 的 MAP。 |

:::note
- 如果 map1 和 map2 具有相同键的值，则输出 MAP 包含来自 map2 的值。
- 如果任一参数为 NULL，函数将返回 NULL 而不报告任何错误。
:::

## 返回类型

Map。

## 示例

```sql
SELECT MAP_CAT({'a':1,'b':2,'c':3}, {'c':5,'d':6});
┌─────────────────────────────────────────────┐
│ map_cat({'a':1,'b':2,'c':3}, {'c':5,'d':6}) │
├─────────────────────────────────────────────┤
│ {'a':1,'b':2,'c':5,'d':6}                   │
└─────────────────────────────────────────────┘
```