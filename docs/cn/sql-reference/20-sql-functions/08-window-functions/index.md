# 时间回溯

时间回溯允许您查询过去某个时间点的数据。

## 语法

```sql
SELECT ...
FROM <table_name>
AT (TIMESTAMP => <timestamp_expression> | OFFSET => <time_offset_expression> | STATEMENT => <query_id>)
[AS] <table_alias>;
```

## 参数

| 参数名称 | 描述 |
| --- | --- |
| `TIMESTAMP => <timestamp_expression>` | 指定查询的时间点。`<timestamp_expression>` 可以是任何返回 `TIMESTAMP` 的表达式。 |
| `OFFSET => <time_offset_expression>` | 指定查询的时间偏移量。`<time_offset_expression>` 可以是任何返回 `INTERVAL` 的表达式。 |
| `STATEMENT => <query_id>` | 指定查询的查询 ID。`<query_id>` 是查询的唯一标识符。 |

## 示例

### 使用时间戳查询

```sql
SELECT * FROM my_table AT (TIMESTAMP => '2023-04-01 12:00:00');
```

### 使用时间偏移量查询

```sql
SELECT * FROM my_table AT (OFFSET => INTERVAL 1 HOUR);
```

### 使用查询 ID 查询

```sql
SELECT * FROM my_table AT (STATEMENT => '8e2bfe70-0000-0000-0000-000000000001');
```

{/*examples*/}

---
title: '窗口函数'
---

## 概述 

窗口函数对一组相关行（“窗口”）进行操作。

对于每个输入行，窗口函数返回一个输出行，该输出行取决于传递给函数的特定行以及窗口中其他行的值。

有两种主要类型的顺序敏感窗口函数：

* `排名相关函数`：排名相关函数根据行的“排名”列出信息。例如，按年度利润降序排列商店，利润最高的商店将排名为1，第二高利润的商店将排名为2，依此类推。

* `窗口框架函数`：窗口框架函数使您能够在窗口中的一组行上执行滚动操作，例如计算运行总计或移动平均值。

## 支持窗口的函数列表

以下列表显示了所有支持窗口的函数。

| 函数名称                                                              | 类别         | 窗口   | 窗口框架     | 备注  |
|-----------------------------------------------------------------------|--------------|--------|--------------|-------|
| [ARRAY_AGG](../07-aggregate-functions/aggregate-array-agg.md)         | 通用         | ✔      |              |       |
| [AVG](../07-aggregate-functions/aggregate-avg.md)                     | 通用         | ✔      | ✔            |       |
| [AVG_IF](../07-aggregate-functions/aggregate-avg-if.md)               | 通用         | ✔      | ✔            |       |
| [COUNT](../07-aggregate-functions/aggregate-count.md)                 | 通用         | ✔      | ✔            |       |
| [COUNT_IF](../07-aggregate-functions/aggregate-count-if.md)           | 通用         | ✔      | ✔            |       |
| [COVAR_POP](../07-aggregate-functions/aggregate-covar-pop.md)         | 通用         | ✔      |              |       |
| [COVAR_SAMP](../07-aggregate-functions/aggregate-covar-samp.md)       | 通用         | ✔      |              |       |
| [MAX](../07-aggregate-functions/aggregate-max.md)                     | 通用         | ✔      | ✔            |       |
| [MAX_IF](../07-aggregate-functions/aggregate-max-if.md)               | 通用         | ✔      | ✔            |       |
| [MIN](../07-aggregate-functions/aggregate-min.md)                     | 通用         | ✔      | ✔            |       |
| [MIN_IF](../07-aggregate-functions/aggregate-min-if.md)               | 通用         | ✔      | ✔            |       |
| [STDDEV_POP](../07-aggregate-functions/aggregate-stddev-pop.md)       | 通用         | ✔      | ✔            |       |
| [STDDEV_SAMP](../07-aggregate-functions/aggregate-stddev-samp.md)     | 通用         | ✔      | ✔            |       |
| [MEDIAN](../07-aggregate-functions/aggregate-median.md)               | 通用         | ✔      | ✔            |       |
| [QUANTILE_CONT](../07-aggregate-functions/aggregate-quantile-cont.md) | 通用         | ✔      | ✔            |       |
| [QUANTILE_DISC](../07-aggregate-functions/aggregate-quantile-disc.md) | 通用         | ✔      | ✔            |       |
| [KURTOSIS](../07-aggregate-functions/aggregate-kurtosis.md)           | 通用         | ✔      | ✔            |       |
| [SKEWNESS](../07-aggregate-functions/aggregate-skewness.md)           | 通用         | ✔      | ✔            |       |
| [SUM](../07-aggregate-functions/aggregate-sum.md)                     | 通用         | ✔      | ✔            |       |
| [SUM_IF](../07-aggregate-functions/aggregate-sum-if.md)               | 通用         | ✔      | ✔            |       |
| [CUME_DIST](cume-dist.md)                                             | 排名相关     | ✔      |              |       |
| [PERCENT_RANK](percent_rank.md)                                       | 排名相关     | ✔      | ✔            |       |
| [DENSE_RANK](dense-rank.md)                                           | 排名相关     | ✔      | ✔            |       |
| [RANK](rank.md)                                                       | 排名相关     | ✔      | ✔            |       |
| [ROW_NUMBER](row-number.md)                                           | 排名相关     | ✔      |              |       |
| [NTILE](ntile.md)                                                     | 排名相关     | ✔      |              |       |
| [FIRST_VALUE](first-value.md)                                         | 排名相关     | ✔      | ✔            |       |
| [FIRST](first.md)                                                     | 排名相关     | ✔      | ✔            |       |
| [LAST_VALUE](last-value.md)                                           | 排名相关     | ✔      | ✔            |       |
| [LAST](last.md)                                                       | 排名相关     | ✔      | ✔            |       |
| [NTH_VALUE](nth-value.md)                                             | 排名相关     | ✔      | ✔            |       |
| [LEAD](lead.md)                                                       | 排名相关     | ✔      |              |       |
| [LAG](lag.md)                                                         | 排名相关     | ✔      |              |       |

## 窗口语法

```sql
<函数> ( [ <参数> ] ) OVER ( { 命名窗口 | 内联窗口 } )

命名窗口 ::=
    { 窗口名称 | ( 窗口名称 ) }

内联窗口 ::=
    [ PARTITION BY <表达式列表> ]
    [ ORDER BY <表达式列表> ]
    [ 窗口框架 ]
```
`命名窗口`是在`SELECT`语句的`WINDOW`子句中定义的窗口，例如：`SELECT a, SUM(a) OVER w FROM t WINDOW w AS ( 内联窗口 )`。

`<函数>`是以下之一（[聚合函数](../07-aggregate-functions/index.md)、排名函数、值函数）。

`OVER`子句指定该函数作为窗口函数使用。

`PARTITION BY`子句允许将行分组为子组，例如按城市、按年等。`PARTITION BY`子句是可选的。您可以分析整个行组而不将其分解为子组。

`ORDER BY`子句对窗口内的行进行排序。

`窗口框架`子句指定窗口框架类型和窗口框架范围。`窗口框架`子句是可选的。如果您省略`窗口框架`子句，默认的窗口框架类型是`RANGE`，默认的窗口框架范围是`UNBOUNDED PRECEDING AND CURRENT ROW`。

## 窗口框架语法

`窗口框架`可以是以下类型之一：

```sql
累积框架 ::=
    {
       { ROWS | RANGE } BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
     | { ROWS | RANGE } BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
    }
```

```sql
滑动框架 ::=
    {
       ROWS BETWEEN <N> { PRECEDING | FOLLOWING } AND <N> { PRECEDING | FOLLOWING }
     | ROWS BETWEEN UNBOUNDED PRECEDING AND <N> { PRECEDING | FOLLOWING }
     | ROWS BETWEEN <N> { PRECEDING | FOLLOWING } AND UNBOUNDED FOLLOWING
    }
```

## 示例

**创建表**
```sql
CREATE TABLE employees (
  employee_id INT,
  first_name VARCHAR,
  last_name VARCHAR,
  department VARCHAR,
  salary INT
);
```

**插入数据**
```sql
INSERT INTO employees (employee_id, first_name, last_name, department, salary) VALUES
  (1, 'John', 'Doe', 'IT', 75000),
  (2, 'Jane', 'Smith', 'HR', 85000),
  (3, 'Mike', 'Johnson', 'IT', 90000),
  (4, 'Sara', 'Williams', 'Sales', 60000),
  (5, 'Tom', 'Brown', 'HR', 82000),
  (6, 'Ava', 'Davis', 'Sales', 62000),
  (7, 'Olivia', 'Taylor', 'IT', 72000),
  (8, 'Emily', 'Anderson', 'HR', 77000),
  (9, 'Sophia', 'Lee', 'Sales', 58000),
  (10, 'Ella', 'Thomas', 'IT', 67000);
```

**示例1：按工资排名员工**

在此示例中，我们使用RANK()函数根据员工的工资按降序排名。最高工资将获得排名1，最低工资将获得最高排名编号。
```sql
SELECT employee_id, first_name, last_name, department, salary, RANK() OVER (ORDER BY salary DESC) AS rank
FROM employees;
```

结果：

| employee_id | first_name | last_name | department | salary | rank |
|-------------|------------|-----------|------------|--------|------|
| 3           | Mike       | Johnson   | IT         | 90000  | 1    |
| 2           | Jane       | Smith     | HR         | 85000  | 2    |
| 5           | Tom        | Brown     | HR         | 82000  | 3    |
| 8           | Emily      | Anderson  | HR         | 77000  | 4    |
| 1           | John       | Doe       | IT         | 75000  | 5    |
| 7           | Olivia     | Taylor    | IT         | 72000  | 6    |
| 10          | Ella       | Thomas    | IT         | 67000  | 7    |
| 6           | Ava        | Davis     | Sales      | 62000  | 8    |
| 4           | Sara       | Williams  | Sales      | 60000  | 9    |
| 9           | Sophia     | Lee       | Sales      | 58000  | 10   |

**示例2：计算每个部门的工资总额**

在此示例中，我们使用带有PARTITION BY的SUM()函数来计算每个部门的工资总额。每行将显示部门和该部门的工资总额。
```sql
SELECT department, SUM(salary) OVER (PARTITION BY department) AS total_salary
FROM employees;
```

结果：

| department | total_salary |
|------------|--------------|
| HR         | 244000       |
| HR         | 244000       |
| HR         | 244000       |
| IT         | 304000       |
| IT         | 304000       |
| IT         | 304000       |
| IT         | 304000       |
| Sales      | 180000       |
| Sales      | 180000       |
| Sales      | 180000       |

**示例3：计算每个部门的工资运行总计**

在此示例中，我们使用带有累积窗口框架的SUM()函数来计算每个部门内的工资运行总计。运行总计是根据员工的工资按employee_id排序计算的。
```sql
SELECT employee_id, first_name, last_name, department, salary, 
       SUM(salary) OVER (PARTITION BY department ORDER BY employee_id
                         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM employees;
```

结果：

| employee_id | first_name | last_name | department | salary | running_total |
|-------------|------------|-----------|------------|--------|---------------|
| 2           | Jane       | Smith     | HR         | 85000  | 85000         |
| 5           | Tom        | Brown     | HR         | 82000  | 167000        |
| 8           | Emily      | Anderson  | HR         | 77000  | 244000        |
| 1           | John       | Doe       | IT         | 75000  | 75000         |
| 3           | Mike       | Johnson   | IT         | 90000  | 165000        |
| 7           | Olivia     | Taylor    | IT         | 72000  | 237000        |
| 10          | Ella       | Thomas    | IT         | 67000  | 304000        |
| 4           | Sara       | Williams  | Sales      | 60000  | 60000         |
| 6           | Ava        | Davis     | Sales      | 62000  | 122000        |
| 9           | Sophia     | Lee       | Sales      | 58000  | 180000        |