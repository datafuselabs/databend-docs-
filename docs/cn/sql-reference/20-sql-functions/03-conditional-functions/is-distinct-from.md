---
title: "IS [ NOT ] DISTINCT FROM"
---

比较两个表达式是否相等（或不相等），并考虑空值（NULL）的情况，这意味着它将 NULL 视为已知值进行相等性比较。

## 语法

```sql
<expr1> IS [ NOT ] DISTINCT FROM <expr2>
```

## 示例

```sql
SELECT NULL IS DISTINCT FROM NULL;

┌────────────────────────────┐
│ null is distinct from null │
├────────────────────────────┤
│ false                      │
└────────────────────────────┘
```