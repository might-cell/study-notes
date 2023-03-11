## 第一章 了解SQL

1、数据库：保存有组织数据的容器，通过DBMS创建和操纵的容器

2、数据库软件：DBMS（数据库管理系统）

3、表：结构化的文件，可用来存储某种特定类型的数据

4、表名：需要保证唯一性，但是在不同的数据库中可以使用相同的表名

5、模式：schema，关于数据库和表的布局及其相关特性的信息（某些情况下，模式等价于数据库）

6、列：表中的一个字段，数据库中每个列中有相应的数据类型

7、数据类型：容许的数据的类型，限制列中存储的数据，还可以帮助正确排序数据等

8、行：表中的一个记录（记录等价于行，行才是正确的术语）

9、主键：表中每一行都应该有可以唯一标识自己的一列，用来表示特定的一行

* 任意两行都不具有相同的主键值
* 每行都必须有一个主键值（主键值不允许NULL）

10、主键规则（MySQL本身强制实施）：

* 主键通常定义在表的一列上，也可以一起使用多个列作为主键

  在使用多列作为主键时，主键的条件必须应用到构成主键的所有列，即所有列值的组合必须是唯一的（单个列的值可以不唯一）

11、主键好习惯：

* 不更新主键列中的值
* 不重用主键列的值
* 不在主键列中使用可能会改变的值（例如，使用名字作为主键来标识某个学生，当学生改名后，必须更改主键）

## 第二章 MySQL简介

12、MySQL：DBMS（数据库管理软件）

* 开源免费使用，免费修改
* 执行速度非常快
* 可信赖，许多重量级公司使用MySQL处理自己的重要数据
* 易于安装和使用

13、MySQL选项和参数：在命令行仅仅输入mysql，可能会出现错误消息

​	因为需要可能需要安全证书，或者因为MySQL没有运行在本地或者默认端口上



## 第三章 使用MySQL

14、选择数据库：最初连接到MySQL时，没有任何数据库打开供自己使用，在执行任意数据库操作之前，需要选择一个数据库

​	USE关键字

```mysql
use crashcourse;
```

​	USE关键字不返回任何结果，记住，必须先使用USE打开数据库，才能对数据库中的数据库进行操作

15、了解数据库和表：事先不知道使用的数据库名，使用SHOW命令来显示信息

​	SHOW命令

```MYSQL
-- 获取当前数据库
show databases;

-- 获取一个数据库内的表
show tables;

-- 显示表中的列
show columns from customers;

-- 显示广泛的服务器状态信息
show status;
```

​	show columns要求给出一个表名，返回表设计的相关信息

16、自动增量：auto_increment，某些表列需要唯一值，MySQL可以自动为每一行分配一个可用的编号，不用再添加一行的时候手动分配唯一值

​	DESCRIBE语句

```mysql
describe customers;
```

​	简化show columns from命令的快捷方式



## 第四章 检索数据

​	SELECT语句

```mysql
select prod_name from products;
```

​	从一个表中或多个表中检索信息，这里返回的数据没有过滤，也没有排序

17、结束SQL语句：多条SQL必须以（;）分隔，不需要在单挑SQL语句后加分号，如果愿意可以总是加分号，加上分号必然没有坏处

18、SQL语句和大小写：SQL语句不区分大小写，SELECT和select等价，开发人员建议对所有的SQL关键字使用大写，对所有的列和表名使用小写，这样易于阅读和调试

19、使用空格：在处理SQL语句，其中的所有空格都被忽略，分成多行更加易于阅读和调试

```mysql
-- 检索多个列
SELECT pro_id, pro_name, prod_price FROM products;

-- 检索所有列
SELECT * FROM products;
```

20、使用通配符：除非需要表中每一列，否则不要使用*通配符，会降低检索和应用程序的性能

​	DISTINCT关键字

```mysql
SELECT DISTINCT vend_id FROM products;
```

​	对于重复的数据，使用DISTINCT关键字去重，DISTINCT关键字应用于所有列，不仅仅是前置其的列

21、限制结果：SELECT返回所有匹配的行，为了返回第一行或者前几行：

​	LIMIT子句

```mysql
SELECT prod_name FROM products LIMIT 5;
```

​	为了得出下一个5行，可以指定要检索的的开始行和行数

```mysql
SELECT prod_name FROM products LIMIT 5, 5;
```

​	第一个数为开始位置，第二个数是要检索的行数

​	带一个值的LIMIT总是从第一行开始的，给出的数就是返回的行数，带两个

22、参数的LIMIT可以指定从行号为第一个值的位置开始

23、行0：检索出来的第一行为行0，而不是行1，因此LIMIT 1, 1将检索出第二行的一条记录

24、完全限定的表名：同时使用表名和列字

```mysql
SELECT products.prod_name FROM products;
```

​	相同的，表名也可以完全限定

```mysql
SELECT products.prod_name FROM crashcourse.products;
```

​	在有一些情形下需要完全限定名



## 第五章 排序检索数据

25、子句：SQL语句有子句构成，有些子句是必须的，而有些是可选的

​	ORDER BY子句：选取一个或者多个列的名字，据此对输出进行排序

```mysql
SELECT prod_name 
FROM products 
ORDER BY prod_name;
```

​	按多个列排序，需要指定列名，列名之间使用逗号分隔开来

```mysql
SELECT prod_id, prod_price, prod_name 
FROM products
ORDER BY prod_price, prod_name;
```

​	在按多个列排序的时候，排序完全按照所规定的的顺序进行。仅在多个行具有相同的prod_price值时，才按照prod_name进行排序，如果所有的prod_price都是唯一的，那么不会按照prod_name进行排序

​	DESC关键字：降序排序

```mysql
SELECT prod_id, prod_price, prod_name 
FROM products
ORDER BY prod_price DESC;

# 对多个列排序
SELECT prod_id, prod_price, prod_name 
FROM products
ORDER BY prod_price DESC, prod_name;
```

​	DESC关键字只应用到位于其前面的列名。如果想要在多各列上进行降序排序，必须对每个列指定DESC关键字

​	ORDER BY与LIMIT结合

```mysql
SELECT prod_price FROM products
ORDER BY prod_price
LIMIT 1;
```

​	找出一个列中最高或者最低的值

26、子句的位置：

* ORDER BY子句必须位于FROM子句之后
* LIMIT必须位于ORDER BY之后
