# 05 SQL语句操作

## 1 操作数据库

对数据库的增删改查

### 1.1 创建数据库

基本语法

- CREATE: 创建数据库和表等对象

```sql
create database 数据库名字; 

-- 如果当前数据库不存在则创建数据库 否则忽略
create database if not exists 数据库名字 ;

-- 创建数据库，如果数据库不存在则创建并且制定当前数据库的默认编码格式是 xxxx
-- utf8mb4 
-- laten1 : 不支持中文 
create database if not exists 数据库名字 charset 编码集名称;

# 总结
# create database name;
# create database if not exists name;
# create database if not exists name charset utf8mb4;
```

```sql
create database if not exists day01;
```

```sql
mysql> create database if not exists day01;
Query OK, 1 row affected (0.00 sec)
```

### 1.2 查看自己有哪些数据库

```sql
-- 查看当前所有创建的数据库
show databases;

-- 模糊查询，查询数据库中带有指定字符的数据库
show databases like "%01%";

-- 查看当前数据库的创建SQL
show create database 数据库名字;
```

```sql
show create database day01;
```

```sql
mysql> show create database day01;
+----------+---------------------------------------------------------------------------------------------------------------------------------+
| Database | Create Database                                                                                                                 |
+----------+---------------------------------------------------------------------------------------------------------------------------------+
| day01    | CREATE DATABASE `day01` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci */ /*!80016 DEFAULT ENCRYPTION='N' */ |
+----------+---------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

```sql
create database if not exists day02 charset "gbk";
show create database day02;
```

```sql
mysql> show create database day02;
+----------+--------------------------------------------------------------------------------------------------+
| Database | Create Database                                                                                  |
+----------+--------------------------------------------------------------------------------------------------+
| day02    | CREATE DATABASE `day02` /*!40100 DEFAULT CHARACTER SET gbk */ /*!80016 DEFAULT ENCRYPTION='N' */ |
+----------+--------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

### 1.3 修改数据库

```sql
-- 修改当前数据库的默认编码集
alter database 数据库名字 charset "utf8mb4";
-- 除了修改默认编码集 还可以修改排列规则 数据库注释 加密选项等
```

```sql
alter database day02 charset "utf8mb4";
mysql> show create database day02;
```

```sql
mysql> alter database day02 charset "utf8mb4";
Query OK, 1 row affected (0.00 sec)

mysql> show create database day02;
+----------+---------------------------------------------------------------------------------------------------------------------------------+
| Database | Create Database                                                                                                                 |
+----------+---------------------------------------------------------------------------------------------------------------------------------+
| day02    | CREATE DATABASE `day02` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci */ /*!80016 DEFAULT ENCRYPTION='N' */ |
+----------+---------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

### 1.4 删除数据库

```sql
-- 删除指定名字的数据库 如果数据库存在
drop database if exists 数据库名字;
```

```sql
mysql> drop database if exists day02;
Query OK, 0 rows affected (0.01 sec)
```

## 2 操作数据表

- 操作当前在数据库中的表

### 2.1 切换数据库

```sql
-- 切换到指定的数据库
use 数据库名字;

-- 查看当前所在数据库的名字
select database();
```

```sql
create database if not exists day01;
use day01;
select database();
```

```sql
mysql> create database if not exists day01;
Query OK, 1 row affected (0.00 sec)

mysql> use day01;
Database changed
mysql> select database();
+------------+
| database() |
+------------+
| day01      |
+------------+
1 row in set (0.00 sec)
```

### 2.2 创建数据表

```sql
-- 创建表的语法
create table  [if not exists]  表名 (
    字段名1    数据类型[ ( 存储空间 )    字段约束 ],
    字段名2    数据类型[ ( 存储空间 )    字段约束 ],
    字段名3    数据类型[ ( 存储空间 )    字段约束 ],
    .....
    字段名n   数据类型[ ( 存储空间 )    字段约束 ],
    primary key(一个 或 多个 字段名)    -- 注意，最后一段定义语句，不能有英文逗号的出现，否则报错。
) [engine = 存储引擎 character set 字符集];
```

````sql
varchar() ---> 字符串类型 1 - 255 
int() ---> 数字类型 4 默认是 8 
````

```sql
create table if not exists user (
	name varchar(10),
    age int(4)
);
```

```sql
mysql> create table if not exists user (
    -> name varchar(10),
    ->   age int(4)
    -> );
Query OK, 0 rows affected (0.02 sec)
```

### 2.3 查看当前库下的表

````sql
-- 查看当前库下的所有表
show tables;

-- 查看当前创建表的SQL语句
show create table 表名;
-- 格式化输出当前的建表语句
show create table 表名 \G;

-- 查看当前表结构
describe 表名;
desc 表名;
````

```sql
mysql> show tables;
+-----------------+
| Tables_in_day01 |
+-----------------+
| user            |
+-----------------+
1 row in set (0.00 sec)
```

```sql
show create table user;
```

```sql
mysql> show create table user;
+-------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table
   |
+-------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| user  | CREATE TABLE `user` (
  `name` varchar(10) DEFAULT NULL,
  `age` int DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------+------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.01 sec)

mysql> show create table user\G
*************************** 1. row ***************************
       Table: user
Create Table: CREATE TABLE `user` (
  `name` varchar(10) DEFAULT NULL,
  `age` int DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)
```

```sql
describe user;
```

```sql
mysql> describe user;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| name  | varchar(10) | YES  |     | NULL    |       |
| age   | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

### 2.4 修改表

```sql
-- 修改当前表的字段类型
-- 只能修改类型 不能修改名字
alter table 表名 modify 字段名 字段类型(宽度);
-- 可以修改类型和名字
alter table 表名 change 字段名 字段类型(宽度);

-- 修改表名 --- 给表重命名
alter table 原表名 rename 新表名;

-- 添加字段
-- 默认在结尾添加字段
alter table 表名 add 字段名 字段类型;
-- 指定位置追加字段
alter table 表名 add 字段名 字段类型 after 原字段名;
-- 在所有字段开头追加字段
alter table 表名 add 字段名 字段类型 first;
```

```sql
alter table user modify name varchar(255);
alter table user rename new_user;
alter table new_user add gender int(4);
alter table new_user add addr varchar(255) after name;
alter table new_user add id int(4) first;
```

```sql
mysql> alter table user modify name varchar(255);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user;
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| name  | varchar(255) | YES  |     | NULL    |       |
| age   | int(4)       | YES  |     | NULL    |       |
+-------+--------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

```sql
mysql> alter table user rename new_user;
Query OK, 0 rows affected (0.00 sec)

mysql> show tables;
+-----------------+
| Tables_in_day01 |
+-----------------+
| new_user        |
+-----------------+
1 row in set (0.00 sec)
```

```sql
mysql> alter table new_user add gender int(4);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc new_user;
+--------+--------------+------+-----+---------+-------+
| Field  | Type         | Null | Key | Default | Extra |
+--------+--------------+------+-----+---------+-------+
| name   | varchar(255) | YES  |     | NULL    |       |
| age    | int(4)       | YES  |     | NULL    |       |
| gender | int(4)       | YES  |     | NULL    |       |
+--------+--------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

```sql
mysql> alter table new_user add addr varchar(255) after name;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc new_user;
+--------+--------------+------+-----+---------+-------+
| Field  | Type         | Null | Key | Default | Extra |
+--------+--------------+------+-----+---------+-------+
| name   | varchar(255) | YES  |     | NULL    |       |
| addr   | varchar(255) | YES  |     | NULL    |       |
| age    | int(4)       | YES  |     | NULL    |       |
| gender | int(4)       | YES  |     | NULL    |       |
+--------+--------------+------+-----+---------+-------+
4 rows in set (0.01 sec)
```

```sql
mysql> alter table new_user add id int(4) first;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc new_user;
+--------+--------------+------+-----+---------+-------+
| Field  | Type         | Null | Key | Default | Extra |
+--------+--------------+------+-----+---------+-------+
| id     | int          | YES  |     | NULL    |       |
| name   | varchar(255) | YES  |     | NULL    |       |
| addr   | varchar(255) | YES  |     | NULL    |       |
| age    | int          | YES  |     | NULL    |       |
| gender | int          | YES  |     | NULL    |       |
+--------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```

### 2.5 删除字段

```sql
-- 删除指定表中的指定字段
alter table 表名 drop 字段名;

-- 删除指定的表，一旦表删除就找不回来了！
drop table 表名;

-- 以绝对路径的形式操作不同的库
create table db2.t1(id int);

-- 重置表信息,清空当前表中的所有数据
TRUNCATE table 表名;
```

```sql
alter table new_user drop addr;
drop table new_user;

create table day01.t1(id int);
```

```sql
mysql> alter table new_user drop addr;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc new_user;
+--------+--------------+------+-----+---------+-------+
| Field  | Type         | Null | Key | Default | Extra |
+--------+--------------+------+-----+---------+-------+
| id     | int          | YES  |     | NULL    |       |
| name   | varchar(255) | YES  |     | NULL    |       |
| age    | int          | YES  |     | NULL    |       |
| gender | int          | YES  |     | NULL    |       |
+--------+--------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

```sql
mysql> drop table new_user;
Query OK, 0 rows affected (0.03 sec)

mysql> show tables;
Empty set (0.00 sec)
```

- 使用 **DROP DATABASE** 命令时要非常谨慎，
- 在执行该命令后，MySQL 不会给出任何提示确认信息。
- DROP DATABASE 删除数据库后，数据库中存储的所有数据表和数据也将一同被删除，而且不能恢复。
- 因此最好在删除数据库之前先将数据库进行备份。

```sql
mysql> create table day01.t1(id int);
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+-----------------+
| Tables_in_day01 |
+-----------------+
| t1              |
+-----------------+
1 row in set (0.00 sec)

mysql> desc t1;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| id    | int  | YES  |     | NULL    |       |
+-------+------+------+-----+---------+-------+
1 row in set (0.00 sec)
```

## 3 操作数据

### 3.1 插入数据

```sql
-- 单条插入数据
INSERT [INTO] <表名> [ <列名1>] VALUES (值1);

-- 多条插入数据
INSERT INTO 表名(字段名1,字段名2,....) VALUES(字段值1,字段值2, ...),(字段值1,字段值2, ...);
```

```sql
create table t2(
	name varchar(25),
    age int(4)
);

-- 插入一条数据 名字是 Dream 年龄是 18
INSERT INTO t2 (name,age) VALUES ("dream",18);

select * from t2;

-- 插入两条数据 名字是 dream_one 18 + dream_two 28 
INSERT INTO t2 (name,age) VALUES ("dream_one",18),("dream_two",28);
```

```sql
mysql> create table t2(
    -> name varchar(25),
    ->   age int(4)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> desc t2;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| name  | varchar(25) | YES  |     | NULL    |       |
| age   | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)


mysql> insert into t2(name, age) values ("sheenagh", 23);
Query OK, 1 row affected (0.01 sec)

mysql> select * from t2;
+----------+------+
| name     | age  |
+----------+------+
| sheenagh |   23 |
+----------+------+
1 row in set (0.00 sec)
```

```sql
mysql> insert into t2 (name, age) values ("alen", 18),("hua", 30);
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from t2;
+----------+------+
| name     | age  |
+----------+------+
| sheenagh |   23 |
| alen     |   18 |
| hua      |   30 |
+----------+------+
3 rows in set (0.00 sec)
```

### 3.2 查看数据

```sql
-- 查看当前表下的所有数据
select * from 表名;

-- 按照字段查看数据
select fiel_name1,fiel_name2 from 表名;
```

```sql
-- 查看当前表下的所有名字
select name from t2;

```

```sql
mysql> select name from t2;
+----------+
| name     |
+----------+
| sheenagh |
| alen     |
| hua      |
+----------+
3 rows in set (0.00 sec)
```

### 3.3 修改表中的数据

```sql
update 表名 set 原字段名=新字段值 where 筛选条件;
```

```sql
-- 修改表 t2 中的姓名为 hua 这个人的年龄为 20 
mysql> update t2 set age=20 where name = "hua";
```

```sql
mysql> update t2 set age=20 where name = "hua";
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select name, age from t2;
+----------+------+
| name     | age  |
+----------+------+
| sheenagh |   23 |
| alen     |   18 |
| hua      |   20 |
+----------+------+
3 rows in set (0.00 sec)
```

### 3.4 删除数据

```sql
-- 删除指定条件的数据  会删除所有符合条件的数据
delete from 表名 where 条件;
```

```sql
-- 删除年龄为 18 岁的 这个人
delete from t2 where age=18;
```

```sql
mysql> delete from t2 where age=18;
Query OK, 1 row affected (0.01 sec)

mysql> select name, age from t2;
+----------+------+
| name     | age  |
+----------+------+
| sheenagh |   23 |
| hua      |   20 |
+----------+------+
2 rows in set (0.00 sec)
```

```sql
mysql> insert into t2(name,age) values("xiaohua",20);
Query OK, 1 row affected (0.01 sec)

mysql> select name, age from t2;
+----------+------+
| name     | age  |
+----------+------+
| sheenagh |   23 |
| hua      |   20 |
| xiaohua  |   20 |
+----------+------+
3 rows in set (0.00 sec)

mysql> delete from t2 where age=20;
Query OK, 2 rows affected (0.01 sec)

mysql> select name, age from t2;
+----------+------+
| name     | age  |
+----------+------+
| sheenagh |   23 |
+----------+------+
1 row in set (0.00 sec)
```


