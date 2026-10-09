# 第 3 课：排序检索数据

> 复习笔记：使用 `ORDER BY` 控制 `SELECT` 查询结果的显示顺序。

## 1. 为什么要显式排序

如果查询中没有 `ORDER BY`，数据库不保证结果顺序。即使某次看起来像按插入顺序返回，数据更新、删除或数据库执行计划变化后，顺序也可能改变。

## 2. 按一个列排序

```sql
SELECT prod_name
FROM Products
ORDER BY prod_name;
```

默认是升序排列：

- 文本通常按字典顺序排列。
- 数字从小到大排列。
- 日期从早到晚排列。

`ORDER BY` 可以使用没有出现在 `SELECT` 列表中的列：

```sql
SELECT prod_name
FROM Products
ORDER BY prod_price;
```

## 3. 按多个列排序

列名用逗号分隔，排序优先级从左到右：

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name;
```

含义是：

1. 先按 `prod_price` 排序。
2. 只有价格相同时，才继续按 `prod_name` 排序。

因此，后面的列只会在前面列值相同的行中发挥作用。

## 4. 按列在 `SELECT` 中的位置排序

也可以使用列的相对位置：

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY 2, 3;
```

这里：

- `2` 代表 `SELECT` 列表中的第二列 `prod_price`。
- `3` 代表第三列 `prod_name`。

这种写法较短，但不够稳妥：如果以后调整 `SELECT` 中的列顺序，排序含义也会随之改变。实际笔记和项目代码中通常更推荐直接写列名。

## 5. 指定升序和降序

### 5.1 降序：`DESC`

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC;
```

`DESC` 表示从大到小、从 Z 到 A 或从晚到早排列。

### 5.2 升序：`ASC`

```sql
SELECT prod_name
FROM Products
ORDER BY prod_name ASC;
```

`ASC` 表示升序，是默认行为，所以通常可以省略。

## 6. 多列使用不同排序方向

排序方向只作用于它前面的那一列：

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC, prod_name ASC;
```

含义是：

- 价格从高到低。
- 价格相同时，名称从 A 到 Z。

如果两列都要降序，必须分别写 `DESC`：

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC, prod_name DESC;
```

## 7. `ORDER BY` 的位置

在一个查询中，`ORDER BY` 应放在其他主要子句之后：

```sql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price >= 5
ORDER BY prod_price DESC;
```

常见顺序可以先记成：

```text
SELECT
FROM
WHERE
ORDER BY
LIMIT
```

本课重点是：`ORDER BY` 通常是查询主体中的最后一个排序子句；如果还要限制返回行数，`LIMIT` 放在它后面。

## 8. 与 `LIMIT` 配合

例如查询价格最高的 5 个产品：

```sql
SELECT prod_id, prod_name, prod_price
FROM Products
ORDER BY prod_price DESC
LIMIT 5;
```

如果没有 `ORDER BY`，`LIMIT 5` 只表示“返回某 5 行”，不代表一定是价格最高的 5 行。

## 9. 排序时的大小写问题

文本排序是否区分大小写，取决于数据库的字符集和排序规则（collation）。如果结果和预期不同，先检查数据库或列使用的排序规则，不要简单假定所有数据库的排序行为完全一样。

## 10. 常见错误

### 把 `ORDER BY` 放在 `WHERE` 前面

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

### 多列排序时漏写逗号

正确：

```sql
ORDER BY prod_price DESC, prod_name ASC;
```

### 以为一个 `DESC` 会影响所有列

```sql
-- 只有 prod_price 降序，prod_name 仍然默认升序
ORDER BY prod_price DESC, prod_name;
```

如果名称也要降序：

```sql
ORDER BY prod_price DESC, prod_name DESC;
```

### `SELECT` 列表最后多写逗号

错误：

```sql
SELECT vend_name,
FROM Vendors
ORDER BY vend_name DESC;
```

正确：

```sql
SELECT vend_name
FROM Vendors
ORDER BY vend_name DESC;
```

## 11. 练习题与参考答案

```sql
USE sql_book;

-- 1. 顾客名称按 Z 到 A 排列
SELECT cust_name
FROM Customers
ORDER BY cust_name DESC;

-- 2. 先按顾客 ID，再按订单日期倒序
SELECT cust_id, order_num, order_date
FROM Orders
ORDER BY cust_id, order_date DESC;

-- 3. 数量从多到少，数量相同时价格从高到低
SELECT quantity, item_price
FROM OrderItems
ORDER BY quantity DESC, item_price DESC;

-- 4. 价格最高的 5 个产品
SELECT prod_id, prod_name, prod_price
FROM Products
ORDER BY prod_price DESC
LIMIT 5;
```

## 12. 记忆要点

```text
ORDER BY column_name              -- 默认升序
ORDER BY column_name ASC          -- 明确升序
ORDER BY column_name DESC         -- 降序
ORDER BY col1, col2                -- 先 col1，再在相同值内按 col2
ORDER BY col1 DESC, col2 ASC       -- 每一列分别指定方向
```
