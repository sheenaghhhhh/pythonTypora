## 01

1. 库，表，记录。表头，表单

   库：database

   在数据库管理系统重存储和组织数据的容器

   表：table

   数据库中的一个基本组成单位 用于存储和展示数据

   记录：record

   也是行，表中的一个数据线

   表头：header

   表中的第一行数据 描述每个字段的含义和名称

   表单：form

   用来收集和展示的界面 通常将数据组织成字典形式

2. SQL语句的类型

   数据定义语言 DDL

   数据操纵语言 DML

   数据控制语言 DCL

3. SQL语句的规范有哪些

   语句关键字不区分大小写

   命令大写，库表字典小写，同名用反引号圈起来

   用;结尾 关键字不可以跨行or简写

4. 写出操作数据库相关的语句

   create database databasename;

   show create database databasename;

   alter database databasename charset "utf8mb4";

   drop database if exist databasename;

5. 如何清空一张表中的数据

   truncate table tablename

6. 什么是引擎，有哪些引擎。默认引擎是哪个

   针对不同文件格式有不同存储方式和处理机制，引擎就是不同的处理机制

   InnoDB、myisam、memory、blackhole

   InnoDB是MySQL5.5 后的默认引擎