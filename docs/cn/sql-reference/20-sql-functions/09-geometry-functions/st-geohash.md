---
title: ST_GEOHASH
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新版本：v1.2.436"/>

返回一个 GEOMETRY 对象的 [geohash](https://en.wikipedia.org/wiki/Geohash)。geohash 是一个简短的 base32 字符串，用于标识包含世界某个位置的测地矩形。可选的精度参数指定返回的 geohash 的 `precision`。例如，传递 5 作为 `precision` 将返回一个较短（5 个字符长）且精度较低的 geohash。

## 语法

```sql
ST_GEOHASH(<geometry> [, <precision>])
```

## 参数

| 参数         | 描述                                                                 |
|--------------|----------------------------------------------------------------------|
| `geometry`   | 该参数必须是一个 GEOMETRY 类型的表达式。                             |
| `[precision]`| 可选。指定返回的 geohash 的精度，默认为 12。                         |

## 返回类型

字符串。

## 示例

```sql
SELECT
  ST_GEOHASH(
    ST_GEOMETRYFROMWKT(
      'POINT(-122.306100 37.554162)'
    )
  ) AS pipeline_geohash;

┌──────────────────┐
│ pipeline_geohash │
├──────────────────┤
│ 9q9j8ue2v71y     │
└──────────────────┘

SELECT
  ST_GEOHASH(
    ST_GEOMETRYFROMWKT(
      'SRID=4326;POINT(-122.35 37.55)'
    ),
    5
  ) AS pipeline_geohash;

┌──────────────────┐
│ pipeline_geohash │
├──────────────────┤
│ 9q8vx            │
└──────────────────┘
```