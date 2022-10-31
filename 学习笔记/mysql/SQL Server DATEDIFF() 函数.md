# SQL Server DATEDIFF() 函数

------

[![SQL Dates](https://www.runoob.com/images/up.gif) SQL Server Date 函数](https://www.runoob.com/sql/sql-dates.html)

------

## 定义和用法

DATEDIFF() 函数返回两个日期之间的天数。

### **语法**

DATEDIFF(datepart,startdate,enddate)

startdate 和 enddate 参数是合法的日期表达式。datepart 参数可以是下列的值：

| datepart | 缩写     |
| :------- | :------- |
| 年       | yy, yyyy |
| 季度     | qq, q    |
| 月       | mm, m    |
| 年中的日 | dy, y    |
| 日       | dd, d    |
| 周       | wk, ww   |
| 星期     | dw, w    |
| 小时     | hh       |
| 分钟     | mi, n    |
| 秒       | ss, s    |
| 毫秒     | ms       |
| 微妙     | mcs      |
| 纳秒     | ns       |