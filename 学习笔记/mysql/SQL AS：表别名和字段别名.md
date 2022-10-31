SQL AS 关键字用于为表或字段起一个临时的别名。别名是临时的，它仅在当前 SQL 语句中奏效，数据库中的实际表名和字段名不会更改。

SELECT 命令的结果集中将显示别名，而不是原始名。

数据库中as主要作用是起别名，常规来说**都可以省略**，但是为了增加可读性，不建议省略。

通常在下列情况中使用别名：

- 有两个名字重复的表，需要为其中一个表起一个别名加以区分，比如 [SELF JOIN](http://c.biancheng.net/sql/self-join.html)。
- 两个表中有重复的字段名，起别名加以区分。
- 表名/字段名较长，或者可读性差。

## 语法

表别名的基本语法如下：

```sql
SELECT column1, column2....
FROM table_name AS alias_name
WHERE [condition];
```


字段别名的基本语法如下：

```sql
SELECT column_name AS alias_name
FROM table_name
WHERE [condition];
```

## 示例

现在有以下两个表，分别是客户表和订单表。

表1：CUSTOMERS 表

```
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |
|  3 | kaushik  |  23 | Kota      |  2000.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | MP        |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+
```


表2：ORDERS 表

```
+-----+---------------------+-------------+--------+
|OID  | DATE                | CUSTOMER_ID | AMOUNT |
+-----+---------------------+-------------+--------+
| 102 | 2009-10-08 00:00:00 |           3 |   3000 |
| 100 | 2009-10-08 00:00:00 |           3 |   1500 |
| 101 | 2009-11-20 00:00:00 |           2 |   1560 |
| 103 | 2008-05-20 00:00:00 |           4 |   2060 |
+-----+---------------------+-------------+--------+
```


\1) 为表起一个别名，代码如下：

```sql
SQL> SELECT C.ID, C.NAME, C.AGE, O.AMOUNT
     FROM CUSTOMERS AS C, ORDERS AS O
     WHERE  C.ID = O.CUSTOMER_ID;
```

执行结果：

```
+----+----------+-----+--------+
| ID | NAME     | AGE | AMOUNT |
+----+----------+-----+--------+
|  3 | kaushik  |  23 |   3000 |
|  3 | kaushik  |  23 |   1500 |
|  2 | Khilan   |  25 |   1560 |
|  4 | Chaitali |  25 |   2060 |
+----+----------+-----+--------+
```


\2) 为字段起一个别名，代码如下：

```sql
SQL> SELECT  ID AS CUSTOMER_ID, NAME AS CUSTOMER_NAME
     FROM CUSTOMERS
     WHERE SALARY IS NOT NULL;
```

执行结果：

```
+-------------+---------------+
| CUSTOMER_ID | CUSTOMER_NAME |
+-------------+---------------+
|           1 | Ramesh        |
|           2 | Khilan        |
|           3 | kaushik       |
|           4 | Chaitali      |
|           5 | Hardik        |
|           6 | Komal         |
|           7 | Muffy         |
+-------------+---------------+
```