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
CREATE DATABASE test01
```

##### 2.删除

```mysql
drop database test01
```

##### 3.修改

```mysql
ALTER DATABASE test01
```

##### 4.选择使用的数据库

```mysql
use test01
```



## (DML) 和  (DDL)。

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















## SQL条件查询



#### SQL查询语法

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

IN 操作符允许我们在 WHERE 子句中规定多个值。(常量)

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

###### 6. JOIN（交集）

**INNER JOIN 用于根据两个或多个表中的列之间的关系，从这些表中查询数据。**（内连接）

INNER 可以省略

```sql
-- 查询出A.ID = B.ID 的所有结果

SELECT * FROM A JOIN B ON A.ID = B.ID
SELECT column_name(s) FROM table_name1 INNER JOIN table_name2 ON table_name1.column_name = table_name2.column_name
```

###### 7. LEFT JOIN/RIGHT JOIN（交集+左/右）

左外连接/右外连接

返回左表中所有的行，右表有匹配则显示，无匹配则为null （右链接相反）

```sql
-- 返回所有 A的行，有 A.ID = B.ID的，把 B 的信息也放进去

SELECT * FROM A LEFT JOIN B ON A.ID = B.ID
SELECT column_name(s) FROM table_name1 LEFT JOIN table_name2 ON table_name1.column_name=table_name2.column_name
```

###### 8. FULL OUTER JOIN （并集）

```sql
-- 返回 A与B的所有行交集， mysql 不支持此查询

SELECT column_name(s) FROM table1 FULL OUTER JOIN table2 ON table1.column_name=table2.column_name;
```

###### 9. UNION/UNION ALL

```sql
-- 合并多个sql 的查询结果到一个返回集合
-- (UNIUN 结果集去重)（UNION ALL 结果集不去重）
-- UNION 内部的每个 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同。

SELECT column_name(s) FROM table1 UNION SELECT column_name(s) FROM table2;

-- UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

```

###### 10.SELECT INTO

```sql
-- 从一个表复制数据，然后把数据插入到另一个新表中。（不存在的表）
-- MySQL 数据库不支持 SELECT ... INTO 语句，但支持 INSERT INTO ... SELECT

SELECT Websites.name, access_log.count, access_log.date INTO WebsitesBackup2016 FROM Websites LEFT JOIN access_log ON Websites.id=access_log.site_id;
```

###### 11.INSERT INTO SELECT

```sql
-- 从一个表复制数据，然后把数据插入到一个已存在的表中。（已存在的表）
-- 目标表中任何已存在的行都不会受影响。

INSERT INTO table2 SELECT * FROM table1;

INSERT INTO table2 (column_name(s)) SELECT column_name(s) FROM table1;
-- 例子
INSERT INTO Websites (name, country) SELECT app_name, country FROM apps WHERE id=1;
```

###### 12.CREATE DATABASE

```sql
-- 创建数据库

CREATE DATABASE dbname;
```

###### 13. CREATE TABLE

```sql
-- 创建表

CREATE TABLE table_name
(
column_name1 data_type(size),
column_name2 data_type(size),
column_name3 data_type(size),
....
);

CREATE TABLE Persons
(
PersonID int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);
```

###### 14. 约束

```sql
-- 约束用于规定表中的数据规则。
-- 1. 创建表的时候规定
-- 2. 对已存在的表通过（ ALTER TABLE） 添加

CREATE TABLE table_name
(
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
....
);
```

- **NOT NULL** - 指示某列不能存储 NULL 值。
- **UNIQUE** - 保证某列的每行必须有唯一的值。
- **PRIMARY KEY** - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- **FOREIGN KEY** - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- **CHECK** - 保证列中的值符合指定的条件。
- **DEFAULT** - 规定没有给列赋值时的默认值。



###### 15.NOT NULL

```sql
-- 强制列不接受 NULL 值

CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);

-- Age 添加 not null 约束
ALTER TABLE Persons MODIFY Age int NOT NULL;
-- Age 删除 not null 约束
ALTER TABLE Persons MODIFY Age int NULL;
```

###### 16.UNIQUE 

```sql
-- 唯一约束

-- 单个字段 添加唯一约束
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
City varchar(255),
UNIQUE (P_Id)
)

-- 多个字段 添加唯一约束，命名
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
City varchar(255),
CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
)

-- 添加
ALTER TABLE Persons ADD UNIQUE (P_Id)
-- 删除
ALTER TABLE Persons DROP INDEX uc_PersonID
```

###### 17.PRIMARY KEY

```sql
-- 主键约束
-- 主键必须包含唯一的值。
-- 主键列不能包含 NULL 值。
-- 每个表都应该有一个主键，并且每个表只能有一个主键。

-- 单一主键
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
PRIMARY KEY (P_Id)
)
-- 联合主键，主键命名
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
)

-- 添加
ALTER TABLE Persons ADD PRIMARY KEY (P_Id)
-- 删除
ALTER TABLE Persons DROP PRIMARY KEY
```

###### 18. FOREIGN KEY

```sql
-- 一个表中的 FOREIGN KEY 指向另一个表中的 UNIQUE KEY(唯一约束的键)。(外键，多对一)
-- FOREIGN KEY 约束用于预防破坏表之间连接的行为。
-- 防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一

CREATE TABLE Orders
(
O_Id int NOT NULL,
OrderNo int NOT NULL,
P_Id int,
PRIMARY KEY (O_Id),    -- 主键
FOREIGN KEY (P_Id) REFERENCES Persons(P_Id) -- Persons(P_Id)，另一个表的表名（字段名）
)

-- 命名外键
CREATE TABLE Orders
(
O_Id int NOT NULL,
OrderNo int NOT NULL,
P_Id int,
PRIMARY KEY (O_Id),
CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id)
)

-- 添加
ALTER TABLE Orders ADD FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
-- 添加并命名
ALTER TABLE Orders ADD CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)

-- 删除
ALTER TABLE Orders DROP FOREIGN KEY fk_PerOrders
```

###### 19. CHECK 

```sql
-- CHECK 约束用于限制列中的值的范围。
-- 如果对单个列定义 CHECK 约束，那么该列只允许特定的值。
-- 如果对一个表定义 CHECK 约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。

-- 指定字段范围
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
CHECK (P_Id>0)
)

-- 多字段与命名
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
City varchar(255),
CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
)

-- 添加
ALTER TABLE Persons ADD CHECK (P_Id>0)
-- 添加多个与命名
ALTER TABLE Persons ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')

-- 删除
ALTER TABLE Persons DROP CHECK chk_Person
```

###### 20. DEFAULT

```sql
-- 用于向列中插入默认值。如果没有规定其他的值，那么会将默认值添加到所有的新记录。

-- 默认值
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    City varchar(255) DEFAULT 'Sandnes'
)

-- 默认系统函数
CREATE TABLE Orders
(
    O_Id int NOT NULL,
    OrderDate date DEFAULT GETDATE()
)

-- 添加
ALTER TABLE Persons ALTER City SET DEFAULT 'SANDNES'
-- 删除
ALTER TABLE Persons ALTER City DROP DEFAULT
```

###### 21. CREATE INDEX

```sql
-- 用于在表中创建索引
-- 更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新，仅仅在常常被搜索的列（以及表）上面创建索引。

-- 创建不唯一索引
CREATE INDEX index_name ON table_name (column_name)
-- 创建唯一索引
CREATE UNIQUE INDEX index_name ON table_name (column_name)
-- 创建联合索引
CREATE INDEX PIndex ON Persons (LastName, FirstName)

-- 删除索引
ALTER TABLE table_name DROP INDEX index_name
```

###### 22. DROP

```sql
-- 删除索引、表和数据库。

ALTER TABLE table_name DROP INDEX index_name
DROP TABLE table_name
DROP DATABASE database_name
```

###### 23. ALTER TABLE

```sql
-- 在已有的表中添加、删除或修改列。

-- 添加
ALTER TABLE table_name ADD column_name datatype
-- 删除
ALTER TABLE table_name DROP COLUMN column_name
-- 修改数据类型
ALTER TABLE Persons ALTER COLUMN DateOfBirth year

```

###### 24. AUTO INCREMENT

```sql
-- 每次插入新记录时，自动地创建主键字段的值 （主键自增）
-- AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。
-- 修改 起始值： ALTER TABLE Persons AUTO_INCREMENT=100

-- 主键自增
CREATE TABLE Persons
(
ID int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
PRIMARY KEY (ID)
)

ALTER TABLE table_name CHANGE column_name column_name data_type(size) constraint_name AUTO_INCREMENT;

ALTER TABLE student CHANGE id id INT( 11 ) NOT NULL AUTO_INCREMENT;
```

###### 25.  IS NULL / IS NOT NULL 

```sql
-- 是否为空

SELECT LastName,FirstName FROM Persons WHERE Address IS NULL
SELECT LastName,FirstName FROM Persons WHERE Address IS NOT NULL
```



#### MYSQL 时间函数

###### 1. NOW()

```sql
-- 当前日期 + 时间
SELECT NOW()

2021-03-10 10:08:17
```

###### 2. CURDATE()

```sql
-- 当前日期
SELECT CURDATE()

2021-03-10
```

###### 3. CURTIME()

```sql
-- 当前时间
SELECT CURTIME()

10:13:31
```

###### 4. DATE()

```sql
-- 获取合法日期的 年月日 部分
SELECT DATE(NOW())
SELECT DATE('2021-03-10 10:17:01')

2021-03-10
```

###### 5. EXTRACT() 

```sql
-- 返回日期/时间的单独部分，比如年、月、日、小时、分钟等等
EXTRACT(unit FROM date)
SELECT EXTRACT(DAY FROM NOW())
SELECT EXTRACT(DAY FROM '2021-03-10 10:41:58')

10

MICROSECOND
SECOND
MINUTE
HOUR
DAY
WEEK
MONTH
QUARTER
YEAR
SECOND_MICROSECOND
MINUTE_MICROSECOND
MINUTE_SECOND
HOUR_MICROSECOND
HOUR_SECOND
HOUR_MINUTE
DAY_MICROSECOND
DAY_SECOND
DAY_MINUTE
DAY_HOUR
YEAR_MONTH
```

###### 6. DATE_ADD()

```sql
-- 向日期添加指定的时间间隔。
DATE_ADD(date,INTERVAL expr type)
SELECT DATE_ADD('2021-03-10 10:41:58',INTERVAL 10 DAY)  -- 正向
SELECT DATE_ADD('2021-03-10 10:41:58',INTERVAL -10 DAY)	-- 负向

2021-03-20 10:41:58

MICROSECOND
SECOND
MINUTE
HOUR
DAY
WEEK
MONTH
QUARTER
YEAR
SECOND_MICROSECOND
MINUTE_MICROSECOND
MINUTE_SECOND
HOUR_MICROSECOND
HOUR_SECOND
HOUR_MINUTE
DAY_MICROSECOND
DAY_SECOND
DAY_MINUTE
DAY_HOUR
YEAR_MONTH
```

###### 7. DATE_SUB()

```sql
-- 从日期减去指定的时间间隔
DATE_SUB(date,INTERVAL expr type)
SELECT DATE_SUB('2021-03-10',INTERVAL -10 DAY) -- 未来时间
SELECT DATE_SUB('2021-03-10',INTERVAL 10 DAY)  -- 过去时间
```

###### 8. DATEDIFF()

```sql
-- 返回两个日期之间的天数 (只比较日期)（date1 - date2）
DATEDIFF(date1,date2)
SELECT DATEDIFF('2021-03-10 00:00:01','2021-03-09 23:59:59')

1
```

###### 9. DATE_FORMAT()

```sql
-- 以不同的格式显示日期/时间数据。格式化日期/时间
DATE_FORMAT(date,format)
SELECT DATE_FORMAT(NOW(),'%Y--%m--%d %H::%i::%s')

2021--03--10 11::16::28

```

| 格式 | 描述                                           |
| :--- | :--------------------------------------------- |
| %a   | 缩写星期名                                     |
| %b   | 缩写月名                                       |
| %c   | 月，数值                                       |
| %D   | 带有英文前缀的月中的天                         |
| %d   | 月的天，数值（00-31）                          |
| %e   | 月的天，数值（0-31）                           |
| %f   | 微秒                                           |
| %H   | 小时（00-23）                                  |
| %h   | 小时（01-12）                                  |
| %I   | 小时（01-12）                                  |
| %i   | 分钟，数值（00-59）                            |
| %j   | 年的天（001-366）                              |
| %k   | 小时（0-23）                                   |
| %l   | 小时（1-12）                                   |
| %M   | 月名                                           |
| %m   | 月，数值（00-12）                              |
| %p   | AM 或 PM                                       |
| %r   | 时间，12-小时（hh:mm:ss AM 或 PM）             |
| %S   | 秒（00-59）                                    |
| %s   | 秒（00-59）                                    |
| %T   | 时间, 24-小时（hh:mm:ss）                      |
| %U   | 周（00-53）星期日是一周的第一天                |
| %u   | 周（00-53）星期一是一周的第一天                |
| %V   | 周（01-53）星期日是一周的第一天，与 %X 使用    |
| %v   | 周（01-53）星期一是一周的第一天，与 %x 使用    |
| %W   | 星期名                                         |
| %w   | 周的天（0=星期日, 6=星期六）                   |
| %X   | 年，其中的星期日是周的第一天，4 位，与 %V 使用 |
| %x   | 年，其中的星期一是周的第一天，4 位，与 %v 使用 |
| %Y   | 年，4 位                                       |
| %y   | 年，2 位                                       |



#### SQL Aggregate 函数（聚合函数）

 函数计算从列中取得的值，返回一个单一的值

###### 1. AVG()

```sql
-- 返回列的平均值
SELECT AVG(column_name) FROM table_name

```

###### 2. COUNT()

```sql
-- COUNT() 函数返回匹配指定条件的行数。
SELECT COUNT(column_name) FROM table_name; -- NULL 不计入
SELECT COUNT(*) FROM table_name;			-- 所有

```

###### 3. FIRST()

```sql
-- 返回指定的列中第一个记录的值。 
SELECT FIRST(column_name) FROM table_name;
-- 只有 MS Access 支持 FIRST() 函数。

```

###### 4. LAST()

```sql
-- 返回指定的列中最后一个记录的值。
SELECT LAST(column_name) FROM table_name;
-- 只有 MS Access 支持 LAST() 函数。

```

###### 5. MAX()

```sql
-- 返回指定列的最大值。
SELECT MAX(column_name) FROM table_name;

```

###### 6. MIN()

```sql
-- 返回指定列的最小值。
SELECT MIN(column_name) FROM table_name;

```

###### 7. SUM()

```sql
-- 返回数值列的总数。
SELECT SUM(column_name) FROM table_name;

```

#### 其他

###### 8. GROUP BY

```sql
-- 根据一个或多个列对结果集进行分组。
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;

-- 多表联查 分组
SELECT Websites.name,COUNT(access_log.aid) AS nums FROM access_log
LEFT JOIN Websites
ON access_log.site_id=Websites.id
GROUP BY Websites.name;

```

###### 9. HAVING

```sql
-- WHERE 关键字无法与聚合函数一起使用。HAVING 可以

```





#### SQL Scalar 函数

数基于输入值，返回一个单一的值。

###### 1. UCASE()

```sql
-- 把字段的值转换为大写。
SELECT UCASE(column_name) FROM table_name;

```

###### 2. LCASE()

```sql
-- 把字段的值转换为小写。
SELECT LCASE(column_name) FROM table_name;
```

###### 3. MID()

```sql
-- 用于从文本字段中提取字符。(截取字符串)
SELECT MID(column_name,start[,length]) FROM table_name;
-- column_name	必需。要提取字符的字段。
-- start	必需。规定开始位置（起始值是 1）。
-- length	可选。要返回的字符数。如果省略，则 MID() 函数返回剩余文本。
SELECT MID(name,1,4) AS ShortTitle FROM Websites;

```

###### 4. LEN()

```sql
-- 返回文本字段中值的长度。(字符的长度)
SELECT LEN(column_name) FROM table_name;

-- mysql
SELECT LENGTH(column_name) FROM table_name;

```

###### 5. ROUND()

```sql
-- 用于把数值字段舍入为指定的小数位数。(四舍五入)
SELECT ROUND(column_name,decimals) FROM table_name;
select ROUND(-1.23); -- -1
select ROUND(1.298, 1); -- 1.3

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

##### 2. MySQL 数据库日期类型字段值的格式

```apl
DATE - 格式：YYYY-MM-DD
DATETIME - 格式：YYYY-MM-DD HH:MM:SS
TIMESTAMP - 格式：YYYY-MM-DD HH:MM:SS
YEAR - 格式：YYYY 或 YY
```

