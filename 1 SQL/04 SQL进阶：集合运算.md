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
通过观察可以发现,商品编号为“ 0001 ”~“ 0003 ”的 3 条记录在两个表中都存在,因此大家可能会认为结果中会出现重复的记录,但是 UNION 等集合运算符通常都会除去重复的记录.




### UNION 与 OR 谓词
### 包含重复行的集合运算 UNION ALL
### bag 模型与 set 模型
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


