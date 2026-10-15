# 第 13 课挑战题：创建高级联结

## 题目 1

使用 `INNER JOIN`，返回每个顾客的名称 `cust_name` 和所有订单号 `order_num`。

## 题目 2

修改上一题的 SQL，列出所有顾客，即使他们没有下过订单。

## 题目 3

使用 `OUTER JOIN` 联结 `Products` 和 `OrderItems`，返回产品名称 `prod_name` 及其相关订单号 `order_num`，并按产品名称排序。

## 题目 4

修改上一题，返回每种产品的总订单数，而不是订单号列表。

## 题目 5

使用 `OUTER JOIN` 和 `COUNT()`，列出供应商 `Vendors.vend_id` 及其可供产品数量，包括没有产品的供应商。由于多个表都有 `vend_id`，每次引用时都要完全限定或使用表别名限定。
