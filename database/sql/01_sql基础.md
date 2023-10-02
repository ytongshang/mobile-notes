# Sql

- [基本语法](#基本语法)
- [DDL](#ddl)
    - [DATABASE](#database)
        - [创建databases](#创建databases)
        - [mysql中database的基本操作](#mysql中database的基本操作)
    - [CREATE TABLE](#create-table)
        - [sql中常用的数据结构](#sql中常用的数据结构)
    - [CREATE VIEW](#create-view)
    - [DROP](#drop)
        - [删除索引](#删除索引)
        - [删除表](#删除表)
        - [删除数据库](#删除数据库)
        - [TRUNCATE TABLE语句](#truncate-table语句)
    - [ALTER](#alter)
        - [增加列](#增加列)
        - [删除列](#删除列)
        - [改变数据类型](#改变数据类型)
    - [INCREMENT](#increment)
- [DML](#dml)
    - [SELECT](#select)
    - [INSERT](#insert)
    - [UPDATE](#update)
    - [DELETE](#delete)
    - [WHERE](#where)
    - [AND 和OR](#and-和or)
    - [ORDER BY](#order-by)
    - [TOP相关](#top相关)
        - [TOP](#top)
        - [TOP PERCENT](#top-percent)
    - [LIKE](#like)
        - [通配符](#通配符)
    - [IN](#in)
    - [BETWEEN](#between)
    - [ALIAS](#alias)
    - [JOIN](#join)
        - [INNER JOIN](#inner-join)
        - [LEFT JOIN](#left-join)
        - [RIGHT JOIN](#right-join)
        - [FULL JOIN](#full-join)
    - [UNION](#union)
    - [SELECT INTO](#select-into)
- [其它](#其它)
    - [Date](#date)
        - [YEAR](#year)
        - [DATE](#date)
        - [TIME](#time)
        - [DATETIME](#datetime)
        - [TIMESTAMP](#timestamp)
        - [相关函数](#相关函数)
        - [查询相关](#查询相关)
    - [Nulls](#nulls)
        - [与NULL相关的函数](#与null相关的函数)
- [函数](#函数)
    - [Aggregate functions](#aggregate-functions)
        - [AVG](#avg)
        - [Count](#count)
        - [FIRST与LAST](#first与last)
        - [MIN与MAX](#min与max)
        - [SUM](#sum)
        - [GROUP BY](#group-by)
        - [Having](#having)
    - [Scalar 函数](#scalar-函数)
        - [字符操作函数](#字符操作函数)
        - [其它函数](#其它函数)

## 基本语法

- sql语句末尾可以有分号，也可以没有分号

- DDL数据定义
    - CREATE DATABASE
    - ALTER DATABASE
    - CREATE TABLE
    - ALTER TABLE
    - DROP TABLE
    - CREATE INDEX
    - DROP INDEX

- DML数据操作语言
    - SELECT
    - UPDATE
    - DELETE
    - INSERT INTO

## DDL

### DATABASE

#### 创建databases

```sql
CREATE DATABASE database_name;
```

#### mysql中database的基本操作

- 显示所有的mysql管理的数据库

 ```sql
SHOW DATABASES;
 ```

- 显示当前使用的数据库

```sql
SELECT DATABASE();
```

- 使用某个数据库

```sql
USE database_name;
```

### CREATE TABLE

```sql
CREATE TABLE 表名称
(
列名称1 数据类型 约束,
列名称2 数据类型 约束,
列名称3 数据类型 约束,
....
)
  -- 一般还可以指定engine和字符集
  ENGINE = InnoDB
  DEFAULT CHARSET = utf8;
```

#### sql中常用的数据结构

数据类型        | 描述
----------------|-----------------------------------------------------------------------------------
integer(size) int(size) smallint(size)tinyint(size)   | 仅容纳整数。在括号内规定数字的最大位数。
decimal(size,d) numeric(size,d) | 容纳带有小数的数字。"size"规定数字的最大位数。"d" 规定小数点右侧的最大位数。
char(size)      | 容纳固定长度的字符串（可容纳字母、数字以及特殊字符）。在括号中规定字符串的长度。
varchar(size)   | 容纳可变长度的字符串（可容纳字母、数字以及特殊的字符）。在括号中规定字符串的最大长度。
date(yyyymmdd)  | 容纳日期。

### CREATE VIEW

- **视图是基于SQL语句的结果集的可视化的表,视图包含行和列，就像一个真实的表**,视图中的字段就是来自一个或多个数据库中的真实的表中的字段
- **视图总是显示最新的数据**,每当用户查询视图时，数据库引擎通过使用视图的 SQL 语句重建数据

```sql
-- 创建视图
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition

-- 更新视图
CREATE OR REPLACE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition

-- 删除视图
DROP VIEW view_name
```

```sql
CREATE VIEW Order_Above_Avg_Price AS
  SELECT O_Id,OrderDate,OrderPrice,Customer
  FROM Orders
  WHERE OrderPrice > (SELECT AVG(OrderPrice)
                      FROM Orders);

SELECT * FROM Order_Above_Avg_Price;
```

### DROP

#### 删除索引

```sql
-- MS SQL Server
DROP INDEX index_name.

-- MYSQL
-- Unique索引删除一样
ALTER TABLE table_name DROP INDEX index_name
```

#### 删除表

```sql
DROP TABLE 表名称

DROP TABLE IF EXISTS 表名称;
```

#### 删除数据库

```sql
DROP DATABASE 数据库名称;

DROP DATABASE IF EXISTS 数据库名称;
```

#### TRUNCATE TABLE语句

- 仅仅删除表内的数据，而不删除表本身

```sql
-- 整体删除，执行效率比较快
TRUNCATE TABLE 表名称;

-- 最后的效果相同
-- 逐条删除，执行效率较慢
DELETE FROM 表名称;
```

### ALTER

#### 增加列

```sql
ALTER TABLE table_name
ADD column_name datatype

-- MySQL
ALTER TABLE Orders
  ADD COLUMN update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP;
```

#### 删除列

```sql
ALTER TABLE table_name
DROP COLUMN column_name

-- MySQL
ALTER TABLE Orders
    DROP COLUMN update_time;
```

#### 改变数据类型

```sql
ALTER TABLE Persons
ALTER COLUMN Birthday year

-- MYSQL语法不所不同
ALTER TABLE Orders
    MODIFY COLUMN update_time VARCHAR(20);

ALTER TABLE Orders
    ADD COLUMN order_desc VARCHAR(128);
-- 修改数据长度
ALTER TABLE Orders
    MODIFY COLUMN order_desc VARCHAR(256);
-- 修改列的名字
ALTER TABLE Orders
    CHANGE order_desc or_desc VARCHAR(256)
```

### INCREMENT

- Auto-increment 会在新记录插入表中时生成一个唯一的数字
- **默认地，AUTO_INCREMENT 的开始值是 1，每条新记录递增 1**
- 要让 AUTO_INCREMENT 序列以其他的值起始，不同的数据库有不同的设置方法

```sql
-- AUTO_INCREMENT默认从1开始，每次增加1
-- 可以在初始设置AUTO_INCREMENT默认开始的值
-- MYSQL
CREATE TABLE Persons
(
P_Id INT NOT NULL AUTO_INCREMENT PRIMARY KEY ,
LastName VARCHAR(255) NOT NULL,
FirstName VARCHAR(255),
Address VARCHAR(255),
City VARCHAR(255)
) AUTO_INCREMENT = 100;

-- 也可以通过ALTER设置AUTO_INCREMENT默认从100开始
ALTER TABLE Persons AUTO_INCREMENT=100
```

## DML

### SELECT

```sql
SELECT 列名称 FROM 表名称
SELECT * FROM 表名称
SELECT DISTINCT 列名称 FROM 表名称
```

### INSERT

```sql
INSERT INTO 表名称 VALUES (值1, 值2,....)

INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

```sql
INSERT INTO Persons
VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing')

-- 插入部分列
INSERT INTO Persons (LastName, Address)
VALUES ('Wilson', 'Champs-Elysees')
```

### UPDATE

```sql
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

```sql
UPDATE Person SET FirstName = 'Fred'
WHERE LastName = 'Wilson'

-- 修改多行
UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing'
WHERE LastName = 'Wilson'
```

### DELETE

- DELETE 语句用于删除表中的行

```sql
DELETE FROM 表名称 WHERE 列名称 = 值
```

```sql
-- 删除某行
DELETE FROM Person
WHERE LastName = 'Wilson'

--可以在不删除表的情况下删除所有的行。这意味着表的结构、属性和索引都是完整的
-- 效果等同于TRUNCATE TABLE语句
DELETE FROM table_name
```

### WHERE

```sql
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
```

操作符  | 描述
--------|--------------
=       | 等于
<>      | 不等于
>       | 大于
<       | 小于
>=      | 大于等于
<=      | 小于等于
BETWEEN | 在某个范围内
LIKE    | 搜索某种模式

- **SQL 使用单引号来环绕文本值**,大部分数据库系统也接受双引号。
- **如果是数值，不要使用引号**

### AND 和OR

```sql
SELECT * FROM Persons
WHERE FirstName='Thomas' AND LastName='Carter'

SELECT * FROM Persons
WHERE firstname='Thomas' OR lastname='Carter'

SELECT * FROM Persons
WHERE (FirstName='Thomas' OR FirstName='William') AND LastName='Carter'
```

### ORDER BY

- **升序:ASC 降序:DESC**
- **指定了ORDER BY，但没有指定ASC，DESC，默认是ASC**
- **排序允许有多个字段**， 先对第一个字段排序，然后对结果按第二个字段，依次类推
- [MySql Order By 多个字段 排序规则](http://blog.csdn.net/xlxxcc/article/details/52250963)

```sql
-- 以Company升序排列，其他字段出现的顺序是自然排序的
SELECT Company, OrderNumber
FROM Orders
ORDER BY Company

-- 先以Company进行升序，然后再以OrderNumber进行升序
SELECT Company, OrderNumber
FROM Orders
ORDER BY Company, OrderNumber

-- 如果没有指定就是ASC
-- 所以实际等同于ORDER BY Company ASC, OrderNumber DESC
SELECT Company, OrderNumber
FROM Orders
ORDER BY Company, OrderNumber DESC
```

### TOP相关

#### TOP

- TOP 子句用于规定要返回的记录的数目
- 并非所有的数据库系统都支持 TOP 子句

```sql
-- SQL Server
SELECT TOP number|percent column_name(s)
FROM table_name
```

```sql
-- MySQL中的limit与top作用类似
SELECT *
FROM Persons
LIMIT 5
```

#### TOP PERCENT

- 希望从上面的 "Persons" 表中选取 50% 的记录

```sql
-- SQL server
SELECT TOP 50 PERCENT * FROM Persons
```

- mysql中不支持top percent,但是可以通过count间接来实现

```sql
-- 可以通过count查询所以的数目，间接实现百分比
mysql> select count(*) as rows from city;

-- 向变量赋值@percent 取前50%的行数
mysql> select count(*)*50/100 into @percent from city;

-- 预设SQL语句
mysql> prepare stmt from "select * from city limit ?";

-- 运行预设SQL语句
mysql> execute stmt using @percent;
```

### LIKE

- LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式,一般要搭配通配符使用

```sql
-- 以B开头的城市
select * from city where Name Like 'B%';

-- 以alya结尾的城市
select * from city where Name like '%alya';

-- 中间含有lany的城市
select * from city where Name like '%lany%';

--中间不含有字母a的城市
select * from city where Name not like '%a%';
```

#### 通配符

- 在搜索数据库中的数据时，您可以使用 SQL 通配符
- **通配符必须和LIKE一起使用**

通配符                      | 描述
----------------------------|----------------------------
%                           | 替代一个或多个字符
_                           | 仅替代一个字符
[charlist]                  | 字符列中的任何单一字符
[!charlist] 或者[^charlist] | 不在字符列中的任何单一字符

### IN

- IN 操作符允许我们在 WHERE 子句中规定多个值

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...)
```

```sql
select * from city where CountryCode in ('USA','UZB');
```

### BETWEEN

- 操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。**这些值可以是数值、文本或者日期**
- **不同的数据库对于区间的闭合是有区别的，如果不清楚，可以考虑使用具体的逻辑操作符**

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name
BETWEEN value1 AND value2
```

```sql
-- 列出人数在1 与40000之间的城市
select * from city where Population between 1 and 40000;

-- 人数不在1与500000之间的城市
-- 对于不在区间内，使用NOT操作符
select * from city where Population not between 1 and 500000;
```

### ALIAS

- 通过使用 SQL，可以为列名称和表名称指定别名（Alias）

```sql
-- 表的别名
SELECT column_name(s)
FROM table_name
AS alias_name

-- 列的别名
SELECT column_name AS alias_name
FROM table_name
```

```sql
-- 假设我们有两个表分别是："Persons" 和 "Product_Orders"
-- 希望列出 "John Adams" 的所有定单
-- 表名的别名使我们书写起来更加的方便
SELECT po.OrderID, p.LastName, p.FirstName
FROM Persons AS p, Product_Orders AS po
WHERE p.LastName='Adams' AND p.FirstName='John'

-- 有意义的别名
SELECT LastName AS Family, FirstName AS Name
FROM Persons
```

### JOIN

- SQL JOIN 子句用于把来自两个或多个表的行结合起来，基于这些表之间的共同字段

#### INNER JOIN

- **INNER JOIN 关键字在表中存在至少一个匹配时返回行**
- INNER JOIN与JOIN是相同的

```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name=table2.column_name;

SELECT column_name(s)
FROM table1
JOIN table2
ON table1.column_name=table2.column_name;
```

#### LEFT JOIN

- **LEFT JOIN 关键字从左表（table1）返回所有的行，即使右表（table2）中没有匹配。如果右表中没有匹配，则结果为 NULL**

```sql
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name=table2.column_name;
```

#### RIGHT JOIN

- **RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。**

```sql
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name=table2.column_name;
```

#### FULL JOIN

- **FULL OUTER JOIN 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行**

```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name=table2.column_name;
```

### UNION

- UNION 操作符用于合并两个或多个 SELECT 语句的结果集,UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名
- **UNION内部的每个SELECT语句必须拥有相同数量的列。列也必须拥有相似的数据类型。每个SELECT语句中的列的顺序必须相同。**
- **UNION命令时需要注意，只能在最后使用一个ORDER BY命令，是将两个查询结果合在一起之后，再进行排序！绝对不能写两个ORDER BY命令**
- **UNION操作符选取不同的值。如果允许重复的值，请使用 UNION ALL**

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

### SELECT INTO

- SELECT INTO 语句从一个表复制数据，然后把数据插入到另一个新表中

```sql
-- 备份Persons表
SELECT *
INTO Persons_backup
FROM Persons

-- MYSQL中不支持SELECT INTO的语法
-- MYSQL中的写法
CREATE TABLE Persons_backup
(SELECT * FROM Persons);

-- 备份Persons表到另一个数据库
SELECT *
INTO Persons IN 'Backup.mdb'
FROM Persons

-- 备份Persons中的部分列
SELECT LastName,FirstName
INTO Persons_backup
FROM Persons

SELECT LastName,Firstname
INTO Persons_backup
FROM Persons
WHERE City='Beijing'
```

## 其它

### Date

-[MySQL数据类型--日期和时间类型](http://blog.csdn.net/u011794238/article/details/50914444)

#### YEAR

- 格式 YYYY 或 YY
- 范围从1901~~2155

#### DATE

- date类型使用4个字节来表示日期。MySQL中是以YYYY-MM-DD的形式显示date类型的值

#### TIME

- time类型使用3个字节来表示时间。MySQL中以HH:MM:SS的形式显示Time类型的值
- **使用current_time()或current_time获得当前的系统时间**

#### DATETIME

- datetime类型使用8个字节来表示日期和时间。MySQL中以‘YYYY-MM-DD HH:MM:SS’的形式来显示dateTime类型的值
- 其中取值范围为1000-01-01 00：00：00〜9999-12-31 23：59：59
- **使用now()来获得当前的系统日期和时间**

```sql
-- MYSQL中设置DEFAULT值与类型TIMESTAMP一样，采用CURRENT_TIMESTAMP
CREATE TABLE Persons3
(
  P_Id      INT          NOT NULL  PRIMARY KEY AUTO_INCREMENT,
  LastName  VARCHAR(255) NOT NULL,
  FirstName VARCHAR(255),
  Address   VARCHAR(255),
  City      VARCHAR(255),
  UpdateTime DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
  AUTO_INCREMENT = 1;
```

#### TIMESTAMP

- timestamp类型使用4个字节来表示日期和时间。
- timestamp类型的范围是从1970-01-01 08:00:01~~2038-01-19 11:14:07。
- MySQL中也是以‘YYYY-MM-DD HH:MM:SS’的形式显示timestamp类型的值。从其形式可以看出，timestamp类型与dateTime类型显示的格式是一样的
- **使用CURRENT_TIMESTAMP来输入系统当前日期与时间**
- **TIMESTAMP类型还有一个很大的特殊点，就是时间是根据时区来显示的!!!!**，哪怕字段值不变，在不同的时区显示的也是不一样的

```sql
-- MYSQL中设置DEFAULT值使用CURRENT_TIMESTAMP
CREATE TABLE Orders (
  Id_O      INT PRIMARY KEY,
  OrderNo   INT NOT NULL,
  Id_P      INT,
  OrderData TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (Id_P) REFERENCES Persons (Id_P),
)
  ENGINE = InnoDB;
```

#### 相关函数

- MYSQL中相关的函数

函数          | 描述
--------------|-------------------------------------
NOW()         | 返回当前的日期和时间
CURDATE()     | 返回当前的日期
CURTIME()     | 返回当前的时间
DATE()        | 提取日期或日期/时间表达式的日期部分
EXTRACT()     | 返回日期/时间按的单独部分
DATE_ADD()    | 给日期添加指定的时间间隔
DATE_SUB()    | 从日期减去指定的时间间隔
DATEDIFF()    | 返回两个日期之间的天数
DATE_FORMAT() | 用不同的格式显示日期/

#### 查询相关

- 如果字段带有时间，仅仅列出日期是查不到数据的

```sql
CREATE TABLE Persons3
(
  P_Id      INT          NOT NULL  PRIMARY KEY AUTO_INCREMENT,
  LastName  VARCHAR(255) NOT NULL,
  FirstName VARCHAR(255),
  Address   VARCHAR(255),
  City      VARCHAR(255),
  UpdateTime DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
  AUTO_INCREMENT = 1;

-- 1  Tom	Green	JiangCun	HangZhou	2018-02-28 19:43:43

-- 查不到数据的
SELECT * FROM Persons3 WHERE UpdateTime='2018-02-28';

-- 可以查到数据
SELECT * FROM Persons3 WHERE UpdateTime='2018-02-28 19:43:43';
```

### Nulls

- NULL值是遗漏的未知数据，
- 如果表中的某个列是可选的，那么我们可以在不向该列添加值的情况下插入新记录或更新已有的记录。
 这意味着该字段将以 NULL 值保存。
- 默认地，表的列可以存放 NULL 值,如果不允许为NULL，使用NOT NULL约束
- **无法比较 NULL 和 0**

```sql
CREATE TABLE Test
(
  id INT PRIMARY KEY AUTO_INCREMENT,
  a INT,
  b INT NOT NULL
);

-- 即使a的数据类型为INT，也是可以插入NULL值的，所以NULL和0是完全不同的
-- b的数据类型NOT NULL，那么在插入时必须指定值
INSERT INTO Test(a,b) VALUES (NULL ,1 );
```

- **无法使用比较运算符来测试 NULL 值，比如 =, <, 或者 <>，必须使用 IS NULL 和 IS NOT NULL 操作符**

```sql
INSERT INTO Test(a,b) VALUES (NULL ,1 );
INSERT INTO Test(a,b) VALUES (100, 5);

-- 使用IS NULL
SELECT * FROM Test WHERE a IS NULL ;

-- 使用IS NOT NULL
SELECT * FROM Test WHERE a Is NOT NULL ;
```

#### 与NULL相关的函数

id | a    | b
---|------|---
1  | null | 1
2  | 100  | 5

```sql
-- 第一行中因为a为null,那么返回的结果为null
SELECT id, a*b
FROM Test;
```

id | a*b
---|------
1  | null
2  | 500

- 我们希望将null当作其它特殊的值处理，比如如果为null,当作0处理

```sql
--SQL Server / MS Access
SELECT id, ISNULL(a,0)*b
FROM Test

-- MYSQL
SELECT id, IFNULL(a,0)*b
FROM Test

-- MYSQL的另一种写法
SELECT id,COALESCE(a,0)*b
FROM Test
```

## 函数

### Aggregate functions

#### AVG

- AVG 函数返回数值列的平均值
- **NULL 值不包括在计算中**

```sql
CREATE TABLE Orders (
  O_Id       INT PRIMARY KEY AUTO_INCREMENT,
  OrderDate  DATETIME        DEFAULT CURRENT_TIMESTAMP,
  OrderPrice INT  NOT NULL,
  Customer   TEXT NOT NULL
)
  DEFAULT CHARSET = utf8;

INSERT INTO Orders (OrderPrice, Customer) VALUES (1000, 'Bush');
INSERT INTO Orders (OrderPrice, Customer) VALUES (1600, 'Carter');
INSERT INTO Orders (OrderPrice, Customer) VALUES (700, 'Bush');
INSERT INTO Orders (OrderPrice, Customer) VALUES (300, 'Bush');
INSERT INTO Orders (OrderPrice, Customer) VALUES (2000, 'Adams');
INSERT INTO Orders (OrderPrice, Customer) VALUES (100, 'Carter');

-- 获得OrderPrice的平均值
SELECT AVG(OrderPrice) AS OrderAverage
FROM Orders;

-- 找出有OrderPrice高于平均值的顾客
SELECT Customer
FROM Orders
WHERE OrderPrice > (SELECT AVG(OrderPrice)
                    FROM Orders);
```

#### Count

- **COUNT(column_name) 函数返回指定列的值的数目,NULL值不计入**
- **COUNT(*) 函数返回表中的记录数**
- **COUNT(DISTINCT column_name) 函数返回指定列的不同值的数目**

```sql
-- Bush的订单数
SELECT COUNT(Customer) AS CustomerOrders
FROM Orders
WHERE Customer='Bush';

-- 所有的订单数
SELECT COUNT(*) AS OrderNumbers
FROM Orders;

-- 有订单的顾客数
SELECT COUNT(DISTINCT Customer) As Customers
FROM Orders;
```

#### FIRST与LAST

- FIRST() 函数返回指定的字段中第一个记录的值
- LAST() 函数返回指定的字段中最后一个记录的值

```sql
-- first
SELECT FIRST(column_name) FROM table_name
SELECT FIRST(OrderPrice) AS FirstOrderPrice FROM Orders

-- last
SELECT LAST(column_name) FROM table_name
SELECT LAST(OrderPrice) AS LastOrderPrice FROM Orders
```

- MYSQL中没有实现first与last，可以使用Order By与limit间接来实现

```sql
-- 找第一条记录和最后一条记录
-- 注意ORDER BY选择的列比较重要，比如这里使用自增Id作为排序的列
SELECT OrderPrice FROM Orders ORDER BY O_Id LIMIT 1;
SELECT OrderPrice FROM Orders ORDER BY O_Id DESC LIMIT 1;
```

#### MIN与MAX

- MIN 函数返回一列中的最小值。**NULL 值不包括在计算中**
- MAX 函数返回一列中的最大值。**NULL 值不包括在计算中。**
- MIN 和 MAX 也可用于文本列，以获得按字母顺序排列的最高或最低值

```sql
SELECT MIN(OrderPrice) AS MinOrderPrice FROM Orders;

SELECT MAX(OrderPrice) AS MinOrderPrice FROM Orders;
```

#### SUM

- SUM 函数返回数值列的总数,**NULL 值不包括在计算中**

```sql
CREATE TABLE FunctionTest (
  id INT PRIMARY KEY AUTO_INCREMENT,
  a  INT,
  b  INT NOT NULL
)
  -- 一般还可以指定engine和字符集
  ENGINE = InnoDB
  DEFAULT CHARSET = utf8;

INSERT INTO FunctionTest(a, b) VALUES (NULL ,1);
INSERT INTO FunctionTest(a, b) VALUES (1, 2);

SELECT SUM(a) FROM FunctionTest;
-- 结果为 1

SELECT SUM(b) FROM FunctionTest;
-- 结果为3
```

#### GROUP BY

- **GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组**
- **GROUP BY X, Y意思是将所有具有相同X字段值和Y字段值的记录放到一个分组里**

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
```

```sql
SELECT Customer, SUM(OrderPrice)
FROM Orders
GROUP BY Customer;

-- group by Customer, OrderDate指将顾客和时间相同的分为一组
SELECT Customer, OrderDate,SUM(OrderPrice)
FROM Orders
GROUP BY Customer, OrderDate;
```

#### Having

- having称为分组滤过条件,也就是说是分组需要的条件,所以**必须与group by联用**
 聚合函数计算的结果可以当条件来使用，因为它无法放在where里，只能通过having这种方式来解决

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value
```

```sql
-- 查找客户 "Bush" 或 "Adams" 拥有超过 1500 的订单总金额
SELECT Customer,SUM(OrderPrice) FROM Orders
WHERE Customer='Bush' OR Customer='Adams'
GROUP BY Customer
HAVING SUM(OrderPrice)>1500
```

### Scalar 函数

- Scalar 函数的操作面向某个单一的值，并返回基于输入值的一个单一的值

#### 字符操作函数

- **UCASE 函数**把字段的值转换为大写
- **LCASE 函数**把字段的值转换为小写

```sql
SELECT UCASE(column_name) FROM table_name
SELECT LCASE(column_name) FROM table_name
```

- **MID 函数**用于从文本字段中提取字符，**注意如果从起始开始是从1开始，而非0**

```sql
-- 从 "City" 列中提取前 3 个字符
SELECT MID(City,1,3) as SmallCity FROM Persons
```

- **LEN 函数**返回文本字段中值的长度

```sql
SELECT LEN(City) as LengthOfCity FROM Persons
```

#### 其它函数

- **ROUND 函数**用于把数值字段舍入为指定的小数位数,针对decimals类型

```sql
SELECT ROUND(column_name,decimals) FROM table_name

-- 价格舍入为最接近的整数
SELECT ProductName, ROUND(UnitPrice,0) as UnitPrice FROM Products
```

- **NOW 函数**返回当前的日期和时间

- **FORMAT 函数**用于对字段的显示进行格式化

```sql
SELECT FORMAT(100.31111,2);
```