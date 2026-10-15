# 第 17 课：创建和操纵表

> 复习笔记：使用 DDL 创建、修改、删除和重命名表结构。

## 1. 创建表

```sql
CREATE TABLE student (
    student_id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    major VARCHAR(20),
    PRIMARY KEY (student_id)
);
```

常见数据类型：`INT`、`DECIMAL`、`VARCHAR(n)`、`DATE`、`DATETIME`、`TEXT`。

## 2. `NULL` 与默认值

- `NULL` 表示缺少值，不等于空字符串或 0。
- `NOT NULL` 强制列必须有值。
- `DEFAULT` 在插入时没有提供值时使用默认值。

```sql
CREATE TABLE orders_demo (
    id INT PRIMARY KEY,
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## 3. 修改表

```sql
ALTER TABLE student ADD email VARCHAR(100);
ALTER TABLE student MODIFY name VARCHAR(50) NOT NULL;
ALTER TABLE student DROP COLUMN email;
```

也可以使用 `ALTER TABLE` 添加主键、外键、唯一约束和索引。

## 4. 删除和重命名

```sql
DROP TABLE student;
RENAME TABLE student TO students;
```

`DROP TABLE` 会删除表结构和数据，执行前必须确认目标。

## 5. 设计建议

创建表时尽早确定主键、外键、非空和唯一约束；统一使用 `utf8mb4` 字符集，并根据业务选择合适的数据类型。
