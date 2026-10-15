# 第 20 课：管理事务处理

> 复习笔记：事务把多条 SQL 作为一个整体执行，保证要么全部成功，要么全部撤销。

## 1. 基本操作

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 100
WHERE id = 1;

UPDATE accounts
SET balance = balance + 100
WHERE id = 2;

COMMIT;
```

发生错误时：

```sql
ROLLBACK;
```

`BEGIN` 也常用于开始事务。`COMMIT` 确认修改，`ROLLBACK` 撤销尚未提交的修改。

## 2. 保存点

```sql
START TRANSACTION;
UPDATE Orders SET cust_id = '1000000001' WHERE order_num = 20005;
SAVEPOINT before_items;
-- 其他操作
ROLLBACK TO SAVEPOINT before_items;
COMMIT;
```

保存点可以只撤销事务中的一部分操作。

## 3. ACID

- 原子性：事务中的操作不可分割。
- 一致性：事务前后数据满足约束。
- 隔离性：并发事务之间尽量互不干扰。
- 持久性：提交后的结果不会因普通故障丢失。

## 4. 注意事项

事务依赖存储引擎；MySQL 中通常使用支持事务的 `InnoDB`。DDL、隐式提交和连接断开可能影响事务边界。事务不要长时间占用锁。
