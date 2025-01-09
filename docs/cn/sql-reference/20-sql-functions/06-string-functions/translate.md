---
title: TRANSLATE
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新于：v1.2.203"/>

通过将特定字符替换为相应的字符来转换给定字符串，替换规则由提供的映射定义。

## 语法

```sql
TRANSLATE('<inputString>', '<charactersToReplace>', '<replacementCharacters>')
```

| 参数                      | 描述                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------|
| `<inputString>`           | 要转换的输入字符串。                                                             |
| `<charactersToReplace>`   | 包含要在输入字符串中替换的字符的字符串。                            |
| `<replacementCharacters>` | 包含与 `<charactersToReplace>` 中字符对应的替换字符的字符串。 |

## 示例

```sql
-- 将 'databend' 中的 'd' 替换为 '$'
SELECT TRANSLATE('databend', 'd', '$');

---
$ataben$

-- 将 'databend' 中的 'd' 替换为 'D'
SELECT TRANSLATE('databend', 'd', 'D');

---
DatabenD

-- 将 'databend' 中的 'd' 替换为 'D'，'e' 替换为 'E'
SELECT TRANSLATE('databend', 'de', 'DE');

---
DatabEnD

-- 从 'databend' 中移除 'd'
SELECT TRANSLATE('databend', 'd', '');

---
ataben
```