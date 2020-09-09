# SQL































## 1. SELECT

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





查询执行顺序

\1. FROM 和 JOINs

FROM 或 JOIN会第一个执行，确定一个整体的数据范围. 如果要JOIN不同表，可能会生成一个临时Table来用于 下面的过程。总之第一步可以简单理解为确定一个数据源表（含临时表)



\2. WHERE

我们确定了数据来源 WHERE 语句就将在这个数据源中按要求进行数据筛选，并丢弃不符合要求的数据行，所有的筛选col属性 只能来自FROM圈定的表. AS别名还不能在这个阶段使用，因为可能别名是一个还没执行的表达式



\3. GROUP BY

如果你用了 GROUP BY 分组，那GROUP BY 将对之前的数据进行分组，统计等，并将是结果集缩小为分组数.这意味着 其他的数据在分组后丢弃.



\4. HAVING

如果你用了 GROUP BY 分组, HAVING 会在分组完成后对结果集再次筛选。AS别名也不能在这个阶段使用.



\5. SELECT

确定结果之后，SELECT用来对结果col简单筛选或计算，决定输出什么数据.



\6. DISTINCT

如果数据行有重复DISTINCT 将负责排重.



\7. ORDER BY

在结果集确定的情况下，ORDER BY 对结果做排序。因为SELECT中的表达式已经执行完了。此时可以用AS别名.



\8. LIMIT / OFFSET

最后 LIMIT 和 OFFSET 从排序的结果中截取部分数据.





## 2. INSERT



INSERT INTO table_name ( field1, field2,...fieldN )

​            VALUES

​            ( value1, value2,...valueN );





## 数据库操作

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





## Oracle