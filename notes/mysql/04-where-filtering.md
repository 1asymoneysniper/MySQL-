# 第 4 课：过滤数据

> 复习笔记：使用 `WHERE` 从表中筛选满足条件的行。

## 1. `WHERE` 的作用

表中通常有很多行，但查询时往往只需要其中一部分。`WHERE` 用来指定筛选条件：

```sql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```

这条语句只返回价格等于 `3.49` 的产品。

基本结构：

```sql
SELECT 列名
FROM 表名
WHERE 条件;
```

在数据库服务器中完成过滤，通常比先返回大量数据、再交给应用程序过滤更高效，也能减少网络传输。

## 2. `WHERE` 的位置

如果同时使用 `WHERE` 和 `ORDER BY`，正确顺序是：

```sql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price >= 5
ORDER BY prod_price DESC;
```

记忆顺序：

```text
SELECT
FROM
WHERE
ORDER BY
LIMIT
```

## 3. 常用比较运算符

| 运算符 | 含义 |
| --- | --- |
| `=` | 等于 |
| `<>` | 不等于 |
| `!=` | 不等于，MySQL 支持 |
| `>` | 大于 |
| `>=` | 大于等于 |
| `<` | 小于 |
| `<=` | 小于等于 |
| `BETWEEN` | 在范围内 |
| `IS NULL` | 是空值 |
| `IS NOT NULL` | 不是空值 |

不同数据库对少数扩展运算符的支持可能不同。MySQL 中最常用的是上表这些写法。

## 4. 检查数值

```sql
-- 小于 10
SELECT prod_name, prod_price
FROM Products
WHERE prod_price < 10;

-- 小于或等于 10
SELECT prod_name, prod_price
FROM Products
WHERE prod_price <= 10;

-- 大于或等于 9
SELECT prod_id, prod_name
FROM Products
WHERE prod_price >= 9;
```

## 5. 检查不相等的值

```sql
SELECT vend_id, prod_name
FROM Products
WHERE vend_id <> 'DLL01';
```

MySQL 中 `<>` 和 `!=` 通常都可以表示“不等于”：

```sql
WHERE vend_id != 'DLL01';
```

## 6. 字符串和数字的引号

字符串通常使用单引号：

```sql
WHERE vend_id = 'DLL01'
```

数值通常不加引号：

```sql
WHERE prod_price >= 9
```

日期也建议使用字符串形式，并遵循明确格式：

```sql
WHERE order_date >= '2020-01-01'
```

## 7. 使用 `BETWEEN` 检查范围

```sql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price BETWEEN 5 AND 10;
```

`BETWEEN` 包含范围两端的值，也就是等价于：

```sql
WHERE prod_price >= 5
  AND prod_price <= 10
```

日期也可以使用 `BETWEEN`，但处理带时间的 `DATETIME` 时要特别注意结束时间是否覆盖完整的一天。

## 8. 检查 `NULL`

`NULL` 表示“没有值”，它不同于：

- `0`
- 空字符串 `''`
- 只有空格的字符串

不能这样判断：

```sql
-- 错误或不能得到预期结果
WHERE cust_email = NULL
```

应该使用 `IS NULL`：

```sql
SELECT cust_name
FROM Customers
WHERE cust_email IS NULL;
```

查询非空值使用 `IS NOT NULL`：

```sql
SELECT cust_name, cust_email
FROM Customers
WHERE cust_email IS NOT NULL;
```

## 9. `NULL` 与不等于

下面的条件不会把 `NULL` 行自动包含进来：

```sql
WHERE cust_email <> 'test@example.com'
```

如果既要排除某个邮箱，又要保留空邮箱，需要明确写出 `NULL` 条件：

```sql
WHERE cust_email <> 'test@example.com'
   OR cust_email IS NULL;
```

## 10. 与排序和限制结合

```sql
SELECT prod_id, prod_name, prod_price
FROM Products
WHERE prod_price BETWEEN 3 AND 6
ORDER BY prod_price ASC
LIMIT 5;
```

这条语句的执行逻辑可以理解为：

1. 从 `Products` 表读取数据。
2. 过滤出价格在 3 到 6 之间的行。
3. 按价格升序排列。
4. 最多返回 5 行。

## 11. 常见错误

### `WHERE` 放在 `ORDER BY` 后面

错误：

```sql
SELECT prod_name
FROM Products
ORDER BY prod_price
WHERE prod_price >= 5;
```

正确：

```sql
SELECT prod_name
FROM Products
WHERE prod_price >= 5
ORDER BY prod_price;
```

### 用 `= NULL` 判断空值

错误：

```sql
WHERE prod_price = NULL
```

正确：

```sql
WHERE prod_price IS NULL
```

### 字符串忘记加引号

正确：

```sql
WHERE vend_id = 'DLL01'
```

### 把 `BETWEEN` 的范围写反

```sql
-- 正确：低值在前，高值在后
WHERE prod_price BETWEEN 3 AND 6
```

## 12. 练习题与参考答案

```sql
USE sql_book;

-- 1. 查询价格为 9.49 的产品
SELECT prod_id, prod_name
FROM Products
WHERE prod_price = 9.49;

-- 2. 查询价格不低于 9 的产品
SELECT prod_id, prod_name
FROM Products
WHERE prod_price >= 9;

-- 3. 查询订购数量达到 100 或更多的不同订单号
SELECT DISTINCT order_num
FROM OrderItems
WHERE quantity >= 100;

-- 4. 查询价格在 3 到 6 之间的产品，并按价格排序
SELECT prod_name, prod_price
FROM Products
WHERE prod_price BETWEEN 3 AND 6
ORDER BY prod_price;
```

## 13. 记忆要点

```text
WHERE 条件                 -- 过滤行
= / <> / !=               -- 等于 / 不等于
> / >= / < / <=            -- 比较数值或日期
BETWEEN low AND high       -- 包含两端的范围
IS NULL                    -- 判断空值
IS NOT NULL                -- 判断非空值
```
