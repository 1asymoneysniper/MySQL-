# 第 18 课：使用视图

> 复习笔记：视图是保存下来的查询结果定义，可以像表一样查询，但通常不直接存储数据副本。

## 1. 创建视图

```sql
CREATE VIEW ProductCustomers AS
SELECT c.cust_name,
       c.cust_contact,
       oi.order_num
FROM Customers AS c
JOIN Orders AS o ON o.cust_id = c.cust_id
JOIN OrderItems AS oi ON oi.order_num = o.order_num;
```

之后可以直接查询：

```sql
SELECT *
FROM ProductCustomers
WHERE order_num = 20007;
```

## 2. 用视图简化复杂查询

```sql
CREATE VIEW VendorLocations AS
SELECT CONCAT(RTRIM(vend_name), ' (', RTRIM(vend_country), ')') AS vend_title
FROM Vendors;

SELECT * FROM VendorLocations;
```

视图可以隐藏复杂联结、统一列名，并限制用户只看到需要的数据。

## 3. 更新和删除视图

```sql
CREATE OR REPLACE VIEW VendorLocations AS
SELECT vend_id, vend_name, vend_country
FROM Vendors;

DROP VIEW VendorLocations;
```

视图是否可更新取决于定义方式。包含聚集、`GROUP BY`、`DISTINCT`、`UNION` 或复杂联结的视图通常不适合直接更新。

## 4. 视图的边界

视图不会自动保存创建时的数据快照；查询视图时，通常仍会读取基础表的当前数据。不要把视图误认为备份。
