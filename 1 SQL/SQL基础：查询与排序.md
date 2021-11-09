# SELECT 语句
通过SELECT语句查询并选取出必要数据的过程称为匹配查询或查询（query），SELECT 语句通过WHERE子句来指定查询数据的条件。

## 从表中选取数据 (SELECT)
基本SELECT语句包含了SELECT和FROM两个子句（clause）。示例如下：
```SQL
SELECT <列名>,
  FROM <表名>;
```

## 从表中选取符合条件的数据 (WHERE)
```SQL
SELECT <列名>, ……
  FROM <表名>
 WHERE <条件表达式>;
 
-- 用来选取product type列为衣服’的记录的SELECT语句
SELECT product_name, product_type
  FROM product
 WHERE product_type = '衣服';
-- 也可以选取出不是查询条件的列（条件列与输出列不同）
SELECT product_name
  FROM product
 WHERE product_type = '衣服';
```

## 相关法则
- 星号（*）代表全部列的意思。
- SQL中可以随意使用换行符，不影响语句执行（但不可插入空行）。
- 设定汉语别名时需要使用双引号（"）括起来。
- 在SELECT语句中使用DISTINCT可以删除重复行。
- 注释是SQL语句中用来标识说明或者注意事项的部分。分为1行注释"-- "和多行注释两种"/* */"。
```SQL
-- 想要查询出全部列时，可以使用代表所有列的星号（*）。
SELECT *
  FROM <表名>;

-- SQL语句可以使用AS关键字为列设定别名（用中文时需要双引号（“”））。
SELECT product_id     AS id,
       product_name   AS name,
       purchase_price AS "进货单价"
  FROM product;

-- 使用DISTINCT删除product_type列中重复的数据
SELECT DISTINCT product_type
  FROM product;
```

# 算术运算符和比较运算符
## 算术运算符
SQL语句中可以使用的四则运算的主要运算符如下：
含义 | 运算符
--- | --- 
加法 |	+
减法 |	-
乘法 |	*
除法 | /

## 比较运算符
SQL常见比较运算符如下：
运算符 |	含义
--- | ---
=	| 和~相等
<> |	和~不相等
>= |	大于等于~
> |	大于~
<= |	小于等于~
< |	小于~
```SQL
-- 选取出sale_price列为500的记录
SELECT product_namep, roduct_type
  FROM product
 WHERE sale_price = 500;
```
## 常用法则
- SELECT子句中可以使用常数或者表达式。
- 使用比较运算符时一定要注意不等号和等号的位置。
- 字符串类型的数据原则上按照字典顺序进行排序，不能与数字的大小顺序混淆。
- 希望选取NULL记录时，需要在条件表达式中使用IS NULL运算符。希望选取不是NULL的记录时，需要在条件表达式中使用IS NOT NULL运算符。
```SQL
-- SQL语句中也可以使用运算表达式
SELECT product_name, sale_price, sale_price * 2 AS "sale_price x2"
  FROM product;

-- WHERE子句的条件表达式中也可以使用计算表达式
SELECT product_name, sale_price, purchase_price
  FROM product
 WHERE sale_price-purchase_price >= 500;
 
/* 对字符串使用不等号
首先创建chars并插入数据
选取出大于‘2’的SELECT语句*/
-- DDL：创建表
CREATE TABLE chars
（chr CHAR（3）NOT NULL，
PRIMARY KEY（chr））;
-- 选取出大于'2'的数据的SELECT语句('2'为字符串)
SELECT chr
  FROM chars
 WHERE chr > '2';
-- 选取NULL的记录
SELECT product_name，purchase_price
  FROM product
 WHERE purchase_price IS NULL;
-- 选取不为NULL的记录
SELECT product_name，purchase_price
  FROM product
 WHERE purchase_price IS NOT NULL;
```


# 逻辑运算符
## NOT运算符
想要表示“不是……”时，除了前文的<>运算符外，还存在另外一个表示否定、使用范围更广的运算符：NOT。
```SQL
-- 选取出销售单价大于等于1000日元的记录
SELECT product_name, product_type, sale_price
  FROM product
 WHERE sale_price >= 1000;
 
 -- 向上述查询条件中添加NOT运算符
SELECT product_name, product_type, sale_price
  FROM product
 WHERE NOT sale_price >= 1000;
```

## AND运算符和OR运算符
AND 相当于“并且”，类似数学中的取交集；
![image](https://user-images.githubusercontent.com/44680953/140971292-652a56ff-8aad-4c4f-875f-6d6f17a6ee06.png)

OR 相当于“或者”，类似数学中的取并集。
![image](https://user-images.githubusercontent.com/44680953/140971324-e57bfd31-3b98-40d1-ba6e-c0cb6d9b2208.png)

## 通过括号优先处理
AND 运算符优先于 OR 运算符，想要优先执行OR运算，可以使用括号：
```SQL
-- 将查询条件原封不动地写入条件表达式，会得到错误结果
SELECT product_name, product_type, regist_date
  FROM product
 WHERE product_type = '办公用品'
   AND regist_date = '2009-09-11'
    OR regist_date = '2009-09-20';

-- 通过使用括号让OR运算符先于AND运算符执行
SELECT product_name, product_type, regist_date
  FROM product
 WHERE product_type = '办公用品'
   AND ( regist_date = '2009-09-11'
        OR regist_date = '2009-09-20');
```

## 真值表
AND 运算符：两侧的真值都为真时返回真，除此之外都返回假。
![image](https://user-images.githubusercontent.com/44680953/140971583-dfdfaa98-62f2-4379-abe4-9193385bc6b1.png)

OR 运算符：两侧的真值只要有一个不为假就返回真，只有当其两侧的真值都为假时才返回假。
![image](https://user-images.githubusercontent.com/44680953/140971630-53b1dd6b-265d-491c-80b7-4a9ce8d2ca73.png)

NOT运算符：只是单纯的将真转换为假，将假转换为真。
![image](https://user-images.githubusercontent.com/44680953/140971670-dc3cfc29-8413-4052-b4d0-ca20d7ee7490.png)

查询条件为P AND（Q OR R）的真值表
![image](https://user-images.githubusercontent.com/44680953/140971731-fdac0252-e94b-4db2-a5c7-764d7e40f5db.png)

## 含有NULL时的真值
NULL的真值结果既不为真，也不为假，因为并不知道这样一个值。
这时真值是除真假之外的第三种值——不确定（UNKNOWN）。
与通常的逻辑运算被称为二值逻辑相对，只有 SQL 中的逻辑运算被称为三值逻辑。

三值逻辑下的AND真值表为：
![image](https://user-images.githubusercontent.com/44680953/140971941-36e8cd0b-9ab0-4d4b-8f94-f623924319d9.png)
**假 > 不确定 > 真**

三值逻辑下的OR真值表为：
![image](https://user-images.githubusercontent.com/44680953/140971982-7840ac6c-e3a3-4e4e-9a27-1fde9f59953f.png)
**真 > 不确定 > 假**

# 对表进行聚合查询
## 聚合函数
## 使用聚合函数删除重复值
## 常用法则

# 对表进行分组
## GROUP BY语句
## 聚合键中包含NULL时
## GROUP BY书写位置
## 在WHERE子句中使用GROUP BY
## 常见错误

# 为聚合结果指定条件
## 用HAVING得到特定分组
## HAVING特点

# 对查询结果进行排序
## ORDER BY
## ORDER BY中列名可使用别名
