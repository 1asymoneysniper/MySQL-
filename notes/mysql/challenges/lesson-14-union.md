# 第 14 课挑战题：组合查询

## 题目 1

使用两个 `SELECT` 和 `UNION`，从 `OrderItems` 返回 `prod_id` 和 `quantity`：一个查询过滤 `quantity = 100`，另一个查询过滤产品 ID 以 `BNBG` 开头。按产品 ID 排序。

## 题目 2

只使用一个 `SELECT`，重写上一题的 SQL。

## 题目 3

组合 `Products.prod_name` 和 `Customers.cust_name`，返回一个结果集，并按产品名称排序。

## 题目 4

下面的 SQL 有问题吗？尝试在不运行的情况下指出：

```sql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state = 'MI'
ORDER BY cust_name;
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state = 'IL'
ORDER BY cust_name;
```
