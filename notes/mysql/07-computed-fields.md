# 第 7 课：创建计算字段

> 复习笔记：在 `SELECT` 查询时拼接文本、执行计算，并使用别名给结果命名。

## 1. 什么是计算字段

计算字段不是表中实际存储的列，而是在查询执行时临时计算出来的结果。例如：

- 把供应商名称和国家拼接成一个显示字段。
- 把多个地址列组合成一段文字。
- 用数量乘以单价得到订单项总价。
- 给查询结果重新命名，方便应用程序引用。

计算通常直接在数据库中完成，应用程序可以像读取普通列一样读取计算结果。

## 2. MySQL 中拼接文本：`CONCAT()`

不同数据库的拼接语法不完全一样。MySQL 推荐使用 `CONCAT()`：

```sql
SELECT CONCAT(vend_name, ' (', vend_country, ')')
FROM Vendors
ORDER BY vend_name;
```

多个参数会按照顺序拼接成一个结果。

不要直接照搬其他数据库的写法：

```sql
-- 这是其他数据库常见的写法，不是 MySQL 中最稳妥的写法
vend_name + '(' + vend_country + ')'
vend_name || '(' || vend_country || ')'
```

## 3. 清理多余空格：`RTRIM()`、`LTRIM()`、`TRIM()`

```sql
SELECT CONCAT(RTRIM(vend_name), ' (', RTRIM(vend_country), ')')
FROM Vendors
ORDER BY vend_name;
```

- `RTRIM()`：删除右侧空格。
- `LTRIM()`：删除左侧空格。
- `TRIM()`：删除两侧空格。

实际使用时可以根据数据类型和表定义决定是否需要清理空格。

## 4. 使用列别名：`AS`

计算结果默认可能没有易读的列名。使用 `AS` 为它命名：

```sql
SELECT CONCAT(RTRIM(vend_name), ' (', RTRIM(vend_country), ')')
       AS vend_title
FROM Vendors
ORDER BY vend_name;
```

`vend_title` 是查询结果中的别名，不会自动在表中创建真实列。

`AS` 在很多数据库中可以省略，但建议保留，代码更容易阅读：

```sql
SELECT prod_price * 0.9 AS sale_price
FROM Products;
```

## 5. 数值计算

基本算术运算符：

| 运算符 | 含义 |
| --- | --- |
| `+` | 加 |
| `-` | 减 |
| `*` | 乘 |
| `/` | 除 |

例如，计算订单项总价：

```sql
SELECT prod_id,
       quantity,
       item_price,
       quantity * item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```

计算字段只在本次查询结果中存在，不会修改原表数据。

## 6. 计算折扣价

```sql
SELECT prod_id,
       prod_price,
       prod_price * 0.9 AS sale_price
FROM Products;
```

这里的 `sale_price` 是原价的 90%，相当于打九折。

## 7. 使用括号控制计算顺序

```sql
SELECT quantity * (item_price - 1.00) AS discounted_total
FROM OrderItems;
```

括号可以明确计算顺序，复杂表达式中建议主动使用。

## 8. 只测试表达式

`SELECT` 不一定需要 `FROM`。可以用它测试简单表达式：

```sql
SELECT 3 * 2 AS result;
SELECT TRIM('  abc  ') AS cleaned_text;
SELECT CURDATE() AS today;
```

函数和日期函数的具体名称可能因数据库而异，这里使用的是 MySQL 写法。

## 9. 常见错误

### 在 MySQL 中使用错误的拼接运算符

推荐：

```sql
SELECT CONCAT(vend_name, ' (', vend_country, ')') AS vend_title
FROM Vendors;
```

### 忘记给复杂结果起别名

推荐：

```sql
SELECT quantity * item_price AS expanded_price
FROM OrderItems;
```

### 误以为计算字段会修改原表

```sql
SELECT prod_price * 0.9 AS sale_price
FROM Products;
```

这只是查询结果，不会把 `prod_price` 永久改成折扣价。要修改表数据，需要使用 `UPDATE`，那是后续课程的内容。

## 10. 练习题与参考答案

```sql
USE sql_book;

-- 1. 为三个原始列设置别名
SELECT vend_id,
       vend_name AS vname,
       vend_address AS vaddress,
       vend_city AS vcity
FROM Vendors
ORDER BY vend_name;

-- 2. 计算打九折后的价格
SELECT prod_id,
       prod_price,
       prod_price * 0.9 AS sale_price
FROM Products;
```

## 11. 记忆要点

```text
CONCAT(a, b, c)       拼接文本
TRIM(text)            去掉两侧空格
RTRIM(text)           去掉右侧空格
expression AS alias   给计算结果命名
quantity * price      计算订单项总价
```
