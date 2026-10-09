# 第 2 课：检索数据

> 复习笔记：使用 `SELECT` 从表中读取数据。示例基于 `sql_book` 数据库。

## 1. 查询前先选择数据库

```sql
USE sql_book;
```

如果出现“表不存在”，先检查当前数据库：

```sql
SELECT DATABASE();
SHOW TABLES;
```

## 2. `SELECT` 的基本结构

```sql
SELECT 列名
FROM 表名;
```

- `SELECT` 后写要查看的列。
- `FROM` 后写数据来源表。
- 一条 SQL 语句通常以分号 `;` 结束。
- SQL 关键字一般不区分大小写，但表名、列名和值是否区分大小写取决于数据库和配置。统一使用大写关键字更容易阅读。

## 3. 检索单个列

```sql
SELECT prod_name
FROM Products;
```

这会返回 `Products` 表中的所有产品名称。没有排序时，不要依赖结果的显示顺序；需要固定顺序时使用 `ORDER BY`（下一课会详细学习）。

## 4. 检索多个列

列名之间用逗号分隔：

```sql
SELECT prod_id, prod_name, prod_price
FROM Products;
```

注意：列名之间使用逗号，不要使用 `AND` 或其他关键字。

## 5. 检索所有列

使用星号 `*`：

```sql
SELECT *
FROM Products;
```

`*` 适合快速查看表结构和数据。正式查询中，通常明确列名更清晰，也能避免表结构变化后返回不需要的列。

## 6. 删除重复结果：`DISTINCT`

```sql
SELECT DISTINCT vend_id
FROM Products;
```

`DISTINCT` 放在 `SELECT` 后、列名之前，表示只返回不重复的结果。

多个列一起使用时，`DISTINCT` 判断的是整行组合是否重复：

```sql
SELECT DISTINCT vend_id, prod_price
FROM Products;
```

## 7. 限制返回行数：MySQL 的 `LIMIT`

### 7.1 只取前几行

```sql
SELECT prod_name
FROM Products
LIMIT 5;
```

### 7. 跳过前几行，再取几行

```sql
SELECT prod_name
FROM Products
LIMIT 4 OFFSET 3;
```

含义是：跳过前 3 行，再返回 4 行。

MySQL 还支持简写形式：

```sql
SELECT prod_name
FROM Products
LIMIT 3, 4;
```

这里容易混淆：逗号前是 `OFFSET`，逗号后是返回行数。因此 `LIMIT 3, 4` 等价于 `LIMIT 4 OFFSET 3`。

如果需要稳定地取“前 5 个”，最好配合排序：

```sql
SELECT prod_id, prod_name
FROM Products
ORDER BY prod_id
LIMIT 5;
```

## 8. SQL 注释

### 单行注释

MySQL 中 `--` 后要有空格：

```sql
-- 查询所有产品
SELECT *
FROM Products;
```

也可以使用 `#`：

```sql
# 查询所有产品
SELECT *
FROM Products;
```

### 多行注释

```sql
/*
  这段内容不会执行。
  可以用来写较长的说明。
*/
SELECT prod_name
FROM Products;
```

## 9. 常见错误

### 忘记选择数据库

```sql
USE sql_book;
```

### `SELECT` 和 `FROM` 写反

正确：

```sql
SELECT prod_name
FROM Products;
```

### 多列之间漏写逗号

正确：

```sql
SELECT prod_id, prod_name, prod_price
FROM Products;
```

### 把 `LIMIT` 和 `OFFSET` 的顺序弄反

```sql
LIMIT 4 OFFSET 3   -- 跳过 3 行，取 4 行
LIMIT 3, 4         -- MySQL 简写，仍然是跳过 3 行，取 4 行
```

### 把注释分隔线当成 SQL

下面这种只有横线的内容不是合法 SQL：

```text
-------------------------------------------------------
```

应改成注释，或直接删除：

```sql
-- ---------------------------------------------------
```

## 10. 练习题

```sql
USE sql_book;

-- 1. 查询所有顾客 ID
SELECT cust_id
FROM Customers;

-- 2. 查询订购过的不同产品 ID
SELECT DISTINCT prod_id
FROM OrderItems;

-- 3. 查询 Customers 的全部列
SELECT *
FROM Customers;

-- 4. 只查询顾客 ID
SELECT cust_id
FROM Customers;
```

## 11. 记忆要点

```text
SELECT 选哪些列
FROM   从哪张表选
DISTINCT 去掉重复结果
LIMIT  限制返回行数
OFFSET 跳过前面的行
```

最基本的查询模板：

```sql
SELECT column_name
FROM table_name;
```
