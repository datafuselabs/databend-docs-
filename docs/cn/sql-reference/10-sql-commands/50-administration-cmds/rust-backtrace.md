---
title: SYSTEM ENABLE / DISABLE EXCEPTION_BACKTRACE
---

import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新于：v1.2.530"/>

控制 Databend 中 Rust 回溯信息的生成。SYSTEM ENABLE EXCEPTION_BACKTRACE 用于在发生 panic 时启用回溯信息以进行调试，而 SYSTEM DISABLE EXCEPTION_BACKTRACE 则禁用回溯信息，以避免额外的开销或敏感信息的暴露。

## 语法

```sql
-- 启用 Rust 回溯信息
SYSTEM ENABLE EXCEPTION_BACKTRACE

-- 禁用 Rust 回溯信息
SYSTEM DISABLE EXCEPTION_BACKTRACE
```