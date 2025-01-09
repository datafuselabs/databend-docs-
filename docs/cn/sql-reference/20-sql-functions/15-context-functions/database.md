---
title: DATABASE
---

返回当前选中的数据库名称。如果未选择任何数据库，则此函数返回 `default`。

## 语法

```sql
DATABASE()
```

## 示例

```sql
SELECT DATABASE();

┌────────────┐
│ database() │
├────────────┤
│ default    │
└────────────┘
```