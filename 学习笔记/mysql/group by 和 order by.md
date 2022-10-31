## group by

group by 只是查询结果去重，并不是真的删掉了重复的记录

## order by

1、order by：进行排序操作，根据某个字段进行升序或者降序，用于对结果集进行排序。依赖校对集。
（1）基本语法：order by 字段名[asc 或者 desc]；-- 默认是asc升序
（2）SQL查询语句：having子句、order by子句，和where字句一样，是用来进行条件判断的。
Where是针对磁盘数据进行判断，进入到内存之后，会进行分组操作，分组结果就需要Having来处理。
有这么一个结论，having能做where能做的几乎所有事情，但是where却不能做having能做的事情。Order by主要就是用来排序操作。
参考博客： sql查询语句order by和 having_weixin_38023156的博客-CSDN博客_orderby与having

2、limit m,n：表示从第m+1条开始，取n条数据；
limit n : 表示从第0条开始，取n条数据，是limit(0,n)的缩写。

SQL Server实现Limit语句：
MySQL有Limit语句，而SQL Server中没有。如何在SQL Server中实现类似的功能？以下方法作为参考：

（1）用TOP子句实现：
top子句的用法如下：

```
/*从table_表中取出前m条记录*/
/*m是从1开始的。m=1时，仅取出一条记录*/
select top m * from table_
```


top子句是在查询完成后才发生作用的，不是用它限定范围然后才查询。有了top子句，如何选择一定范围内的记录？例如，选择第6-10条的记录，可以先选择前5条的记录，然后选择不在这5条记录中的记录，再选择这些记录的前5条。

```
/*a为表名称，a.id为a表的主键*/
select top 5 * from a
where a.id not in (
   select top 5 id from a 
)
```


由此，选择第m到n条记录的语句为

```
select top (n-m+1) * from a
where a.id not in (
   select top m id from a
)

```

当然还可以使用where子句进行条件查询。这种方式实际上需要进行两次查询，而且需要知道查询的记录的标识列。使用起来较为麻烦。

（2）用ROW_NUMBER()加where子句实现
row_number()函数获取行号，我们可以使用where子句选择行号在一定范围内的记录。在使用row_number()的过程中需要对查询结果排序。代码如下：

```
/*这里按照getdate()排序，当然也可也改成按照其它顺序，如列等排序*/
SELECT * FROM
(
SELECT ROW_NUMBER()over(order by getdate()) AS rownumber,* from a
) AS #a
WHERE #a.rownum>=m AND #a.rownum<=n
```


在以上的语句中，选择ROW_NUMBER（）（其别名为rownumber）和需要查询的内容，将其作为临时表#a,再选择rownumber在一定范围内的记录。
在这种方式下，只进行一次查询，然后在所有查询结果的临时表中选择记录。且不需要知道查询的记录的标识列。推荐使用这种方法，因为它较为简单也易于使用。
当有一个查询为select …，我们需要查询其结果的第m-n条记录时，只需要将其第一个“select”去掉，加入以下语句的相应位置即可：

```
SELECT * FROM
(
SELECT ROW_NUMBER()over(order by getdate()) AS rownumber,/*这里是加入去掉第一个"select"后的查询语句的地方*/
) AS #a
WHERE #a.rownum>=m AND #a.rownum<=n

```

