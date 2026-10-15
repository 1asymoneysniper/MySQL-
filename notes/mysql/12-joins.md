# 第 12 课：联结表

> 复习笔记：使用表之间的关联列，把分散在多个表中的数据组合到同一个查询结果中。

## 1. 内连接

```sql
SELECT v.vend_name,
       p.prod_name,
       p.prod_price
FROM Vendors AS v
INNER JOIN Products AS p
    ON v.vend_id = p.vend_id;
```

`ON` 指定联结条件。列名相同并不是联结的必要条件，但有歧义时要使用表名或别名限定列名。

## 2. 隐式联结与显式联结

旧写法：

```sql
FROM Vendors v, Products p
WHERE v.vend_id = p.vend_id
```

推荐使用显式 `JOIN ... ON`，因为联结条件和过滤条件分开，阅读和排错更容易。

## 3. 联结多个表

```sql
SELECT o.order_num,
       c.cust_name,
       p.prod_name,
       oi.quantity
FROM Orders AS o
JOIN Customers AS c ON c.cust_id = o.cust_id
JOIN OrderItems AS oi ON oi.order_num = o.order_num
JOIN Products AS p ON p.prod_id = oi.prod_id
WHERE o.order_num = 20007;
```

## 4. 完全限定列名

当多个表都有同名列时，必须写成：

```sql
v.vend_id
p.vend_id
```

表别名比完整表名更简洁，也能减少歧义。

## 5. 易错点

少写一个联结条件会产生笛卡尔积，结果行数会异常膨胀。每增加一张表，都要确认它与已有结果之间的关联条件。
