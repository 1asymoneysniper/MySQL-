# 第 16 课：更新和删除数据

> 复习笔记：使用 `UPDATE` 修改已有行，使用 `DELETE` 删除行。最重要的安全原则是先用相同条件执行 `SELECT`。

## 1. 更新单列或多列

```sql
UPDATE Customers
SET cust_email = 'new@example.com'
WHERE cust_id = '1000000001';
```

```sql
UPDATE Products
SET prod_price = prod_price * 1.1,
    prod_name = CONCAT(prod_name, ' (updated)')
WHERE vend_id = 'DLL01';
```

没有 `WHERE` 会更新整张表。

## 2. 删除数据

```sql
DELETE FROM Customers
WHERE cust_id = '1000000007';
```

没有 `WHERE` 会删除表中所有行，但不会删除表结构。删除表结构使用 `DROP TABLE`。

## 3. 操作前检查

```sql
SELECT * FROM Products
WHERE vend_id = 'DLL01';
```

确认筛选结果后，再执行对应的 `UPDATE` 或 `DELETE`。必要时先备份，并在事务中执行。

## 4. 外键和删除顺序

如果父表记录被子表外键引用，数据库可能阻止删除。通常应先处理子表记录，或按业务规则配置级联删除，不能随意绕过完整性约束。

## 5. 三条口诀

```text
先 SELECT，后 UPDATE/DELETE
WHERE 写清楚，范围要确认
重要修改放进事务，确认后再 COMMIT
```
