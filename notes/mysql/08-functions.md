# 第 8 课：使用函数处理数据

> 复习笔记：用函数处理文本、日期、数值和系统信息。函数名称可能因数据库而不同，以下以 MySQL 为主。

## 1. 文本处理函数

```sql
SELECT vend_name,
       UPPER(vend_name) AS upper_name,
       LOWER(vend_name) AS lower_name,
       TRIM(vend_name) AS clean_name
FROM Vendors;
```

- `CONCAT(a, b)`：拼接字符串。
- `UPPER()` / `LOWER()`：转大写或小写。
- `TRIM()`、`LTRIM()`、`RTRIM()`：去除空格。
- `LENGTH()`：返回字符串长度。
- `SUBSTRING(str, start, length)`：截取字符串。

```sql
SELECT CONCAT(RTRIM(vend_name), ' (', vend_country, ')') AS vend_title
FROM Vendors
ORDER BY vend_name;
```

## 2. 日期和时间函数

```sql
SELECT order_num,
       order_date,
       YEAR(order_date) AS order_year,
       MONTH(order_date) AS order_month,
       DAY(order_date) AS order_day
FROM Orders;
```

常用函数：`CURDATE()`、`NOW()`、`DATE()`、`YEAR()`、`MONTH()`、`DAY()`、`DATEDIFF()`。

```sql
SELECT order_num
FROM Orders
WHERE YEAR(order_date) = 2020;
```

日期过滤时，注意列中是否包含时间部分。需要查询一天的记录时，通常使用半开区间更安全：

```sql
WHERE order_date >= '2020-01-01'
  AND order_date <  '2020-01-02'
```

## 3. 数值函数

```sql
SELECT ABS(-10) AS absolute_value,
       ROUND(3.14159, 2) AS rounded_value,
       CEILING(3.2) AS ceiling_value,
       FLOOR(3.8) AS floor_value;
```

常见函数：`ABS()`、`ROUND()`、`CEILING()`、`FLOOR()`、`MOD()`。

## 4. 重要提醒

函数会影响可移植性：同一个需求在 MySQL、SQL Server、Oracle 中可能使用不同函数。写 SQL 前要确认目标数据库。函数放在 `WHERE` 中有时会让索引难以生效，应优先考虑直接比较原始列。

## 5. 记忆要点

```text
文本：CONCAT / TRIM / UPPER / LOWER
日期：YEAR / MONTH / DAY / DATEDIFF
数值：ROUND / ABS / CEILING / FLOOR
```
