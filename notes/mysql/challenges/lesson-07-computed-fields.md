# 第 7 课挑战题：创建计算字段

## 题目 1

从 `Vendors` 表中检索：

- `vend_id`
- `vend_name`
- `vend_address`
- `vend_city`

将结果列重命名为：

- `vend_name` → `vname`
- `vend_city` → `vcity`
- `vend_address` → `vaddress`

最后按供应商名称排序，可以使用原始列名或别名。

## 题目 2

商店进行九折促销。从 `Products` 表中返回：

- `prod_id`
- 原价 `prod_price`
- 计算得到的促销价 `sale_price`

提示：促销价等于原价乘以 `0.9`。
