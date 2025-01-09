---
title: FUSE_ENCODING
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新于：v1.2.162"/>

返回应用于表中特定列的编码类型。它帮助您了解数据在表中如何以原生格式进行压缩和存储。

## 语法

```sql
FUSE_ENCODING('<数据库名称>', '<表名称>', '<列名称>')
```

该函数返回一个包含以下列的结果集：

| 列名              | 数据类型          | 描述                                                                                                                                                                              |
|-------------------|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VALIDITY_SIZE     | Nullable(UInt32) | 表示列中每行是否具有非空值的位图值的大小。此位图用于跟踪列数据中空值的存在与否。                                                                                                   |
| COMPRESSED_SIZE   | UInt32           | 列数据压缩后的大小。                                                                                                                                                               |
| UNCOMPRESSED_SIZE | UInt32           | 应用编码前列数据的大小。                                                                                                                                                           |
| LEVEL_ONE         | String           | 应用于列的主要或初始编码。                                                                                                                                                         |
| LEVEL_TWO         | Nullable(String) | 在初始编码后应用于列的次要或递归编码方法。                                                                                                                                         |

## 示例

```sql
-- 创建一个包含整数列 'c' 的表，并应用 'Lz4' 压缩
CREATE TABLE t(c INT) STORAGE_FORMAT = 'native' COMPRESSION = 'lz4';

-- 向表中插入数据。
INSERT INTO t SELECT number FROM numbers(2048);

-- 分析表 't' 中列 'c' 的编码
SELECT LEVEL_ONE, LEVEL_TWO, COUNT(*) 
FROM FUSE_ENCODING('default', 't', 'c') 
GROUP BY LEVEL_ONE, LEVEL_TWO;

level_one   |level_two|count(*)|
------------+---------+--------+
DeltaBitpack|         |       1|

-- 向表 't' 中插入 2,048 行值为 1 的数据
INSERT INTO t (c)
SELECT 1
FROM numbers(2048);

SELECT LEVEL_ONE, LEVEL_TWO, COUNT(*) 
FROM FUSE_ENCODING('default', 't', 'c') 
GROUP BY LEVEL_ONE, LEVEL_TWO;

level_one   |level_two|count(*)|
------------+---------+--------+
OneValue    |         |       1|
DeltaBitpack|         |       1|
```