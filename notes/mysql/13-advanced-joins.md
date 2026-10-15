# 第 13 课：创建高级联结

> 复习笔记：表别名、自联结、自然联结、外联结，以及联结后的聚集统计。

## 1. 表别名

```sql
SELECT v.vend_name, p.prod_name
FROM Vendors AS v
JOIN Products AS p
    ON p.vend_id = v.vend_id;
```

别名只在当前查询中有效，尤其适合长表名、自联结和多表查询。

## 2. 自联结

同一张表在查询中使用两次。比如查找与某产品同一供应商提供的其他产品：

```sql
SELECT p1.prod_id, p1.prod_name
FROM Products AS p1
JOIN Products AS p2
    ON p1.vend_id = p2.vend_id
WHERE p2.prod_id = 'DTNTR'
  AND p1.prod_id <> p2.prod_id;
```

## 3. 外联结

`LEFT JOIN` 保留左表所有行，即使右表没有匹配：

```sql
SELECT v.vend_id, p.prod_id
FROM Vendors AS v
LEFT JOIN Products AS p
    ON p.vend_id = v.vend_id;
```

MySQL 中 `LEFT OUTER JOIN` 与 `LEFT JOIN` 等价。

## 4. 外联结与 `COUNT()`

```sql
SELECT v.vend_id,
       COUNT(p.prod_id) AS product_count
FROM Vendors AS v
LEFT JOIN Products AS p
    ON p.vend_id = v.vend_id
GROUP BY v.vend_id;
```

统计右表数量时要数右表非空字段。`COUNT(*)` 会把没有匹配产品的保留行也算作 1。

## 5. 联结和聚集

如果一个订单项表中同一个订单有多行，统计订单数量时要区分“订单项行数”和“不同订单数”：

```sql
SELECT p.prod_name,
       COUNT(DISTINCT oi.order_num) AS total_orders
FROM Products AS p
LEFT JOIN OrderItems AS oi
    ON oi.prod_id = p.prod_id
GROUP BY p.prod_id, p.prod_name
ORDER BY p.prod_name;
```

## 6. 易错点

自然联结依赖同名列，容易意外联结多余列；实际项目中通常优先写出明确的 `ON` 条件。
