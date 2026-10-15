# 第 9 课：汇总数据

> 复习笔记：使用聚集函数对多行数据进行计数、求和、求平均值、求最大值和最小值。

## 1. 常用聚集函数

```sql
SELECT AVG(prod_price) AS avg_price,
       MAX(prod_price) AS max_price,
       MIN(prod_price) AS min_price,
       SUM(prod_price) AS total_price,
       COUNT(*) AS row_count
FROM Products;
```

- `COUNT(*)`：统计行数，包括字段为 `NULL` 的行。
- `COUNT(column)`：只统计该列非 `NULL` 的行。
- `AVG()`：平均值。
- `MAX()` / `MIN()`：最大值和最小值。
- `SUM()`：总和。

```sql
SELECT COUNT(cust_email) AS email_count
FROM Customers;
```

## 2. 条件汇总

```sql
SELECT AVG(prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';
```

`WHERE` 会先筛选行，然后聚集函数只对筛选后的结果计算。

## 3. `DISTINCT` 与聚集

```sql
SELECT AVG(DISTINCT prod_price) AS avg_distinct_price
FROM Products;
```

`DISTINCT` 先排除重复值，再进行聚集。`COUNT(DISTINCT column)` 可统计不重复值：

```sql
SELECT COUNT(DISTINCT vend_id) AS vendor_count
FROM Products;
```

## 4. 订单金额示例

```sql
SELECT SUM(quantity * item_price) AS order_total
FROM OrderItems
WHERE order_num = 20005;
```

## 5. 易错点

`COUNT(*)` 和 `COUNT(column)` 对 `NULL` 的处理不同。`AVG()`、`SUM()` 等通常会忽略 `NULL`，如果所有值都是 `NULL`，结果可能也是 `NULL`，需要时可使用 `COALESCE()` 设置默认值。

```sql
SELECT COALESCE(SUM(quantity), 0) AS total_quantity
FROM OrderItems;
```
