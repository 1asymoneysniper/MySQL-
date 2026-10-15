# 第 11 课挑战题：使用子查询

## 题目 1

使用子查询，返回购买价格为 10 美元或以上产品的顾客列表。使用 `OrderItems` 找到匹配的订单号，再使用 `Orders` 找到顾客 ID。

## 题目 2

使用子查询确定购买产品 `BR01` 的订单，并从 `Orders` 返回对应的顾客 ID `cust_id` 和订单日期 `order_date`，按订单日期排序。

## 题目 3

返回购买产品 `BR01` 的所有顾客电子邮件地址 `cust_email`。最内层从 `OrderItems` 返回 `order_num`，中间层从 `Orders` 返回 `cust_id`。

## 题目 4

返回顾客 ID `cust_id`，并使用子查询计算每个顾客订购金额 `total_ordered`，按金额从大到小排序。

## 题目 5

从 `Products` 返回所有产品名称 `prod_name`，以及计算列 `quant_sold`，其中包含每种产品的销售总数量。使用子查询和 `SUM(quantity)` 完成。
