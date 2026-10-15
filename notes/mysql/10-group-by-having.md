# 第 10 课：分组数据

> 复习笔记：使用 `GROUP BY` 把行分组，再对每组进行聚集；使用 `HAVING` 过滤分组结果。

## 1. 创建分组

```sql
SELECT vend_id,
       COUNT(*) AS num_prods
FROM Products
GROUP BY vend_id;
```

每个 `vend_id` 是一组，`COUNT(*)` 对每组分别计算。

## 2. `WHERE` 与 `HAVING`

```sql
SELECT vend_id,
       COUNT(*) AS num_prods
FROM Products
WHERE prod_price >= 4
GROUP BY vend_id
HAVING COUNT(*) >= 2;
```

- `WHERE`：分组前过滤行。
- `HAVING`：分组后过滤组，适合过滤聚集结果。

## 3. 分组、排序和查询顺序

```sql
SELECT order_num,
       COUNT(*) AS items
FROM OrderItems
GROUP BY order_num
HAVING COUNT(*) >= 3
ORDER BY items DESC, order_num;
```

逻辑顺序可以记为：

```text
FROM/JOIN -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY -> LIMIT
```

## 4. 常见注意事项

1. `SELECT` 中出现的非聚集列，通常必须出现在 `GROUP BY` 中。
2. 不要用 `WHERE COUNT(*) > 1`，聚集结果应使用 `HAVING`。
3. `GROUP BY` 不是排序保证；需要排序时必须显式写 `ORDER BY`。
4. 使用 `LEFT JOIN` 统计右表记录时，应统计右表非空字段，例如 `COUNT(p.prod_id)`。
