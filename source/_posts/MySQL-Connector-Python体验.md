---
title: MySQL Connector/Python体验
tags:
  - mysql
  - python
index_img: 'http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/connector.jpg'
banner_img: 'http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/connector.jpg'
date: 2020-02-26 23:24:41
---

```
import mysql.connector 

config = {
    'user': 'root',
    'password': '123456',
    'host': 'cirray.cn',
    'port': '3306',
    'database': 'db_test'
}

cnx = mysql.connector.connect(**config)
cur = cnx.cursor(buffered=True)
cur.execute("select * from tb_user limit 5")
print(cur.fetchall())
cur.close()
cnx.close()
```
## 前言
直接先上一段代码。根据配置连接MySQL Server,执行查询,打印结果,关闭连接。这段代码很简单，这就是MySQL Connector的初体验。我实在不想写SQLAlchemy了，不想写的云里雾里，我只想回到单纯写SQL的快感！

## pip安装
安装之前确保本地装了MySQL(client)。
```
(venv) $ pip install mysql-connector-python
```

## Create Database
```
>>> import mysql.connector 
>>> 
>>> config = {
...     'user': 'root',
...     'password': '123456',
...     'host': 'cirray.cn',
...     'port': '3306',
...     'database': 'db_test'
... }
>>> 
>>> cnx = mysql.connector.connect(**config)
>>> cur = cnx.cursor(buffered=True)
>>> cur.execute("SHOW DATABASES")
>>> for x in cur:
...     print(x)
... 
('information_schema',)
('db_jds',)
('db_jds_test',)
('mysql',)
('performance_schema',)
('sys',)
>>> cur.execute("CREATE DATABASE mydatabase")
```

## Create Table
```
cur.execute("CREATE TABLE customers (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(255), address VARCHAR(255))")

cur.execute("ALTER TABLE customers ADD COLUMN id INT AUTO_INCREMENT PRIMARY KEY")
```

## Insert 
```
sql = "INSERT INTO customers (name, address) VALUES (%s, %s)"
val = ("John", "Highway 21")
cur.execute(sql,val)

# 注意语句 cnx.commit(). 做更改必须要有commit().
cnx.commit()
print(cur.rowcount,"record inserted.")
print("The inserted ID:", cur.lastrowid)

sql = "INSERT INTO customers (name, address) VALUES (%s, %s)"
val = [
  ('Peter', 'Lowstreet 4'),
  ('Amy', 'Apple st 652'),
  ('Hannah', 'Mountain 21'),
  ('Michael', 'Valley 345')
]
cur.executemany(sql,val)
cnx.commit()
print(cur.rowcount, "was inserted.")
```

## Select
> cur.fetchall()方法可以拿到最后执行语句的所有行(记录)
> cur.fetchone()方法则只返回结果的第一行
```
cur.execute("SELECT name,address FROM customers")
result = cur.fetchall()
print(result)

cur.execute("SELECT * FROM customers")
result = cur.fetchone()
print(result)
```

## Where
```
sql = "SELECT * FROM customers WHERE address LIKE '%way%'"
cur.execute(sql)
result = cur.fetchall()
print(result)
```

## Delete
```
sql = "DELETE FROM customers WHERE address='Mauntain 21'"
cur.execute(sql)
cnx.commit()
print(cur.rowcount,"records deleted")
```

## Update
```
sql = "UPDATE customers SET address='Canyon 123' WHERE address='Valley 345'"
cur.execute(sql)
cnx.commit()
print(cur.rowcount,"records affected")
```

## Limit
```
cur.execute("SELECT * FROM tb_user LIMIT 5 OFFSET 10")
result = cur.fetchall()
print(result)
```

## Join
> JOIN == INNER JOIN
```
sql = "SELECT users.name AS user, products.name AS favorite FROM users INNER JOIN products ON users.fav=products.id"
cur.execute(sql)
result = cur.fetchall()
print(result)
```

