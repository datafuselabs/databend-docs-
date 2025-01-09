---
title: BITMAP_OR_COUNT
---

通过对位图执行逻辑 OR 操作，计算设置为 1 的位数。

## 语法

```sql
BITMAP_OR_COUNT( <bitmap> )
```

## 示例

```sql
SELECT BITMAP_OR_COUNT(TO_BITMAP('1, 3, 5'));

┌───────────────────────────────────────┐
│ bitmap_or_count(to_bitmap('1, 3, 5')) │
├───────────────────────────────────────┤
│                                     3 │
└───────────────────────────────────────┘
```