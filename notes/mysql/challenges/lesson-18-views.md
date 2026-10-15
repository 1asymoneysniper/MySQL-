# 第 18 课挑战题：使用视图

## 题目 1

创建名为 `CustomersWithOrders` 的视图。视图包含 `Customers` 表中的所有列，但只保留下过订单的顾客。可以在 `Orders` 表上使用 `JOIN` 过滤顾客，并使用 `SELECT` 保证返回正确列。

## 题目 2

下面的 SQL 有问题吗？尝试在不运行的情况下指出：

```sql
CREATE VIEW OrderItemsExpanded AS
SELECT order_num,
       prod_id,
       quantity,
       item_price,
       quantity * item_price AS expanded_price
FROM OrderItems
ORDER BY order_num;
```
