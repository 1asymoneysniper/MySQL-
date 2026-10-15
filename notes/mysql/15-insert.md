# 第 15 课：插入数据

> 复习笔记：使用 `INSERT` 向表中添加新行，也可以把查询结果插入另一张表。

## 1. 插入完整行

推荐明确写出列名：

```sql
INSERT INTO Customers
    (cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
VALUES
    ('1000000006', 'Toy Land', '123 Main St', 'Chicago', 'IL', '60001', 'USA');
```

## 2. 插入部分列

未指定的列必须允许 `NULL` 或有默认值：

```sql
INSERT INTO Customers (cust_id, cust_name)
VALUES ('1000000007', 'New Customer');
```

字符串和日期使用单引号，数字一般不加引号。

## 3. 插入多行

```sql
INSERT INTO Products (prod_id, vend_id, prod_name, prod_price)
VALUES
    ('TEST01', 'DLL01', 'Test product 1', 5.99),
    ('TEST02', 'DLL01', 'Test product 2', 7.99);
```

## 4. `INSERT ... SELECT`

```sql
INSERT INTO CustomersCopy (cust_id, cust_name)
SELECT cust_id, cust_name
FROM Customers;
```

目标表必须事先存在，且目标列和查询返回列的数量、顺序、类型要匹配。

## 5. 安全检查

插入前确认主键没有重复，外键对应的父记录存在；生产环境中不要依赖列的默认顺序。
