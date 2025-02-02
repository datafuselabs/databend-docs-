---
title: 查询性能分析
---

查询性能分析是指特定 SQL 语句执行过程的图形化表示或可视化分解。它本质上是 [EXPLAIN](/sql/sql-commands/explain-cmds/explain) 命令的图形化版本，提供了查询的执行计划和性能细节的洞察。

## 访问查询性能分析

查询性能分析可以直接在 Databend Cloud 中访问。要查看查询的性能分析，请转到 **监控** > **SQL 历史记录**。从历史记录列表中选择一个 SQL 语句，然后点击 **查询性能分析** 标签。如果您使用的是私有化部署的 Databend，可以使用 [EXPLAIN](/sql/sql-commands/explain-cmds/explain) 命令作为替代。

## 查询性能分析包含的内容

以下是一个查询性能分析的示例，它由一组层次结构中的三个操作节点组成。在执行 SQL 语句时，Databend Cloud 会按照从下到上的顺序处理这些节点。查询性能分析中包含的操作节点数量和类型取决于您的 SQL 语句的具体内容。有关常见操作节点及其统计字段，请参见 [常见操作节点及字段](#常见操作节点及字段)。

![alt text](/img/cloud/query-profile-1.png)

_请注意，每个节点标题中的括号数字表示节点 ID，并不表示执行步骤。_

查询性能分析附带一组信息面板，提供更多详细信息。上面的示例包括两个信息面板：

| 面板         | 描述                                                                                                                           |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| 最耗时的节点 | 列出执行时间最长的节点。                                                                                                       |
| 性能分析概览 | 显示 CPU 和 I/O 所花费时间的百分比。请注意，如果您选择一个节点，此信息面板将显示您选择的节点的特定信息，而不是整个查询的信息。 |

如果您点击 `TableScan [4]` 节点，您会注意到右侧增加了两个信息面板：

![alt text](/img/cloud/query-profile-2.png)

| 面板     | 描述                                                                 |
| -------- | -------------------------------------------------------------------- |
| 统计信息 | 包括扫描进度、扫描的字节数、从缓存中扫描的百分比、扫描的分区等信息。 |
| 属性     | 显示特定于节点的详细信息。显示的字段根据节点的功能而有所不同。       |

## 常见操作节点及字段

解释计划包括各种操作节点，具体取决于您希望 Databend 解释的 SQL 语句。以下是常见操作节点及其字段的列表：

- **TableScan**: 从表中读取数据。
  - table: 表的全名。例如，`catalog1.database1.table1`。
  - read rows: 要读取的行数。
  - read bytes: 要读取的数据字节数。
  - partition total: 表的总分区数。
  - partition scanned: 要读取的分区数。
  - push downs: 要推送到存储层进行处理的过滤器和限制。
- **Filter**: 过滤读取的数据。
  - filters: 用于过滤数据的谓词表达式。表达式评估返回 false 的数据将被过滤掉。
- **EvalScalar**: 评估标量表达式。例如，`SELECT a+1 AS b FROM t` 中的 `a+1`。
  - expressions: 要评估的标量表达式。
- **AggregatePartial** & **AggregateFinal**: 按键聚合并返回聚合函数的结果。
  - group by: 用于聚合的键。
  - aggregate functions: 用于聚合的函数。
- **Sort**: 按键排序数据。
  - sort keys: 用于排序的表达式。
- **Limit**: 限制返回的行数。
  - limit: 要返回的行数。
  - offset: 在返回任何行之前要跳过的行数。
- **HashJoin**: 使用 Hash Join 算法执行两个表的 Join 操作。Hash Join 算法会选择其中一个表作为构建端来构建 Hash 表。然后使用另一个表作为探测端从 Hash 表中读取匹配的数据以形成结果。
  - join type: JOIN 类型（INNER、LEFT OUTER、RIGHT OUTER、FULL OUTER、CROSS、SINGLE 或 MARK）。
  - build keys: 构建端用于构建 Hash 表的表达式。
  - probe keys: 探测端用于从 Hash 表中读取数据的表达式。
  - filters: 非等价 JOIN 条件，例如 `t.a > t1.a`。
- **Exchange**: 在 Databend 查询节点之间交换数据以进行分布式并行计算。
  - exchange type: 数据重新分区类型（Hash、Broadcast 或 Merge）。
