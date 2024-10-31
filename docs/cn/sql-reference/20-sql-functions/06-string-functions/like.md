---
title: LIKE
---

使用SQL模式进行模式匹配。返回1（TRUE）或0（FALSE）。如果expr或pat为NULL，则结果为NULL。

## 语法

```sql
<expr> LIKE <pattern>
```

## 示例

```sql
SELECT name, category FROM system.functions WHERE name like 'tou%' ORDER BY name;
+----------+------------+
| name     | category   |
+----------+------------+
| touint16 | conversion |
| touint32 | conversion |
| touint64 | conversion |
| touint8  | conversion |
+----------+------------+
```