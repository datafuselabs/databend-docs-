---
title: H3_UNIDIRECTIONAL_EDGE_IS_VALID
---

确定提供的 H3Index 是否是有效的单向边索引。如果是单向边，则返回 1，否则返回 0。

## 语法

```sql
H3_UNIDIRECTIONAL_EDGE_IS_VALID(h3)
```

## 示例

```sql
SELECT H3_UNIDIRECTIONAL_EDGE_IS_VALID(1248204388774707199);

┌──────────────────────────────────────────────────────┐
│ h3_unidirectional_edge_is_valid(1248204388774707199) │
├──────────────────────────────────────────────────────┤
│ true                                                 │
└──────────────────────────────────────────────────────┘
```