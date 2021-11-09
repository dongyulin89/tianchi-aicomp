# 视图
## 什么是视图
视图是一个虚拟的表，不同于直接操作数据表，视图是依据SELECT语句来创建的（会在下面具体介绍），所以操作视图时会根据创建视图的SELECT语句生成一张虚拟表，然后在这张虚拟表上做SQL操作。

## 视图与表有什么区别
《sql基础教程第2版》用一句话非常凝练的概括了视图与表的区别 —— “是否保存了实际的数据”
![image](https://user-images.githubusercontent.com/44680953/140976531-35eb373d-79cb-4a8f-aee8-b6431ee609bd.png)

下面这句顺口溜也方便大家记忆视图与表的关系：“视图不是表，视图是虚表，视图依赖于表”。

## 为什么会存在视图
那既然已经有数据表了，为什么还需要视图呢？主要有以下几点原因：
- 通过定义视图可以将频繁使用的SELECT语句保存以提高效率。
- 通过定义视图可以使用户看到的数据更加清晰。
- 通过定义视图可以不对外公开数据表全部字段，增强数据的保密性。
- 通过定义视图可以降低数据的冗余。

## 如何创建视图
创建视图的基本语法如下：
```SQL
CREATE VIEW <视图名称>(<列名1>,<列名2>,...) AS <SELECT语句>
```
![image](https://user-images.githubusercontent.com/44680953/140976819-d2532e07-92ef-4b6f-88b9-eceb1cc0fba4.png)
需要注意的是视图名在数据库中需要是唯一的，不能与其他视图和表重名。

### 定义视图时不能使用 ORDER BY 语句
为什么不能使用 ORDER BY 子句呢？这是因为视图和表一样，数据行都是没有顺序的。下面这样定义视图是错误的。
```SQL
CREATE VIEW productsum (product_type, cnt_product)
AS
SELECT product_type, COUNT(*)
  FROM product
 GROUP BY product_type
 ORDER BY product_type;
 ```
**在 MySQL中视图的定义是允许使用 ORDER BY 语句的，但是若从特定视图进行选择，而该视图使用了自己的 ORDER BY 语句，则视图定义中的 ORDER BY 将被忽略。**

### 基于单表的视图
我们在product表的基础上创建一个视图，如下：
```SQL
CREATE VIEW productsum (product_type, cnt_product)
AS
SELECT product_type, COUNT(*)
  FROM product
 GROUP BY product_type ;
``` 
创建的视图如下图所示：
![image](https://user-images.githubusercontent.com/44680953/140977302-a2ef69f6-b18e-4c32-b965-8f037cfa9944.png)

### 基于多表的视图
为了学习多表视图，我们再创建一张表，相关代码如下：
```SQL
CREATE TABLE shop_product
(shop_id    CHAR(4)       NOT NULL,
 shop_name  VARCHAR(200)  NOT NULL,
 product_id CHAR(4)       NOT NULL,
 quantity   INTEGER       NOT NULL,
 PRIMARY KEY (shop_id, product_id));
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000A',	'东京',		'0001',	30);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000A',	'东京',		'0002',	50);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000A',	'东京',		'0003',	15);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0002',	30);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0003',	120);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0004',	20);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0006',	10);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0007',	40);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000C',	'大阪',		'0003',	20);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000C',	'大阪',		'0004',	50);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000C',	'大阪',		'0006',	90);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000C',	'大阪',		'0007',	70);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000D',	'福冈',		'0001',	100);
```
我们在product表和shop_product表的基础上创建视图。
```SQL
CREATE VIEW view_shop_product(product_type, sale_price, shop_name)
AS
SELECT product_type, sale_price, shop_name
  FROM product,
       shop_product
 WHERE product.product_id = shop_product.product_id;
```
创建的视图如下图所示
![image](https://user-images.githubusercontent.com/44680953/140977434-52129f51-d430-45ae-b922-4dffe34cbb0b.png)


## 如何修改视图结构
修改视图结构的基本语法如下：
```SQL
ALTER VIEW <视图名> AS <SELECT语句>
```
我们修改上方的productSum视图为
```SQL
ALTER VIEW productSum
    AS
        SELECT product_type, sale_price
          FROM Product
         WHERE regist_date > '2009-09-11';
```
此时productSum视图内容如下图所示
![image](https://user-images.githubusercontent.com/44680953/140977998-8fc8fb18-ff76-40e1-8f2e-0aa1d44543ac.png)

## 如何更新视图内容
对于一个视图来说，如果包含以下结构的任意一种都是不可以被更新的：
- 聚合函数 SUM()、MIN()、MAX()、COUNT() 等。
- DISTINCT 关键字。
- GROUP BY 子句。
- HAVING 子句。
- UNION 或 UNION ALL 运算符。
- FROM 子句中包含多个表。

视图归根结底还是从表派生出来的，因此，如果原表可以更新，那么 视图中的数据也可以更新。反之亦然，如果视图发生了改变，而原表没有进行相应更新的话，就无法保证数据的一致性了。

因为我们刚刚修改的productSum视图不包括以上的限制条件，我们来尝试更新一下视图
```SQL
UPDATE productsum
   SET sale_price = '5000'
 WHERE product_type = '办公用品';
```
此时我们再查看productSum视图，可以发现数据已经更新了  
![image](https://user-images.githubusercontent.com/44680953/140978283-895c7c3a-7d89-4461-8c41-0c4ceb93e0c6.png)  
此时观察原表也可以发现数据也被更新了
![image](https://user-images.githubusercontent.com/44680953/140978344-896163b7-df0e-4769-a41b-f41998dbbe46.png)  

不知道大家看到这个结果会不会有疑问，刚才修改视图的时候是设置product_type='办公用品'的商品的sale_price=5000，为什么原表的数据只有一条做了修改呢？
还是因为视图的定义，视图只是原表的一个窗口，所以它修改也只能修改透过窗口能看到的内容。

**注意：这里虽然修改成功了，但是并不推荐这种使用方式。而且我们在创建视图时也尽量使用限制不允许通过视图来修改表**

## 如何删除视图
删除视图的基本语法如下：
```SQL
DROP VIEW <视图名1> [ , <视图名2> …]
```
我们删除刚才创建的productSum视图
```SQL
DROP VIEW productSum;
```

# 子查询
一个子查询的例子
```SQL
SELECT stu_name
FROM (
         SELECT stu_name, COUNT(*) AS stu_cnt
          FROM students_info
          GROUP BY stu_age) AS studentSum;
```

## 什么是子查询
子查询指一个查询语句嵌套在另一个查询语句内部的查询，这个特性从 MySQL 4.1 开始引入，在 SELECT 子句中先计算子查询，子查询结果作为外层另一个查询的过滤条件，查询可以基于一个表或者多个表。

## 子查询和视图的关系
子查询就是将用来定义视图的 SELECT 语句直接用于 FROM 子句当中。其中AS studentSum可以看作是子查询的名称，而且由于子查询是一次性的，所以子查询不会像视图那样保存在存储介质中， 而是在 SELECT 语句执行之后就消失了。

## 嵌套子查询
```SQL
SELECT product_type, cnt_product
FROM (SELECT *
        FROM (SELECT product_type, 
                      COUNT(*) AS cnt_product
                FROM product 
               GROUP BY product_type) AS productsum
        WHERE cnt_product = 4) AS productsum2;
```
其中最内层的子查询我们将其命名为 productSum，这条语句根据product_type分组并查询个数，第二层查询中将个数为4的商品查询出来，最外层查询product_type和cnt_product两列。

**虽然嵌套子查询可以查询出结果，但是随着子查询嵌套的层数的叠加，SQL语句不仅会难以理解而且执行效率也会很差，所以要尽量避免这样的使用。**

## 标量子查询
标量就是单一的意思，那么标量子查询也就是单一的子查询，它要求我们执行的SQL语句只能返回一个值，也就是要返回表中具体的某一行的某一列。

让我们看如何通过标量子查询语句查询出销售单价高于平均销售单价的商品。
```SQL
SELECT product_id, product_name, sale_price
  FROM product
 WHERE sale_price > (SELECT AVG(sale_price) FROM product);
```
上面的这条语句首先后半部分查询出product表中的平均售价，前面的sql语句在根据WHERE条件挑选出合适的商品。

由于标量子查询的特性，导致标量子查询不仅仅局限于 WHERE 子句中，通常任何可以使用单一值的位置都可以使用。也就是说，能够使用常数或者列名的地方，无论是 SELECT 子句、GROUP BY 子句、HAVING 子句，还是 ORDER BY 子句，几乎所有的地方都可以使用。

我们还可以这样使用标量子查询：
```SQL
SELECT product_id,
       product_name,
       sale_price,
       (SELECT AVG(sale_price)
          FROM product) AS avg_price
  FROM product;
```
查询结果如下图所示
![image](https://user-images.githubusercontent.com/44680953/140979808-529ddc23-7290-4393-84ef-e105886b818b.png)

## 关联子查询
关联子查询既然包含关联两个字那么一定意味着查询与子查询之间存在着联系。这种联系是如何建立起来的呢？

我们先看一个例子：
```SQL
SELECT product_type, product_name, sale_price
  FROM product AS p1
 WHERE sale_price > (SELECT AVG(sale_price)
                       FROM product AS p2
                      WHERE p1.product_type = p2.product_type
                      GROUP BY product_type);
```
看一下这个例子的执行结果
![image](https://user-images.githubusercontent.com/44680953/140980091-82072703-d91b-49cc-87a0-f4e1b23613bd.png)

通过上面的例子我们可以发现，关联子查询就是通过一些标志将内外两层的查询连接起来起到过滤数据的目的。

### 关联子查询 与 子查询
子查询：查询出销售单价高于平均销售单价的商品
```SQL
SELECT product_id, product_name, sale_price
  FROM product
 WHERE sale_price > (SELECT AVG(sale_price) FROM product);
```
关联子查询：选取出各商品种类中高于该商品种类的平均销售单价的商品
```SQL
SELECT product_type, product_name, sale_price
  FROM product ASp1
 WHERE sale_price > (SELECT AVG(sale_price)
                       FROM product ASp2
                      WHERE p1.product_type =p2.product_type
                   GROUP BY product_type);
```

第二条SQL语句也就是关联子查询中我们将外面的product表标记为p1，将内部的product设置为p2，而且通过WHERE语句连接了两个查询。

关联子查询的执行过程
- 首先执行不带WHERE的主查询
- 根据主查询讯结果匹配product_type，获取子查询结果
- 将子查询结果再与主查询结合执行完整的SQL语句

**在子查询中像标量子查询，嵌套子查询或者关联子查询可以看作是子查询的一种操作方式即可。**

# 函数
所谓函数，类似一个黑盒子，你给它一个输入值，它便按照预设的程序定义给出返回值，输入值称为参数。

函数大致分为如下几类：
- 算术函数 （用来进行数值计算的函数）
- 字符串函数 （用来进行字符串操作的函数）
- 日期函数 （用来进行日期操作的函数）
- 转换函数 （用来转换数据类型和值的函数）
- 聚合函数 （用来进行数据聚合的函数）

## 算数函数
## 字符串函数
## 日期函数
## 转换函数

# 谓词
## 什么是谓词
## LIKE 谓词
用于字符串的部分一致查询
## BETWEEN 谓词
用于范围查询
## IS NULL、 IS NOT NULL 谓词
用于判断是否为NULL
## IN 谓词
OR的简便用法
### 使用子查询作为IN谓词的参数
## EXIST 谓词

# CASE 表达式
## 什么是 CASE 表达式？
## CASE表达式的使用方法
