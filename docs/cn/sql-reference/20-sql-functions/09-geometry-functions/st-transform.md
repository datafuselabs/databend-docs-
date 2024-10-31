---
title: ST_TRANSFORM
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新: v1.2.606"/>

将一个GEOMETRY对象从一个[空间参考系统 (SRS)](https://en.wikipedia.org/wiki/Spatial_reference_system)转换到另一个。如果你只需要更改SRID而不改变坐标（例如，如果SRID不正确），请使用[ST_SETSRID](st-setsrid.md)。

## 语法

```sql
ST_TRANSFORM(<geometry> [, <from_srid>], <to_srid>)
```

## 参数

| 参数          | 描述                                                                                                                                               |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<geometry>`  | 参数必须是GEOMETRY对象类型的表达式。                                                                                               |
| `<from_srid>` | 可选的SRID，用于标识输入GEOMETRY对象的当前SRS，如果省略此参数，则使用输入GEOMETRY对象中指定的SRID。 |
| `<to_srid>`   | 标识要使用的SRS的SRID，将输入GEOMETRY对象转换为使用此SRS的新对象。                                         |

## 返回类型

Geometry。

## 示例

```sql
SET GEOMETRY_OUTPUT_FORMAT = 'EWKT'

SELECT ST_TRANSFORM(ST_GEOMFROMWKT('POINT(389866.35 5819003.03)', 32633), 3857) AS transformed_geom

┌───────────────────────────────────────────────┐
│                transformed_geom               │
├───────────────────────────────────────────────┤
│ SRID=3857;POINT(1489140.093766 6892872.19868) │
└───────────────────────────────────────────────┘

SELECT ST_TRANSFORM(ST_GEOMFROMWKT('POINT(4.500212 52.161170)'), 4326, 28992) AS transformed_geom

┌──────────────────────────────────────────────┐
│               transformed_geom               │
├──────────────────────────────────────────────┤
│ SRID=28992;POINT(94308.670475 464038.168827) │
└──────────────────────────────────────────────┘

```