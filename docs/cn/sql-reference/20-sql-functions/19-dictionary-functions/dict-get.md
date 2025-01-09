---
title: DICT_GET
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新于：v1.2.636"/>

使用提供的键表达式从字典中检索指定属性的值。

## 语法

```sql
DICT_GET([db_name.]<dict_name>, '<attr_name>', <key_expr>)
```

| 参数       | 描述                                                                                                                                                       |
|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| dict_name  | 字典的名称。                                                                                                                                               |
| attr_name  | 你想要检索值的字典中的属性名称。                                                                                                                           |
| key_expr   | 用于在字典中定位特定条目的键表达式。它表示字典主键的值，以检索相应的数据。                                                                                 |

## 示例

参见[使用示例](/guides/query/dictionary#usage-example)。