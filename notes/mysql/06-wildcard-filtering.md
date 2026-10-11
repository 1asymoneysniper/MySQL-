# 第 6 课：用通配符进行过滤

> 复习笔记：使用 `LIKE` 和通配符，按“模式”而不是完整值进行文本搜索。

## 1. `LIKE` 的作用

普通比较适合已知的完整值；当只知道文本的一部分时，可以使用 `LIKE`：

```sql
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE 'Fish%';
```

通配符搜索主要用于文本字段。搜索模式可以由普通文字和通配符组成。

## 2. `%`：匹配任意数量的字符

`%` 可以匹配 0 个、1 个或多个字符。

```sql
-- 以 Fish 开头
WHERE prod_name LIKE 'Fish%'

-- 任意位置包含 bean bag
WHERE prod_name LIKE '%bean bag%'

-- 以 F 开头、以 y 结尾
WHERE prod_name LIKE 'F%y'
```

查找邮箱的一部分时也很常见：

```sql
WHERE email LIKE 'b%@example.com'
```

`%` 不匹配 `NULL`。此外，前导 `%` 往往无法有效利用普通索引，数据量很大时查询可能较慢。

## 3. `_`：匹配恰好一个字符

```sql
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '__ inch teddy bear';
```

两个下划线恰好匹配两个字符，因此可以匹配 `12 inch teddy bear`，但不能匹配只有一个数字的 `8 inch teddy bear`。

对比：

```sql
-- 恰好两个字符
LIKE '__ inch teddy bear'

-- 前面可以是任意数量的字符
LIKE '% inch teddy bear'
```

## 4. 方括号通配符的兼容性

有些数据库支持使用 `[JM]%` 匹配以 J 或 M 开头的文本，但 MySQL 不支持这种 SQL Server 风格的方括号集合通配符。

在 MySQL 中建议使用等价的逻辑条件：

```sql
WHERE cust_contact LIKE 'J%'
   OR cust_contact LIKE 'M%'
```

## 5. `NOT LIKE`

查找不包含某段文字的记录：

```sql
SELECT prod_name, prod_desc
FROM Products
WHERE prod_desc NOT LIKE '%toy%'
ORDER BY prod_name;
```

如果还要处理 `NULL`，记得 `NOT LIKE` 不会自动把 `NULL` 包含进来：

```sql
WHERE prod_desc NOT LIKE '%toy%'
   OR prod_desc IS NULL;
```

## 6. 组合多个 `LIKE`

```sql
SELECT prod_name, prod_desc
FROM Products
WHERE prod_desc LIKE '%toy%'
  AND prod_desc LIKE '%carrots%';
```

这表示两个词都出现，但不强制要求它们在文本中按特定顺序出现。

如果要求先出现 `toy`，后出现 `carrots`：

```sql
WHERE prod_desc LIKE '%toy%carrots%'
```

## 7. 大小写和尾部空格

是否区分大小写由 MySQL 的字符集和排序规则决定。`'fish%'` 是否匹配 `Fish...`，应以当前数据库配置为准。

有些字符类型会用空格补齐长度，可能影响“以某字符结尾”的模式。必要时可以配合 `RTRIM()` 清理右侧空格，或者在模式末尾增加 `%`。

## 8. 使用通配符的建议

- 能用 `=`、范围比较等普通运算符时，优先使用普通运算符。
- 尽量避免以 `%` 开头的搜索模式，尤其是在大表上。
- 仔细检查通配符的位置，`%` 和 `_` 的含义不同。
- 确认搜索列的数据类型是文本类型。

## 9. 练习题与参考答案

```sql
USE sql_book;

-- 1. 描述中包含 toy
SELECT prod_name, prod_desc
FROM Products
WHERE prod_desc LIKE '%toy%';

-- 2. 描述中不包含 toy
SELECT prod_name, prod_desc
FROM Products
WHERE prod_desc NOT LIKE '%toy%'
ORDER BY prod_name;

-- 3. 描述中同时包含 toy 和 carrots
SELECT prod_name, prod_desc
FROM Products
WHERE prod_desc LIKE '%toy%'
  AND prod_desc LIKE '%carrots%';

-- 4. 描述中先出现 toy，后出现 carrots
SELECT prod_name, prod_desc
FROM Products
WHERE prod_desc LIKE '%toy%carrots%';
```

## 10. 记忆要点

```text
LIKE 'Fish%'       以 Fish 开头
LIKE '%bean bag%'  任意位置包含 bean bag
LIKE '__ inch%'    开头恰好两个字符
NOT LIKE '%toy%'   不包含 toy
```
