┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│     sale_id     │    product_id   │    sale_date   │     quantity    │        total_price       │
├─────────────────┼─────────────────┼────────────────┼─────────────────┼──────────────────────────┤
│               1 │             101 │ 2023-01-15     │               5 │ 100.00                   │
│               3 │             103 │ 2023-03-25     │              10 │ 200.00                   │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Example-3: Conditional INSERT FIRST

This example demonstrates conditional INSERT FIRST, where records satisfying multiple conditions are inserted into only the first matching table.

1. Create three tables: products, `high_quantity_sales`, `high_price_sales`, and `sales_data_source`. Then, insert three sales records into the `sales_data_source` table.

```sql
-- Create the high_quantity_sales table
CREATE TABLE high_quantity_sales (
    sale_id INT,
    product_id INT,
    sale_date DATE,
    quantity INT,
    total_price DECIMAL(10, 2)
);

-- Create the high_price_sales table
CREATE TABLE high_price_sales (
    sale_id INT,
    product_id INT,
    sale_date DATE,
    quantity INT,
    total_price DECIMAL(10, 2)
);

-- Create the sales_data_source table
CREATE TABLE sales_data_source (
    sale_id INT,
    product_id INT,
    sale_date DATE,
    quantity INT,
    total_price DECIMAL(10, 2)
);

-- Insert data into the sales_data_source table
INSERT INTO sales_data_source (sale_id, product_id, sale_date, quantity, total_price)
VALUES
    (1, 101, '2023-01-15', 5, 100.00),
    (2, 102, '2023-02-20', 3, 75.00),
    (3, 103, '2023-03-25', 10, 200.00);
```

2. Insert rows into multiple tables based on specific conditions using conditional INSERT FIRST. Records with a quantity greater than 4 are inserted into the `high_quantity_sales` table, and those with a total price exceeding 50 are inserted into the `high_price_sales` table. However, only the first matching condition is executed.

```sql
-- Conditional INSERT FIRST: Inserts each row into multiple tables, but stops after the first successful insertion.
INSERT FIRST
    WHEN quantity > 4 THEN INTO high_quantity_sales
    WHEN total_price > 50 THEN INTO high_price_sales
SELECT * FROM sales_data_source;

SELECT * FROM high_quantity_sales;

┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│     sale_id     │    product_id   │    sale_date   │     quantity    │        total_price       │
├─────────────────┼─────────────────┼────────────────┼─────────────────┼──────────────────────────┤
│               1 │             101 │ 2023-01-15     │               5 │ 100.00                   │
│               3 │             103 │ 2023-03-25     │              10 │ 200.00                   │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘

SELECT * FROM high_price_sales;

┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│     sale_id     │    product_id   │    sale_date   │     quantity    │        total_price       │
├─────────────────┼─────────────────┼────────────────┼─────────────────┼──────────────────────────┤
│               2 │             102 │ 2023-02-20     │               3 │ 75.00                    │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

In this example, the record with `sale_id` 1 is inserted into the `high_quantity_sales` table because it matches the first condition (`quantity > 4`). The record with `sale_id` 3 is also inserted into the `high_quantity_sales` table for the same reason. The record with `sale_id` 2 is inserted into the `high_price_sales` table because it matches the second condition (`total_price > 50`), but only after the first condition was not met.

┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│     sale_id     │    product_id   │    sale_date   │     quantity    │        total_price       │
├─────────────────┼─────────────────┼────────────────┼─────────────────┼──────────────────────────┤
│               1 │             101 │ 2023-01-15     │               5 │ 100.00                   │
│               2 │             102 │ 2023-02-20     │               3 │ 75.00                    │
│               3 │             103 │ 2023-03-25     │              10 │ 200.00                   │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

3. 清空 `high_quantity_sales` 和 `high_price_sales` 表中的数据。

```sql
TRUNCATE TABLE high_quantity_sales;

TRUNCATE TABLE high_price_sales;
```

4. 使用条件性 INSERT FIRST 根据特定条件将行插入多个表中。对于每一行，在第一次成功插入后停止，因此，与步骤 2 中的条件性 INSERT ALL 结果相比，销售记录 ID 为 1 和 3 的记录仅插入到 `high_quantity_sales` 表中。

```sql
-- 条件性 INSERT FIRST：将每一行插入多个表中，但在第一次成功插入后停止。
INSERT FIRST
    WHEN quantity > 4 THEN INTO high_quantity_sales
    WHEN total_price > 50 THEN INTO high_price_sales
SELECT * FROM sales_data_source;


SELECT * FROM high_quantity_sales;

┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│     sale_id     │    product_id   │    sale_date   │     quantity    │        total_price       │
├─────────────────┼─────────────────┼────────────────┼─────────────────┼──────────────────────────┤
│               1 │             101 │ 2023-01-15     │               5 │ 100.00                   │
│               3 │             103 │ 2023-03-25     │              10 │ 200.00                   │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘

SELECT * FROM high_price_sales;

┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│     sale_id     │    product_id   │    sale_date   │     quantity    │        total_price       │
├─────────────────┼─────────────────┼────────────────┼─────────────────┼──────────────────────────┤
│               2 │             102 │ 2023-02-20     │               3 │ 75.00                    │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 示例-3：使用显式别名插入

此示例演示了在 VALUES 子句中使用别名，根据雇佣日期在 '2023-02-01' 之后，将 `employees` 表中的行有条件地插入到 `employee_history` 表中。

1. 创建两个表，`employees` 和 `employee_history`，并将示例员工数据插入到 `employees` 表中。

```sql
-- 创建表
CREATE TABLE employees (
    employee_id INT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    hire_date DATE
);

CREATE TABLE employee_history (
    employee_id INT,
    full_name VARCHAR(100),
    hire_date DATE
);

INSERT INTO employees (employee_id, first_name, last_name, hire_date)
VALUES
    (1, 'John', 'Doe', '2023-01-01'),
    (2, 'Jane', 'Smith', '2023-02-01'),
    (3, 'Michael', 'Johnson', '2023-03-01');
```

2. 使用带有别名的条件性插入，将 `employees` 表中的记录转移到 `employee_history` 表中，过滤雇佣日期在 '2023-02-01' 之后的记录。

```sql
INSERT ALL
    WHEN hire_date >= '2023-02-01' THEN INTO employee_history
        VALUES (employee_id, full_name, hire_date) -- 使用别名 'full_name' 插入
SELECT employee_id, CONCAT(first_name, ' ', last_name) AS full_name, hire_date -- 将连接的全名别名为 'full_name'
FROM employees;

SELECT * FROM employee_history;

┌─────────────────────────────────────────────────────┐
│   employee_id   │     full_name    │    hire_date   │
│ Nullable(Int32) │ Nullable(String) │ Nullable(Date) │
├─────────────────┼──────────────────┼────────────────┤
│               2 │ Jane Smith       │ 2023-02-01     │
│               3 │ Michael Johnson  │ 2023-03-01     │
└─────────────────────────────────────────────────────┘
```