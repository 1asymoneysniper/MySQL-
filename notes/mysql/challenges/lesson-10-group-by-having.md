# 第 10 课挑战题：分组数据

## 题目 1

返回每个订单号 `order_num` 的订单行数 `order_lines`，并按 `order_lines` 排序。

## 题目 2

返回名为 `cheapest_item` 的字段，该字段包含每个供应商成本最低的产品价格 `prod_price`，并按价格从低到高排序。

## 题目 3

返回至少包含 100 项产品的所有订单号，数量来自 `OrderItems.quantity`。

## 题目 4

返回总价至少为 1000 的所有订单号。总价使用 `item_price * quantity` 计算，并按订单号排序。

## 题目 5

下面的 SQL 有问题吗？尝试在不运行的情况下指出问题：

```sql
SELECT order_num, COUNT(*) AS items
FROM OrderItems
GROUP BY items
HAVING COUNT(*) >= 3
ORDER BY items, order_num;
```
