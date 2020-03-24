---
title: SQL查询写法总结
date: 2020-02-23 21:38:49
tags: ["technology","mysql"]
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/library.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/library.jpg
---
## 前言
SQL应该不用再多做介绍了。如果没听说过不了解SQL，可以看这个W3School教程[https://www.w3schools.com/sql/sql_intro.asp](https://www.w3schools.com/sql/sql_intro.asp). 本文和大多数教程不一样，不阐述、不解释、不运行。只对SQL的用法作总结罗列和示例。方便记忆和快速查询。

## Select
```
SELECT column1, column2,... FROM table_name;
SELECT * FROM table_name;
```

## Select Distinct
```
SELECT DISTINCT column1, column2,... FROM table_name;
```

## Where
```
SELECT column1, column2,... FROM table_name WHERE condition;

SELECT * FROM Customers WHERE Country='Mexico';
```
Where从句中的Operator有 =,>,<,>=,<=,<>(或!=),BETWEEN,LIKE,IN

## And, Or, Not
```
SELECT column1, column2,... FROM table_name WHERE condition1 AND condition2 AND condition3 ...;
SELECT column1, column2,... FROM table_name WHERE condition1 OR condition2 OR condition3 ...;
SELECT column1, column2,... FROM table_name WHERE NOT condition;

SELECT * FROM Customers WHERE Country='Germany' AND City='Berlin';
SELECT * FROM Customers WHERE City='Berlin' OR City='Munchen';
SELECT * FROM Customers WHERE NOT Country='Germany';
```

## Order By
```
SELECT column1, column2,... FROM table_name ORDER BY column1, column2, ... ASC|DESC;

SELECT * FROM Customers ORDER BY Country,CustomerName;
SELECT * FROM Customers ORDER BY Country DESC;
```

## Insert Into
```
INSERT INTO table_name(column1,column2,column3,...) VALUES(value1,value2,value3,...);
INSERT INTO table_name VALUES(value1,value2,value3,...);
```

## NULL值
```
SELECT column_names FROM table_name WHERE column_name IS NULL;
SELECT column_names FROM table_name WHERE column_name IS NOT NULL;
```

## Update
```
UPDATE table_name SET column1=value1, column2=value2,... WHERE condition;

UPDATE Customers SET ContactName='Juan' WHERE Country='Mexico';
```

## Delete
```
DELETE FROM table_name WHERE condition;

DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
DELETE FROM Customers;
```

## Select Limit
```
SELECT column_names FROM table_name WHERE condition LIMIT number;
```

## Min and Max
```
SELECT MIN(column_name) FROM table_name WHERE condition;
SELECT MAX(column_name) FROM table_name WHERE condition;

SELECT MIN(Price) as SmallestPrice FROM Products;
SELECT MAX(Price) as LargestPrice FROM Products;
```

## Count, Avg, Sum
> NULL值不被COUNT, 也不被AVG和SUM计算。
```
SELECT COUNT(column_name) FROM table_name WHERE condition;
SELECT AVG(column_name) FROM table_name WHERE condition;
SELECT SUM(column_name) FROM table_name WHERE condition;

SELECT COUNT(ProductID) FROM Products;
SELECT AVG(Price) FROM Products;
SELECT SUM(Quantity) FROM OrderDetails;
```

## Like
> % 代表0个、1个或多个字符
> _ 代表1个字符
```
SELECT column1, column2,... FROM table_name WHERE columnN LIKE pattern;

SELECT * FROM Customers WHERE CustomerName LIKE '%or%';
```

## In
> IN操作符可以看作是OR conditions的简写。
```
SELECT column_names FROM table_name WHERE column_name IN (value1,value2,...);
SELECT column_names FROM table_name WHERE column_name IN (SELECT STATEMENT);

SELECT * FROM Customers WHERE Country NOT IN ('Germany','France','UK');
SELECT * FROM Customers WHERE Country IN (SELECT Country FROM Suppliers);
```

## Between
> 选出一个范围内的值，这个值可以是数字、文本或日期。
```
SELECT column_names FROM table_name WHERE column_name BETWEEN value1 AND value2;

SELECT * FROM Products WHERE Price BETWEEN 10 AND 20 AND CategoryID NOT IN (1,2,3);
SELECT * FROM Products WHERE ProductName NOT BETWEEN 'Carnarvon Tigers' AND 'Mozzarella di Giovanni' ORDER BY ProductName;
SELECT * FROM Orders WHERE OrderDate BETWEEN '1996-07-01' AND '1996-07-31';
```

## Aliases
```
SELECT column_name AS alias_name FROM table_name;
SELECT column_names FROM table_name AS alias_name;

SELECT CustomerName, CONCAT(Address,', ',PostalCode,', ',City,', ',Country) AS Address FROM Customers;
SELECT o.OrderID,o.OrderDate,c.CustomerName FROM Customers AS c, Orders AS o WHERE c.CustomerName="Around the Horn" AND c.CustomerID=o.CustomerID;
```

## Joins
> INNER JOIN 选出两个表里都匹配的值
```
SELECT column_names FROM table1 INNER JOIN table2 ON table1.column_name=table2.column_name;

SELECT Orders.OrderID, Customers.CustomerName FROM Orders INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
SELECT Orders.OrderID, Customers.CustomerName, Shippers.ShipperName FROM ((Orders INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID) INNER JOIN Shippers ON Orders.ShipperID=Shippers.ShipperID);
```
> LEFT JOIN 选出左边表里所有记录和右边表里的匹配记录，右边如果没有匹配则显示NULL。
```
SELECT column_names FROM table1 LEFT JOIN table2 ON table1.column_name=table2.column_name;

SELECT Customers.CustomerName, Orders.OrderID FROM Customers LEFT JOIN Orders ON Customers.CustomerID=Orders.CustomerID ORDER BY Customers.CustomerName;
```
> RIGHT JOIN 选出右边表里的所有记录和左边表里的匹配记录，左边如果没有匹配则显示NULL。
```
SELECT column_names FROM table1 RIGHT JOIN table2 ON table1.column_name=table2.column_name;

SELECT Orders.OrderID, Employees.LastName, Employees.FirstName FROM Orders RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID ORDER BY Orders.OrderID;
```
> FULL JOIN 即 FULL OUTER JOIN,选出两个表里匹配的所有记录，注意这个返回的结果集会非常大。
```
SELECT column_names FROM table1 FULL OUTER JOIN table2 ON table1.column_name=table2.column_name WHERE condition;
```
> SELF JOIN,table和自己join. 如示例选出在同一城市里的customers.
```
SELECT column_names FROM table1 T1, table1 T2 WHERE condition;

SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City FROM Customers A, Customers B WHERE A.CustomerID<>B.CustomerID AND A.City=B.City ORDER BY A.City;
```

## Union
> UNION用来合并结果集，两个结果集必须1.有相同的列数；2.列的数据类型相似；3.列的顺序一致
> UNION只选出不重复值，UNION可以选出重复值。
```
SELECT column_names FROM table1 UNION SELECT column_names FROM table2;

SELECT City FROM Customers UNION SELECT City FROM Suppliers ORDER BY City;
SELECT City FROM Customers UNION ALL SELECT City FROM Suppliers ORDER BY City;
SELECT City,Country FROM Customers WHERE Country='Germany' UNION SELECT City,Country FROM Suppliers WHERE Country='Germany' ORDER BY City;
```

## Group By
> GROUP BY通常和聚合函数(COUNT,MAX,MIN,SUM,AVG)一起使用
```
SELECT column_names FROM table_name WHERE condition GROUP BY column_names ORDER BY column_names;

SELECT COUNT(CustomerID), Country FROM Customers GROUP BY Country;
SELECT COUNT(CustomerID), Country FROM Customers GROUP BY Country ORDER BY COUNT(CustomerID) DESC;
SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders FROM Orders LEFT JOIN Shippers ON Orders.ShipperID=Shippers.ShipperID GROUP BY ShipperName;
```
> 最后这条列出了每个快递商运送的订单数。

## Having
> WHERE关键字不能用于聚合函数，此时用HAVING
```
SELECT column_names FROM table_name WHERE condition GROUP BY column_names HAVING condition ORDER BY column_name;

SELECT COUNT(CustomerID),Country FROM Customers GROUP BY Country HAVING COUNT(CustomerID)>5;
SELECT COUNT(CustomerID),Country FROM Customers GROUP BY Country HAVING COUNT(CustomerID)>5 ORDER BY COUNT(CustomerID) DESC;
SELECT Employees.LastName,COUNT(Orders.OrderID) AS NumberOfOrders FROM (Orders INNER JOIN Employees ON Orders.EmployeeID=Employees.EmployeeID) GROUP BY LastName HAVING COUNT(Orders.OrderID)>10;
```
> 最后两个示例，一为列出每个国家的customer数，这个数字须大于5；二为列出employee中有超过10个订单的人的姓和订单数。

## Exists
```
SELECT column_names FROM table_name WHERE EXISTS (SELECT column_name FROM table_name WHERE condition);

SELECT SupplierName FROM Suppliers WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID=Suppliers.SupplierID AND Price<20);
```
> 示例为列出有产品价少于20的供应商

## Any, All
```
SELECT column_names FROM table_name WHERE column_name OPERATOR ANY (SELECT column_name FROM table_name WHERE condition);
SELECT column_names FROM table_name WHERE column_name OPERATOR ALL (SELECT column_name FROM table_name WHERE condition);

SELECT ProductName FROM Products WHERE ProductID = ANY (SELECT ProductID FROM OrderDetails WHERE Quantity=10);
SELECT ProductName FROM Products WHERE ProductID = ANY (SELECT ProductID FROM OrderDetails WHERE Quantity>99);
SELECT ProductName FROM Products WHERE ProductID = ALL (SELECT ProductID FROM OrderDetails WHERE Quantity=10);
```
> 这里的OPERATOR必须是比较操作符 (=,<>,!=,>,>=,<,<=)
> 第二个示例为列出产品名字，它在OrderDetails表中有一个记录quantity>99

## Comments
```
--SELECT * FROM Customers;
SELECT * FROM Products;

SELECT * From Customers -- WHERE City='Berlin';
```

## Select Into <略>

## Insert Into Select <略>

## Case <略>

## IFNULL <略>

## Stored Procedures <略>
