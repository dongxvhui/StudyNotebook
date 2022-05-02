# MySQL必知必会

### 1.连接数据库

MySQL连接命令

一、MySQL 连接本地数据库，用户名为“root”，密码“123”（注意：“-p”和“123” 之间不能有空格）

~~~
C:>mysql -h localhost -u root -p123456
~~~



二、MySQL 连接远程数据库（xxx.xxx.0.xxx），端口“3306”，用户名为“root”，密码“123”

~~~
C:>mysql -h xxx.xxx.0.xxx -P 3306 -u root -p123456
~~~



三、MySQL 连接本地数据库，用户名为“root”，隐藏密码

~~~
C:>mysql -h localhost -u root -p

Enter password:
~~~



四、MySQL 连接本地数据库，用户名为“root”，指定所连接的数据库为“test”

~~~
C:>mysql -h localhost -u root -p123 -D test

mysql>select database();
~~~

查看版本

~~~
mysql>status;
~~~

退出

~~~
mysql> exit;
Bye
~~~





### 2.查看

查看数据库

~~~
mysql> SHOW DATABASES;
~~~

选中一个数据库

~~~
mysql> USE crashcourse（数据库名）;
Database changed
~~~

显示表

~~~
mysql>  SHOW TABLES;
~~~

返回行

~~~
mysql> SHOW COLUMNS FROM customers;
~~~



### 3.检索

检索单个列

~~~
mysql> SELECT prod_name FROM products;
~~~

检索多个列

~~~
mysql> SELECT prod_id,prod_name,prod_price FROM products;
~~~

检索所有列

~~~
mysql> SELECT * FROM products;
~~~

注：SELECT * 一般情况下不建议使用

检索不同的行

~~~
mysql> SELECT vend_id FROM products;
~~~

检索不同的行（返回唯一主键）

~~~
mysql> SELECT DISTINCT vend_id FROM products;
~~~

限制结果

返回前几行

~~~
mysql> SELECT prod_name FROM products LIMIT 5;
~~~

指定开始行和行数

~~~
mysql> SELECT prod_name FROM products LIMIT 5,5;
~~~

使用完全限定的表名查找

~~~
mysql> SELECT products.prod_name FROM products;
~~~

### 4.排序

单列排序

~~~
mysql> SELECT prod_name FROM products ORDER BY prod_name;
~~~

多列排序

~~~
mysql>  SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price,prod_name;
~~~

指定排序方向（降序排序）

~~~
mysql> SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC;
~~~

多列降序排序

~~~
mysql> SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC,prod_name;
~~~

关键词：升序排序：ASC(默认)，降序排序：DESC(需要指定)

极值检索

~~~
mysql> SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC LIMIT 1;  //最贵
mysql>  SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price LIMIT 1;   //最便宜
~~~

### 5.过滤数据

WHERE 子句

~~~
mysql> SELECT prod_name,prod_price FROM products WHERE prod_price = 2.50;
~~~

WHERE 子句操作符

![weread_image_13288455595189](D:\project\工作空间\mdfile\图源\weread_image_13288455595189.jpeg)

检查单个值

~~~
mysql> SELECT prod_name,prod_price FROM products WHERE prod_name = 'fuses';

mysql> SELECT prod_name,prod_price FROM products WHERE prod_price <10;

mysql>  SELECT prod_name,prod_price FROM products WHERE prod_price <= 10;

（错误语句，会报错）SELECT prod_name,prod_price FROM products WHERE prod_price <> 10;

mysql>  SELECT prod_name,prod_price FROM products WHERE prod_price != 10;

~~~

不匹配检查

~~~
mysql> SELECT prod_name,prod_price FROM products WHERE prod_id <> 1003;

或

mysql>  SELECT prod_name,prod_price FROM products WHERE prod_id != 1003;
~~~

范围值检查

~~~
mysql>  SELECT prod_name,prod_price FROM products WHERE prod_price BETWEEN 5 AND 10;
~~~

空值检查

~~~
mysql> SELECT prod_name  FROM products WHERE prod_price IS NULL;
~~~

~~~
mysql> SELECT cust_id FROM customers WHERE cust_email IS NULL;
~~~



### 6.数据过滤

组合WHERE子句

AND操作符

~~~
mysql> SELECT prod_id,prod_price,prod_name FROM products WHERE vend_id=1003 AND prod_price <= 10;
~~~

OR操作符

~~~
mysql> SELECT prod_id,prod_price,prod_name FROM products WHERE vend_id=1003 AND prod_price <= 10;
~~~

注：AND的优先级高于OR.

计算次序（加小括号提高优先级）

~~~
mysql> SELECT prod_name,prod_price FROM products WHERE (vend_id=1002 OR vend_id =1003) AND prod_price >=10;
~~~

IN操作符

~~~
mysql> SELECT prod_name,prod_price FROM  products WHERE vend_id IN (1002,1003) ORDER BY prod_name;
~~~

等同于

~~~
mysql> SELECT prod_name,prod_price FROM  products WHERE vend_id=1002 OR vend_id =1003 ORDER BY prod_name;
~~~

NOT操作符

~~~
mysql> SELECT prod_name,prod_price FROM  products WHERE vend_id NOT IN (1002,1003) ORDER BY prod_name;
~~~



### 7.通配符过滤

LIKE操作符

百分号（%）通配符（%表示任何字符出现任意次数）

~~~
mysql> SELECT prod_id,prod_name FROM products WHERE prod_name LIKE 'jet%';
~~~

~~~
mysql>  SELECT prod_id,prod_name FROM products WHERE prod_name LIKE '%anvil%';
~~~

~~~
mysql>  SELECT prod_id,prod_name FROM products WHERE prod_name LIKE 's%e';
~~~

下划线（_）通配符   （下划线匹配单个字符）

~~~
mysql> SELECT prod_id,prod_name FROM products WHERE prod_name LIKE '_ ton anvil';
~~~

### 8.正则表达式

基本字符匹配

~~~
mysql> SELECT prod_name FROM products WHERE prod_name REGEXP '1000' ORDER BY prod_name;
~~~

~~~
mysql>  SELECT prod_name FROM products WHERE prod_name REGEXP '.000' ORDER BY prod_name;
~~~

OR匹配

~~~
SELECT prod_name FROM products WHERE prod_name REGEXP '.000' ORDER BY prod_name;
~~~

匹配几个字符之一

~~~
mysql> SELECT prod_name FROM products WHERE prod_name REGEXP '[123] Ton' ORDER BY prod_name;
~~~

匹配范围

~~~
mysql> SELECT prod_name FROM products WHERE prod_name REGEXP '[1-5] Ton' ORDER BY prod_name;
~~~

疑问：匹配数字后的字符（Ton)首字母要大写，不然报错？

匹配特殊字符

~~~
mysql>  SELECT prod_name FROM products WHERE prod_name REGEXP '\\.' ORDER BY prod_name;
~~~

空白元字符

![weread_image_37844424744207](D:\project\工作空间\mdfile\图源\weread_image_37844424744207.jpeg)

字符类

![weread_image_37848818640508](D:\project\工作空间\mdfile\图源\weread_image_37848818640508.jpeg)

重复元字符

![weread_image_37855204400036](D:\project\工作空间\mdfile\图源\weread_image_37855204400036.jpeg)

匹配多个实例

~~~
mysql>  SELECT prod_name FROM products WHERE prod_name REGEXP '\\([0-9] stick?\\)' ORDER BY prod_name;
~~~

~~~
mysql>  SELECT prod_name FROM products WHERE prod_name REGEXP '[[:digit:]]{4}' ORDER BY prod_name;
或
mysql>  SELECT prod_name FROM products WHERE prod_name REGEXP '[0-9][0-9][0-9][0-9]' ORDER BY prod_name;
~~~

定位符

![weread_image_38766536996355](D:\project\工作空间\mdfile\图源\weread_image_38766536996355.jpeg)

~~~
mysql> SELECT prod_name FROM products WHERE prod_name REGEXP '^[0-9\\.]' ORDER BY prod_name;
~~~

### 9.创建计算字段

计算字段

拼接字段

~~~
mysql> SELECT Concat(vend_name, '(', vend_country,')') FROM vendors ORDER BY vend_name;
~~~

~~~
mysql>  SELECT Concat(vend_name, '(',RTrim( vend_country),')') FROM vendors ORDER BY vend_name;
~~~

使用别名

~~~
mysql>  SELECT Concat(vend_name, '(',RTrim( vend_country),')') AS vend_title FROM vendors ORDER BY vend_name;
~~~

执行算术计算

~~~
mysql> SELECT prod_id,quantity,item_price, quantity*item_price AS expanded_price FROM orderitems WHERE order_num=20005;
~~~

MySQL算术操作符

![weread_image_40490754353823](D:\project\工作空间\mdfile\图源\weread_image_40490754353823.jpeg)

### 10.使用数据处理函数

文本处理函数

Upper(转换为大写)

~~~
mysql> SELECT vend_name, Upper(vend_name) AS vend_name_upcase FROM vendors ORDER BY vend_name;
~~~

常用的文本处理函数

![weread_image_41024154148098](D:\project\工作空间\mdfile\图源\weread_image_41024154148098.jpeg)

相似发音匹配函数

~~~
mysql> SELECT cust_name, cust_contact FROM customers WHERE Soundex(cust_contact) = Soundex('Y Lie');
~~~

日期和时间处理函数

常用日期和时间处理函数

![weread_image_41295753270182](D:\project\工作空间\mdfile\图源\weread_image_41295753270182.jpeg)

检索某一天的订单：

~~~~
mysql> SELECT cust_id,order_num FROM orders WHERE Date(order_date)='2005-09-01';
~~~~

检索某个月的订单：

~~~
mysql> SELECT cust_id,order_num FROM orders WHERE Date(order_date) BETWEEN '2005-09-01' AND '2005-09-30';
或
mysql> SELECT cust_id,order_num FROM orders WHERE Year(order_date)=2005 AND Month(order_date) =9;
~~~

数值处理函数

常用数值处理函数

![weread_image_41868923094859](D:\project\工作空间\mdfile\图源\weread_image_41868923094859.jpeg)

### 11.汇总数据

SQL聚集函数

![weread_image_8880922283589](D:\project\工作空间\mdfile\图源\weread_image_8880922283589.jpeg)

AVG()函数

~~~
mysql>  SELECT AVG(prod_price) AS avg_price FROM products;

mysql> SELECT AVG(prod_price) AS avg_price FROM products WHERE vend_id = 1003;
~~~

COUN()函数

~~~
mysql> SELECT COUNT(*) AS num_cust FROM customers;

mysql> SELECT COUNT(cust_email) AS num_cust FROM customers;
~~~

MAX()函数

~~~
mysql> SELECT MAX(prod_price) AS max_price FROM products;
~~~

MIN()函数

~~~
mysql> SELECT MIN(prod_price) AS min_price FROM products;
~~~

SUM()函数

~~~
mysql> SELECT SUM(quantity) AS items_ordered FROM orderitems WHERE order_num = 20005;

mysql> SELECT SUM(item_price*quantity) AS total_price FROM orderitems WHERE order_num = 20005;
~~~

聚集不同值

~~~
mysql> SELECT AVG(DISTINCT prod_price) AS avg_price FROM products WHERE vend_id = 1003;
~~~

组合聚集函数

~~~
mysql> SELECT COUNT(*) AS num_items,MIN(prod_price) AS price_min, MAX(prod_price) AS price_max, AVG(prod_price) AS price_avg FROM products;
~~~

### 12.分组数据

数据分组

~~~
mysql> SELECT COUNT(*) AS num_prods FROM products WHERE vend_id = 1003;
~~~

创建分组

~~~
mysql> SELECT vend_id, COUNT(*) AS num_prods FROM products GROUP BY vend_id;
~~~

过滤分组

~~~
mysql> SELECT cust_id, COUNT(*) AS orders FROM orders GROUP BY cust_id HAVING COUNT(*) >= 2;
~~~

~~~
mysql> SELECT vend_id, COUNT(*) AS num_prods FROM products WHERE prod_price >= 10 GROUP BY vend_id HAVING COUNT(*) >= 2;
~~~

分组和排序

ORDER BY 与 GROUP BY

![weread_image_11713053918655](D:\project\工作空间\mdfile\图源\weread_image_11713053918655.jpeg)

GROUP BY

~~~
mysql> SELECT order_num, SUM(quantity*item_price) AS ordertotal FROM orderitems GROUP BY order_num HAVING SUM(quantity*item_price) >= 50;
~~~

ORDER BY

~~~
mysql> SELECT order_num, SUM(quantity*item_price) AS ordertotal FROM orderitems GROUP BY order_num HAVING SUM(quantity*item_price) >= 50 ORDER BY ordertotal;
~~~

SELECT子句顺序

![weread_image_12229344321375](D:\project\工作空间\mdfile\图源\weread_image_12229344321375.jpeg)

### 13.子查询

子查询过滤

~~~
mysql> SELECT cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems WHERE prod_id = 'TNT2');
~~~

~~~
mysql> SELECT cust_name,cust_contact FROM customers WHERE cust_id IN (SELECT cust_id FROM orders WHERE order_num IN(SELECT order_num FROM orderitems WHERE prod_id = 'TNT2'));
~~~

作为计算字段使用子查询

~~~
mysql> SELECT cust_name,cust_state,(SELECT COUNT(*) FROM orders WHERE orders.cust_id = customers.cust_id) AS orders FROM customers ORDER BY cust_name;
~~~

~~~
mysql>  SELECT cust_name,cust_state,(SELECT COUNT(*) FROM orders WHERE cust_id = cust_id) AS orders FROM customers ORDER BY cust_name;
~~~

### 14.联结表

创建联结（等值联结）

~~~
mysql>  SELECT vend_name, prod_name, prod_price FROM vendors,products WHERE vendors.vend_id = products.vend_id ORDER BY vend_name, prod_name;
~~~

WHERE 子句的重要性

~~~
mysql> SELECT vend_name, prod_name, prod_price FROM vendors,products ORDER BY vend_name, prod_name;   //错误示范
~~~

错误示范表明，如果没有WHERE子句则会返回很多多余的无关输出



内部联结

~~~
mysql> SELECT vend_name, prod_name,prod_price FROM vendors INNER JOIN products ON vendors.vend_id = products.vend_id;
~~~

多表联结

~~~
mysql>  SELECT prod_name, vend_name, prod_price, quantity FROM orderitems,products,vendors WHERE products.vend_id =vendors.vend_id AND orderitems.prod_id = products.prod_id AND order_num =20005;
~~~

（同子查询 -子查询过滤第二个例子），联结的语句更简洁

~~~
mysql> SELECT cust_name,cust_contact FROM customers,orders,orderitems WHERE customers.cust_id = orders.cust_id AND orderitems.order_num = orders.order_num AND prod_id = 'TNT2';
~~~

### 15.创建高级联结

表别名

~~~
mysql> SELECT cust_name, cust_contact FROM customers AS c, orders AS o, orderitems AS oi WHERE c.cust_id = o.cust_id AND  oi .order_num = o.order_num AND prod_id = 'TNT2';
~~~

使用不同类型的联结

自联结

子查询

~~~
mysql> SELECT prod_id, prod_name FROM products WHERE vend_id = (SELECT vend_id FROM products WHERE prod_id = 'DTNTR');
~~~

联结查询

~~~
mysql> SELECT p1.prod_id, p1.prod_name FROM products AS p1, products AS p2 WHERE p1.vend_id = p2.vend_id AND p2.prod_id = 'DTNTR';
~~~

自然联结

~~~
mysql>  SELECT c.*,o.order_num, o.order_date, oi.prod_id, oi.quantity, oi.item_price FROM customers AS c, orders AS o, orderitems AS oi WHERE c.cust_id = o.cust_id AND oi.order_num = o.order_num AND prod_id = 'FB';
~~~

外部联结

左外联

~~~
mysql> SELECT customers.cust_id, orders.order_num FROM customers LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id;
~~~

右外联

~~~
mysql> SELECT customers.cust_id, orders.order_num FROM customers RIGHT OUTER JOIN orders ON customers.cust_id = orders.cust_id;
~~~

使用带聚集函数的联结

~~~
mysql> SELECT customers.cust_name,customers.cust_id,COUNT(orders.order_num) AS num_ord FROM customers INNER JOIN orders ON customers.cust_id = orders.cust_id GROUP BY customers.cust_id;
~~~

聚集函数示例

~~~~
mysql> SELECT customers.cust_name,customers.cust_id,COUNT(orders.order_num) AS num_ord FROM customers LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id GROUP BY customers.cust_id;
~~~~

### 16.组合查询

UNION

~~~
mysql> SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price <= 5 UNION SELECT vend_id, prod_id, prod_price FROM products WHERE vend_id IN (1001,1002);
~~~

包含或取消重复的行

不取消重复的行

~~~
mysql> SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price <= 5 UNION ALL SELECT vend_id, prod_id, prod_price FROM products WHERE vend_id IN (1001,1002);
~~~

对组合查询结果排序

~~~
mysql> SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price <= 5 UNION SELECT vend_id, prod_id, prod_price FROM products WHERE vend_id IN (1001,1002) ORDER BY vend_id, prod_price;
~~~

### 17.全文本搜索

启用全文本搜索（略）

~~~
mysql> CREATE TABLE productnotes (note_id int NOT NULL AUTO_INCREMENT,pro_id char(10) NOT NULL, note_date datetime NOT NULL, note_text text  NULL, PRIMARY KEY(note_id), FULLTEXT(note_text)) ENGINE=MyISAM;
~~~

进行全文本搜索

~~~
mysql> SELECT note_text FROM productnotes WHERE Match(note_text) Against('rabbit');
~~~

使用查询扩展

~~~
mysql> SELECT note_text FROM productnotes WHERE Match(note_text) Against('anvils' WITH QUERY EXPANSION);
~~~

布尔文本搜索

![weread_image_5800369256640](D:\project\工作空间\mdfile\图源\weread_image_5800369256640.jpeg)

布尔操作符的用法

匹配包含rabbit和bait的行

~~~~
mysql> SELECT note_text FROM productnotes WHERE Match(note_text) Against('+rabbit +bait' IN BOOLEAN MODE);
~~~~

匹配包含rabbit或bait的行

~~~
mysql> SELECT note_text FROM productnotes WHERE Match(note_text) Against('rabbit bait' IN BOOLEAN MODE);
~~~

匹配短语rabbit bait

~~~
mysql> SELECT note_text FROM productnotes WHERE Match(note_text) Against('"rabbit bait"' IN BOOLEAN MODE);
~~~

匹配rabbit和carrot,增加前者的等级，降低后者的等级。

~~~
mysql> SELECT note_text FROM productnotes WHERE Match(note_text) Against('>rabbit <carrot' IN BOOLEAN MODE);
~~~

匹配词safe和combination,降低后者的等级

~~~
mysql> SELECT note_text FROM productnotes WHERE Match(note_text) Against('+safe +(<combination)' IN BOOLEAN MODE);
~~~



### 18.插入数据

单行插入

~~~
mysql> INSERT INTO customers(cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country, cust_contact, cust_email) VALUES('Pep E. LAPew', '100 Main Street', 'Los Angeles', 'CA', '90046', 'USA', NULL, NULL);
~~~

多行插入

~~~
mysql> INSERT INTO customers(cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country) VALUES('Dell JPeter', '10 Main Street', 'New York', 'CA', '90038', 'USA');INSERT INTO customers(cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country) VALUES('M. Martian', '42 Galaxy Way', 'New York', 'NY', '11213', 'USA');
~~~

插入检索出的数据

~~~
INSERT INTO customers(cust_id,cust_contact,cust_email,cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country) SELECT cust_id,cust_contact,cust_email,cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country FROM custnew;
~~~

报错：ERROR 1146 (42S02): Table 'crashcourse.custnew' doesn't exist



### 19.更新和删除数据

**更新数据**

~~~
mysql> UPDATE customers SET cust_email = 'elmer@fudd.com' WHERE cust_id = 10005;
Query OK, 1 row affected (0.02 sec)
~~~

更新多个列

~~~
mysql> UPDATE customers SET cust_name = 'The Fudds', cust_email = 'elmers@fudd.com' WHERE cust_id = 10005;
Query OK, 1 row affected (0.02 sec)
Rows matched: 1  Changed: 1  Warnings: 0
~~~

删除某个列的值（设置为NULL)

~~~
mysql> UPDATE customers SET cust_email = NULL WHERE cust_id = 10005;
Query OK, 1 row affected (0.02 sec)
Rows matched: 1  Changed: 1  Warnings: 0
~~~

**删除数据**

~~~~
mysql> DELETE FROM customers WHERE cust_id = 10006;
Query OK, 1 row affected (0.03 sec)
~~~~



### 20.创建和操纵表

创建表

~~~
mysql>  CREATE TABLE users(user_id int NOT NULL AUTO_INCREMENT, user_name char(50) NOT NULL, user_address char(50) NULL,user_email char(255) NULL, PRIMARY KEY (user_id)) ENGINE=InnoDB;
Query OK, 0 rows affected (0.07 sec)
~~~

NOT NULL:不接受该列没有值， NULL：该列可以没有值或缺值。

PRIMARY KEY (user_id)：主键，值必须唯一

AUTO_INCREMENT：自增

（位于NOT NULL后面）DEFAULT 1:指定默认值为1

ENGINE=InnoDB(MyISAM,MEMORY):指定引擎为InnoDB(MyISAM,MEMORY)



更新表

添加一个列

~~~
mysql> ALTER TABLE vendors ADD vend_phone CHAR(20);
Query OK, 0 rows affected (0.06 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~

删除刚添加的列

~~~
mysql> ALTER TABLE vendors DROP COLUMN vend_phone;
Query OK, 0 rows affected (0.08 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~

删除表

~~~
mysql> DROP TABLE cuStomers2;
Query OK, 0 rows affected (0.05 sec)
~~~

重命名表

~~~
mysql> RENAME TABLE customers2 TO customers3;
Query OK, 0 rows affected (0.04 sec)
~~~

对多个表重命名

~~~
mysql> RENAME TABLE customers3 TO customers4,users TO user;
Query OK, 0 rows affected (0.08 sec)
~~~



### 21.使用视图

使用视图简化复杂的联结

~~~
mysql> CREATE VIEW productcustomers AS SELECT cust_name, cust_contact, prod_id FROM customers,orders,orderitems WHERE customers.cust_id = orders.cust_id AND orderitems.order_num = orders.order_num;
Query OK, 0 rows affected (0.06 sec)

mysql> SELECT cust_name,cust_contact FROM productcustomers WHERE prod_id = 'TNT2';
~~~

用视图重新格式化检索出的数据

~~~~
mysql> CREATE VIEW vendorlocations AS SELECT Concat(RTrim(vend_name),'(',RTrim(vend_country),')') AS vend_title FROM vendors ORDER BY vend_name;
Query OK, 0 rows affected (0.04 sec)

mysql> SELECT * FROM vendorlocations;
~~~~

使用视图过滤不想要的数据

~~~
mysql> CREATE VIEW customeremaillist AS SELECT cust_id,cust_name,cust_email FROM customers WHERE cust_email IS NOT NULL;
Query OK, 0 rows affected (0.03 sec)

mysql> SELECT * FROM customeremaillist;

~~~

使用视图与计算字段

~~~
mysql> CREATE VIEW orderitemsexpanded AS SELECT order_num,prod_id,quantity,item_price,quantity*item_price AS expanded_price FROM orderitems;
Query OK, 0 rows affected (0.03 sec)

mysql> SELECT * FROM orderitemsexpanded WHERE order_num = 20005;
~~~

更新视图（略）

### 22.使用存储过程

创建存储过程(大坑！！！！！)

~~~
mysql> DELIMITER //
mysql> CREATE PROCEDURE productpricing()
    -> BEGIN
    -> SELECT Avg(prod_price) AS priceaverage FROM products;
    -> END //
Query OK, 0 rows affected (0.03 sec)
~~~

使用存储过程

~~~~
mysql> DELIMITER ;
mysql> CALL productpricing();
~~~~

删除存储过程

~~~
mysql> DROP PROCEDURE productpricing;
Query OK, 0 rows affected (0.03 sec)
~~~

使用参数

~~~
mysql> DELIMITER //
mysql> CREATE PROCEDURE productpricing(OUT pl DECIMAL(8,2), OUT ph DECIMAL(8,2), OUT pa DECIMAL(8,2))
    -> BEGIN
    -> SELECT Min(prod_price) INTO pl FROM products; SELECT Max(prod_price) INTO ph FROM products; SELECT avg(prod_price) INTO pa FROM products;
    -> END //
Query OK, 0 rows affected (0.05 sec)
~~~

指定变量

~~~
mysql> CALL productpricing(@pricelow, @pricehigh, @priceaverage);
Query OK, 1 row affected, 1 warning (0.04 sec)
~~~

获得变量

~~~
单值
mysql> SELECT @priceaverage;
多值
mysql> SELECT @pricehigh, @pricelow, @priceaverage;
~~~

使用IN和OUT 参数

~~~
mysql> DELIMITER //
SELECT Sum(item_price*quantity) FROM orderit' at line 1
mysql> CREATE PROCEDURE ordertotal(IN onumber INT, OUT ototal DECIMAL(8,2))
    -> BEGIN
    ->  SELECT Sum(item_price*quantity) FROM orderitems WHERE order_num = onumber INTO ototal;
    ->  END //
Query OK, 0 rows affected (0.03 sec)
~~~

调用

~~~
mysql> DELIMITER ;
mysql> CALL ordertotal(20005, @total);
Query OK, 1 row affected (0.01 sec)

mysql> SELECT @total;

另一个订单
mysql> CALL ordertotal(20009, @total);
Query OK, 1 row affected (0.00 sec)

mysql> SELECT @total;
~~~

建立智能存储过程

~~~
mysql> DELIMITER //
mysql>  CREATE PROCEDURE ordertotal1(IN onumber INT, IN taxable BOOLEAN, OUT ototal DECIMAL(8,2)) COMMENT 'Obtain order total, optionally adding tax'
    -> BEGIN
    -> DECLARE total DECIMAL(8,2);
    -> DECLARE taxrate INT DEFAULT 6;
    ->  SELECT Sum(item_price*quantity) FROM orderitems WHERE order_num = onumber INTO total;
    -> IF taxable THEN SELECT total+(total/100*taxrate) INTO total;
    ->  END IF;
    ->  SELECT total INTO ototal;
    ->  END //
~~~

使用

~~~
mysql>  DELIMITER ;
mysql>  CALL ordertotal1(20005, 0, @total); SELECT @total;
Query OK, 1 row affected (0.00 sec)

mysql>  CALL ordertotal1(20005, 1, @total); SELECT @total;
~~~

检查存储过程

显示用来创建一个存储过程的CREATE语句

~~~
mysql> SHOW CREATE PROCEDURE ordertotal;
~~~

获得包括何时，由谁创建等详细信息的存储过程列表

~~~~
mysql> SHOW PROCEDURE STATUS LIKE 'ordertotal';
~~~~

### 23.游标

创建、打开、关闭一个游标

~~~~
mysql> DELIMITER //
mysql> CREATE PROCEDURE processorders()
    -> BEGIN
    -> DECLARE ordernumbers CURSOR FOR SELECT order_num FROM orders;
    -> OPEN ordernumbers;
    -> CLOSE ordernumbers;
    -> END//
Query OK, 0 rows affected (0.03 sec)
~~~~

一个完整的存储过程、游标、逐行处理以及存储过程调用其他存储过程的工作样例

~~~
mysql> CREATE PROCEDURE processorders()
    -> BEGIN
    -> DECLARE done BOOLEAN DEFAULT 0;
    -> DECLARE o INT;
    -> DECLARE t DECIMAL(8,2);
    -> DECLARE ordernumbers CURSOR FOR SELECT order_num FROM orders;
    -> DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;
    -> CREATE TABLE IF NOT EXISTS ordertotals(order_num INT, total DECIMAL(8,2));
    -> OPEN ordernumbers;
    -> REPEAT
    -> FETCH ordernumbers INTO o;
    -> CALL ordertotal(o, 1, t);
    -> INSERT INTO ordertotals(order_num, total) VALUES(o, t);
    -> UNTIL done END REPEAT;
    -> CLOSE ordernumbers;
    -> END//
Query OK, 0 rows affected (0.03 sec)
~~~

查看表（上一阶段没有创建表，原因不明）

~~~
mysql>  SELECT * FROM ordertotals// 
~~~

报错：ERROR 1146 (42S02): Table 'crashcourse.ordertotals' doesn't exist 原因不明

### 24.触发器

创建触发器（正确输入）

~~~
mysql> CREATE TRIGGER newproduct AFTER INSERT ON products FOR EACH ROW SELECT 'Product added' INTO @arg;
Query OK, 0 rows affected (0.04 sec)
或
mysql> CREATE TRIGGER newproduct AFTER INSERT ON products FOR EACH ROW SELECT 'Product added' INTO @ee;
Query OK, 0 rows affected (0.05 sec)
~~~

**注：按照书中的例子会报错：Not allowed to return a result set from a trigger，因为从MySQL5以后不支持触发器返回结果集（本人MySQL版本 8.0.28）。**

删除触发器

~~~
mysql> DROP TRIGGER newproduct;
Query OK, 0 rows affected (0.02 sec)
~~~

使用触发器

1.INSERT触发器

~~~
创建
mysql> CREATE TRIGGER neworder AFTER INSERT ON orders FOR EACH ROW SELECT NEW.order_num INTO @arg;
Query OK, 0 rows affected (0.05 sec)
测试（插入）
mysql> INSERT INTO orders(order_date, cust_id) VALUES(Now(), 10001);
Query OK, 1 row affected (0.04 sec)
显示
mysql> SELECT @arg;
~~~

2.DELETE触发器

~~~
mysql> DELIMITER //
mysql> CREATE TRIGGER deleteorder BEFORE DELETE ON orders FOR EACH ROW
    -> BEGIN
    -> INSERT INTO archive_orders(order_num, order_date, cust_id)
    -> VALUES(OLD.order_num, OLD.order_date, OLD.cust_id);
    -> END//
Query OK, 0 rows affected (0.03 sec)
~~~

3.UPDATE触发器

~~~
mysql> CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors FOR EACH ROW SET NEW.vend_state = Upper(NEW.vend_state);
Query OK, 0 rows affected (0.06 sec)
~~~

### 25.事务

控制事务处理

使用ROLLBACK(回滚)

~~~
mysql> SELECT * FROM ordertotals; START TRANSACTION; DELETE FROM ordertotals; SELECT * FROM ordertotals; ROLLBACK; SELECT * FROM ordertotals;
Empty set (0.03 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Empty set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Empty set (0.00 sec)
~~~

使用COMMIT(提交)

~~~
mysql> START TRANSACTION; DELETE FROM orderitems WHERE order_num = 20010; DELETE FROM orders WHERE order_num =20010; COMMIT;
Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.04 sec)

ERROR 1146 (42S02): Table 'crashcourse.archive_orders' doesn't exist
Query OK, 0 rows affected (0.00 sec)
~~~

使用保留点

~~~
mysql> SAVEPOINT delete1;
Query OK, 0 rows affected (0.00 sec)
~~~

回滚

~~~~
mysql> ROLLBACK TO delete1;
ERROR 1305 (42000): SAVEPOINT delete1 does not exist（无名报错）
~~~~

更改默认的提交行为

~~~
mysql> SET autocommit=0;
Query OK, 0 rows affected (0.00 sec)
~~~

### 26.全球化和本地化

使用字符集和校对顺序

查看支持的字符集列表

~~~
mysql> SHOW CHARACTER SET;
~~~

查看支持的校对的列表

~~~
mysql> SHOW COLLATION;
~~~

确认所用的字符集和校对

~~~
mysql> SHOW VARIABLES LIKE 'character%'; SHOW VARIABLES LIKE 'collation%';
~~~

给表指定字符集和校对

~~~
mysql> CREATE TABLE mytable( columnn1 INT, columnn2 VARCHAR(10)) DEFAULT CHARACTER SET hebrew COLLATE hebrew_general_ci;
Query OK, 0 rows affected (0.06 sec)
~~~

对每个列设置字符集和校对

~~~
mysql> CREATE TABLE mytable1( columnn1 INT, columnn2 VARCHAR(10), columnn3 VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_general_ci) DEFAULT CHARACTER SET hebrew COLLATE hebrew_general_ci;
Query OK, 0 rows affected (0.04 sec)
~~~

用与创建表时不同的校对顺序排序特定的SELECT语句

~~~
mysql> SELECT * FROM customers ORDER BY lastname, firstname COLLATE latin1_general_cs;
ERROR 1054 (42S22): Unknown column 'lastname' in 'order clause'（报错）
~~~

### 27.安全管理

管理用户

获取所有用户账户列表

~~~~
mysql> USE mysql;
Database changed
mysql> SELECT user FROM user;
~~~~

创建用户账号

~~~
mysql> CREATE USER ben IDENTIFIED BY 'p@$$wOrd';
Query OK, 0 rows affected (0.07 sec)
~~~

重命名用户账号

~~~
mysql> RENAME USER ben TO bforta;
Query OK, 0 rows affected (0.01 sec)
~~~

删除用户账号

~~~
mysql> DROP USER bforta;
Query OK, 0 rows affected (0.03 sec)
~~~

设置访问权限

查看赋予用户账号的权限

~~~
mysql> SHOW GRANTS FOR bforta;
~~~

授予用户权限（在crahcourse.*数据库使用SELECT访问权限）

~~~
mysql> GRANT SELECT ON crashcourse.* TO bforta;
Query OK, 0 rows affected (0.03 sec)
~~~

撤销用户的访问权限（注：被撤销的访问权限必须存在，否则会出错）、

~~~~
mysql> REVOKE SELECT ON crashcourse.* FROM bforta;
Query OK, 0 rows affected (0.03 sec)
~~~~

权限

![weread_image_2629063967432](D:\project\工作空间\mdfile\图源\weread_image_2629063967432.jpeg)



简化多次授权

~~~~
mysql> GRANT SELECT , INSERT ON crashcourse.* TO bforta;
Query OK, 0 rows affected (0.02 sec)
~~~~



更改口令

~~~~
mysql> ALTER USER bforta IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.03 sec)
~~~~



### 28.数据库维护

检查表键是否正确

~~~~
mysql> ANALYZE TABLE crashcourse.orders;
~~~~

针对许多问题对表进行检查

~~~
mysql> CHECK TABLE crashcourse.orders,crashcourse.orderitems;
~~~

诊断启动问题

~~~
help;

~~~

查看日志（略）

### 29.改善性能

1.首先， MySQL（与所有DBMS一样）具有特定的硬件建议。在学习和研究MySQL时，使用任何旧的计算机作为服务器都可以。但对用于生产的服务器来说，应该坚持遵循这些硬件建议。

2.一般来说，关键的生产DBMS应该运行在自己的专用服务器上。
3.MySQL是用一系列的默认设置预先配置的，从这些设置开始通常是很好的。但过一段时间后你可能需要调整内存分配、缓冲区大小等。（为查看当前设置，可使用SHOW VARIABLES;和SHOW STATUS;。）
4.MySQL一个多用户多线程的DBMS，换言之，它经常同时执行多个任务。如果这些任务中的某一个执行缓慢，则所有请求都会执行缓慢。如果你遇到显著的性能不良，可使用SHOW PROCESSLIST显示所有活动进程（以及它们的线程ID和执行时间）。你还可以用KILL命令终结某个特定的进程（使用这个命令需要作为管理员登录）。
5.总是有不止一种方法编写同一条SELECT语句。 应该试验联结、并、子查询等，找出最佳的方法。
6.使用EXPLAIN语句让MySQL解释它将如何执行一条SELECT语句。
7.一般来说，存储过程执行得比一条一条地执行其中的各条MySQL语句快。
8.应该总是使用正确的数据类型。
9.决不要检索比需求还要多的数据。换言之，不要用SELECT *（除非你真正需要每个列）。
10.有的操作（包括INSERT）支持一个可选的DELAYED关键字，如果使用它，将把控制立即返回给调用程序，并且一旦有可能就实际执行该操作。
11.在导入数据时，应该关闭自动提交。你可能还想删除索引（包括FULLTEXT索引），然后在导入完成后再重建它们。
12.必须索引数据库表以改善数据检索的性能。确定索引什么不是一件微不足道的任务，需要分析使用的SELECT语句以找出重复的WHERE和ORDER BY子句。如果一个简单的WHERE子句返回结果所花的时间太长，则可以断定其中使用的列（或几个列）就是需要索引的对象。
13.你的SELECT语句中有一系列复杂的OR条件吗？通过使用多条SELECT语句和连接它们的UNION语句，你能看到极大的性能改进。
14.索引改善数据检索的性能，但损害数据插入、删除和更新的性能。如果你有一些表，它们收集数据且不经常被搜索，则在有必要之前不要索引它们。（索引可根据需要添加和删除。）
15.LIKE很慢。一般来说，最好是使用FULLTEXT而不是LIKE。
16.数据库是不断变化的实体。一组优化良好的表一会儿后可能就面目全非了。由于表的使用和内容的更改，理想的优化和配置也会改变。
17.最重要的规则就是，每条规则在某些条件下都会被打破。
