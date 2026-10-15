# 第 22 课：高级 SQL 特性

> 复习笔记：约束保证数据合法，索引提高查询效率，触发器在数据变化时自动执行逻辑。

## 1. 约束

### 主键

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL
);
```

主键唯一且非空，每张表通常有一个主键。

### 外键

```sql
ALTER TABLE Orders
ADD CONSTRAINT fk_orders_customer
FOREIGN KEY (cust_id) REFERENCES Customers(cust_id);
```

外键保证子表中的引用确实存在于父表，维护引用完整性。

### 唯一、检查和非空

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    email VARCHAR(100) NOT NULL UNIQUE,
    age INT CHECK (age >= 18)
);
```

MySQL 版本和存储引擎会影响部分约束行为，使用前确认目标版本。

## 2. 索引

```sql
CREATE INDEX idx_products_vend_id
ON Products(vend_id);

SHOW INDEX FROM Products;
DROP INDEX idx_products_vend_id ON Products;
```

索引可以加快 `WHERE`、`JOIN`、`ORDER BY` 等操作，但会占用空间，并增加 `INSERT`、`UPDATE`、`DELETE` 的维护成本。不要给每个字段都建索引，应根据真实查询和执行计划决定。

## 3. 触发器

触发器会在 `INSERT`、`UPDATE` 或 `DELETE` 时自动执行：

```sql
DELIMITER //

CREATE TRIGGER products_before_insert
BEFORE INSERT ON Products
FOR EACH ROW
BEGIN
    IF NEW.prod_price < 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'price cannot be negative';
    END IF;
END //

DELIMITER ;
```

`NEW.column` 表示即将写入的新值；在更新触发器中还可以使用 `OLD.column` 表示旧值。

## 4. 数据库安全

使用最小权限原则，只授予用户完成工作所需的权限；不要在应用程序中硬编码 root 密码；生产环境中要保护备份和连接信息。

## 5. 选择顺序

```text
先用约束保证正确
再根据查询建立索引
最后谨慎使用触发器封装自动逻辑
```
