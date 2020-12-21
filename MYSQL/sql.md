# SQL

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

    ：SQL Server  number数量 percent百分数
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