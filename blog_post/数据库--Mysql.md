---
title:数据库--Mysql
tags:
	-数据库
	-Mysql
	-java
date:2020-07-23 12:12:57
---

# 数据库--Mysql

## mysql数据库介绍

### 什么是MySQL数据库：

1:	mysql数据库是一种开放源代码的关系型数据库管理系统(RDBMS),使用最常用的数据库管理语言--**结构化查询语言(SQL)**进行数据库管理。

2:	mysql数据库开放源代码的,因此任何人都

可以在General Public License的许可下下载并根据个性化的需要对其进行修改。

3:	mysql因为其速度,可靠性和适应性而备受关注。大多数人都认为在不需要**事务化处理**的情况下,mysql是管理内容最好的数据库.

## 对SQL语句分类

### 1.数据库定义语言-DDL(Data Defination Language):

用于创建和定义数据库对象,并且将对这些对象的定义保存到数据库字典中等一系列对数据库对象的操作.

1. CREATE -- 创建
2. DROP -- 删除
3. ALTER -- 修改
4. RENAME -- 修改表名
5. TRUNCATE -- 删除表中数据
6. DESC -- 查询表中的字段
7. SHOW -- 查询该数据库信息

### 2.数据库控制语言-DCL(Data Control Language):

用于修改数据库中的权限

1. GRANT --
2. REVOKE --

### 3.数据库操纵语言-DML(DML Manipulation Language):

主要是对逻辑结构等的操作,包括表结构,视图和索引

1. SELECT -- 查询
2. INSERT --插入
3. UPDATE --更改
4. DELETE --删除
5. CALL -- 调用存储过程
6. MERGE --
7. COMMIT --提交事务
8. ROLLBACK -- 回滚事务
9. START TRANSACTION -- 开启事务



### 4.实现数据库中的基本操作:



```sql
net start mysql80 #启动mysql服务
mysql -u root -p #连接数据库
```

 <img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200608235052939.png" alt="image-20200608235052939" style="zoom:50%;" />

#### DDL的操作:

```mysql
create database test_1 character set utf8MB4;  #创建数据库 test_1
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200608235402348.png" alt="image-20200608235402348" style="zoom: 80%;" />

​	

```mysql
drop database test_1;	#删除数据库  test_1
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200608235636336.png" alt="image-20200608235636336" style="zoom:80%;" />

```mysql
show databases;		#查询mysql服务器下的所有数据库
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609000021202.png" alt="image-20200609000021202" style="zoom:80%;" />

```mysql
show create database test_1;	#查看某一个数据库的详细信息
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609000501264.png" alt="image-20200609000501264" style="zoom: 80%;" />

```mysql
use test_1;		#使连接并使用test_1数据库
select database();	#查询现在用户连接的是哪个数据库
```

![image-20200609000703300](C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609000703300.png)

```mysql
alter database test_1 character set utf8MB4		#改变库的编码
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609001117190.png" alt="image-20200609001117190" style="zoom:80%;" />

#### DCL操作:

#### DML操作:

```mysql
#创建表
create table test_table_1(
	S_ID int auto_increment primary key comment '序列号',
    S_NAME varchar(40) not null unique comment '学生姓名',
    S_NUMBER bigint(20) not null unique comment '学号',
    S_MATH FLOAT COMMENT '数学成绩'
)
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609004324106.png" alt="image-20200609004324106" style="zoom:80%;" />

```mysql
drop table test_table_1;	#删除表 drop table 表名
desc test_table_1;	#查询表中的字段信息
show create table test_table_1;	#打印一张表的创建信息
```



```mysql
#对表名和表中字段进行的操作
#修改表名
rename table test_table_1 to test_table_2;
#添加字段
alter table test_table_2 add S_CHINESE float;
#删除字段
alter table test_table_2 drop S_CHINESE;
#对字段进行重命名
alter table test_table_2 change S_ID id int;
#对字段的数据类型进行修改
alter table test_table_2 change S_MATH S_MATH int;
```



```MYSQL
#插入数据

#第一种插入:当有括号写字段名是可以以随意设置插入的字段值
	insert into test_table_1(S_NAME,S_NUMBER,S_MATH) values('裴一一','1913040433','90');
	insert into test_table_1(S_ID,S_NAME,S_NUMBER,S_MATH) values('2','裴晨栋','1913040432','80');	
#第二种插入:set来确定数据
    insert into test_table_1 set S_NAME='裴一发儿',S_NUMBER='1913040434',S_MATH='100';
#第三种插入:连续插入多条数据
	insert into test_table_1(S_NAME,S_NUMBER,S_MATH) values('裴一二','1913040435','110'),('裴一三','1913040436','120'),('裴一四','1913040437','120');
#当表名后不带字段值是必须添加完成的字段数据
insert into user value (null,'张三','123');
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609005816157.png" alt="image-20200609005816157" style="zoom:80%;" />

```mysql
#更改操作
update test_table_1 set S_NUMBER='1913040438' WHERE S_ID='6';	#更改
update test_table_1 set S_NUMBER='1913040437' WHERE S_ID='6';	#改回
```

更改前:

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609010411691.png" alt="image-20200609010411691" style="zoom:80%;" />

更改后:

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609010517045.png" alt="image-20200609010517045" style="zoom:80%;" />

``` MYSQL
#删除操作
delete from test_table_1  where S_ID='6';
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609011220900.png" alt="image-20200609011220900" style="zoom:80%;" />

```mysql
#查找
select * from test_table_1;
select S_ID,S_NAME,S_NUMBER FROM test_Table_1;
select S_NUMBER as 学号 FROM test_table_1 where S_NAME='裴晨栋';
```

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200609011858705.png" alt="image-20200609011858705" style="zoom:80%;" />

其余的DML操作会在后面展示;

## 对数据库进行备份与还原

### 1.数据库备份

```mysql
#数据库备份-格式：mysqldump -h主机名  -P端口 -u用户名 -p密码 –database 数据库名 > 文件名.sql
mysqldump -uroot -p test_1>C:\test_1_back.sql
#备份数据库中的指定表 若为空则备份整个数据库
mysqldump -uroot -p test_1 test_table_1 test_table_2 >C:\test_1_back.sql
#同时备份多个数据库
mysqldump -uroot -p --databases test_1 04_mysql>test_1_back.sql
#同时备份所有数据库
mysqldump –all-databases > test_1_back.sql
```

### 2.数据库还原

```mysql
#还原备份的文本数据:首先进入到mysql环境-->创建一个数据库-->在库下还原数据-->source 备份的数据库脚本
create database test_1_back;
#第一种需要在某一数据库下使用
source C:\test_1_back.sql;
#第二种在全局使用 mysqldump -h主机名  -P端口 -u用户名 -p密码 –database 数据库名 < 文件名.sql
mysql -uroot -p test_1_back < C:\test_1_back.sql;
```

## 事务

### 概念

> ​			处理一组信息的操作集合构成一个事务

### 事务的四个条件

#### 			**原子性**

> ​				一个事务中的所有操作,要么全部完成不会结束在中间某个环节.事务在执行过程中出现错误,会被回滚到事务开始前的状态,就像这个事务从来没有执行过一样.

#### 			一致性

> 在事务开始和结束以后,数据库的完整性没有被破坏.

#### 			**隔离性**

> 数据库允许多个并发事务同时对其数据进行读写和修改能力,隔离可以防止多个事务并发执行时由于交叉执行而导致数据的不一致.事务隔离分为不同级别,包括:读未提交(Read uncommitted) ,读提交(read committed) ,可重复读(repeatable read) 和串行化(Serializable);

#### 			**持久性:**

> 事务处理结束后,对数据的修改是永久的,几遍系统故障也不会丢失.

```mysql
#开启事务
start transaction
#回滚事务
rollback
#提交事务
commit
#查询mysql软件的事务的隔离界别
slelct @@transaction_isolation
#设置mysql软件默认的隔离级别
set global transaction isolation 事务级别名

```

| 事务隔离级别               | 脏读 | 不可重复/ | 幻读 |
| -------------------------- | :--- | --------- | ---- |
| 读未提交(read uncommitted) | 是   | 是        | 是   |
| 不可重复读(read commited)  | 否   | 是        | 是   |
| 可重复读(repeatable read)  | 否   | 否        | 是   |
| 串行化(serializable)       | 否   | 否        | 否   |

### 脏读

> 事务中的修改,即使没有提交,对其它事务也是都可见的,事务可以读取未提交的数据,这也被称为脏读.

| 时间 | 客户端A                                                      | 客户端B                               |
| ---- | ------------------------------------------------------------ | ------------------------------------- |
| T1   | 设置事务的隔离级别为read uncomitted                          | 设置事务隔离级别为read uncommited     |
| T2   | 开始事务A     start transaction                              |                                       |
| T3   | 裴 一 一数学成绩提高10分 update test_table_2 set S_MATH=S_MATH+10; |                                       |
| T4   |                                                              | 开始事务                              |
| T5   |                                                              | select * from test_table_2 where id=1 |
| T6   |                                                              | 提交事务 commit                       |
| T7   | 回滚 rollback                                                |                                       |
| T8   | select * from test_table_2 where id=1                        |                                       |
| T9   | 提交事务 commit                                              |                                       |

### 不可重复读

> 一个事务从开始到结束所做的任何修改在任何时候都是不可见的,只有结束后才能被其它事务看到,但有时执行
>
> 同样的查询是会得到不同的结果.

​		

| 时间 | 客户端A                         | 客户端B                       |
| ---- | ------------------------------- | ----------------------------- |
| T1   | 设置事务隔离级别为read commited | 设置事务隔离级别read commited |
| T2   | 开始事务A                       |                               |
| T3   | 查询id=1的信息                  |                               |
| T4   |                                 | 开始事务                      |
| T5   |                                 | id=1的学生的成绩加10          |
| T6   |                                 | 事务B提交                     |
| T7   | 查询id=1的信息                  |                               |
| T8   | 事务A提交                       |                               |

​	两次查询结果不一,第一次查询为90第二次查询为100,但都是在一个事务内,因为有B事务更改id=1的学生的数学成绩此时A事务可以查询到数据更改.说明不可重复读可以查询别的事务已经提交的和本事务相同对象的的结果.

### 幻读

> 当某一事务在读取某个范围内的记录时,另一个事务又在该范围内插入了新的记录,当之前的书屋再次读取该范围的记录时,会产生幻行.InnoDB存储引擎通过多版本控制并发控制(MVCC)解决了幻读问题

| 时间 | 客户端A                           | 客户端B                           |
| ---- | --------------------------------- | --------------------------------- |
| T1   | 设置事务隔离界别为repeatable read | 设置事务隔离界别为repeatable read |
| T2   | 开始事务A                         |                                   |
| T3   |                                   | 开始事务B                         |
| T4   |                                   | 插入一个新的信息                  |
| T5   |                                   | 事务B提交                         |
| T6   | 查询表并未新插入的信息            |                                   |
| T7   | 插入一行和B插入的同样的信息       |                                   |
| T8   | 出现报错                          |                                   |

## 数据类型

### 整数和浮点数

| 类型         | 大小    | 范围(有符号)                                                 | 范围(无符号)                                                 | 用途         |
| ------------ | ------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------ |
| tinint       | 1 byte  | (-128,127)                                                   | (0，255)                                                     | 小整数       |
| smallint     | 2 bytes | (-32768,32767)                                               | (0，65 535)                                                  | 大整数       |
| mediumint    | 3 bytes | (-8 388 608，8 388 607)                                      | (0，16 777 215)                                              | 大整数       |
| int或integer | 4 bytes | (-2 147 483 648，2 147 483 647)                              | (0，4 294 967 295)                                           | 大整数       |
| bigint       | 8 bytes | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)      | (0，18 446 744 073 709 551 615)                              | 极大整数     |
| float        | 4 bytes | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | (0，1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度浮点数 |
| double       | 8 bytes | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度浮点数 |

### 时间

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200611215433933.png" alt="image-20200611215433933" style="zoom:80%;" />



### 字符串类型

<img src="C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200611215520525.png" alt="image-20200611215520525" style="zoom:80%;" />

![image-20200611215949949](C:%5CUsers%5Cpeichendong%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200611215949949.png)

## 聚合函数

```mysql
#在mysql中使用函数使用select 关键字调用:select 函数名(字段) from 表名
#找出最大值
SELECT MAX(字段) AS 最大值 FROM 表名
#找出最小值
SELECT MIN(字段) AS 最低值 FROM 表名
#求平均数
SELECT AVG(字段) AS 平均值 FROM 表名
#求和
SELECT SUM(字段) AS 总值 FROM 表名
#统计记录: 如果字段的值为U=NULL则此字段对应的数据条数不在统计之内.
#为解决上述问题:在统计某一张表中的所有数据记录是最好使用
COUNT(*);

```

## 常用函数

```mysql
#获取当前系统时间,时间格式包括时分秒
SELECT NOW() AS 当前系统时间
#获取当前的时分秒
SELECT CURTIME();
#获取当前的年月日
SELECT CURDATE();

#向上取舍
SELECT CEIL(2.3)
#得3
#向下取舍
SELECT RAND(2.3)
#得2

#随机数 不用接受参数,返回0-1之间的小数
SELECT RAND();
#时间格式函数 时间格式可以修改
DATE_FORMAT(时间字段名,'%Y/%m/%d/ %H:%i:%s') AS 时间字段别名
```

## 查询

### 升序降序

```mysql
USE bank;
#降序(DESC)
SELECT * FROM test_table_1 ORDER BY S_NUMBER DESC;
#升序(ASC)
SELECT * FROM test_table_1 ORDER BY S_NUMBER ASC;
```



### 同时查询多条语句

```mysql
select S_NAME where S_ID=1 or S_ID=2 or S_ID=3;
select S_NAME where S_ID in(1,2,3);
select S_NAME WHERE S_ID NOT IN(1,2,3);
```

### 分组查询

```mysql
#查询数学成绩中各种出现的分数
select S_MATH from test_table_1 group by S_MATH;
#查询数学成绩中是否有150分
select S_MATH from test_table_1 where S_MATH=150;
#查询数学成绩中是否有150分,通过group by 查询
select S_MATH from test_table_1 group by S_MATH having S_MATH=150; 
```

### 分页查询

```mysql
#在test_table_1表中有7条数据记录 每页显示3条
SELECT * FROM test_table_1 LIMIT 0,3;
#获取第二页数据
SELECT * FROM test_table_1 LIMIT 3,3; 
#获取第三页的数据
SELECT * FROM test_table_1 LIMIT 6,3;
```

### 分表查询

```mysql

select * from 表1，表2，表3...表n where 条件

seelt 字段名1,字段名2,字段名3,字段名4 from 表1,表2,表3 where 条件

# union union all:可以将两个查询语句的结果进行合并,合并的前提是两个查询语句的数据结构是一样的
#union可以自动去重
#union all 不能够去重

```

### 模糊查询

```mysql
#查询test_table_1表中以裴子开头的所有名字
select * from test_table_1 where S_NAME LIKE '裴%'
#查询test_table_1表中以一为名字中间部分的名字
select * from test_table_1 where S_NAME like '%一%'
#查询test_table_1表中以儿结尾的名字
select * from test_table_1 where S_NAME like '%儿'
```

### 内外连接

> 内连接
>
> 关键字：inner join on
>
> 语句：select * from a_table a inner join b_table bon a.a_id = b.b_id;
>
> 说明：组合两个表中的记录，返回关联字段相符的记录，也就是返回两个表的交集（阴影）部分。

> 外连接
>
> 关键字：left join on / left outer join on
>
> 语句：select * from a_table a left join b_table bon a.a_id = b.b_id;
>
> left join 是left outer join的简写，它的全称是左外连接，是外连接中的一种。
>
> 左(外)连接，左表(a_table)的记录将会全部表示出来，而右表(b_table)只会显示符合搜索条件的记录。右表记录不足的地方均为NULL。

> ​		右连接
>
> 关键字：right join on / right outer join on
>
> 语句：select * from a_table a right outer join b_table b on a.a_id = b.b_id;
>
> 说明：
>
> right join是right outer join的简写，它的全称是右外连接，是外连接中的一种。
>
> 与左(外)连接相反，右(外)连接，左表(a_table)只会显示符合搜索条件的记录，而右表(b_table)的记录将会全部表示出来。左表记录不足的地方均为NULL。

## 视图

> 在真实表中构建的一个虚表

​	

```mysql
create view view_all 
as select t.S_ID,t.S_NAME,t.S_NUMBER,t.S_MATH
from test_table_1 t
WHERE t.S_ID in(1,2,3);
#对视图的增删改查与对表的增删改查一模一样
```

## 存储过程

```mysql
#存储过程的语法:

DELIMITER //
CREATE PROCEDURE 存储过程名(参数名1参数类型1,参数名2,参数类型2)
BEGIN	
	代码块;
END//
DELIMITER;

DELIMITER //
CREATE PROCEDURE 存储过程名(IN 输入参数名 参数类型,OUT 输出参数名 参数类型)
BEGIN
	代码块;
END //
DELIMITER;	

#调用存储过程: call 存储过程名()

#几种循环结构
https://blog.csdn.net/qq_41453285/article/details/90180268
```

