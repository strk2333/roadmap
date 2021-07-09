# MySQL Crash Course

## 第一章

数据库：保存有组织的数据的容器，是通过 DBMS 创建操作的容器。

模式：关于数据库和表的布局及特性的信息。



数据库管理系统 DBMS



主键：一列（或一组列，多个列），其值能够唯一区分表中每个行。

唯一标识表中每行的这个列（或一组列）称为主键。

主键值规则

- 任意两行都不具有相同的主键值；
- 每行都必须具有一个主键值（主键列不允许NULL值）。

** 应该总是定义主键

** 主键的良好习惯

- 不更新主键列的值；
- 不重用主键列的值；
- 不再主键列中使用可能会更改的值。



## 第二章

DBMS 分为两类

- 基于共享文件系统的 DBMS 
  - 包括 Microsoft Access，FileMaker，用于桌面用途，通常不用于高端或更关键的应用。
- 基于客户机-服务器的 DBMS
  - 包括 MySQL，Oracle，Microsoft SQL Server等
  - 服务器部分运行在称为数据库服务器的计算机上，负责所有的数据访问和处理，例如 MySQL DBMS
  - 客户机是与用户打交道的软件。例如 MySQL 提供的工具，Web应用开发语言（JSP，PHP），程序设计语言（C，C++，Java）



MySQL 版本变化

- 4，InnoDB 引擎，增加事务处理，并，改进全文本搜索等的支持。
- 4.1，对函数库，子查询，集成帮助等的重要增加。
- 5，存储过程，触发器，游标，视图等



## 第三章 使用MySQL



```mysql
-- 选择数据库
USE database_name;

-- 显示可用数据库列表
SHOW DATABASES;

-- 返回当前数据库中可用表
SHOW TABLES;

-- 给出表中列信息
SHOW COLUMNS FROM a_table_name;
DESCRIBE a_table_name;

-- 其他
SHOW STATUS;
SHOW CREATE DATABASE; SHOW CREATE TABLE; -- 显示 sql 语句 
SHOW GRANTS; -- 显示安全权限
SHOW ERRORS;
SHOW WARNINGS;

```



## 第四章 检索数据

```mysql
-- 检索单列
SELECT prod_name FROM products;

-- 检索多列
SELECT prod_id, prod_name, prod_price FROM products;

-- 检索所有列 （检索不需要的列会造成降低性能）
SELECT * FROM products;

-- 返回不同的值（不能部分使用 DISTINCT）
SELECT DISTINCT vend_id FROM products;
SELECT DISTINCT vend_id, prod_price FROM products; 

-- 限制结果
-- 从行0开始
-- LIMIT 5,5 返回从行5开始的5行
-- MySQL5 LIMIT 4 OFFSET 3, 从行3开始，取4行
SELECT prod_name FROM products LIMIT 5;
SELECT prod_name FROM products LIMIT 5,5;

-- 限定名
SELECT products.prod_name FROM products;
SELECT products.prod_name FROM crashcourse.products;

```

** MySQL 4.1及之前的版本中，数据库名，表名，列名等标识符默认区分大小写，4.1.1之后默认不区分。





## 第五章 排序检索数据



子句 clause

SQL语句由子句构成，有些子句是必需的，而有些是可选的。



```mysql
-- 排序（默认升序，a-z）
SELECT prod_name FROM products ORDER BY prod_name;

-- 按多列排序
SELECT prod_name FROM products ORDER BY prod_price, prod_name;

-- 指定排序方向 (降序)
SELECT prod_name FROM products ORDER BY prod_name DESC;

-- 多列排序，指定降序
SELECT prod_name, prod_price FROM products ORDER BY prod_price DESC, prod_name;

-- 最值
SELECT prod_price FROM products ORDER BY prod_price DESC LIMIT 1;
```



** ORDER BY 需要是 SELECT 语句里的最后一条





## 第六章 过滤数据



```mysql
-- WHERE子句
-- 不区分大小写，可以匹配Fuse
SELECT prod_name, prod_price FROM products WHERE prod_name = 'fuse';

-- WHERE子句操作符
= <> != < <= > >= BETWEEN

-- 空值检查
-- NULL与不匹配，过滤选择出不具有特定值的行时，过滤结果不会包含具有NULL值的行
SELECT prod_name FROM products WHERE prod_name IS NOT NULL;

```





## 第七章 数据过滤



```mysql
-- AND OR
SELECT prod_id, prod_price FROM products WHERE vend_id = 1003 AND prod_price <= 10;
SELECT prod_id, prod_price FROM products WHERE vend_id = 1003 OR vend_id = 1004;

-- AND OR 组合，AND优先级更高，先被计算
-- 使用圆括号避免不必要的干扰
WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10;

-- IN
SELECT prod_name, prod_price FROM products WHERE vend_id IN (1002, 1003) ORDER BY prod_name;

-- NOT
-- 否定它之后所跟的任何条件
NOT IN, NOT BETWEEN, NOT EXISTS

```



为什么使用 IN 操作符

- 当列表较长时，IN 的语法更清楚直观
- 计算次序更容易管理
- 速度一般比 OR 更快
- IN 可以包含其他 SELECT 语句





## 第八章 使用通配符进行过滤



```mysql
-- LIKE 技术上说是谓词，不是操作符，但是结果是一样的
-- 通配符（wildcard）

-- % 任意字符出现任意次数
-- 默认不区分大小写，可以配置区分大小写
-- 可以匹配0个字符
-- 注意尾空格，在最后附加%，或者用函数去掉空格
-- 注意NULL，%无法匹配NULL
SELECT prod_name, prod_price FROM products WHERE prod_name LIKE 'jet%';

-- _ 匹配单个字符，不能多也不能少
SELECT prod_name, prod_price FROM products WHERE prod_name LIKE '_ ton anvil';

```



使用通配符的技巧

- 不要过度使用。如果其他操作符可以达到目的，不要使用通配符。
- 除非有必要，不要把它们用在搜索模式的开始处。放在开始处搜索是最慢的。
- 仔细注意通配符的位置。





## 第九章 使用正则表达式进行搜索



```mysql
-- REGEXP
SELECT prod_name FROM products WHERE prod_name REGEXP '.000' ORDER BY prod_name;

-- REGEXP 在列值中匹配，LIKE在整列匹配
-- 使用定位符 ^$ 实现整列匹配

-- 默认不区分大小写，使用 BINARY 可以区分大小写
WHERE prod_name REGEXP BINARY 'JetPack .000';

-- |
'1000|2000'

-- [] 匹配几个字符之一
'[123] Ton'
'[1|2|3] Ton' -- 同上
'1|2|3 Ton' -- 1 或 2 或 3 Ton

-- 匹配范围
[1-5] [a-z]

-- 匹配特殊字符 \\
'\\.'
\\f	-- 换页 
\\n -- 换行
\\r -- 回车
\\t -- 制表
\\v -- 纵向制表

-- 匹配字符类
-- 任意字母数字（同[a-zA-Z0-9]）
[:alnum:]
-- 任意字符（同[a-zA-Z]）
[:alpha:]
-- 空格和制表（同[\\t]）
[:blank:]
-- ASCII控制字符（ASCII 0到31和127）
[:cntrl:]
-- 任意数字（同[0-9]）
[:digit:]
-- 与[:print:]相同，但不包括空格
[:graph:]
-- 任意小写字母（同[a-z]）
[:lower:]
-- 任意可打印字符
[:print:]
-- 既不在[:alnum:]，又不在[:cntrl:]的任意字符
[:punct:]
-- 包括空白在内的任意空白字符（同[\\f\\n\\r\\t\\v]）
[:space:]
-- 任意大写字母（同[A-Z]）
[:upper:]
-- 任意十六位进制数字（同[a-fA-F0-9]）
[:xdigit:]


-- 匹配多个实例
* -- 0或多个匹配
+ -- 1或多个匹配 ={1,}
? -- 0或1个匹配 ={0,1}
{n} -- 指定数目的匹配
{n,} -- 不少于指定数目的匹配
{n, m} -- 匹配数目的范围（m不超过255）

-- 匹配4个连在一起的数字
[[:digit:]]{4}

-- 定位符
^ -- 文本开始
$ -- 文本结尾
[[:<:]] -- 词的开始
[[:>:]] -- 词的结尾

-- ^
-- 集合中（用[]定义）,用来否定该集合
-- 否则，用来指串的开始处

-- 测试正则
-- REGEXP 没有匹配返回0，匹配成功返回1
SELECT 'hello' REGEXP '[0-9]';

```





## 第十章 创建计算字段



字段 一般用在计算字段的连接上

列 实际的表列





```mysql
-- 拼接字段
-- Concat
-- RTrim LTrim Trim
-- AS 别名（alias）导出列（derived column）
SELECT Concat(RTrim(vend_name), '(', vend_country, ')') AS vend_title
FROM vendors
ORDER BY vend_name;

-- 执行算术计算
SELECT prop_id, quantity*item_price AS expanded_price
FROM orderitems;

-- SELECT 测试函数

```





## 第十一章 使用数据处理函数



函数不如 SQL 可移植性强



```mysql
-- 文本处理函数
Left()		-- 返回串左边的字符
Length()	-- 返回串的长度
Locate()	-- 找出串的一个子串
Lower()		-- 将串转换为小写
LTrim()		-- 去掉串左边的空格
Right()
RTrim()
Soundex()	-- 返回串的SOUNDEX值
SubString()	-- 返回子串的字符
Upper()

-- SOUNDEX 将任何文本串转换为描述其语音表示的字母数字模式的算法。比较发音。
SELECT cust_name, cust_contact
FROM customers
WHERE Soundex(cust_contact) = Soundex('Y Lie'); -- = Y Lee


-- 日期和时间处理函数
AddDate()		-- 增加一个日期（天，周等）
AddTime()		-- 增加一个时间（时，分等）
CurDate()		-- 返回当前日期
CurTime()		-- 返回当前时间
Date()			-- 返回日期时间的日期部分
DateDiff()		-- 计算两个日期差
Date_Add()		-- 灵活的日期运算函数
Date_Format()	-- 返回一个格式化的日期或时间串
Day()			-- 返回日期的天数部分
DayOfWeek()		-- 返回星期几
Hour()			-- 返回时间的小时部分
Minute()		-- 返回时间的分钟部分
Month()			-- 返回日期的月份部分
Now()			-- 返回当前日期和时间
Second()		-- 返回时间的秒部分
Time()			-- 返回时间日期的时间部分
Year()			-- 返回日期的年份部分

-- Date()
-- 直接使用字段的日期进行比较，可能会由于时间不是默认的00:00:00而匹配失败
-- 可以使用Date()函数，取日期部分进行比较
SELECT cust_id, order_num
FROM orders
WHERE Date(order_date) = '2005-09-01';

-- Year() Month()
SELECT cust_id, order_num
FROM orders
WHERE Year(order_date) = 2005 AND Month(order_date) = 9

-- 数值处理函数
Abs()
Cos()
Exp()
Mod()
Pi()
Rand()
Sin()
Sqrt()
Tan()

```



** 建议总是使用4位数字的年份。4位数字更加可靠明确。MySQL支持两位数字的年份，00-69处理为2000-2069，70-99处理为1970-1999。





## 第十二章 汇总数据



```mysql
-- 聚集函数
AVG()
COUNT()
MAX()
MIN()
SUM()

-- AVG
-- AVG 忽略NULL值
SELECT AVG(prod_price) as avg_price FROM products;

-- COUNT
COUNT(*) -- 对表中行的数目进行计数，包含NULL值
COUNT(column) -- 对特定列的有值的行进行计数，忽略NULL值

-- MAX
-- MAX 忽略NULL值
-- 用于文本时，返回最后面的行

-- MIN
-- MIN 忽略NULL值
-- 用于文本时，返回最前面的行

-- SUM
-- 所有聚集函数都可以用来执行多个列上的计算
SELECT SUM(item_price*quantity) AS total_price FROM products;

```



聚集函数的用途

- 确定表中行数（或满足某个条件或包含某个特定值的行数）
- 获得表中行组的和
- 找出表列（或所有行或某些特定的行）的最大值，最小值和平均值



聚集不同的值

- ALL 默认
- DISTINCT 排除相同项
  - DISTINCT必须指定列名，不能用于计算或表达式
  - DISTINCT用于MAX，MIN是没有意义的



组合聚集函数

```mysql
SELECT COUNT(*), MIN(prod_price), MAX(prod_price), AVG(prod_price)
FROM products;
```





## 第十三章 分组数据



```mysql
-- 数据分组
SELECT vend_id, COUNT(*)
FROM products
GROUP BY vend_id;

-- WITH ROLLUP
-- 可以得到每个分组和每个分组汇总级别的值
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id WITH ROLLUP;

-- 过滤分组
-- HAVING 支持所有WHERE操作符
SELECT cust_id, COUNT(*) AS orders
FROM orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;

```



GROUP BY 规定

- 可以包含任意数目的列。
- 如果GROUP BY嵌套了分组， 数据将在最后规定的分组上进行汇总。也就是，建立分组时，指定的所有列都一起计算。
- GROUP BY子句中列出的每个列都必须是检索列或有效表达式（不能是聚集函数）。如果在SELECT中使用表达式，则必须在GROUP BY子句中指定相同的表达式。不能使用别名。
- 除了聚集函数意外，SELECT语句中的每个列都必须在GROUP BY子句中给出。
- 如果分组列有NULL值，则NULL作为一个分组返回。多个NULL值将分为一组。
- GROUP BY必须出现在WHERE之后，ORDER BY之前。



HAVING

- HAVING过滤基于分组聚集值，WHERE基于特定行值
- HAVING在数据分组后过滤，WHERE在分组前过滤。WHERE排除的行不在分组中，可能改变计算值，从而影响基于这些值过滤掉的分组。

```mysql
-- 列出具有两个或两个以上，价格大于10的产品的供应商
SELECT vend_id, COUNT(*) AS num_prods
FROM products
WHERE prod_price > 10
GROUP BY vend_id
HAVING COUNT(*) >= 2;
```



分组和排序

| ORDER BY                                 | GROUP BY                                           |
| ---------------------------------------- | -------------------------------------------------- |
| 排序产生的输出                           | 分组行。但输出的可能不是分组顺序                   |
| 任意列都可以使用（非选择的列也可以使用） | 只能使用选择列或表达式列，必须使用每个选择列表达式 |
| 不一定需要                               | 如果与聚集函数一起使用列（或表达式），则必须使用   |



SELECT子句顺序

| 子句     | 说明             | 是否必须                 |
| -------- | ---------------- | ------------------------ |
| SELECT   | 返回的列或表达式 | 是                       |
| FROM     | 检索目标         | 仅在从表中选择数据时使用 |
| WHERE    | 行级过滤         | 否                       |
| GROUP BY | 分组说明         | 仅在按组计算聚集时使用   |
| HAVING   | 组级过滤         | 否                       |
| ORDER BY | 输出排序         | 否                       |
| LIMIT    | 检索行数         | 否                       |





## 第十四章 使用子查询



```mysql
-- 子查询
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id = 'TNT2');)

```



** 子查询的嵌套层数没有限制，由于性能限制，尽量嵌套少一点。

** 列必须匹配。WHERE子句中使用子查询，应保证SELECT语句与WHERE子句有相同数目的列。通常子查询将返回单个列且与单个列匹配，如果需要也可以使用多个列。



相关子查询

涉及外部查询的子查询





## 第十五章 公主联结



```mysql
-- 创建联结
SELECT vend_name, prod_name, prod_price
FROM vendors, products
WHERE vendors.vend_id = products.vend_id
ORDER BY vend_name, prod_name;

```



外键（foreign key）

某个表中的一列，包含另一个表的主键值，定义了两个表之间的关系。



可伸缩性（scale）

能够适应不断增加的工作量而不失败。



```mysql
-- 笛卡尔积 等值联结
-- 检出条数为两表行数的乘积
SELECT vend_name, prod_name, prod_price
FROM vendors, products
ORDER BY vend_name, prod_name;

-- 内部联结 内连接
SELECT vend_name, prod_name, prod_price
FROM vendors INNER JOIN products
ON vendros.vend_id = products.vend_id;

-- 联结多表
SELECT prod_name, vend_name, prod_price, quantity
FROM orderitems, products, vendors
WHERE products.vend_id = vendors.vend_id
AND orderitems.prod_id = products.prod_id
AND order_num = 20005;

```





## 第十六章 创建高级联结



使用表的别名

- 缩短SQL语句
- 使用相同的表



```mysql
-- 三种联结
-- 自联结 自连接
-- 一般自联结要比子查询要快
SELECT p1.prod_id, p1.prod_name
FROM products AS p1, products AS p2
WHERE p1.vend_id = p2.vend_id AND p2.prod_id = 'DTNTR';

-- 自然联结
-- 自然连接排除重复的联结列，使每个列只返回一次
SELECT c.*, o.order_num, o.order_date, oi.prod_id, oi.quantity, oi.item_price
FROM customers AS c, orders AS o, orderitems AS oi
WHERE c.cust_id = o.cust_id
AND oi.order_num = o.order_num
AND prod_id = 'FB';

-- 外部联结
-- 包含没有关联行的行
SELECT customers.cust_id, orders.order_num
FROM customers LEFT JOIN orders
ON customers.cust_id = orders.cust_id;

-- 使用带聚集函数的联结
-- 按客户分组，返回每个客户的订单数
SELECT customers.cust_name,
	   customers.cust_id,
	   COUNT(orders.order_num) AS num_ord
FROM customers INNER JOIN orders
ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;
-- 其他联结
SELECT customers.cust_name,
	   customers.cust_id,
	   COUNT(orders.order_num) AS num_ord
FROM customers LEFT OUTER JOIN orders
ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;

```





## 第十七章 组合查询



组合查询和多个 WHERE 条件

二者可以相互转换，比较使用性能更好的方法



```mysql
-- UNION
-- ORDER BY 只允许出现在最后一条 SELECT 语句中，不允许使用多个 ORDER BY 子句
-- 最后出现的 ORDER BY 是对最终结果的排序，和最后一个 SELECT 语句没关系
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id in (1001,1002)
ORDER BY vend_id, prod_price;


```



UNION规则

- 必须由两条或两条以上的 SELECT 语句组成，语句之间用关键字 UNION 分隔
- 每个查询必须包含相同的列，表达式或聚集函数（次序不需要相同）
- 列数据类型必须兼容：类型不必完全相同，但必须是 DBMS 可以隐式转换的类型



UNION 自动去除重复行，如果不需要去除，可使用UNION ALL（WHERE 无法做到 UNION ALL 的效果）



 

## 第十八章 全文本搜索



MyISAM 支持，InnoDB 不支持

```mysql
-- 启用全文搜索
-- 可以在创建表的时候指定，或者稍后指定（这种情况下所有已有数据必须立即索引）
-- 不要在导入数据的时候使用 FULLTEXT，应该先导入所有数据，再修改表，定义 FULLTEXT
CREATE TABLE productnotes
(
	note_id		int			NOT NULL AUTO_INCREMENT,
	prod_id		char(10)	NOT NULL,
	note_date	datetime	NOT NULL,
	note_text	text		NULL,
	PRIMARY KEY(note_id),
	FULLTEXT(note_text)
) ENGINE=MyISAM;

-- 进行全文本搜索
-- Match() 指定被搜索的列，Match 列出的值必须与 FULLTEXT 中定义的相同，多个列的话次序也要一致
-- Against() 指定要使用的表达式
-- 搜索不区分大小写，除非使用 BINARY 方式
-- 与 LIKE 相比，可以做到对结果排序，LIKE 则不行
-- sql 中的 rank 为等级值
SELECT note_text， Match(note_text) Against('rabbit') AS rank
FROM productnotes
WHERE Match(note_text) Against('rabbit');

-- 使用查询扩展
-- 表中的行越多，使用查询扩展返回的结果越好
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('anvil' WITH QUERY EXPANSION);

-- 布尔文本搜索 MySQL支持全文本搜索的另外一种形式 布尔模式（boolean mode)
-- 匹配heavy，排除包含rope*的行
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('heavy -rope*' IN BOOLEAN MODE);

'+rabbit +bait' 包含词 rabbit 和 bait 的行
'rabbit bait' 包含两个里至少一个词
'"rabbit bait"' 匹配短语 rabbit bait
'>rabbit <carrot' 匹配两个词，增加前者等级，减少后者等级
'+safe +(<combination)' 匹配两个词，降低后者等级

-- 

```





使用查询扩展时，对数据和索引进行两遍扫描来完成搜索

- 首先进行基本的全文本搜索
- 其次MySQL检查这些匹配行，并选择所有有用的词
- 然后MySQL再次进行全文本搜索，这次的条件增加所有有用的词



布尔操作符

| 布尔操作符 | 说明                 |
| ---------- | -------------------- |
| +          | 包含，词必须存在     |
| -          | 排除，词必须不出现   |
| >          | 包含，而且增加等级值 |
| <          | 包含，而且减少等级值 |
| ()         | 把词组成子表达式     |
| ~          | 取消一个词的排序值   |
| *          | 词尾通配符           |
| ""         | 定义一个短语         |



全文本搜索的使用说明

- 在索引全文本数据时，短词被忽略且从索引中排除。短语指具有3个或3个以下字符的词（此数目可更改）
- MySQL带有一个内建的非用词（stopword）列表，这些词在索引全文本数据时将被忽略。此列表可以被覆盖
- 许多次出现的频率很高，搜索它们没有用处（返回太多结果）。因此MySQL制定了50%规则，如果一个词出现在50%以上的行中，则将作为非用词忽略。此规则不适用于IN BOOLEAN MODE
- 如果表中行数少于3行，则全文本搜索不返回结果（因为每个词或者不出现，或者至少出现在50%的行中）
- 忽略词中的单引号。例如，don't 索引为 dont
- 不具有词分隔符（汉语日语）的语言不能恰当地返回全文本搜索结果
- 仅在 MyISAM 数据库引擎中支持全文本搜索
- 没有邻近操作符





### 插入数据



```mysql
-- INSERT
-- INSERT 语句一般不产生输出
INSERT INTO customers
VALUES(NULL, 'SID', ...);

-- 建议使用明确给出列的 INSERT 语句
-- 列与值要一一对应
-- 列的定义允许NULL或给出默认值时，可以省略列
INSERT INTO customers(type, name, ...)
VALUES(NULL, 'SID', ...);

-- 降低 INSERT 语句的优先级，优先数据检索
INSERT LOW_PRIORITY INTO

-- 插入多个行
-- 单条语句插入多条，比多条语句插入单条快，可以提高数据库处理性能
INSERT INTO customers(...)
VALUES(...), (...);

-- 插入检索出的数据
-- SELECT 的列名不重要，重要的是顺序
INSERT INTO customers(...)
SELECT xxx, xxx, xxx, ... FROM custnew;

```





## 第二十章 更新和删除数据



```mysql
-- UPDATE
-- 不要省略多行
UPDATE customers
SET cust_email = 'elmer@fudd.com'
WHERE cust_id = 10005;

-- 更新多列
-- 多列之间逗号分隔
UPDATE customers
SET cust_email = 'elmer@fudd.com',
	cust_name = 'The Fudds'
WHERE cust_id = 10005;

-- IGNORE 关键字
-- 如果 UPDATE 语句更新多行，更新中发生了错误，则整个语句的操作被取消，已更新的将恢复到操作前的状态。如果需要在错误时也继续更新，可使用 IGNORE 关键字
UPDATE IGNORE customers ...

-- DELETE
DELETE FROM customers
WHERE cust_id = 10006;

-- 如果想删除表中所有行，可使用 TRUNCATE TABLE 语句，完成相同的工作，速度更快。TRUNCATE 实际是删除原来的表并重新创建一个表

```



** 不要省略WHERE子句



更新和删除的指导原则

- 除非确实打算更新删除每一行，否则绝对不要使用不带 WHERE 子句的 UPDATE 或 DELETE 语句
- 保证每个表都有主键，尽可能像 WHERE 子句一样使用它
- 在对 UPDATE 或 DELETE 语句使用 WHERE 子句前，先用 SELECT 进行测试，保证过滤的是正确的记录，防止出现错误的操作
- 使用强制实施引用完整性的数据库，这样 MySQL 将不允许删除具有与其他表相关联的数据的行
- MySQL 没有撤销按钮





## 第二十一章 创建和操作表



```mysql
-- CREATE TABLE
-- NULL：允许 NULL 值也允许插入更新时不给出值
-- 主键：主键只能使用不允许 NULL 值的列
-- 自增：每个表只允许有一个AUTO_INCREMENT列，并且它必须被索引（如，通过它称为主键）
-- 默认值：MySQL不支持函数设置默认值，只支持常量
CREATE TABLE customers
(
	cust_id		int			NOT NULL AUTO_INCREMENT,
    cust_name	char(50)	NOT NULL,
    cust_type	int			NOT NULL DEFAULT 1,
    PRIMARY KEY (cust_id)
) ENGINE=InnoDB;

-- IF NOT EXISTS 仅在表不存在时创建它
CREATE TABLE IF NOT EXISTS xxx

-- 更新表
ALTER TABLE vendors
ADD vend_phone CHAR(20);

ALTER TABLE vendors
DROP COLUMN vend_phone;

ALTER TABLE orderitems
ADD CONSTRAINT fk_orderitems_orders
FOREIGN KEY (order_num) REFERENCES orders (order_num);

-- 删除表
DROP TABLE customers2;

-- 重命名表
RENAME TABLE customers2 TO customers;
RENAME TABLE customers2 TO customers,
			 vendors2 TO vendors;

```



### 自增

覆盖AUTO_INCREMENT

如果一个列被指定为AUTO_INCREMENT，可以简单地在INSERT语句中指定一个值，只要保证唯一即可。后续的增量将使用该插入值。



确定AUTO_INCREMENT的值

```mysql
-- 返回最后一个 AUTO_INCREMENT 的值
SELECT last_insert_id();

```



### 引擎类型

- InnoDB 可靠的事务处理引擎，不支持全文本搜索
- MEMORY 在功能上等同于 MyISAM，但由于数据存储在内存不是硬盘上，速度很快，适合临时表
- MyISAM 性能极高的引擎，支持全文本搜索，不支持事务处理

引擎类型可以混用。

外键不能跨引擎。





复杂的表结构修改一般需要手动删除过程，一般包括以下步骤

- 用新的列布局创建新表
- 使用INSERT SELECT语句从旧表复制数据到新表，如果需要，可使用转换函数和计算字段
- 检验包含数据的新表
- 重命名旧表（如果确定，可以删除）
- 用旧表原来的名字重命名新表
- 根据需要，重新创建触发器，存储过程，索引和外键







## 第二十二章 使用视图



```mysql
-- VIEW
-- 创建视图
CREATE VIEW
-- 查看创建视图语句
SHOW CREATE VIEW viewname;
-- 删除视图
DROP VIEW viewname;
-- 更新视图，可以先 DROP 再 CREATE，也可以直接用 CREATE OR REPLACE
CREATE OR REPALCE VIEW viewname

-- 创建
CREATE VIEW productcustomers AS
SELECT cust_name, cust_contact, prod_id
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num;
  
-- 用视图重新格式化检索出的数据
CREATE VIEW vendorlocations AS
SELECT Concat(RTrim(vend_name), '（', RTrim(vend_country), '）') AS vend_title
FROM vendors
ORDER BY vend_name;

-- 用视图过滤不想要的数据
CREATE VIEW customeremaillist AS
SELECT cust_id, cust_name, cust_email
FROM customers
WHERE cust_email IS NOT NULL;

SELECT * FROM customeremaillist;

-- 使用视图与计算字段
CREATE VIEW orderitemexpanded AS
SELECT prod_id, quantity, item_price, quantity*item_price AS expanded_price
FROM orderitems
WHERE order_num = 2005;

```





视图的作用

- 重用SQL语句
- 简化复杂的 SQL 操作。编写查询后，可以方便地重用不必知道具体细节
- 使用表的组成部分而不是整个表
- 保护数据。授予用户表的特定部分的访问权限
- 更改数据格式和表示。



视图的限制和规则

- 与表一样，视图必须唯一命名（不能与别的视图或表重名）
- 视图创建数目没有限制
- 为了创建视图，必须有足够的访问权限
- 视图可以嵌套
- ORDER BY 可以用于视图，但如果从该视图检索到的 SELECT 语句也带有 ORDER BY，那么视图中的 ORDER BY将被覆盖
- 视图不能索引，也不能有关联的触发器或默认值
- 视图和表可以一起使用。例如一个 SELECT 语句里联结表和视图。



更新视图

更新视图实际上是更新基表

如果MySQL不能正确地确定被更新的基数据，则不允许更新。即，如果视图定义中有以下操作，则不能进行视图的更新：

- 分组（使用 GROUP BY 和 HAVING）
- 联结
- 子查询
- 并
- 聚集函数
- DISTINCT
- 导出（计算）列

视图主要用于数据检索





## 第二十三章 使用存储过程



```mysql
-- 使用存储过程
-- 执行
CALL productpricing(@pricelow,
					@pricehigh,
					@priceaverage);

-- 创建
-- 如果有参数，可以在括号中给出
CREATE PROCEDURE productpricing()
BEGIN
	SELECT Avg(prod_price) AS priceaverage
	FROM products
END;

-- 临时改变 mysql 命令行的分隔符
-- 事实上，mysql 命令行的默认分隔符和 MySQL 的默认分隔符都是;。这会让;字符不出现在保存的存储过程中，出现句法错误。
DELIMITER //
CREATE PROCEDURE ..
BEGIN
..
END //
DELIMITER ;

-- 删除存储过程
DROP PROCEDURE productpricing;
DROP PROCEDURE IF EXISTS productpricing; -- 防止错误

-- 使用参数
-- IN 传递给存储过程
-- OUT 从存储过程传出
-- INOUT 对存储过程传入传出
-- 记录集不是允许的类型，不能通过一个参数返回多行多列
CREATE PROCEDURE productpricing(
	OUT pl DECIMAL(8, 2),
    OUT ph DECIMAL(8, 2),
    OUT pa DECIMAL(8, 2)
)
BEGIN
	SELECT Min(prod_price)
	INTO pl
	FROM products;
	SELECT Max(prod_price)
	INTO ph
	FROM products;
	SELECT Avg(prod_price)
	INTO pa
	FROM products;
END;
-- 调用
-- 调用后不会有任何返回
CALL productpricing(@pricelow,
                    @pricehigh,
                    @priceaverage);
-- 显示结果
SELECT @priceaverage;

-- 另一个例子
CREATE PROCEDURE ordertotal(
	IN onumber INT,
    OUT ototal DECIMAL(8, 2)
)
BEGIN
	SELECT Sum(item_price*quantity)
	FROM orderitems
	WHERE order_num = onumber
	INTO ototal
END;
CALL ordertotal(20005, @total);
SELECT @total;

-- 智能存储过程
-- Name: ordertotal
-- Parameters: onumber = order nubmer
-- 			   taxable = 0 if not taxable, 1 if taxable
-- 			   ototal = order total variable
CREATE PROCEDURE ordertotal(
	IN onumber INT,
    IN taxable BOOLEAN,
    OUT ototal DEDIMAL(8,2)
) COMMENT 'Obtain order total, optionally adding tax'
BEGIN
	-- Declare varible for total
	DECLARE total DECIMAL(8,2);
	-- Declare tax percentage
	DECLARE taxrate INT DEFAULT 6;
	
	-- Get the order total
	SELECT Sum(item_price*quantity)
	FROM orderitems
	WHERE order_num = onumber
	INTO total;
	
	-- Is this taxable?
	IF taxable THEN
		-- YES, so add taxrate to the total
		SELECT total+(total/100*taxrate) INTO total;
	END IF;
	
	-- And finally, save to out variable
	SELECT total INTO ototal;
END;
CALL ordertotal(20005, 0, @total);
SELECT @total;
CALL ordertotal(20005, 1, @total);
SELECT @total;

-- 检查存储过程
-- 显示语句
SHOW CREATE PROCEDURE ordertotal;
-- 详细信息
SHOW PROCEDURE STATUS LIKE 'ordertotal';

```



使用存储过程的理由

- 通过把处理封装在容易使用的单元中，简化复杂的操作
- 保证数据完整性，一致性
- 简化对变动的管理
- 提高性能





## 第二十四章 使用游标



```mysql
-- 创建游标
CREATE PROCEDURE processorders()
BEGIN
	DECLARE ordernumbers CURSOR
	FOR
	SELECT order_num FROM orders;
END;

-- 打开和关闭游标
-- CLOSE 释放游标使用的所有内部内存和资源，因此不需要时都应该关闭
-- 如果不关闭，会在 END 语句自动关闭
OPEN ordernumbers;
CLOSE ordernumbers;

-- 使用游标数据
-- FETCH 分别访问游标每一行，指定检索数据
CREATE PROCEDURE processorders()
BEGIN
	DECLARE o INT;
	DECLARE ordernumbers CURSOR
	FOR
	SELECT order_num FROM orders;
	OPEN ordernumbers;
	FETCH ordernumbers INTO o;
	CLOSE ordernumbers;
END;

-- 循环检索数据，从第一行到最后一行
CREATE PROCEDURE processorders()
BEGIN
	DECLARE done BOOLEAN DEFAULT 0;
	DECLARE o INT;
	DECLARE t DECIMAL(8,2);
	
	DECLARE ordernumbers CURSOR;
	FOR
	SELECT order_num FROM orders;
	DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done = 1;
	CREATE TABLE IF NOT EXISTS ordertotals
		(order_num INT, total DECIMAL(8,2));
	OPEN ordernumbers;
	REPEAT
		FETCH ordernumbers INTO o;
		CALL ordertotal(o, 1, t);
		INSERT INTO ordertotals(order_num, total)
		VALUES(o, t);
	UNTIL done END REPEAT;
	CLOSE ordernumbers;
END;

```



有时，需要在检索出来的行中前进或后退一行或多行，这就需要使用游标。游标是一个存储在 MySQL 服务器上的数据库查询，它不是一条 SELECT 语句，而是被该语句检索出来的结果集。



使用游标

- 使用前需要声明（定义）
- 声明后，必须打开游标以供使用
- 对于填有数据的游标，根据需要取出（检索）各行
- 在结束游标使用时，必须关闭游标
- -- 可以频繁打开关闭游标







## 第二十五章 使用触发器





```mysql
-- 创建触发器
CREATE TRIGGER newproduct AFTER INSERT ON products
FOR EACH ROW SELECT 'Product added';

-- 删除触发器
-- 触发器不能刷新，为了修改必须先删除它，然后重新创建
DROP TRIGGER newproduct;

```



触发器是 MySQL 响应以下任意语句而自动执行的一条 MySQL 语句（或位于 BEGIN 与 END语句之间的一组语句）

- DELETE
- INSERT
- UPDATE

其他 MySQL 语句不支持触发器



创建触发器需要给出 4 条信息

- 唯一的触发器名
- 触发器关联的表
- 触发器应该响应的活动
- 触发器何时执行



MySQL5 中，触发器名表中唯一，数据库里不是唯一，建议数据库里也保持唯一



只有表支持触发器，视图不支持，临时表也不支持



触发器按每个表每个事件每次地定义，每个表每个事件每次只允许一个触发器。因此，每个表最多支持6个触发器（每条 INSERT，UPDATE，DELETE 的之前之后）。

单一触发器不能与多个事件或多个表关联，所以如果你需要一个对 INSERT 和 UPDATE 操作执行的触发器，则应该定义两个触发器。



如果 BEFORE 触发器失败，则 MySQL 将不执行请求的操作，另外，如果 BEFORE 触发器或语句本身失败，MySQL 将不执行 AFTER 触发器。



INSERT 触发器

- 在 INSERT 触发器代码内，可引用一个名为 NEW 的虚拟表，访问被插入的行
- 在 BEFORE INSERT 触发器中，NEW 中的值也可以被更新（允许更改被插入的值）
- 对于 AUTO_INCREMENT 列，NEW 在 INSERT 执行之前包含0，在 INSERT 执行之后包含新的自动生成值

```mysql
-- INSERT
CREATE TRIGGER neworder AFTER INSERT ON orders
FOR EACH ROW SELECT NEW.order_num;

```



通常，将 BEFORE 用于数据验证和净化。本提示也适用于 UPDATE 触发器。



DELETE 触发器

- 在 DELETE 触发器代码内，你可以引用 OLD 虚拟表，访问被删除的行
- OLD 的值是只读的，不能更新

```mysql
-- DELETE
CREATE TRIGGER deleteorder BEFORE DELETE ON orders
FOR EACH ROW
BEGIN
	INSERT INTO archive_orders(order_num, order_date, cust_id)
	VALUES(OLD.order_num, OLD.order_date, OLD.cust_id);
END;

```



UPDATE 触发器

- 可以引用 OLD 虚拟表访问 UPDATE 之前的值，可以引用 NEW 虚拟表访问新更新的值
- 在 BEFORE UPDATE 触发器中，NEW 中的值可能也被更新（允许更改将要用于 UPDATE 的值）
- OLD 只读

```
-- UPDATE
CREATE TRIGGER updateorder BEFORE UPDATE ON vendors
FOR EACH ROW SET NEW.vend_state = Upper(NEW.vend_state);
```



MySQL 不支持 CALL 语句，不支持存储过程





## 第二十六章 管理事务处理





```mysql
-- 开始
START TRANSACTION

-- ROLLBACK
-- ROLLBACK 只能在一个事务处理内使用（START TRANSACTION 之后）
-- 不能回退 SELECT，CREATE，DROP
SELECT * FROM ordertotals;
START TRANSACTION;
DELETE FROM ordertotals;
SELECT * FROM ordertotals;
ROLLBACK;
SELECT * FROM ordertotals;

-- 事务处理中，提交不会隐含地进行，需要 COMMIT 语句进行提交
-- COMMIT 仅在不出错时，提交修改
START TRANSACTION;
DELETE FROM orderitems WHERE order_num = 20010;
DELETE FROM orders WHERE order_num = 20010;
COMMIT;

-- 保留点
-- 保留点越多越好
-- 保留点在事务完成时自动释放，MySQL5以来，也可以用 RELEASE SAVEPOINT 明确释放保留点
SAVEPOINT delete1;
ROLLBACK TO delete1;

-- 更改默认的提交方式
-- autocommit 标志针对每个连接而不是服务器的
SET autocommit = 0;

```



事务处理可以用来维护数据库的完整性，它保证成批的 MySQL 操作要么完全执行，要么完全不执行。



COMMIT 或 ROLLBACK 后，事务会隐式关闭





## 第二十七章 全球化和本地化



```mysql
-- 显示可用字符集
SHOW CHARACTER SET;

-- 显示可用校对
SHOW COLLATION;

-- 确认所用的字符集和校对
SHOW VARIABLES LIKE 'character%';
SHOW VARIABLES LIKE 'collation%';

-- 为表指定字符集和校对
CREATE TABLE mytable
(
	column1 INT,
    column2 VARCHAR(10)
) DEFAULT CHARACTER SET hebrew
  COLLATE hebrew_general_ci;
  
-- 对特定列指定字符集和校对
CREATE TABLE mytable
(
	column1 INT,
    column2 VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_general_ci
) DEFAULT ...;

-- 对 SELECT 语句指定校对
-- 该示例在不区分大小写的表上，进行大小写区分搜索。反过来也是可以的
SELECT * FROM customers
ORDER BY lastname, firstname COLLATE latin1_general_cs;

-- COLLATE 还可以用于 GROUP BY，HAVING，聚集函数，别名等

```







## 第二十八章 安全管理



```mysql
-- 管理用户
USE mysql;
SELECT user FROM users;

-- 创建用户账号
CREATE USER ben IDENTIFIED BY 'p@$$w0rd';

-- 重命名一个用户账号
-- MySQL 5之前不支持，需要 UPDATE 直接更新 user 表
RENAME USER ben TO bforta;

-- 删除用户账号
-- MySQL 5之前只删除账号，需要先 REVOKE 删除账号相关权限，然后 DROP USER
DROP USER bforta;

-- 设置访问权限
SHOW GRANTS FOR bforta;
GRANT SELECT ON crashcourse.* TO bforta;
REVOKE SELECT ON crashcourse.* FROM bforta;

-- 简化多次授权
GRANT SELECT, INSERT ON crashcourse.* TO bforta;

-- 更改口令
SET PASSWORD FOR bforta = Password('n3w p@$$w0rd');

-- 设置自己的口令
SET PASSWORD = Password('n3w p@$$w0rd');

```



MySQL 用户账户和信息存储在 mysql 数据库中，一般不需要直接访问该数据库和表。



指定散列口令

IDENTIFIED BY 指定的口令为纯文本，MySQL 将在保存到 user 表之前对其进行加密。为了作为散列值指定口令，使用 IDENTIFIED BY PASSWORD



GRANT 和 INSERT 都可以创建用户账号

GRANT 不如 CREATE USER 简单清楚

INSERT 不安全



GRANT 和 REVOKE 可以在以下几个层次上控制访问权限

- 整个服务器，使用 GRANT ALL 和 REVOKE ALL
- 整个数据库，使用 ON database.*
- 特定的表，使用 ON database.table
- 特定的列
- 特定的存储过程







## 第二十九章 数据库维护





```mysql
-- 为了保证所有数据被写到磁盘，需要先 FLUSH TABLES
-- 检查表键是否正确
ANALYZE TABLE orders;

-- 检查表
CHECK TABLE orders, orderitems;

-- 诊断启动问题
mysqld --help 显示帮助
mysqld --safe-mode 装载减去某些最佳配置的服务器
mysqld --verbose 显示全文本信息
mysqld --version 显示版本信息

```



日志文件

- 错误日志
  - 通常名为 hostname.err，位于 data 目录中。名称可以用 --log-error 命令行选项修改
- 查询日志
  - hostname.log，--log
- 二进制日志
  - hostname-bin，--log-bin
- 缓慢查询日志
  - hostname-slow，--log-slow-queries







## 第三十章 改善性能



- 生产环境应该遵循 MySQL 的硬件建议
- 一般来说，关键的生产 DBMS 应该运行在自己的专用服务器上
- 运行一段时间后，可能需要调整内存分配，缓冲区大小等（SHOW VARIABLES，SHOW STATUS 查看当前设置）
- SHOW PROCESSLIST 显示所有的活动进程。KILL 终结某个特定的进程（需要作为管理员登录）
- 总有不止一种 SELECT 的编写方式，应该试验联结，并，子查询等，找出最佳方法
- 使用 EXPLAIN 语句让 MySQL 解释它如何执行一条 SELECT 语句
- 一般来说，存储过程执行比各条 MySQL 语句单独执行更快
- 不要检索比需求多的数据。不要用 SELECT *（除非你确定要使用）
- 有的操作（包括 INSERT）支持一个可选的 DELAYED 关键字，如果使用它，将把控制立即返回给调用程序，并且一旦有可能就实际执行该操作
- 导入数据时，应该关闭自动提交。还可以删除索引（包括 FULLTEXT 索引），然后在导入完成后再重建它们
- 必须索引数据库表以改善数据检索性能
- 使用多条 SELECT 和连接的 UNION 语句代替 OR 条件，会有性能提升
- 索引改善检索性能，损害插入，删除，更新的性能
- LIKE 很慢，最好使用 FULLTEXT 而不是 LIKE
- 数据库是变化的实体，优化良好的表后面可能会面目全非，需要调整配置优化
- 最重要的规则：每条规则都可能在某些条件下被打破





















