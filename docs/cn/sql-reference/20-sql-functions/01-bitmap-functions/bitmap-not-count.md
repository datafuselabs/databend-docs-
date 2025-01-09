---
title: BITMAP_NOT_COUNT
---

通过对位图执行逻辑 NOT 操作，计算位图中设置为 0 的位数。

## 语法

```sql
BITMAP_NOT_COUNT( <bitmap> )
```

## 示例

```sql
SELECT BITMAP_NOT_COUNT(TO_BITMAP('1, 3, 5'));

┌────────────────────────────────────────┐
│ bitmap_not_count(to_bitmap('1, 3, 5')) │
├────────────────────────────────────────┤
│                                      3 │
└────────────────────────────────────────┘
```