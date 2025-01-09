---
title: APPROX_COUNT_DISTINCT
---

使用 [HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog) 算法估算数据集中不同值的数量。

HyperLogLog 算法通过使用较少的内存和时间来提供唯一元素数量的近似值。在处理可以接受估计结果的大数据集时，考虑使用此函数。以牺牲一些准确性为代价，这是一种快速且高效的返回不同计数的方法。

要获得准确的结果，请使用 [COUNT_DISTINCT](aggregate-count-distinct.md)。有关更多解释，请参见 [示例](#examples)。

## 语法

```sql
APPROX_COUNT_DISTINCT(<expr>)
```

## 返回类型

整数。

## 示例

**创建表并插入示例数据**
```sql
CREATE TABLE user_events (
  id INT,
  user_id INT,
  event_name VARCHAR
);

INSERT INTO user_events (id, user_id, event_name)
VALUES (1, 1, 'Login'),
       (2, 2, 'Login'),
       (3, 3, 'Login'),
       (4, 1, 'Logout'),
       (5, 2, 'Logout'),
       (6, 4, 'Login'),
       (7, 1, 'Login');
```

**查询示例：估算不同用户 ID 的数量**
```sql
SELECT APPROX_COUNT_DISTINCT(user_id) AS approx_distinct_user_count
FROM user_events;
```

**结果**
```sql
| approx_distinct_user_count |
|----------------------------|
|             4              |
```