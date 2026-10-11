# 第 5 课挑战题：高级数据过滤

## 题目 1

从 `Vendors` 表中检索供应商名称（`vend_name`），只返回美国加利福尼亚州的供应商。过滤条件需要同时考虑国家和州。

## 题目 2

从 `OrderItems` 表中找出订购数量至少为 100 的 `BR01`、`BR02` 或 `BR03` 产品。返回订单号（`order_num`）、产品 ID（`prod_id`）和数量（`quantity`）。注意正确处理 `AND` 与 `OR` 的求值顺序。

## 题目 3

使用 `AND` 完成价格范围过滤：返回价格在 3 到 6 之间的产品名称和价格，并按价格排序。

## 题目 4

判断下面的 SQL 是否有问题，并说明应如何调整子句顺序：

```sql
SELECT vend_name
FROM Vendors
ORDER BY vend_name
WHERE vend_country = 'USA' AND vend_state = 'CA';
```
