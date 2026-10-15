# 第 14 课：组合查询

> 复习笔记：使用 `UNION` 把多个 `SELECT` 的结果合并为一个结果集。

## 1. 基本语法

```sql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';
```

`UNION` 默认去除重复行；`UNION ALL` 保留重复行：

```sql
SELECT ...
UNION ALL
SELECT ...;
```

## 2. UNION 的规则

每个 `SELECT` 必须：

1. 返回相同数量的列。
2. 对应列的数据类型兼容。
3. 通常使用相同顺序表达相同含义。

列名一般由第一个 `SELECT` 决定：

```sql
SELECT cust_name AS name FROM Customers
UNION
SELECT vend_name FROM Vendors;
```

## 3. 排序

整个组合结果只能在最后使用一个 `ORDER BY`：

```sql
SELECT cust_name AS name FROM Customers
UNION
SELECT vend_name FROM Vendors
ORDER BY name;
```

## 4. 什么时候使用 UNION

- 多个查询返回相同结构，但来源或过滤条件不同。
- 合并顾客和供应商等不同表中的相同类型信息。

如果目标是把不同表的列横向拼在一起，应使用 `JOIN`，不是 `UNION`。
