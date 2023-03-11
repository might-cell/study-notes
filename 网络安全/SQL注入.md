SQL Injection Base

<img src="C:/Users/%E4%BF%AE%E9%9B%AF%E5%A4%A9/AppData/Roaming/Typora/typora-user-images/image-20230310211510163.png" alt="image-20230310211510163" style="zoom:67%;float:left" />



information_schema

信息库，保存mysql服务器维护的所有其他数据库的信息：

* 数据库名
* 数据库表、
* 表字段的数据类型与访问权限
* ...



| 表名       | 简介                            |
| ---------- | ------------------------------- |
| SCHEMAMETA | 当前MYSQL实例中所有的数据库信息 |
| TABLES     | 关于数据中表的信息              |
| COLUMNS    | 关于表中列信息                  |



mysql

MYSQL的核心数据库，存储如下信息：

* 用户
* 权限设置
* 关键字
* ...



performance_schema

内存数据库，数据放在内存中直接操作的数据库，内存读写速度高于磁盘读写速度



sys

功能如下：

* 查询谁使用了最多的资源（基于IP和用户）

* 查询哪张表被访问最多