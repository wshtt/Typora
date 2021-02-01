# SQL



##### 简介

- SQL 指结构化查询语言，全称是 Structured Query Language。
- RDBMS: 关系数据库管理系统（Relational Database Management System）是指包括相互联系的逻辑组织和存取这些数据的一套程序 (数据库管理系统软件)。关系数据库管理系统就是管理关系数据库，并将数据逻辑组织的系统。
- SQL 用于 管理 RDBMS
- SQL 的范围包括表数据插入、查询、更新和删除，数据库模式创建和修改，以及数据访问控制。
- SQL 不区分大小写

##### RDBMS

- RDBMS中的数据存储在数据库对象称为表。表由列和行的组成。
- 列就是数据的字段,行就是每一条数据

```js
id		name		age

1		li			10
2		wang		11
3		zhang		12
```

- 约束是对表执行对数据的列的规则，这些约束用于限制数据的类型进入表中，并且确保数据库中的数据的准确性和可靠性。约束它可能是列级或表级，列级约束仅应用于一列，表级约束应用于整个表。

### 数据库操作

##### 1.创建

```sql
CREATE DATABASE database-name 
```

##### 2.删除

```mysql
drop database dbname
```

##### 3.修改

```mysql
ALTER DATABASE
```

##### 4.选择使用的数据库

```mysql
use dbname
```



## 数据操作语言 (DML) 和 数据定义语言 (DDL)。

SQL 分为 数据操作语言 (DML) 和 数据定义语言 (DDL)。

### 一、数据操作语言 (DML)

- *SELECT* - 从数据库表中获取数据
- *UPDATE* - 更新数据库表中的数据
- *DELETE* - 从数据库表中删除数据
- *INSERT INTO* - 向数据库表中插入数据



###### 1.SELECT

SELECT 语句用于从表中选取数据。结果被存储在一个结果表中（称为结果集）。

```sql
-- 格式

SELECT 列名称 FROM 表名称

SELECT * FROM Persons

SELECT name,age FROM Persons

```

###### 2.**SELECT DISTINCT**

DISTINCT： 结果集去重

```sql
-- 去重

SELECT DISTINCT 列名称 FROM 表名称

SELECT DISTINCT name FROM Persons
```

###### 3.WHERE

WHERE： 后面跟条件

```sql
-- 基础用法
-- 值为文本需要使用单引号，值为数字则不需要

SELECT 列名称 FROM 表名称 WHERE 列 运算符 值

SELECT * FROM Persons WHERE City = 'Beijing'
```

###### 4.**AND**

**AND** ：的所有条件都需要成立

```sql
-- and

SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'
```

###### 5.**OR**

**OR**：条件中的一个成立

```sql
-- or 的内容一般用（）包起来

SELECT * FROM Persons WHERE firstname='Thomas' OR lastname='Carter'

SELECT * FROM Persons WHERE (FirstName='Thomas' OR FirstName='William') AND LastName='Carter'
```

###### 6.ORDER BY

ORDER BY ：用于对结果集排序

```mysql
-- 排序分为 ASC：正序 和 DESC：倒叙
-- ASC 可以省略，默认ASC

SELECT Company, OrderNumber FROM Orders ORDER BY Company

SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC

-- 多个排序字段，先根据第一个排序，第一个一样则根据第二个排序

SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber
```

###### 7.INSERT INTO

INSERT INTO 语句用于向表格中插入新的行。

```sql
-- 添加所有值，顺序要正确

INSERT INTO 表名称 VALUES (值1, 值2,....)

-- 选择性添加值，根据字段顺序

INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

###### 8.Update 

Update 语句用于修改表中的数据。

```sql
-- 修改语法

UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值

UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson' 

-- 修改多个元素
UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing' WHERE LastName = 'Wilson'
```

###### 9.DELETE

DELETE 语句用于删除表中的行。

```sql
-- 删除

DELETE FROM 表名称 WHERE 列名称 = 值

DELETE FROM Person WHERE LastName = 'Wilson' 

-- 删除所有行

DELETE FROM table_name
DELETE * FROM table_name
```



### 二、数据定义语言 (DDL)

- *CREATE DATABASE* - 创建新数据库
- *ALTER DATABASE* - 修改数据库
- *CREATE TABLE* - 创建新表
- *ALTER TABLE* - 变更（改变）数据库表
- *DROP TABLE* - 删除表
- *CREATE INDEX* - 创建索引（搜索键）
- *DROP INDEX* - 删除索引















## 条件查询



###### 1. SQL TOP /LIMIT

TOP 子句用于规定要返回的记录的数目。分页

并非所有的数据库系统都支持 TOP 子句

```sql
-- 分页，每页十条

-- SQL Server
SELECT TOP number|percent column_name(s) FROM table_name

-- MySQL 0,初始值； 10，返回条数
SELECT * FROM Persons LIMIT 0,10

-- Oracle
SELECT * FROM Persons WHERE ROWNUM <= 10

-- SELECT TOP 前两条
SELECT TOP 2 * FROM Persons

-- SELECT TOP 50 PERCENT 前 50%
SELECT TOP 50 PERCENT * FROM Persons
```

###### 2. LIKE /NOT LIKE

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。pattern

```sql
-- 格式

SELECT * FROM table_name WHERE column_name LIKE pattern
SELECT * FROM Persons WHERE City NOT LIKE '%lon%'
```



```sql
-- SQL 通配符
-- 在搜索数据库中的数据时，SQL 通配符可以替代一个或多个字符。
-- SQL 通配符必须与 LIKE 运算符一起使用。
-- 模糊查询使用 % 做通配符
'%b'
'b%'
'%b%'

'[ALN]%'	-- 以字母 A L N 开头的
'[!ALN]%'	-- 不以 A L N 开头的
```

|           通配符           |            描述            |
| :------------------------: | :------------------------: |
|             %              |     替代一个或多个字符     |
|             _              |       仅替代一个字符       |
|         [charlist]         |   字符列中的任何单一字符   |
| [^charlist]或者[!charlist] | 不在字符列中的任何单一字符 |

###### 3. IN

IN 操作符允许我们在 WHERE 子句中规定多个值。

```sql
-- 格式 

SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,...)
SELECT * FROM Persons WHERE LastName IN ('Adams','Carter')
```

###### 4. BETWEEN ... AND
```sql
-- 格式

SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2;

-- not between
SELECT * FROM Websites WHERE name NOT BETWEEN 'A' AND 'H';    

-- between 与 in
SELECT * FROM Websites WHERE (alexa BETWEEN 1 AND 20) AND country NOT IN ('USA', 'IND');
```

###### 5. Alias

Alias：为列名称和表名称指定别名

as ：可以省略

```sql
-- 表的别名
SELECT column_name(s) FROM table_name AS alias_name

-- 列的别名
SELECT column_name AS alias_name FROM table_name
```

###### 6. JOIN

**INNER JOIN 用于根据两个或多个表中的列之间的关系，从这些表中查询数据。**（内连接）

INNER 可以省略

```sql
-- 查询出A.ID = B.ID 的所有结果

SELECT * FROM A JOIN B ON A.ID = B.ID
SELECT column_name(s) FROM table_name1 INNER JOIN table_name2 ON table_name1.column_name = table_name2.column_name
```

###### 7. LEFT JOIN/RIGHT JOIN

左外连接/右外连接

返回左表中所有的行，右表有匹配则显示，无匹配则为null （右链接相反）

```sql
-- 返回所有 A的行，有 A.ID = B.ID的，把 B 的信息也放进去

SELECT * FROM A LEFT JOIN B ON A.ID = B.ID
SELECT column_name(s) FROM table_name1 LEFT JOIN table_name2 ON table_name1.column_name=table_name2.column_name
```



### 通配符 与like 一起使用

```sql
%  代表一个或多个
_  代表一个
[afa] 代表其中任意一个字符
[!fdd] 不在字符列中的任何单一字符
[^sfn] 不在字符列中的任何单一字符
```

### 别名 Alias
```sql
表的别名 select column_name FROM table_name AS alias_name
列的别名 select column_name AS alias_name FROM table_name
```

### 链接 JOIN   INNER JOIN 
```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo 
  FROM Persons, Orders 
  WHERE Persons.Id_P = Orders.Id_P 
  
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
  FROM Persons
  INNER JOIN Orders
  ON Persons.Id_P = Orders.Id_P
  ORDER BY Persons.LastName
```





### 其他



##### 1. in 与 exists

外查询表大，子查询表小，选择IN；外查询表小，子查询表大，选择EXISTS；若两表差不多大，则差不多。

###### in

```sql
select * from A where id in(select id from B)

-- in()中的查询只执行一次，它查询出B中的所有的id并缓存起来，然后检查A表中查询出的id在缓存中是否存在，如果存在则将A的查询数据加入到结果集中，直到遍历完A表中所有的结果集为止。
```

###### exists

```sql
select a.* from A a where EXISTS(select 1 from B b where a.id=b.id)

-- EXISTS()查询会执行SELECT * FROM A查询，执行A.length次，并不会将EXISTS()查询结果结果进行缓存，因为EXISTS()查询返回一个布尔值true或flase，它只在乎EXISTS()的查询中是否有记录，与具体的结果集无关。
-- EXISTS()查询是将主查询的结果集放到子查询中做验证，根据验证结果是true或false来决定主查询数据结果是否得以保存。
```

