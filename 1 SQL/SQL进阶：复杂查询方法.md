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
创建的视图如下图所示：  
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
此时productSum视图内容如下图所示：  
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
为了演示其他的几个算数函数，在此构造samplemath表
```SQL
-- DDL ：创建表
USE shop;
DROP TABLE IF EXISTS samplemath;
CREATE TABLE samplemath
(m float(10,3),
n INT,
p INT);

-- DML ：插入数据
START TRANSACTION; -- 开始事务
INSERT INTO samplemath(m, n, p) VALUES (500, 0, NULL);
INSERT INTO samplemath(m, n, p) VALUES (-180, 0, NULL);
INSERT INTO samplemath(m, n, p) VALUES (NULL, NULL, NULL);
INSERT INTO samplemath(m, n, p) VALUES (NULL, 7, 3);
INSERT INTO samplemath(m, n, p) VALUES (NULL, 5, 2);
INSERT INTO samplemath(m, n, p) VALUES (NULL, 4, NULL);
INSERT INTO samplemath(m, n, p) VALUES (8, NULL, 3);
INSERT INTO samplemath(m, n, p) VALUES (2.27, 1, NULL);
INSERT INTO samplemath(m, n, p) VALUES (5.555,2, NULL);
INSERT INTO samplemath(m, n, p) VALUES (NULL, 1, NULL);
INSERT INTO samplemath(m, n, p) VALUES (8.76, NULL, NULL);
COMMIT; -- 提交事务
-- 查询表内容
SELECT * FROM samplemath;
+----------+------+------+
| m        | n    | p    |
+----------+------+------+
|  500.000 |    0 | NULL |
| -180.000 |    0 | NULL |
|     NULL | NULL | NULL |
|     NULL |    7 |    3 |
|     NULL |    5 |    2 |
|     NULL |    4 | NULL |
|    8.000 | NULL |    3 |
|    2.270 |    1 | NULL |
|    5.555 |    2 | NULL |
|     NULL |    1 | NULL |
|    8.760 | NULL | NULL |
+----------+------+------+
11 rows in set (0.00 sec)
```

- ABS: 绝对值
语法：`ABS( 数值 )`
ABS 函数用于计算一个数字的绝对值，表示一个数到原点的距离。
当 ABS 函数的参数为NULL时，返回值也是NULL。

- MOD: 求余数
语法：`MOD( 被除数，除数 )`
MOD 是计算除法余数（求余）的函数，是 modulo 的缩写。小数没有余数的概念，只能对整数列求余数。
注意：主流的 DBMS 都支持 MOD 函数，只有SQL Server 不支持该函数，其使用%符号来计算余数。

- ROUND: 四舍五入
语法：`ROUND( 对象数值，保留小数的位数 )`
ROUND 函数用来进行四舍五入操作。
注意：当参数 保留小数的位数 为变量时，可能会遇到错误，请谨慎使用变量。

```SQL
SELECT m,
ABS(m)ASabs_col ,
n, p,
MOD(n, p) AS mod_col,
ROUND(m,1)ASround_colS
FROM samplemath;
+----------+---------+------+------+---------+-----------+
| m        | abs_col | n    | p    | mod_col | round_col |
+----------+---------+------+------+---------+-----------+
|  500.000 | 500.000 |    0 | NULL |    NULL |     500.0 |
| -180.000 | 180.000 |    0 | NULL |    NULL |    -180.0 |
|     NULL |    NULL | NULL | NULL |    NULL |      NULL |
|     NULL |    NULL |    7 |    3 |       1 |      NULL |
|     NULL |    NULL |    5 |    2 |       1 |      NULL |
|     NULL |    NULL |    4 | NULL |    NULL |      NULL |
|    8.000 |   8.000 | NULL |    3 |    NULL |       8.0 |
|    2.270 |   2.270 |    1 | NULL |    NULL |       2.3 |
|    5.555 |   5.555 |    2 | NULL |    NULL |       5.6 |
|     NULL |    NULL |    1 | NULL |    NULL |      NULL |
|    8.760 |   8.760 | NULL | NULL |    NULL |       8.8 |
+----------+---------+------+------+---------+-----------+
11 rows in set (0.08 sec)
```

## 字符串函数
字符串函数也经常被使用，为了学习字符串函数，在此我们构造samplestr表。
```SQL
-- DDL ：创建表
USE  shop;
DROP TABLE IF EXISTS samplestr;
CREATE TABLE samplestr
(str1 VARCHAR (40),
str2 VARCHAR (40),
str3 VARCHAR (40)
);
-- DML：插入数据
START TRANSACTION;
INSERT INTO samplestr (str1, str2, str3) VALUES ('opx',	'rt', NULL);
INSERT INTO samplestr (str1, str2, str3) VALUES ('abc', 'def', NULL);
INSERT INTO samplestr (str1, str2, str3) VALUES ('太阳',	'月亮', '火星');
INSERT INTO samplestr (str1, str2, str3) VALUES ('aaa',	NULL, NULL);
INSERT INTO samplestr (str1, str2, str3) VALUES (NULL, 'xyz', NULL);
INSERT INTO samplestr (str1, str2, str3) VALUES ('@!#$%', NULL, NULL);
INSERT INTO samplestr (str1, str2, str3) VALUES ('ABC', NULL, NULL);
INSERT INTO samplestr (str1, str2, str3) VALUES ('aBC', NULL, NULL);
INSERT INTO samplestr (str1, str2, str3) VALUES ('abc哈哈',  'abc', 'ABC');
INSERT INTO samplestr (str1, str2, str3) VALUES ('abcdefabc', 'abc', 'ABC');
INSERT INTO samplestr (str1, str2, str3) VALUES ('micmic', 'i', 'I');
COMMIT;
-- 确认表中的内容
SELECT * FROM samplestr;
+-----------+------+------+
| str1      | str2 | str3 |
+-----------+------+------+
| opx       | rt   | NULL |
| abc       | def  | NULL |
| 太阳      | 月亮 | 火星 |
| aaa       | NULL | NULL |
| NULL      | xyz  | NULL |
| @!#$%     | NULL | NULL |
| ABC       | NULL | NULL |
| aBC       | NULL | NULL |
| abc哈哈   | abc  | ABC  |
| abcdefabc | abc  | ABC  |
| micmic    | i    | I    |
+-----------+------+------+
11 rows in set (0.00 sec)
```

- CONCAT: 拼接
语法：`CONCAT(str1, str2, str3)`
MySQL中使用 CONCAT 函数进行拼接。

- LENGTH: 字符串长度
语法：`LENGTH( 字符串 )`

- LOWER: 小写转换
LOWER 函数只能针对英文字母使用，它会将参数中的字符串全都转换为小写。该函数不适用于英文字母以外的场合，不影响原本就是小写的字符。
类似的， UPPER 函数用于大写转换。

- REPLACE: 字符串的替换
语法：`REPLACE( 对象字符串，替换前的字符串，替换后的字符串 )`

- SUBSTRING: 字符串的截取
语法：`SUBSTRING （对象字符串 FROM 截取的起始位置 FOR 截取的字符数）`
使用 SUBSTRING 函数 可以截取出字符串中的一部分字符串。截取的起始位置从字符串最左侧开始计算，索引值起始为1。

- SUBSTRING_INDEX: 字符串按索引截取
语法：`SUBSTRING_INDEX (原始字符串， 分隔符，n)`
该函数用来获取原始字符串按照分隔符分割后，第 n 个分隔符之前（或之后）的子字符串，支持正向和反向索引，索引起始值分别为 1 和 -1。

```SQL
SELECT SUBSTRING_INDEX('www.mysql.com', '.', 2);
+------------------------------------------+
| SUBSTRING_INDEX('www.mysql.com', '.', 2) |
+------------------------------------------+
| www.mysql                                |
+------------------------------------------+
1 row in set (0.00 sec)
SELECT SUBSTRING_INDEX('www.mysql.com', '.', -2);
+-------------------------------------------+
| SUBSTRING_INDEX('www.mysql.com', '.', -2) |
+-------------------------------------------+
| mysql.com                                 |
+-------------------------------------------+
1 row in set (0.00 sec)

- 获取第1个元素比较容易，获取第2个元素/第n个元素可以采用二次拆分的写法。
SELECT SUBSTRING_INDEX('www.mysql.com', '.', 1);
+------------------------------------------+
| SUBSTRING_INDEX('www.mysql.com', '.', 1) |
+------------------------------------------+
| www                                      |
+------------------------------------------+
1 row in set (0.00 sec)
SELECT SUBSTRING_INDEX(SUBSTRING_INDEX('www.mysql.com', '.', 2), '.', -1);
+--------------------------------------------------------------------+
| SUBSTRING_INDEX(SUBSTRING_INDEX('www.mysql.com', '.', 2), '.', -1) |
+--------------------------------------------------------------------+
| mysql                                                              |
+--------------------------------------------------------------------+
1 row in set (0.00 sec)
```

## 日期函数
CURRENT_DATE – 获取当前日期
```SQL
SELECT CURRENT_DATE;
+--------------+
| CURRENT_DATE |
+--------------+
| 2020-08-08   |
+--------------+
1 row in set (0.00 sec)
```

CURRENT_TIME – 当前时间
```SQL
SELECT CURRENT_TIME;
+--------------+
| CURRENT_TIME |
+--------------+
| 17:26:09     |
+--------------+
1 row in set (0.00 sec)
```

CURRENT_TIMESTAMP – 当前日期和时间
```SQL
SELECT CURRENT_TIMESTAMP;
+---------------------+
| CURRENT_TIMESTAMP   |
+---------------------+
| 2020-08-08 17:27:07 |
+---------------------+
1 row in set (0.00 sec)
```

EXTRACT – 截取日期元素
语法：`EXTRACT(日期元素 FROM 日期)`
使用 EXTRACT 函数可以截取出日期数据中的一部分，例如“年”，“月”，或者“小时”“秒”等。该函数的返回值并不是日期类型而是数值类型
```SQL
SELECT CURRENT_TIMESTAMP as now,
EXTRACT(YEAR   FROM CURRENT_TIMESTAMP) AS year,
EXTRACT(MONTH  FROM CURRENT_TIMESTAMP) AS month,
EXTRACT(DAY    FROM CURRENT_TIMESTAMP) AS day,
EXTRACT(HOUR   FROM CURRENT_TIMESTAMP) AS hour,
EXTRACT(MINUTE FROM CURRENT_TIMESTAMP) AS MINute,
EXTRACT(SECOND FROM CURRENT_TIMESTAMP) AS second;
+---------------------+------+-------+------+------+--------+--------+
| now                 | year | month | day  | hour | MINute | second |
+---------------------+------+-------+------+------+--------+--------+
| 2020-08-08 17:34:38 | 2020 |     8 |    8 |   17 |     34 |     38 |
+---------------------+------+-------+------+------+--------+--------+
1 row in set (0.00 sec)
```

## 转换函数
“转换”这个词的含义非常广泛，在 SQL 中主要有两层意思：一是数据类型的转换，简称为类型转换，在英语中称为cast；另一层意思是值的转换。

- CAST: 类型转换
语法：`CAST（转换前的值 AS 想要转换的数据类型）`
```SQL
-- 将字符串类型转换为数值类型
SELECT CAST('0001' AS SIGNED INTEGER) AS int_col;
+---------+
| int_col |
+---------+
|       1 |
+---------+
1 row in set (0.00 sec)

-- 将字符串类型转换为日期类型
SELECT CAST('2009-12-14' AS DATE) AS date_col;
+------------+
| date_col   |
+------------+
| 2009-12-14 |
+------------+
1 row in set (0.00 sec)
```

- COALESCE: 将NULL转换为其他值
语法：`COALESCE(数据1，数据2，数据3……)`
COALESCE 是 SQL 特有的函数。该函数会返回可变参数 A 中左侧开始第 1个不是NULL的值。参数个数是可变的，因此可以根据需要无限增加。
在 SQL 语句中将 NULL 转换为其他值时就会用到转换函数。
```SQL
SELECT COALESCE(NULL, 11) AS col_1,
COALESCE(NULL, 'hello world', NULL) AS col_2,
COALESCE(NULL, NULL, '2020-11-01') AS col_3;
+-------+-------------+------------+
| col_1 | col_2       | col_3      |
+-------+-------------+------------+
|    11 | hello world | 2020-11-01 |
+-------+-------------+------------+
1 row in set (0.00 sec)
```

# 谓词
## 什么是谓词
谓词就是返回值为真值的函数。包括`TRUE / FALSE / UNKNOWN`。
谓词主要有以下几个：
- LIKE
- BETWEEN
- IS NULL、IS NOT NULL
- IN
- EXISTS

## LIKE 谓词
用于字符串的部分一致查询（前方一致/中间一致/后方一致）

首先我们来创建一张表
```SQL
-- DDL ：创建表
CREATE TABLE samplelike
( strcol VARCHAR(6) NOT NULL,
PRIMARY KEY (strcol)
samplelike);
-- DML ：插入数据
START TRANSACTION; -- 开始事务
INSERT INTO samplelike (strcol) VALUES ('abcddd');
INSERT INTO samplelike (strcol) VALUES ('dddabc');
INSERT INTO samplelike (strcol) VALUES ('abdddc');
INSERT INTO samplelike (strcol) VALUES ('abcdd');
INSERT INTO samplelike (strcol) VALUES ('ddabc');
INSERT INTO samplelike (strcol) VALUES ('abddc');
COMMIT; -- 提交事务
SELECT * FROM samplelike;
+--------+
| strcol |
+--------+
| abcdd  |
| abcddd |
| abddc  |
| abdddc |
| ddabc  |
| dddabc |
+--------+
6 rows in set (0.00 sec)
```

- 前方一致：选取出“dddabc”
前方一致即作为查询条件的字符串（这里是“ddd”）与查询对象字符串起始部分相同。
```SQL
SELECT *
FROM samplelike
WHERE strcol LIKE 'ddd%';
+--------+
| strcol |
+--------+
| dddabc |
+--------+
1 row in set (0.00 sec)
```
其中的`%`是代表“零个或多个任意字符串”的特殊符号，本例中代表“以ddd开头的所有字符串”。



- 中间一致：选取出“abcddd”,“dddabc”,“abdddc”
中间一致即查询对象字符串中含有作为查询条件的字符串，无论该字符串出现在对象字符串的最后还是中间都没有关系。
```SQL
SELECT *
FROM samplelike
WHERE strcol LIKE '%ddd%';
+--------+
| strcol |
+--------+
| abcddd |
| abdddc |
| dddabc |
+--------+
3 rows in set (0.00 sec)
```

- 后方一致：选取出“abcddd“
后方一致即作为查询条件的字符串（这里是“ddd”）与查询对象字符串的末尾部分相同。
```SQL
SELECT *
FROM samplelike
WHERE strcol LIKE '%ddd';
+--------+
| strcol |
+--------+
| abcddd |
+--------+
1 row in set (0.00 sec)
```

- `_`下划线匹配任意 1 个字符
使用 `_`（下划线）来代替 `%`，与 `%` 不同的是，它代表了“任意 1 个字符”。
```SQL
SELECT *
FROM samplelike
WHERE strcol LIKE 'abc__';
+--------+
| strcol |
+--------+
| abcdd  |
+--------+
1 row in set (0.00 sec)
```

## BETWEEN 谓词
用于范围查询，需要使用三个参数
```SQL
-- 选取销售单价为100～ 1000元的商品
SELECT product_name, sale_price
FROM product
WHERE sale_price BETWEEN 100 AND 1000;
+--------------+------------+
| product_name | sale_price |
+--------------+------------+
| T恤          |       1000 |
| 打孔器       |        500 |
| 叉子         |        500 |
| 擦菜板       |        880 |
| 圆珠笔       |        100 |
+--------------+------------+
5 rows in set (0.00 sec)
```

BETWEEN 的特点就是结果中会包含 100 和 1000 这两个临界值，也就是`闭区间`。如果不想让结果中包含临界值，那就必须使用 < 和 >。
```SQL
SELECT product_name, sale_price
FROM product
WHERE sale_price > 100
AND sale_price < 1000;
+--------------+------------+
| product_name | sale_price |
+--------------+------------+
| 打孔器       |        500 |
| 叉子         |        500 |
| 擦菜板       |        880 |
+--------------+------------+
3 rows in set (0.00 sec)
```

## IS NULL、 IS NOT NULL 谓词
为了选取出某些值为 NULL 的列的数据，不能使用 =，而只能使用特定的谓词IS NULL。
```SQL
SELECT product_name, purchase_price
FROM product
WHERE purchase_price IS NULL;
+--------------+----------------+
| product_name | purchase_price |
+--------------+----------------+
| 叉子         |           NULL |
| 圆珠笔       |           NULL |
+--------------+----------------+
2 rows in set (0.00 sec)
```

与此相反，想要选取 NULL 以外的数据时，需要使用IS NOT NULL。
```SQL
SELECT product_name, purchase_price
FROM product
WHERE purchase_price IS NOT NULL;
+--------------+----------------+
| product_name | purchase_price |
+--------------+----------------+
| T恤          |            500 |
| 打孔器       |            320 |
| 运动T恤      |           2800 |
| 菜刀         |           2800 |
| 高压锅       |           5000 |
| 擦菜板       |            790 |
+--------------+----------------+
6 rows in set (0.00 sec)
```

## IN 谓词（OR的简便用法）
多个查询条件取并集时可以选择使用or语句。
```SQL
-- 通过OR指定多个进货单价进行查询
SELECT product_name, purchase_price
FROM product
WHERE purchase_price = 320
OR purchase_price = 500
OR purchase_price = 5000;
+--------------+----------------+
| product_name | purchase_price |
+--------------+----------------+
| T恤          |            500 |
| 打孔器       |            320 |
| 高压锅       |           5000 |
+--------------+----------------+
3 rows in set (0.00 sec)
```
虽然上述方法没有问题，但还是存在一点不足之处，那就是随着希望选取的对象越来越多， SQL 语句也会越来越长，阅读起来也会越来越困难。这时， 我们就可以使用IN 谓词 `IN(值1, 值2, 值3, …)`来替换上述 SQL 语句。
```SQL
SELECT product_name, purchase_price
FROM product
WHERE purchase_price IN (320, 500, 5000);
+--------------+----------------+
| product_name | purchase_price |
+--------------+----------------+
| T恤          |            500 |
| 打孔器       |            320 |
| 高压锅       |           5000 |
+--------------+----------------+
3 rows in set (0.00 sec)
```

反之，希望选取出“进货单价不是 320 元、 500 元、 5000 元”的商品时，可以使用否定形式NOT IN来实现。
```SQL
SELECT product_name, purchase_price
FROM product
WHERE purchase_price NOT IN (320, 500, 5000);
+--------------+----------------+
| product_name | purchase_price |
+--------------+----------------+
| 运动T恤      |           2800 |
| 菜刀         |           2800 |
| 擦菜板       |            790 |
+--------------+----------------+
3 rows in set (0.00 sec)
```

注意：使用IN 和 NOT IN 时是无法选取出NULL数据的。

### IN和子查询
IN 谓词（NOT IN 谓词）具有其他谓词所没有的用法，那就是可以使用子查询作为其参数。子查询就是 SQL内部生成的表，因此也可以说“能够将表作为 IN 的参数”。同理，我们还可以说“能够将视图作为 IN 的参数”。

在此，我们创建一张新表shopproduct显示出哪些商店销售哪些商品。
```SQL
-- DDL ：创建表
DROP TABLE IF EXISTS shopproduct;
CREATE TABLE shopproduct
(  shop_id CHAR(4)     NOT NULL,
 shop_name VARCHAR(200) NOT NULL,
product_id CHAR(4)      NOT NULL,
  quantity INTEGER      NOT NULL,
PRIMARY KEY (shop_id, product_id) -- 指定主键
);
-- DML ：插入数据
START TRANSACTION; -- 开始事务
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000A', '东京', '0001', 30);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000A', '东京', '0002', 50);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000A', '东京', '0003', 15);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0002', 30);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0003', 120);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0004', 20);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0006', 10);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0007', 40);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000C', '大阪', '0003', 20);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000C', '大阪', '0004', 50);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000C', '大阪', '0006', 90);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000C', '大阪', '0007', 70);
INSERT INTO shopproduct (shop_id, shop_name, product_id, quantity) VALUES ('000D', '福冈', '0001', 100);
COMMIT; -- 提交事务
SELECT * FROM shopproduct;
+---------+-----------+------------+----------+
| shop_id | shop_name | product_id | quantity |
+---------+-----------+------------+----------+
| 000A    | 东京      | 0001       |       30 |
| 000A    | 东京      | 0002       |       50 |
| 000A    | 东京      | 0003       |       15 |
| 000B    | 名古屋      | 0002       |       30 |
| 000B    | 名古屋      | 0003       |      120 |
| 000B    | 名古屋      | 0004       |       20 |
| 000B    | 名古屋      | 0006       |       10 |
| 000B    | 名古屋      | 0007       |       40 |
| 000C    | 大阪      | 0003       |       20 |
| 000C    | 大阪      | 0004       |       50 |
| 000C    | 大阪      | 0006       |       90 |
| 000C    | 大阪      | 0007       |       70 |
| 000D    | 福冈      | 0001       |      100 |
+---------+-----------+------------+----------+
13 rows in set (0.00 sec)
```
由于单独使用商店编号（shop_id）或者商品编号（product_id）不能区分表中每一行数据，因此指定了 2 列作为主键（primary key）对商店和商品进行组合，用来唯一确定每一行数据。

假设我么需要取出大阪在售商品的销售单价，该如何实现呢？
第一步，取出大阪门店的在售商品 `product_id` ;
第二步，取出大阪门店在售商品的销售单价 `sale_price`
```SQL
-- step1：取出大阪门店的在售商品 `product_id`
SELECT product_id
FROM shopproduct
WHERE shop_id = '000C';
+------------+
| product_id |
+------------+
| 0003       |
| 0004       |
| 0006       |
| 0007       |
+------------+
4 rows in set (0.00 sec)

-- step2：取出大阪门店在售商品的销售单价 `sale_price`
SELECT product_name, sale_price
FROM product
WHERE product_id IN (SELECT product_id
                        FROM shopproduct
                        WHERE shop_id = '000C');
+--------------+------------+
| product_name | sale_price |
+--------------+------------+
| 运动T恤      |       4000 |
| 菜刀         |       3000 |
| 叉子         |        500 |
| 擦菜板       |        880 |
+--------------+------------+
4 rows in set (0.00 sec)
```

子查询是从最内层开始执行的（由内而外），因此，上述语句的子查询执行之后，sql 展开成下面的语句
```SQL
-- 子查询展开后的结果
SELECT product_name, sale_price
  FROM product
  WHERE product_id IN ('0003', '0004', '0006', '0007');
+--------------+------------+
| product_name | sale_price |
+--------------+------------+
| 运动T恤      |       4000 |
| 菜刀         |       3000 |
| 叉子         |        500 |
| 擦菜板       |        880 |
+--------------+------------+
4 rows in set (0.00 sec)
```
可以看到，子查询转换之后变为 in 谓词用法，你理解了吗？
或者，你会疑惑既然 in 谓词也能实现，那为什么还要使用子查询呢？这里给出两点原因：
①：实际生活中，某个门店的在售商品是不断变化的，使用 in 谓词就需要经常更新 sql 语句，降低了效率，提高了维护成本；
②：实际上，某个门店的在售商品可能有成百上千个，手工维护在售商品编号真是个大工程。

使用子查询即可保持 sql 语句不变，极大提高了程序的可维护性，这是系统开发中需要重点考虑的内容。

### NOT IN和子查询
NOT IN 同样支持子查询作为参数，用法和 in 完全一样。
```SQL
-- NOT IN 使用子查询作为参数，取出未在大阪门店销售的商品的销售单价
SELECT product_name, sale_price
  FROM product
 WHERE product_id NOT IN (SELECT product_id
                            FROM shopproduct
                           WHERE shop_id = '000A');
+--------------+------------+
| product_name | sale_price |
+--------------+------------+
| 菜刀         |       3000 |
| 高压锅       |       6800 |
| 叉子         |        500 |
| 擦菜板       |        880 |
| 圆珠笔       |        100 |
+--------------+------------+
5 rows in set (0.00 sec)
```


## EXIST 谓词
### EXIST谓词的使用方法
谓词的作用就是 “判断是否存在满足某种条件的记录”。
如果存在这样的记录就返回真（TRUE），如果不存在就返回假（FALSE）。
EXIST（存在）谓词的主语是“记录”。

我们继续以 IN和子查询 中的示例，使用 EXIST 选取出大阪门店在售商品的销售单价。
```SQL
SELECT product_name, sale_price
  FROM product AS p
 WHERE EXISTS (SELECT *
                 FROM shopproduct AS sp
                WHERE sp.shop_id = '000C'
                  AND sp.product_id = p.product_id);
+--------------+------------+
| product_name | sale_price |
+--------------+------------+
| 运动T恤      |       4000 |
| 菜刀         |       3000 |
| 叉子         |        500 |
| 擦菜板       |        880 |
+--------------+------------+
4 rows in set (0.00 sec)
```

### EXIST的参数
之前我们学过的谓词，基本上都是像“列 LIKE 字符串”或者“ 列 BETWEEN 值 1 AND 值 2”这样需要指定 2 个以上的参数，而 EXIST 的左侧并没有任何参数。因为 EXIST 是只有 1 个参数的谓词。 所以，EXIST 只需要在右侧书写 1 个参数，该参数通常都会是一个子查询。
```SQL
EXIST (SELECT *
          FROM shopproduct AS sp
          WHERE sp.shop_id = '000C'
          AND sp.product_id = p.product_id)  
```
上面这样的子查询就是唯一的参数。确切地说，由于通过条件“SP.product_id = P.product_id”将 product 表和 shopproduct表进行了联接，因此作为参数的是关联子查询。 EXIST 通常会使用关联子查询作为参数。

### 子查询中的SELECT *
由于 EXIST 只关心记录是否存在，因此返回哪些列都没有关系。 EXIST 只会判断是否存在满足子查询中 WHERE 子句指定的条件“商店编号（shop_id）为 '000C'，商品（product）表和商店商品（shopproduct）表中商品编号（product_id）相同”的记录，只有存在这样的记录时才返回真（TRUE）。因此，使用下面的查询语句，查询结果也不会发生变化。
```SQL
SELECT product_name, sale_price
  FROM product AS p
 WHERE EXISTS (SELECT 1 -- 这里可以书写适当的常数
                 FROM shopproduct AS sp
                WHERE sp.shop_id = '000C'
                  AND sp.product_id = p.product_id);
+--------------+------------+
| product_name | sale_price |
+--------------+------------+
| 运动T恤      |       4000 |
| 菜刀         |       3000 |
| 叉子         |        500 |
| 擦菜板       |        880 |
+--------------+------------+
4 rows in set (0.00 sec)
```
大家可以把在 EXIST 的子查询中书写 SELECT * 当作 SQL 的一种习惯。

### 使用NOT EXIST替换NOT IN
就像 EXIST 可以用来替换 IN 一样， NOT IN 也可以用NOT EXIST来替换。

下面的代码示例取出，不在大阪门店销售的商品的销售单价。
```SQL
SELECT product_name, sale_price
  FROM product AS p
 WHERE NOT EXISTS (SELECT *
                     FROM shopproduct AS sp
                    WHERE sp.shop_id = '000A'
                      AND sp.product_id = p.product_id);
+--------------+------------+
| product_name | sale_price |
+--------------+------------+
| 菜刀         |       3000 |
| 高压锅       |       6800 |
| 叉子         |        500 |
| 擦菜板       |        880 |
| 圆珠笔       |        100 |
+--------------+------------+
5 rows in set (0.00 sec)
```

NOT EXIST 与 EXIST 相反，当“不存在”满足子查询中指定条件的记录时返回真（TRUE）。


# CASE 表达式
## 什么是 CASE 表达式？
CASE 表达式是函数的一种。是 SQL 中数一数二的重要功能，有必要好好学习一下。
CASE 表达式是在区分情况时使用的，这种情况的区分在编程中通常称为`（条件）分支`。
CASE表达式的语法分为`简单CASE表达式`和`搜索CASE表达式`两种。由于搜索CASE表达式包含简单CASE表达式的全部功能。本课程将重点介绍搜索CASE表达式。

语法：
```SQL
CASE WHEN <求值表达式> THEN <表达式>
     WHEN <求值表达式> THEN <表达式>
     WHEN <求值表达式> THEN <表达式>
     .
     .
     .
ELSE <表达式>
END  
```
上述语句执行时，依次判断 when 表达式是否为真值，是则执行 THEN 后的语句，如果所有的 when 表达式均为假，则执行 ELSE 后的语句。
无论多么庞大的 CASE 表达式，最后也只会返回一个值。


## CASE表达式的使用方法
假设现在 要实现如下结果：
```SQL
A ：衣服
B ：办公用品
C ：厨房用具  
```
因为表中的记录并不包含“A ： ”或者“B ： ”这样的字符串，所以需要在 SQL 中进行添加。并将“A ： ”“B ： ”“C ： ”与记录结合起来。

- 应用场景1：根据不同分支得到不同列值
```SQL
SELECT  product_name,
        CASE WHEN product_type = '衣服' THEN CONCAT('A ： ',product_type)
             WHEN product_type = '办公用品'  THEN CONCAT('B ： ',product_type)
             WHEN product_type = '厨房用具'  THEN CONCAT('C ： ',product_type)
             ELSE NULL
        END AS abc_product_type
  FROM  product;
+--------------+------------------+
| product_name | abc_product_type |
+--------------+------------------+
| T恤          | A ： 衣服        |
| 打孔器       | B ： 办公用品    |
| 运动T恤      | A ： 衣服        |
| 菜刀         | C ： 厨房用具    |
| 高压锅       | C ： 厨房用具    |
| 叉子         | C ： 厨房用具    |
| 擦菜板       | C ： 厨房用具    |
| 圆珠笔       | B ： 办公用品    |
+--------------+------------------+
8 rows in set (0.00 sec)
```
ELSE 子句也可以省略不写，这时会被默认为 ELSE NULL。但为了防止有人漏读，还是希望大家能够显示地写出 ELSE 子句。
此外， CASE 表达式最后的“END”是不能省略的，请大家特别注意不要遗漏。忘记书写 END 会发生语法错误，这也是初学时最容易犯的错误。

- 应用场景2：实现列方向上的聚合
通常我们使用如下代码实现行的方向上不同种类的聚合（这里是 sum）
```SQL
SELECT product_type,
       SUM(sale_price) AS sum_price
  FROM product
 GROUP BY product_type;  
+--------------+-----------+
| product_type | sum_price |
+--------------+-----------+
| 衣服         |      5000 |
| 办公用品      |       600 |
| 厨房用具      |     11180 |
+--------------+-----------+
3 rows in set (0.00 sec)
```
假如要在列的方向上展示不同种类额聚合值，该如何写呢？
```SQL
sum_price_clothes | sum_price_kitchen | sum_price_office
------------------+-------------------+-----------------
             5000 |             11180 |              600  
```
聚合函数 + CASE WHEN 表达式即可实现该效果
```SQL
-- 对按照商品种类计算出的销售单价合计值进行行列转换
SELECT SUM(CASE WHEN product_type = '衣服' THEN sale_price ELSE 0 END) AS sum_price_clothes,
       SUM(CASE WHEN product_type = '厨房用具' THEN sale_price ELSE 0 END) AS sum_price_kitchen,
       SUM(CASE WHEN product_type = '办公用品' THEN sale_price ELSE 0 END) AS sum_price_office
  FROM product;
+-------------------+-------------------+------------------+
| sum_price_clothes | sum_price_kitchen | sum_price_office |
+-------------------+-------------------+------------------+
|              5000 |             11180 |              600 |
+-------------------+-------------------+------------------+
1 row in set (0.00 sec)
```

- 应用场景3：实现行转列
假设有如下图表的结构  
![image](https://user-images.githubusercontent.com/44680953/141161294-5292551f-e3fd-4b1b-87a7-a4392f5b0c06.png)  
计划得到如下的图表结构  
![image](https://user-images.githubusercontent.com/44680953/141161358-b380a10e-10f1-4590-b6bd-a321545bc0c7.png)  
聚合函数 + CASE WHEN 表达式即可实现该转换  
```SQL
-- CASE WHEN 实现数字列 score 行转列
SELECT name,
       SUM(CASE WHEN subject = '语文' THEN score ELSE null END) as chinese,
       SUM(CASE WHEN subject = '数学' THEN score ELSE null END) as math,
       SUM(CASE WHEN subject = '外语' THEN score ELSE null END) as english
  FROM score
 GROUP BY name;
+------+---------+------+---------+
| name | chinese | math | english |
+------+---------+------+---------+
| 张三 |      93 |   88 |      91 |
| 李四 |      87 |   90 |      77 |
+------+---------+------+---------+
2 rows in set (0.00 sec)
```
上述代码实现了数字列 score 的行转列，也可以实现文本列 subject 的行转列
```SQL
-- CASE WHEN 实现文本列 subject 行转列
SELECT name,
       MAX(CASE WHEN subject = '语文' THEN subject ELSE null END) as chinese,
       MAX(CASE WHEN subject = '数学' THEN subject ELSE null END) as math,
       MIN(CASE WHEN subject = '外语' THEN subject ELSE null END) as english
  FROM score
 GROUP BY name;
+------+---------+------+---------+
| name | chinese | math | english |
+------+---------+------+---------+
| 张三 | 语文    | 数学 | 外语    |
| 李四 | 语文    | 数学 | 外语    |
+------+---------+------+---------+
2 rows in set (0.00 sec
```

总结：
- 当待转换列为数字时，可以使用SUM AVG MAX MIN等聚合函数；
- 当待转换列为文本时，可以使用MAX MIN等聚合函数
