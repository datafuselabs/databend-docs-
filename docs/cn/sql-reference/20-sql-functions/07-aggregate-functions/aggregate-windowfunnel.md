---
title: WINDOW_FUNNEL
description: 漏斗分析
---

<p align="center">
<img src="https://datafuse-1253727613.cos.ap-hongkong.myqcloud.com/learn/databend-funnel.png" width="550"/>
</p>

## WINDOW_FUNNEL

类似于 ClickHouse 中的 `windowFunnel`（它们由同一位作者创建），它在滑动时间窗口中搜索事件链，并计算链中事件的最大数量。

该函数的工作原理如下：

-   函数搜索触发链中第一个条件的数据，并将事件计数器设置为 1。这是滑动窗口开始的时刻。

-   如果链中的事件在窗口内按顺序发生，计数器会增加。如果事件序列中断，计数器不会增加。

-   如果数据中有多个事件链，且它们的完成点不同，函数只会输出最长链的大小。

```sql
WINDOW_FUNNEL( <window> )( <timestamp>, <cond1>, <cond2>, ..., <condN> )
```

**参数**

-   `<timestamp>` — 包含时间戳的列名。支持的数据类型：整数类型和日期时间类型。
-   `<cond>` — 描述事件链的条件或数据。必须是 `Boolean` 数据类型。

**参数**

-   `<window>` — 滑动窗口的长度，它是第一个条件和最后一个条件之间的时间间隔。`window` 的单位取决于 `timestamp` 本身，并且会有所不同。通过表达式 `timestamp of cond1 <= timestamp of cond2 <= ... <= timestamp of condN <= timestamp of cond1 + window` 确定。

**返回值**

滑动时间窗口内连续触发条件的最大数量。
分析选择中的所有链。

类型：`UInt8`。

**示例**

确定一段时间是否足够让用户在在线商店中选择手机并购买两次。

设置以下事件链：

1. 用户登录商店账户 (`event_name = 'login'`)。
2. 用户访问页面 (`event_name = 'visit'`)。
3. 用户将商品添加到购物车 (`event_name = 'cart'`)。
4. 用户完成购买 (`event_name = 'purchase'`)。

```sql
CREATE TABLE events(user_id BIGINT, event_name VARCHAR, event_timestamp TIMESTAMP);

INSERT INTO events VALUES(100123, 'login', '2022-05-14 10:01:00');
INSERT INTO events VALUES(100123, 'visit', '2022-05-14 10:02:00');
INSERT INTO events VALUES(100123, 'cart', '2022-05-14 10:04:00');
INSERT INTO events VALUES(100123, 'purchase', '2022-05-14 10:10:00');

INSERT INTO events VALUES(100125, 'login', '2022-05-15 11:00:00');
INSERT INTO events VALUES(100125, 'visit', '2022-05-15 11:01:00');
INSERT INTO events VALUES(100125, 'cart', '2022-05-15 11:02:00');

INSERT INTO events VALUES(100126, 'login', '2022-05-15 12:00:00');
INSERT INTO events VALUES(100126, 'visit', '2022-05-15 12:01:00');
```

输入表：

```sql
+---------+------------+----------------------------+
| user_id | event_name | event_timestamp            |
+---------+------------+----------------------------+
|  100123 | login      | 2022-05-14 10:01:00.000000 |
|  100123 | visit      | 2022-05-14 10:02:00.000000 |
|  100123 | cart       | 2022-05-14 10:04:00.000000 |
|  100123 | purchase   | 2022-05-14 10:10:00.000000 |
|  100125 | login      | 2022-05-15 11:00:00.000000 |
|  100125 | visit      | 2022-05-15 11:01:00.000000 |
|  100125 | cart       | 2022-05-15 11:02:00.000000 |
|  100126 | login      | 2022-05-15 12:00:00.000000 |
|  100126 | visit      | 2022-05-15 12:01:00.000000 |
+---------+------------+----------------------------+
```

找出用户 `user_id` 在一小时滑动窗口内能通过链的进度。

查询：

```sql
SELECT
    level,
    count() AS count
FROM
(
    SELECT
        user_id,
        window_funnel(3600000000)(event_timestamp, event_name = 'login', event_name = 'visit', event_name = 'cart', event_name = 'purchase') AS level
    FROM events
    GROUP BY user_id
)
GROUP BY level ORDER BY level ASC;
```

:::tip

`event_timestamp` 类型是时间戳，`3600000000` 是一小时的时间窗口。

:::

结果：

```sql
+-------+-------+
| level | count |
+-------+-------+
|     2 |     1 |
|     3 |     1 |
|     4 |     1 |
+-------+-------+
```

* 用户 `100126` 的级别是 2 (`login -> visit`)。
* 用户 `100125` 的级别是 3 (`login -> visit -> cart`)。
* 用户 `100123` 的级别是 4 (`login -> visit -> cart -> purchase`)。