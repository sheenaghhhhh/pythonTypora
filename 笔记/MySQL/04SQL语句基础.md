# 04 SQL语句基础

## 1 引入

- 前面的学习中我们提到，mysql是关系型数据库，
- 所以我们要操作mysql就需要使用SQL（结构化查询语言）。



## 2 SQL规范

- 在数据库管理系统中，**SQL语句关键字不区分大小写(但建议用大写)** ，参数区分大小写。
- 建议命令大写，数据库名、数据表名、字段名统一小写，**如数据库名、数据表名、字段名与关键字同名，使用反引号 圈起来**，避免冲突。
- SQL语句可单行或多行书写，**默认以英文分号（;）结尾**，**关键词不能跨多行或简写**。
- 字符串跟日期类型的值都要以 单引号括起来，单词之间需要使用半角的空格隔开。
- 用空格和缩进来提高SQL语句的可读性。



## 3 注释语法

```sql
# SQL:

-- 单行注释

# 方式一 : -- 
# 方式二 : # 

/*
多行注释
多行注释
*/
```

```python
# 对比：
# Python中的注释注释

'''
Python中的多行注释
'''
```



## 4 SQL类型

### 4.1 数据定义语言（Data Definition Language，DDL）

- 用于创建或删除数据库以及数据表的语句，DDL包含以下几种指令：
- CREATE: 创建数据库和表等对象
- DROP: 删除数据库和表等对象
- ALTER: 修改数据库和表等对象的结构

### 4.2 数据操纵语言（Data Manipulation Language，DML）

- 用于对数据表中的数据进行增删查改的。
- SELECT: 查询表中的数据
- INSERT: 向表中插入新数据
- UPDATE: 变更表中的数据
- DELETE: 删除表中的数据

### 4.3 数据控制语言（Data Control Language，DCL）

- 用于对控制数据库的操作权限的，包括用户权限以及数据操作权限。
- COMMIT: 确认对数据库中的数据进行的变更
- ROLLBACK: 取消对数据库中的数据进行的变更
- GRANT: 赋予用户操作权限
- REMOVE: 取消用户的操作权限



## 5 常用命令

| 命令   | 描述                                                     |
| ------ | -------------------------------------------------------- |
| help   | 查看系统帮助想你想                                       |
| status | 查看数据库管理系统的状态信息                             |
| exit   | 退出数据库终端连接                                       |
| quit   | 退出数据库终端连接                                       |
| \c     | 当打错命令了，想换行重新写时可以在错误命令后面跟着\c回车 |

```sql
mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.31, for macos10.14 (x86_64) using  EditLine wrapper

Connection id:		90
Current database:	
Current user:		root@
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		5.7.31 MySQL Community Server (GPL)
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	utf8mb4
Db     characterset:	utf8mb4
Client characterset:	utf8mb4
Conn.  characterset:	utf8mb4
UNIX socket:		/tmp/mysql.sock
Uptime:			19 days 21 hours 22 min 22 sec

Threads: 1  Questions: 181  Slow queries: 0  Opens: 99  Flush tables: 1  Open tables: 94  Queries per second avg: 0.000
--------------
```
