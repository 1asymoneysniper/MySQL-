# 第 11 课：使用子查询

> 复习笔记：把一个 `SELECT` 嵌套在另一个 SQL 语句中，用内部查询结果辅助外部查询。

## 1. 用 `IN` 过滤

查询购买了某产品的顾客：

```sql
SELECT cust_name, cust_contact
FROM Customers
WHERE cust_id IN (
    SELECT cust_id
    FROM Orders
    WHERE order_num IN (
        SELECT order_num
        FROM OrderItems
        WHERE prod_id = 'RGAN01'
    )
);
```

子查询由内向外执行：先找订单，再找顾客编号，最后找顾客信息。

## 2. 子查询作为计算字段

```sql
SELECT cust_name,
       (SELECT COUNT(*)
        FROM Orders
        WHERE Orders.cust_id = Customers.cust_id) AS orders
FROM Customers;
```

这里的子查询是相关子查询：内部查询引用了外部查询当前行的 `Customers.cust_id`。

## 3. 子查询与联结的选择

- 子查询适合表达“先找出一组 ID，再用这组 ID 过滤”的逻辑。
- 多表关联和统计通常使用 `JOIN` 更直观，也可能更容易优化。
- 子查询必须放在括号中，复杂 SQL 要换行并缩进。

## 4. 常见陷阱

- `NOT IN` 遇到子查询结果中的 `NULL` 时，可能得不到预期结果；这时可考虑 `NOT EXISTS`。
- 子查询返回多行时，不能使用只接受单个值的 `=`，应使用 `IN`。
