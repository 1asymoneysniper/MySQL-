# 第 19 课：使用存储过程

> 复习笔记：存储过程是保存在数据库中的一组 SQL 语句，可接收参数并重复调用。

## 1. 为什么使用存储过程

- 封装复杂业务逻辑。
- 减少重复 SQL。
- 统一权限入口，隐藏底层表结构。
- 把多步操作集中管理。

## 2. MySQL 创建和调用

```sql
DELIMITER //

CREATE PROCEDURE ProductCountByVendor(IN vendor_id CHAR(5))
BEGIN
    SELECT COUNT(*) AS product_count
    FROM Products
    WHERE vend_id = vendor_id;
END //

DELIMITER ;

CALL ProductCountByVendor('DLL01');
```

`DELIMITER` 是客户端命令，用来避免过程体中的分号被提前当成结束符。

## 3. 参数类型

- `IN`：输入参数。
- `OUT`：输出参数。
- `INOUT`：既作为输入又作为输出。

```sql
DELIMITER //
CREATE PROCEDURE GetCustomerCount(OUT total INT)
BEGIN
    SELECT COUNT(*) INTO total FROM Customers;
END //
DELIMITER ;

CALL GetCustomerCount(@total);
SELECT @total;
```

## 4. 管理存储过程

```sql
SHOW PROCEDURE STATUS;
DROP PROCEDURE IF EXISTS ProductCountByVendor;
```

存储过程语法、变量和错误处理比普通查询更依赖具体 DBMS，使用前应查阅 MySQL 版本文档。
