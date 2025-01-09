---
title: JSON_PATH_EXISTS
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新于：v1.2.119"/>

检查 JSON 数据中是否存在指定的路径。

## 语法

```sql
JSON_PATH_EXISTS(<json_data>, <json_path_expression>)
```

- json_data: 指定要搜索的 JSON 数据。它可以是 JSON 对象或数组。

- json_path_expression: 指定要从 JSON 数据的根（由 `$` 表示）开始检查的路径。您还可以在表达式中包含条件，使用 `@` 来引用当前正在评估的节点或元素，以过滤结果。

## 返回类型

该函数返回：

- 如果 JSON 数据中存在指定的 JSON 路径（以及条件，如果有），则返回 `true`。
- 如果 JSON 数据中不存在指定的 JSON 路径（以及条件，如果有），则返回 `false`。
- 如果 json_data 或 json_path_expression 为 NULL 或无效，则返回 NULL。

## 示例

```sql
SELECT JSON_PATH_EXISTS(parse_json('{"a": 1, "b": 2}'), '$.a ? (@ == 1)');

----
true


SELECT JSON_PATH_EXISTS(parse_json('{"a": 1, "b": 2}'), '$.a ? (@ > 1)');

----
false

SELECT JSON_PATH_EXISTS(NULL, '$.a');

----
NULL

SELECT JSON_PATH_EXISTS(parse_json('{"a": 1, "b": 2}'), NULL);

----
NULL
```