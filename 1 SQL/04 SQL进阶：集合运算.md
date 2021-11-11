# 表的加减法
## 什么是集合运算
在标准 SQL 中, 分别对检索结果使用 UNION, INTERSECT, EXCEPT 来将检索结果进行并,交和差运算, 像UNION,INTERSECT, EXCEPT这种用来进行集合运算的运算符称为集合运算符。  

![image](https://user-images.githubusercontent.com/44680953/141325898-c5349f20-957e-46b3-a89a-9ab6a70a62b8.png)  
![image](https://user-images.githubusercontent.com/44680953/141325922-da8c2768-3449-4bb1-b40e-b07cc3b6e598.png)  


## UNION 并运算
### UNION
接下来我们演示UNION的具体用法及查询结果:
```SQL
SELECT product_id, product_name
  FROM product
UNION
SELECT product_id, product_name
  FROM product2;
```
上述结果包含了两张表中的全部商品  
![image](https://user-images.githubusercontent.com/44680953/141326795-490ec166-91ca-4b31-95f1-949e0697867f.png)  
通过观察可以发现,商品编号为“ 0001 ”~“ 0003 ”的 3 条记录在两个表中都存在,因此大家可能会认为结果中会出现重复的记录,但是 **UNION 等集合运算符通常都会除去重复的记录** .

**练习题:** 
假设连锁店想要增加毛利率超过 50%或者售价低于 800 的货物的存货量, 请使用 UNION 对分别满足上述两个条件的商品的查询结果求并集.  
结果应该类似于:  
![image](https://user-images.githubusercontent.com/44680953/141327001-b709b3ae-204c-4819-8f90-d658677d4256.png)  
```SQL
-- 参考答案:
SELECT  product_id,product_name,product_type
       ,sale_price,purchase_price
  FROM product 
 WHERE sale_price<800
  
 UNION
 
SELECT  product_id,product_name,product_type
       ,sale_price,purchase_price
  FROM product 
 WHERE sale_price>1.5*purchase_price;
```
思考: 如果不使用 UNION 该怎么写查询语句?
```SQL
-- 参考答案:
SELECT  product_id,product_name,product_type
       ,sale_price,purchase_price
  FROM product 
 WHERE sale_price < 800 
    OR sale_price > 1.5 * purchase_price;
```


### UNION 与 OR 谓词
确实, 对于同一个表的两个不同的筛选结果集, 使用 UNION 对两个结果集取并集, 和把两个子查询的筛选条件用 OR 谓词连接, 会得到相同的结果, 但倘若要将两个不同的表中的结果合并在一起, 就不得不使用 UNION 了.    

**练习题 :**
分别使用 UNION 或者 OR 谓词,找出毛利率不足 30%或毛利率未知的商品.  
```SQL
-- 使用 OR 谓词
SELECT * 
  FROM product 
 WHERE sale_price / purchase_price < 1.3 
    OR sale_price / purchase_price IS NULL;
    
-- 使用 UNION
SELECT * 
  FROM product 
 WHERE sale_price / purchase_price < 1.3
 
 UNION
SELECT * 
  FROM product 
 WHERE sale_price / purchase_price IS NULL;
```

### 包含重复行的集合运算 UNION ALL
SQL 语句的 UNION 会对两个查询的结果集进行合并和去重, 这种去重不仅会去掉两个结果集相互重复的, 还会去掉一个结果集中的重复行. 但在实践中有时候需要需要不去重的并集, 在 UNION 的结果中保留重复行的语法其实非常简单,只需要在 UNION 后面添加 ALL 关键字就可以了.  

例如, 想要知道 product 和 product2 中所包含的商品种类及每种商品的数量, 第一步,就需要将两个表的商品种类字段选出来, 然后使用 UNION ALL 进行不去重地合并. 接下来再对两个表的结果按 product_type 字段分组计数.  
```SQL
-- 保留重复行
SELECT product_id, product_name
  FROM product
 UNION ALL
SELECT product_id, product_name
  FROM product2;
```
查询结果如下:  
![image](https://user-images.githubusercontent.com/44680953/141327650-cae83346-a9c4-4993-b307-b0074c858ef6.png)


### bag 模型与 set 模型
Bag 是和 set 类似的一种数学结构, 不一样的地方在于: bag 里面允许存在重复元素, 如果同一个元素被加入多次, 则袋子里就有多个该元素.
通过上述 bag 与 set 定义之间的差别我们就发现, 使用 bag 模型来描述数据库中的表在很多时候更加合适.  

是否允许元素重复导致了 set 和 bag 的并交差等运算都存在一些区别. 以 bag 的交为例, 由于 bag 允许元素重复出现, 对于两个 bag, 他们的并运算会按照: **1.该元素是否至少在一个 bag 里出现过, 2.该元素在两个 bag 中的最大出现次数** 这两个方面来进行计算. 因此对于 A = {1,1,1,2,3,5,7}, B = {1,1,2,2,4,6,8} 两个 bag, 它们的并就等于 {1,1,1,2,2,3,4,5,6,7,8}.


### 隐式类型转换



## INTERSECT 交运算
### bag 的交运算

## EXCEPT 差运算
### 差集,补集与表的减法
### EXCEPT 与 NOT 谓词
### EXCEPT ALL 与bag 的差
### INTERSECT 与 AND 谓词

## 对称差
### 借助并集和差集迂回实现交集运算 INTERSECT

# 连结(JOIN)

## 内连结(INNER JOIN)
### 使用内连结从两个表获取信息
### 结合 WHERE 子句使用内连结
### 结合 GROUP BY 子句使用内连结
### 自连结(SELF JOIN)
### 内连结与关联子查询
### 自然连结(NATURAL JOIN)
### 使用连结求交集

## 外连结(OUTER JOIN)
### 左连结与右连接
### 使用左连结从两个表获取信息
### 结合 WHERE 子句使用左连结
### 在 MySQL 中实现全外连结

## 多表连结
### 多表进行内连结
### 多表进行外连结

## 非等值连结(ON)
### 非等值自左连结(SELF JOIN)

## 交叉连结(CROSS JOIN)(笛卡尔积)
### 连结与笛卡儿积的关系

## 连结的特定语法和过时语法


