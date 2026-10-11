# 第 3 课挑战题：排序检索数据

## 题目 1

从 `Customers` 表中检索顾客名称（`cust_name`），并按从 Z 到 A 的顺序显示。

## 题目 2

从 `Orders` 表中检索顾客 ID（`cust_id`）和订单号（`order_num`），先按顾客 ID 排序，再按订单日期倒序排列。

## 题目 3

从 `OrderItems` 表中显示数量（`quantity`）和价格（`item_price`），先按数量从多到少排序，数量相同时再按价格从高到低排序。

## 题目 4

判断下面的 SQL 是否有问题，并说明原因：

```sql
SELECT vend_name,
FROM Vendors
ORDER vend_name DESC;
```
