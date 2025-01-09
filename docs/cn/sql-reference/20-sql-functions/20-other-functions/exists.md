---
title: EXISTS
---

exists 条件与子查询结合使用，如果子查询返回至少一行，则认为“满足”条件。

## 语法

```sql
WHERE EXISTS ( <子查询> );
```

## 示例
```sql
SELECT number FROM numbers(5) AS A WHERE exists (SELECT * FROM numbers(3) WHERE number=1); 
+--------+
| number |
+--------+
|      0 |
|      1 |
|      2 |
|      3 |
|      4 |
+--------+
```