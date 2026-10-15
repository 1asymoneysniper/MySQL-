# 第 21 课：使用游标

> 复习笔记：游标允许按行遍历查询结果，适合必须逐行处理的过程式逻辑，但普通 SQL 集合操作通常更快。

## 1. 游标的生命周期

```text
DECLARE 游标 -> OPEN 游标 -> FETCH 取行 -> CLOSE 游标
```

游标一般在存储过程中使用，并配合循环和结束标志。

## 2. MySQL 示例

```sql
DELIMITER //

CREATE PROCEDURE ListMissingEmails()
BEGIN
    DECLARE done INT DEFAULT 0;
    DECLARE current_name VARCHAR(50);
    DECLARE cur CURSOR FOR
        SELECT cust_name
        FROM Customers
        WHERE cust_email IS NULL;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

    OPEN cur;

    read_loop: LOOP
        FETCH cur INTO current_name;
        IF done = 1 THEN
            LEAVE read_loop;
        END IF;
        SELECT current_name AS customer_name;
    END LOOP;

    CLOSE cur;
END //

DELIMITER ;
```

## 3. 为什么谨慎使用

游标逐行处理，通常比一次性使用 `UPDATE`、`JOIN`、`GROUP BY` 等集合操作慢，也会增加锁和代码复杂度。优先尝试用一条集合 SQL 完成任务，确实需要逐行逻辑时再使用游标。
