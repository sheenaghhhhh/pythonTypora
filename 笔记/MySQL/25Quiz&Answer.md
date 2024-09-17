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



## 02

1. 什么是数据库

   用来组织、存储和管理数据的仓库

2. 数据库的本质是什么

   一个基于网络通信的应用程序

   作用就是在服务端能够存储数据，在客户端能够查看

3. 数据库的分类有哪些各有什么特点

   关系型数据库 彼此之间有关系 以表结构来存储数据 支持复杂的操作 增删改查

   非关系型数据库 以键值对的形式来存储数据 基于内存工作 读取数据很快 数据库关闭重启就丢失了

4. 什么是SQL语句，SQL语句有哪些分类

   SQL语句就是结构化查询语言语句

   DDL Data Definition Language 数据定义语言

   DML Data Manipulation Language 数据操纵语言

   DCL Data Control Language 数据控制语言

5. SQL语句有什么特点

   关键字不区分大小写

   如果数据库 数据表 字段名和关键字同名 用反引号圈起来

   用;结尾 关键字不跨行或简写

6. 常用的SQL语句有哪些

   ```sql
   -- 创建数据库
   create database name;
   create database if not exists name;
   create database if not exists name charset utf8mb4;
   
   -- 查看数据库
   show databases;
   show databases like "%01";
   show create database name;
   
   -- 修改数据库
   alter database name charset "utf8mb4";
   
   -- 删除数据库
   drop database if exist name;
   
   -- 切换数据库
   use name;
   
   -- 查看当前所在数据库名称
   select database();
   
   -- 建表
   create table  [if not exists]  表名 (
       字段名1    数据类型[ ( 存储空间 )    字段约束 ],
       字段名2    数据类型[ ( 存储空间 )    字段约束 ],
       字段名3    数据类型[ ( 存储空间 )    字段约束 ],
       .....
       字段名n   数据类型[ ( 存储空间 )    字段约束 ],
       primary key(一个 或 多个 字段名)
   ) [engine = 存储引擎 character set 字符集];
   
   -- 查看当前库下的所有表
   show tables;
   
   
   -- 查看当前创建表的SQL语句
   show create table name;
   show create table name \G
   
   -- 查看表结构
   describe name;
   desc name;
   
   -- 修改表字段类型
   alter table 表名 modify 字段名 字段类型(宽度);
   -- 修改表字段类型+名
   alter table 表名 change 字段名 字段类型(宽度);
   
   -- 修改表名
   alter table 原名 rename 新表名;
   
   -- 添加字段
   alter table 表名 add 字段名 字段类型;
   -- 在指定位置添加字段
   alter table 表名 add 字段名 字段类型 after 某字段名; 
   -- 开头追加
   alter table 表名 add 字段名 字段类型 first;
   
   -- 删除指定字段
   alter table 表名 drop 字段名;
   -- 删除表
   drop table name;
   -- 以绝对路径的形式操作不同的库
   create table db2.t1(id int)
   -- 重置表信息 清空数据
   truncate table name;
   
   -- 插入数据
   insert [into] 表名 [列名] values 值;
   insert [into] 表名 [列名] values 值1, 值2;
   
   -- 查看数据
   select * from name;
   select id, age from name;
   
   -- 修改
   update name set 原字段名=新字段值 where 筛选条件;
   -- 删除
   delete from name where 条件;
   
   
   ```

   

## 03

1. 什么是存储引擎，及其特点。默认存储引擎版本

   存储引擎就是面对不同文件格式对应的不同的处理机制

   InnoDB是 MySQL5.5之后的默认存储引擎 支持事务/行锁/外键

   myisam是 MySQL5.5之前的默认存储引擎 安全性弱一点 但快

2. 什么是严格模式，如何查看，如何修改

   严格模式是在操作数据的时候为了保障数据的安全性指定的一些规则

   ```sql
   -- 查看
   show variables like "%mode%";
   
   -- 修改
   set sessison sql_mode="严格模式";
   set global sql_mode="严格模式";
   ```

3. 什么是静态方法，什么是动态方法

   静态方法 - 非绑定方法 @statisticmethod 类.func() 对象.func()

   动态方法 - 绑定给类 @classmethod

   动态方法 - 绑定给对象

   一方面是定义时写法区别，另一方面是调用时的区别

4. 如何将方法属性变成数据属性

   使用@property





## 04

1. 讲讲你知道的约束条件有哪些，各有什么作用，如何使用

   null / not null 插入数据的时候是否为空

   unique 插入的数据是否为该字段的唯一值

   primary key 主键限制的规则 不能重复也不能空 有索引

   foriegn key 外键限制的规则

2. 什么是可迭代对象，什么是迭代器对象

   可迭代对象 有``__iter__``方法的对象

   迭代器对象 有``__iter__``和``__next__``方法的对象

3. 三次握手和四次挥手的流程

   ```python
   # 三次握手
   # 发生在TCP建立连接的过程中
   
   # 第一次握手: 由客户端发起(携带SYN=1, seq=x), 发送给服务端
   # 第二次握手: 由服务端发起(携带SYN=1, ACK=1, seq=y, ack=x+1), 发送给客户端
   # 第三次握手: 由客户端发起(携带ACK=1, seq=x+1, ack=y+1), 发送给服务端
   # 服务端同意建立连接
   
   
   # 四次挥手
   # 发生在TCP断开连接的过程中
   
   # 第一次挥手：由客户端发起(携带FIN=1, seq=u) ，发送给服务端
   # 第二次挥手：由服务端发起(携带ACK=1, seq=v, ack=u+1) ，发送给客户端 --- 我可能还有数据没有传输完成不允许断开
   # 第三次挥手：由服务端发起(携带FIN=1, seq=w, ack=u+1) ，发送给客户端 --- 数据传输完成了 允许断开连接
   # 第四次挥手：由客户端发起(携带ACK=1, seq=u+1, ack=w+1) ，发送给服务端
   ```

4. 127.0.0.1和localhost的区别

   127.0.0.1 是 主机的IP地址

   localhost 是 主机名 进一步解析成地址

5. 什么是mac地址，广播，ethernet，internet

   mac地址：每块网卡在被出厂的时候烧制的唯一一串Mac地址，用12位16进制数进行表示，前6位是厂商编号 后6位是流水线号

   广播：广播是一种将消息发送给同一网络中所有设备的方式，所有设备都会收到并决定是否处理该消息

   ethernet：以太网协议。是大家处在同一个局域网的分组方式标准

   internet：因特网

 



## 05 







## 06

1. 什么是索引。你都知道哪些索引，如何添加索引

   索引是存储引擎用于快速找到记录的一种数据结构

   聚簇索引、辅助索引、唯一索引、全文索引

   ```sql
   create index 索引名字 on 表名(字段名);
   ```

2. 什么是事务，事务的四大特性，如何使用事务

   事务是指一系列相关操作的集合，这些操作被视为一个不可分割的工作单元。

   原子性、一致性、隔离性、持久性；

   ```sql
   # 1.开启事务 : start transaction
   # 2.提交事务 : commit
   # 3.回滚事务 : rollback

3. 你觉得MySQL使用规范有哪些

   关键字不区分大小写

   如果数据库、数据表、字段名和关键字同名了 用反引号圈起来

   以;结尾

   关键字不跨行不简写

   日期 字符串 用单引号

   可以用空格和缩进来提高可读性

   -- 看一下昨天的东西看这个题是不是写新的

   规范设计 主键 索引等

   ![image-20240913085735873](assets/image-20240913085735873.png)

4. 行锁和表锁是什么

   行锁 ： 一个事务操作一条数据的时候其他事务没办法操作当前行

   表级锁 ： 一个事务操作一条数据的时候直接对整张表加锁