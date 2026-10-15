# 第 12 课挑战题：联结表

## 题目 1

返回 `Customers.cust_name` 和 `Orders.order_num`，并按顾客名称、订单号排序。分别尝试简单等值联结语法和 `INNER JOIN` 语法。

## 题目 2

在上一题的基础上增加 `OrderTotal` 列，返回每个订单的总价。可以使用 `OrderItems` 的子查询，或联结 `OrderItems` 并使用聚集函数。

## 题目 3

使用简单等值联结，检索订购产品 `BR01` 的日期。结果应与第 11 课挑战题 2 相同。

## 题目 4

使用 ANSI `INNER JOIN` 语法，重新完成第 11 课挑战题 3，返回购买 `BR01` 产品的所有顾客电子邮件。

## 题目 5

使用联结、聚集函数和分组，返回订单总价至少为 1000 的顾客名称和订单总价。需要联结 `Customers`、`Orders` 和 `OrderItems`，使用 `GROUP BY`、`HAVING`，并按顾客名称排序。
