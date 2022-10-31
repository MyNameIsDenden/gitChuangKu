# [SQL语句中 NOT IN 子句的“正确打开方式”](https://www.cnblogs.com/robothy/p/7373798.html)

在写SQL语句的时候，若where条件是判断用户不在某个集合当中，我们习惯使用 where 列名 not in (集合) 子句，这种写法本身没有问题，但实践过程中却发现很多人在写类似的SQL语句时，写的代码存在隐患，而这种隐患往往难以发现。

 

**1. 存在隐患的写法**

首先,我们来评估一条简单的SQL语句的输出结果。语句如下：

```
select 1 from dual where 1 not in(2, null)
```

简单，输出结果是1嘛。

错！答案是没有输出结果。

为什么会这样？数据库管理系统在执行查询之前，会对上面语句进行简单的转化，转化之后的语句如下：

```
select 1 from dual where 1 != 2 and 1 != null
```

显然，where后面的表达式中，**1!=2**的结果是**true**,但**1!=null**的结果是**不确定**，**true** 和 **不确定**进行与运算的结果**非true**,所以上述语句的没有输出结果。

分析完了上面这条语句，我们再来看一个问题，假设有员工表DEMPLOYEES和部门表DEPARTMENTS,写SQL语句找出2015年没有招人的部门。我们可以很快的写出下面语句：

```
select DEPARTMENT_ID, DEPARTMENT_NAME from DEPARTMENTS where DEPARTMENT_ID not in(
    select DEPARTMENT_ID from EMPLOYEES where HIRE_DATE like '%2015%'
)
```

若员工表EMPLOYEES中存在还未分配部门的员工时，子查询中的结果集中会含NULL元素，这就和开始我们评估的语句套上了，语句将无结果返回。那么，该如何写这种形式的语句才能避免隐患呢？

**2. NOT IN 子句正确的打开方式**

为了避免**NOT IN**子句的隐患，最简单的方式就是不使用NOT IN子句，而使用其他的形式达到相同的效果。你老是出问题，我不跟你玩行了吧！例如使用 not exists子句将上述语句改写正确之后代码如下：

```
select DEPARTMENT_ID, DEPARTMENT_NAME from DEPARTMENTS where not exists (
    select * from EMPLOYEES where EMPLOYEES.DEPARTMENT_ID = DEPARTMENTS.DEPARTMENT_ID and  HIRE_DATE like '%2015%'
)
```

若还是想用 NOT IN子句，怎么办？有方法，就是对子查询返回的结果进行处理，如果为空值NULL，则给它赋一个非空值。这种对空值NULL的处理，各类数据库管理系统都有相应函数函数支持，例如SQL Server中的 **ISNULL**函数，Oracle 中的**NVL**，**NVL2**，**COALESE**，以及MySQL中的**IFNULL** ， **COALESE**。

以SQL Server为例，如下SQL语句也能够像NOT EXISTS子句一样避免隐患。

```
select DEPARTMENT_ID, DEPARTMENT_NAME from DEPARTMENTS where DEPARTMENT_ID != all(
    select ISNULL(DEPARTMENT_ID,'') from EMPLOYEES where HIRE_DATE like '%2015%'
)
```

**3. 小结**

在使用NOT IN子句时，若子查询结果集中可能包含空值NULL，则代码存在隐患，要消除这种隐患，应该对子查询结果集中的空值进行处理。

 

**附：**基于SQL Server的完整实验代码

假设公司ERP系统中有两个表，员工表EMPLOYEES和部门表DEPARTMENTS（如下所示），老板要找出2015年没有招人的部门号和部门名称。请写出查询的SQL语句。

**员工表 EMPLOYEES**

| EMPLOYEE_ID | EMPLOYEE_NAME | HIRE_DATE  | DEPARTMENT_ID |
| ----------- | ------------- | ---------- | ------------- |
| 170101      | Bob           | 2016-02-02 | 001           |
| 170102      | Alice         | 2015-02-05 | 003           |
| 170103      | Tony          | 2016-03-04 | 002           |
| 170105      | Aaron         | 2016-08-03 | 002           |
| 170107      | Rex           | 2016-10-11 | NULL          |

**部门表 DEPARTMENTS**

| DEPARTMENT_ID | DEPARTMENT_NAME | MANAGER_ID |
| ------------- | --------------- | ---------- |
| 001           | Administration  | 170101     |
| 002           | IT              | 170103     |
| 003           | Finance         | 170102     |

创建表SQL语句。

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)展开代码

 上述题目，我们很容易产生这样的思路，先在员工表 EMPLOYEES 中找出2015年雇佣的员工所在的部门号，作为一个部门号子集合，然后在部门表 DEPARTMENTS 中找出不在该子集中的部门号和部门ID，即为要查找的结果。于是有了如下查询SQL语句：

```
select DEPARTMENT_ID, DEPARTMENT_NAME from DEPARTMENTS where DEPARTMENT_ID not in(
    select DEPARTMENT_ID from EMPLOYEES where HIRE_DATE like '%2015%'
)
```

然而，由于子查询中存在一个空值，所以SQL Server数据库管理系统执行上述语句之后将返回0条结果。而实际上我们可以从上表中看到，2015年没有招人的部门号和部门名称有{001, Administration } 部门，故上面的查询语句存在BUG。

为保证查询出我们期望的结果，这里使用SQL Server的**ISNULL**函数对子查询中的空值进行处理，处理之后SQL语句为：

```
select DEPARTMENT_ID, DEPARTMENT_NAME from DEPARTMENTS where DEPARTMENT_ID not in(
    select ISNULL(DEPARTMENT_ID,'') from EMPLOYEES where HIRE_DATE like '%2015%'
)
```

SQL Server DBMS将返回给我们期望的结果：

```
DEPARTMENT_ID    DEPARTMENT_NAME
001    　　　　　　Administration
002              IT
```