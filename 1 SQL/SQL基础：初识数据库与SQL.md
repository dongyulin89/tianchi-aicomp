# 初识数据库
数据库是将大量数据保存起来，通过计算机加工而成的可以 进行高效访问的数据集合。该数据集合称为`数据库（Database，DB）`。用来管理数据库的计算机系统称为`数据库管理系统（Database Management System，DBMS）`。

## DBMS的种类
DBMS 主要通过数据的保存格式（数据库的种类）来进行分类，现阶段主要有以下 5 种类型.
- 层次数据库（Hierarchical Database，HDB）
- 关系数据库（Relational Database，RDB）
- 面向对象数据库（Object Oriented Database，OODB）
- XML数据库（XML Database，XMLDB）
- 键值存储系统（Key-Value Store，KVS），举例：MongoDB

其中`关系数据库（RDB）`的`数据库管理系统（DBMS）`称为`关系数据库管理系统（Relational Database Management System，RDBMS）`，较具有代表性的 RDBMS 有如下 5 种。
- Oracle Database：甲骨文公司的RDBMS
- SQL Server：微软公司的RDBMS
- DB2：IBM公司的RDBMS
- PostgreSQL：开源的RDBMS
- MySQL：开源的RDBMS

`SQL语言`的数据库关系系统，也就是关系数据库管理系统（RDBMS）的操作方法。

## RDBMS的常见系统结构
RDBMS 中最常见的系统结构就是`客户端/服务器类型（C/S类型）`
![image](https://user-images.githubusercontent.com/44680953/140949158-31ecaf3e-4d99-4f2d-831f-50f3fd656dab.png)

## 数据库安装
本次学习大家可以选择使用阿里云数据库服务器或者本地安装数据库进行学习，在下面对应的学习教程中也给告诉了大家如何创建本次学习需要的数据库表和数据，所以大家必须使用一个方式安装数据库，才能完成后面学习。

### 阿里云MySQL服务器使用介绍
节约篇幅，具体相关介绍以及给大家写到pdf里了，大家点击链接即可进入查看：
http://tianchi-media.oss-cn-beijing.aliyuncs.com/dragonball/SQL/other/阿里云MySQL服务器使用介绍.pdf
优点： 操作使用方便，未来趋势（数据上云），导入、导出数据方便，运行速度快。
缺点： 需要付费购买，不过现在对开发者有优惠活动，基础版本 1核1G，存储空间20G的，目前优惠价半年只需9.9元，一杯奶茶钱不到。

### 本地MySQL环境搭建方法介绍
节约篇幅，具体相关介绍以及给大家写到pdf里了，大家点击链接即可进入查看：
http://tianchi-media.oss-cn-beijing.aliyuncs.com/dragonball/SQL/other/本地MySQL环境搭建方法介绍.pdf
优点： 免费，增强动手能力。
缺点： 安装、配置麻烦，数据导入、导出耗时长。


# 初识 SQL

## 概念介绍
![image](https://user-images.githubusercontent.com/44680953/140949693-673a0294-af34-4047-8060-49c2fe6dc43b.png)
- 行：记录
- 列：字段
- 行x列：单元格

SQL 语句可以分为以下三类.
`DDL（Data Definition Language，数据定义语言）` 用来创建或者删除存储数据用的`数据库`以及`数据库中的表`等对象。
- CREATE ： 创建数据库和表等对象
- DROP ： 删除数据库和表等对象
- ALTER ： 修改数据库和表等对象的结构

`DML（Data Manipulation Language，数据操纵语言）` 用来查询或者变更表中的`记录`。
- SELECT ：查询表中的数据
- INSERT ：向表中插入新数据
- UPDATE ：更新表中的数据
- DELETE ：删除表中的数据

`DCL（Data Control Language，数据控制语言）` 用来`确认或者取消`对数据库中的数据进行的变更。除此之外，还可以对 RDBMS 的用户是否有`权限`操作数据库中的对象（数据库表等）进行设定。
- COMMIT ： 确认对数据库中的数据进行的变更
- ROLLBACK ： 取消对数据库中的数据进行的变更
- GRANT ： 赋予用户操作权限
- REVOKE ： 取消用户的操作权限

实际使用的 SQL 语句当中有 90% 属于 DML，本课程会以 DML 为中心进行讲解。

## SQL的基本书写规则
- SQL语句要以分号（ ; ）结尾
- SQL 不区分关键字的大小写，但是插入到表中的数据是区分大小写的
- win 系统默认不区分表名及字段名的大小写
- linux / mac 默认严格区分表名及字段名的大小写
- 本教程已统一调整表名及字段名的为小写，以方便初学者学习使用。
- 常数的书写方式是固定的（'abc', 1234, '26 Jan 2010', '10/01/26', '2010-01-26'…）
- 单词需要用半角空格或者换行来分隔

## 数据库的创建（ CREATE DATABASE 语句）
语法：
```SQL
CREATE DATABASE < 数据库名称 > ;
```
创建本课程使用的数据库
```SQL
CREATE DATABASE shop;
```

## 表的创建（ CREATE TABLE 语句）
## 命名规则
## 数据类型的指定
## 约束的设置
## 表的删除和更新
## 向 product 表中插入数据
