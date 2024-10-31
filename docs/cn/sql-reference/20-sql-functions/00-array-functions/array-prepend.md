---
title: ARRAY_PREPEND
---

将元素前置到数组中。

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