---
title: TIMEZONE
---

返回当前连接的时区。

Databend 默认使用 UTC（协调世界时）作为时区，并允许您将时区更改为您当前的地理位置。有关可以分配给 `timezone` 设置的值，请参考 https://docs.rs/chrono-tz/latest/chrono_tz/enum.Tz.html。详情请参见以下示例。

## 语法

```
SELECT TIMEZONE();
```

## 示例

```sql
-- 返回当前时区
SELECT TIMEZONE();

┌────────────┐
│ timezone() │
├────────────┤
│ UTC        │
└────────────┘

-- 将时区设置为中国标准时间
SET timezone='Asia/Shanghai';

SELECT TIMEZONE();

┌───────────────┐
│   timezone()  │
├───────────────┤
│ Asia/Shanghai │
└───────────────┘
```