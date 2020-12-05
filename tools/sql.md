# SQL















## General

### 1. SELECT

```
// Select 查询语法
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

- 条件查询

- - = < <= > >= != <>
  - BETWEEN ... AND ...
  - NOT BETWEEN ... AND ...
  - LIKE
  - NOT LIKE
  - %
  - _
  - IN (...)
  - NOT IN (...)
  - 注意字符串数据用引号



| **COUNT(*****)**, **COUNT(***column***)** | 计数！COUNT(*) 统计数据行数，COUNT(column) 统计column非NULL的行数. |
| ----------------------------------------- | ------------------------------------------------------------ |
| **MIN(***column***)**                     | 找column最小的一行.                                          |
| **MAX(***column***)**                     | 找column最大的一行.                                          |
| **AVG(***column*)                         | 对column所有行取平均值.                                      |
| **SUM(***column***)**                     | 对column所有行求和.                                          |





```
// 选取出唯一的结果的语法
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s);

// GROUP BY

// 结果排序（ordered results）
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;

// limited查询
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC, column2 ASC/DESC, ...
LIMIT num_limit OFFSET num_offset;

// 用INNER JOIN 连接表的语法
SELECT column, another_table_column AS column2, …
FROM mytable （主表） AS 
INNER JOIN another_table （要连接的表）
    ON mytable.id = another_table.id (想象一下刚才讲的主键连接，两个相同的连成1条)
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
GROUP BY column;
HAVING group_condition;
```







role_number() OVER(ORDER BY xxx)

rank()

ntile()





SELECT year,

(COUNT(name) * 100 / (SELECT COUNT(*) FROM employees))

 AS rate FROM

(SELECT 1 AS year UNION SELECT 3 UNION SELECT 5 UNION SELECT 7)

LEFT JOIN employees ON year = years_employed

GROUP BY years_employed

ORDER BY year;



SELECT role,

CAST(years_employed / 3 * 3 AS VARCHAR(10)) || "-" ||

CAST(years_employed / 3 * 3 + 3  AS VARCHAR(10))

AS year,

COUNT(*) AS total

FROM employees

GROUP BY role, year

ORDER BY role, year



SELECT title,

-(domestic_sales + international_sales - (

SELECT MAX(domestic_sales + international_sales) FROM movies

JOIN boxoffice ON movies.id = boxoffice.movie_id

)) AS gap

FROM movies

JOIN boxoffice ON movies.id = boxoffice.movie_id

ORDER BY gap DESC;





#### 查询执行顺序

1. FROM 和 JOINs

FROM 或 JOIN会第一个执行，确定一个整体的数据范围. 如果要JOIN不同表，可能会生成一个临时Table来用于 下面的过程。总之第一步可以简单理解为确定一个数据源表（含临时表)



2. WHERE

我们确定了数据来源 WHERE 语句就将在这个数据源中按要求进行数据筛选，并丢弃不符合要求的数据行，所有的筛选col属性 只能来自FROM圈定的表. AS别名还不能在这个阶段使用，因为可能别名是一个还没执行的表达式



3. GROUP BY

如果你用了 GROUP BY 分组，那GROUP BY 将对之前的数据进行分组，统计等，并将是结果集缩小为分组数.这意味着 其他的数据在分组后丢弃.



4. HAVING

如果你用了 GROUP BY 分组, HAVING 会在分组完成后对结果集再次筛选。AS别名也不能在这个阶段使用.



5. SELECT

确定结果之后，SELECT用来对结果col简单筛选或计算，决定输出什么数据.



6. DISTINCT

如果数据行有重复DISTINCT 将负责排重.



7. ORDER BY

在结果集确定的情况下，ORDER BY 对结果做排序。因为SELECT中的表达式已经执行完了。此时可以用AS别名.



8. LIMIT / OFFSET

最后 LIMIT 和 OFFSET 从排序的结果中截取部分数据.







#### JOIN

LEFT [OUTER] JOIN



LEFT JOIN Address USING(PersonId)



1、 on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左边表中的记录。
2、where条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有left join的含义（必须返回左边表的记录）了，条件不为真的就全部过滤掉。







1. 如果想对右表进行限制，则一定要在`on`条件中进行，若在`where`中进行则可能导致数据缺失，导致左表在右表中无匹配行的行在最终结果中不出现，违背了我们对`left join`的理解。因为对左表无右表匹配行的行而言，遍历右表后`b=FALSE`,所以会尝试用`NULL`补齐右表，但是此时我们的`P2`对右表行进行了限制，NULL若不满足`P2`(`NULL`一般都不会满足限制条件，除非`IS NULL`这种)，则不会加入最终的结果中，导致结果缺失。
2. 如果没有`where`条件，无论`on`条件对左表进行怎样的限制，左表的每一行都至少会有一行的合成结果，对左表行而言，若右表若没有对应的行，则右表遍历结束后`b=FALSE`，会用一行`NULL`来生成数据，而这个数据是多余的。所以对左表进行过滤必须用where。







### 2. INSERT

```
INSERT INTO 表名称 VALUES (值1, 值2,....)
INSERT INTO 表名称 (列1, 列2,...) VALUES (值1, 值2,....)
```

INSERT INTO table_name ( field1, field2,...fieldN )

​            VALUES

​            ( value1, value2,...valueN );



### 3. UPDATE

```
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```



### 4. DELETE

```
DELETE FROM 表名称 WHERE 列名称 = 值
```



### 5. 其他语句

#### UNION





#### 聚合函数

##### SUM

SUM(0) 返回0

SUM(1) 返回记录条数

SUM(3) 返回3倍的记录条数



##### COUNT

count(distinct student) 去重



#### JOIN ON

1. 内连接 
   - [INNER] JOIN ON
2. 外连接
   - LEFT [OUTER] JOIN ON
   - RIGHT [OUTER] JOIN ON
3. 自连接
4. 交叉连接（笛卡尔积）

USING ON

交叉连接实际上是将两个表进行笛卡尔积运算，结果表是由第一个表的每一行与第二个表的每一行拼接后形成的表，称为‘笛卡尔积表’，结果表的行数等于两个表的行数之积。

如果两张表的数据量都比较大的话，那样就会占用很大的内存空间这显然是不合理的。所以，我们在进行表连接查询的时候一般都会使用JOIN xxx ON xxx的语法，ON语句的执行是在JOIN语句之前的，也就是说两张表数据行之间进行匹配的时候，会先判断数据行是否符合ON语句后面的条件，再决定是否JOIN。

**因此，有一个显而易见的SQL优化的方案是，当两张表的数据量比较大，又需要连接查询时，应该使用 FROM table1 JOIN table2 ON xxx的语法，避免使用 FROM table1,table2 WHERE xxx 的语法，因为后者会在内存中先生成一张数据量比较大的笛卡尔积表，增加了内存的开销。**





select t.request_at `Day`, round(

  sum(

​    if(t.status='completed', 0, 1)

  ) / count(t.id), 2

) `Cancellation Rate`

from Trips t

join Users u1 on (t.driver_id = u1.users_id and u1.banned = 'No')

join Users u2 on (t.client_id = u2.users_id and u2.banned = 'No')

where t.request_at between '2013-10-01' and '2013-10-03'

group by t.request_at



#### TOP / LIMIT

TOP 子句用于规定要返回的记录的数目。

```sql
SELECT TOP number|percent column_name(s)
FROM table_name
```



limit 的用法为： `select * from tableName limit i,n`

- tableName：表名
- i：为查询结果的索引值(默认从0开始)，当i=0时可省略i
- n：为查询结果返回的数量
- i与n之间使用英文逗号","隔开



#### OVER(ORDER BY xxx) 开窗函数

row_number()  rank() dense_rank()



count() over(partition by ... order by ...)：求分组后的总数。
　　max() over(partition by ... order by ...)：求分组后的最大值。
　　min() over(partition by ... order by ...)：求分组后的最小值。
　　avg() over(partition by ... order by ...)：求分组后的平均值。
　　lag() over(partition by ... order by ...)：取出前n行数据。　　

　　lead() over(partition by ... order by ...)：取出后n行数据。

　　ratio_to_report() over(partition by ... order by ...)：Ratio_to_report() 括号中就是分子，over() 括号中就是分母。

　　percent_rank() over(partition by ... order by ...)：



#### 条件语句

##### IF

`IF( expr1 , expr2 , expr3 )`

expr1 的值为TRUE，则返回值为 expr2 
expr1 的值为FALSE，则返回值为 expr3

##### IFNULL

`IFNULL( expr1 , expr2 )`

判断第一个参数expr1是否为NULL：

　　　　如果expr1不为空，直接返回expr1；

　　　　如果expr1为空，返回第二个参数 expr2  

常用在算术表达式计算和组函数中，用来对null值进行转换处理(返回值是数字或者字符串)



##### NULLIF

`NULLIF(expr1,expr2)：如果两个参数相等则返回NULL，否则返回第一个参数的值expr1`





#### 排序函数

- 必须有包含 ORDER BY 的 OVER 子句

- 分组内从1开始排序



```sql
-- row_number
select ROW_NUMBER() OVER(order by [SubTime] desc) as row_num,* from [Order]

-- rank
select RANK() OVER(order by [UserId]) as rank,* from [Order] 

-- dense_rank
select DENSE_RANK() OVER(order by [UserId]) as den_rank,* from [Order]

-- ntile
select NTILE(4) OVER(order by [SubTime] desc) as ntile,* from [Order]
```





#### 过程函数

CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT

BEGIN

 SET N:=N-1;

 RETURN (

   \# Write your MySQL query statement below.

   select (

​       select Salary

​     from Employee

​     order by Salary desc

​     limit n,1

​    )

 );

END





```
CASE WHEN condition THEN result

[WHEN...THEN...]

ELSE result

END

CASE SCORE WHEN 'A' THEN '优' ELSE '不及格' END
CASE SCORE WHEN 'B' THEN '良' ELSE '不及格' END
CASE SCORE WHEN 'C' THEN '中' ELSE '不及格' END

CASE WHEN SCORE = 'A' THEN '优'
     WHEN SCORE = 'B' THEN '良'
     WHEN SCORE = 'C' THEN '中' ELSE '不及格' END
```





#### 日期函数

##### 日期增减

```
dateadd(datepart,number,date) 
```

datepart:是規定應向日期的哪一部分返回新值的參數。下列是sql server支持的日期部分\縮寫及含義。 



日期部分   縮寫          含義 
year  yy,yyyy   年份 
quarter   qq,q  季度 
month   mm,m  月份 
dayofyear   dy,y  日 
day           dd,d
week  wk,ww  星期 
hour   hh  小時 
minute   mi,n  分鐘 
second   ss,s  秒 
millisecond   ms  毫秒 

number:是用來增加datepart的值。正數表示增加，負數表示減少，如果指定的是非整數值，則忽略此值的小 
數部分，不做四舍五入處理，例如，dateadd(day,1.7,date),表示date增加1天。 
date:是返回datetime或smalldatetime值或日期格式字符串的表達式。 





##### 日期差值

datediff()

```sql
-- mysql
datediff(date1, date2) -- 1 date1在date2后一天

-- oracle
datediff(day, date1, date2)
```

datepart:規定了應在日期的哪一部分計算差值。 
startdate:計算的開始日期。 
enddate:計算的終止日期。 

set datefirst函數是設置一周的第一天是星期幾。



#### 计算函数

##### MOD

取余，比%更高效

 mod(n1,n2)  n1%n2 







### 数据库操作

ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';



```
CREATE USER 'username'@'host' IDENTIFIED BY 'password';

GRANT privileges ON databasename.tablename TO 'username'@'host'

GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
GRANT ALL ON *.* TO 'pig'@'%';

GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;

REVOKE privilege ON databasename.tablename FROM 'username'@'host';

DROP USER 'username'@'host';
```



















## MYSQL



1.登录MySQL

mysql -u root -p 

2.添加新用户（允许所有ip访问）

```sql
create user 'test'@'*' identified by '123456';
#（test:用户名，*：所有ip地址，123456：密码）
```

3.创建数据库

create database testdb;

4.为新用户分配权限

```sql
grant all privileges on testdb.* to 'test'@'%' identified by '123456';
```



5.刷新权限

```sql
flush privileges;
```







ALTER user 'root'@'%' IDENTIFIED BY '1fuc.ker';

SET PASSWORD FOR 'root'@'localhost' = 'adminadmin';

ALTER USER IDENTIFIED WITH caching_sha2_password; 

alter user root@localhost identified with mysql_native_password by 'adminadmin';

update user set plugin='mysql_native_password’;







使用网上介绍的方法修改root用户的密码： 

\# mysqladmin -uroot -p password ’newpassword’ 

Enter password: 

mysqladmin: connect to server at ’localhost’ failed 

error: ’Access denied for user ’root’@’localhost’ (using password: YES)’ 

现在终于被我找到了解决方法，如下（请先测试方法三，谢谢！）： 

方法一： 

\# /etc/init.d/mysql stop 

\# mysqld_safe --user=mysql --skip-grant-tables --skip-networking & 

\# mysql -u root mysql 

mysql> UPDATE user SET Password=PASSWORD(’newpassword’) where USER=’root’; 

mysql> FLUSH PRIVILEGES; 

mysql> quit 

\# /etc/init.d/mysql restart 

\# mysql -uroot -p 

Enter password: <输入新设的密码newpassword> 

mysql> 

方法二： 

直接使用/etc/mysql/debian.cnf文件中[client]节提供的用户名和密码: 

\# mysql -udebian-sys-maint -p 

Enter password: <输入[client]节的密码> 

mysql> UPDATE user SET Password=PASSWORD(’newpassword’) where USER=’root’; 

mysql> FLUSH PRIVILEGES; 

mysql> quit 

\# mysql -uroot -p 

Enter password: <输入新设的密码newpassword> 

mysql> 

方法三： 

这种方法我没有进行过测试，因为我的root用户默认密码已经被我修改过了，那位有空测试一下，把结果告诉我，谢谢！ 

\# mysql -uroot -p 

Enter password: <输入/etc/mysql/debian.cnf文件中[client]节提供的密码> 





### Tips

对于 MySQL 解决方案，如果要转义用作列名的保留字，可以在关键字之前和之后使用撇号。例如 **`Rank`**



## Oracle