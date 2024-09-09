---
title: NEXTVAL
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新: v1.2.453"/>

从序列中检索下一个值。

## 语法

```sql
NEXTVAL(<sequence_name>)
```

## 返回类型

整数。

## 示例

此示例展示了NEXTVAL函数如何与序列一起工作：

```sql
CREATE SEQUENCE my_seq;

SELECT
  NEXTVAL(my_seq),
  NEXTVAL(my_seq),
  NEXTVAL(my_seq);

┌─────────────────────────────────────────────────────┐
│ nextval(my_seq) │ nextval(my_seq) │ nextval(my_seq) │
├─────────────────┼─────────────────┼─────────────────┤
│               1 │               2 │               3 │
└─────────────────────────────────────────────────────┘
```

此示例展示了如何使用序列和NEXTVAL函数来自动生成并分配唯一标识符给表中的行。

```sql
-- 创建一个名为staff_id_seq的新序列
CREATE SEQUENCE staff_id_seq;

-- 创建一个名为staff的新表，包含staff_id、name和department列
CREATE TABLE staff (
    staff_id INT,
    name VARCHAR(50),
    department VARCHAR(50)
);

-- 向staff表中插入新行，使用staff_id_seq序列的下一个值作为staff_id列
INSERT INTO staff (staff_id, name, department)
VALUES (NEXTVAL(staff_id_seq), 'John Doe', 'HR');

-- 向staff表中插入另一行，使用staff_id_seq序列的下一个值作为staff_id列
INSERT INTO staff (staff_id, name, department)
VALUES (NEXTVAL(staff_id_seq), 'Jane Smith', 'Finance');

SELECT * FROM staff;

┌───────────────────────────────────────────────────────┐
│     staff_id    │       name       │    department    │
├─────────────────┼──────────────────┼──────────────────┤
│               2 │ Jane Smith       │ Finance          │
│               1 │ John Doe         │ HR               │
└───────────────────────────────────────────────────────┘
```