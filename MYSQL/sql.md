# SQL



##### 简介

- SQL 指结构化查询语言，全称是 Structured Query Language。
- RDBMS: 关系数据库管理系统（Relational Database Management System）是指包括相互联系的逻辑组织和存取这些数据的一套程序 (数据库管理系统软件)。关系数据库管理系统就是管理关系数据库，并将数据逻辑组织的系统。
- SQL 用于 管理 RDBMS
- SQL 的范围包括表数据插入、查询、更新和删除，数据库模式创建和修改，以及数据访问控制。

##### RDBMS

- RDBMS中的数据存储在数据库对象称为表。表由列和行的组成。
- 列就是数据的字段,行就是每一条数据

```
id		name		age

1		li			10
2		wang		11
3		zhang		12
```

- 约束是对表执行对数据的列的规则，这些约束用于限制数据的类型进入表中，并且确保数据库中的数据的准确性和可靠性。约束它可能是列级或表级，列级约束仅应用于一列，表级约束应用于整个表。

### 数据库操作

##### 创建

```sql
CREATE DATABASE database-name 
```

##### 删除

```sql
drop database dbname
```

##### 修改

```sql
ALTER DATABASE
```

##### 选择使用的数据库

```sql
use dbname
```





### like 模糊查询
```sql
'%b'
'b%'
'%b%'
```

### limit top rownum  查询前num 条
```sql
limit Mysql
top 
rownum Oracle

SQL Server  number数量 percent百分数
SELECT TOP number|percent column_name(s) FROM table_name       
    ：MySQL
SELECT column_name(s) FROM table_name LIMIT number   
    ：Oracle
SELECT column_name(s) FROM table_name WHERE ROWNUM <= number                      
```

### in 
```sql
SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,...)
```

### between and
```mysql
SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2;
    : not between
SELECT * FROM Websites WHERE name NOT BETWEEN 'A' AND 'H';                      
    : between 与 in
SELECT * FROM Websites WHERE (alexa BETWEEN 1 AND 20) AND country NOT IN ('USA', 'IND');
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