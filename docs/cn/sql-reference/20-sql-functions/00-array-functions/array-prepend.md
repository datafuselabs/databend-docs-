---
title: ARRAY_PREPEND
---

在数组的开头添加一个元素。

## 语法

```sql
ARRAY_PREPEND( <element>, <array> )
```

## 示例

```sql
SELECT ARRAY_PREPEND(1, [3, 4]);

┌──────────────────────────┐
│ array_prepend(1, [3, 4]) │
├──────────────────────────┤
│ [1,3,4]                  │
└──────────────────────────┘
```