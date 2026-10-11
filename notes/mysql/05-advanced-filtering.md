# 第 5 课：高级数据过滤

> 复习笔记：使用 `AND`、`OR`、`IN` 和 `NOT` 组合更复杂的 `WHERE` 条件。

## 1. `AND`：同时满足多个条件

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01'
  AND prod_price <= 4;
```

`AND` 要求一行同时满足左右两边的条件。多个条件可以继续用 `AND` 连接。

## 2. `OR`：满足任意一个条件

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01'
   OR vend_id = 'BRS01';
```

`OR` 只要求满足其中一个条件。

## 3. `AND` 与 `OR` 混用时使用括号

`AND` 的优先级高于 `OR`。为了避免误解，应主动使用括号：

```sql
SELECT prod_name, prod_price
FROM Products
WHERE (vend_id = 'DLL01' OR vend_id = 'BRS01')
  AND prod_price >= 10;
```

不建议依赖默认优先级。括号可以明确表达“先执行哪一组条件”。

## 4. `IN`：匹配值列表

下面两种写法逻辑相同：

```sql
WHERE vend_id = 'DLL01'
   OR vend_id = 'BRS01'
```

```sql
WHERE vend_id IN ('DLL01', 'BRS01')
```

完整示例：

```sql
SELECT prod_name, prod_price
FROM Products
WHERE vend_id IN ('DLL01', 'BRS01')
ORDER BY prod_name;
```

`IN` 在候选值较多时更清晰，也更容易和其他条件组合。值列表必须放在圆括号中。

## 5. `NOT`：否定条件

```sql
SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01'
ORDER BY prod_name;
```

简单的不等于条件也可以写成：

```sql
WHERE vend_id <> 'DLL01'
```

`NOT` 在否定 `IN`、`BETWEEN` 等复杂条件时更有帮助：

```sql
WHERE vend_id NOT IN ('DLL01', 'BRS01')
```

## 6. 常见组合模板

```sql
-- 同时满足
WHERE 条件1 AND 条件2

-- 满足任意一个
WHERE 条件1 OR 条件2

-- 先处理括号中的条件
WHERE (条件1 OR 条件2) AND 条件3

-- 不在列表中
WHERE column_name NOT IN ('A', 'B')

-- 不在范围内
WHERE price NOT BETWEEN 3 AND 6
```

## 7. 常见错误

### 忽略优先级

```sql
-- 容易产生误解
WHERE A OR B AND C
```

```sql
-- 更清晰
WHERE (A OR B) AND C
```

### `IN` 列表漏写括号或逗号

```sql
WHERE vend_id IN ('DLL01', 'BRS01');
```

### `ORDER BY` 放在 `WHERE` 前

```sql
SELECT vend_name
FROM Vendors
WHERE vend_country = 'USA'
  AND vend_state = 'CA'
ORDER BY vend_name;
```

## 8. 练习题与参考答案

```sql
USE sql_book;

-- 1. 查询美国加利福尼亚州的供应商
SELECT vend_name
FROM Vendors
WHERE vend_country = 'USA'
  AND vend_state = 'CA';

-- 2. 查询 BR01、BR02 或 BR03 中订购数量至少为 100 的订单项
SELECT order_num, prod_id, quantity
FROM OrderItems
WHERE prod_id IN ('BR01', 'BR02', 'BR03')
  AND quantity >= 100;

-- 3. 查询价格在 3 到 6 之间的产品并排序
SELECT prod_name, prod_price
FROM Products
WHERE prod_price >= 3
  AND prod_price <= 6
ORDER BY prod_price;
```

## 9. 记忆要点

```text
AND       所有条件都满足
OR        任意条件满足
IN        匹配一个值列表
NOT       否定后面的条件
(...)     明确控制求值顺序
```
