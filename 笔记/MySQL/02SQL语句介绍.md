# 02 SQL语句介绍

## 1 SQL语句的由来

```python
# 1.socket通信
# 所有的网络通信都是基于socket
# socket介于应用层和传输层之间的抽象层
# 服务端：负责接收消息处理消息
# 客户端：负责发送消息提交请求

# 2.SQL语句的由来
# 于是大家都基于 socket 通信 去写自己公司的数据库
# 操作服务端的语句都是自己公司写的 - 别人想要操作你的数据库就必须学习你们公司规定的语句

# 后来大家就约定俗成 我们使用一套通用语句来操作数据库
# SQL语句就出生了
# SQL语句是操作关系型数据库的通用语句

# 3.NoSQL语句
# NoSQL语句操作非关系型数据库的通用语句xxxxxxxxxx # 1.socket通信# 所有的网络通信都是基于socket# socket介于应用层和传输层之间的抽象层# 服务端：负责接收消息处理消息# 客户端：负责发送消息提交请求# 2.SQL语句的由来# 于是大家都基于 socket 通信 去写自己公司的数据库# 操作服务端的语句都是自己公司写的 - 别人想要操作你的数据库就必须学习你们公司规定的语句# 后来大家就约定俗成 我们使用一套通用语句来操作数据库# SQL语句就出生了# SQL语句是操作关系型数据库的通用语句# 3.NoSQL语句# NoSQL语句操作非关系型数据库的通用语句存储数据的演变过程
```



## 2 库/表/记录/表头/表单

```python
# 1.库 database
# 指在数据库管理系统重存储和组织数据的容器
# 文件夹
# 2.表 table
# 表是数据库中的一个基本组成单位 用于存储和展示数据
# excel表
# 3.记录 record
# 记录也被成为行，是表中的一个数据项
# 在表中的一行数据
# 4.标题 header
# 表中的第一行数据 通常用于描述每个字段的含义和名称
# excel表中的第一行表头
# 5.表单 form
# 表单就是用来收集和展示数据的界面 通常会将数据组织成 字典形式
```



## 小结

```python
# ● 库：
#   ○ 相当于我们的文件夹
# ● 表：
#   ○ 相当于我们的文件
# ● 记录：
#   ○ 相当于我们一行行的数据
# ● 表头：
#   ○ 表格的第一行字段
# ● 表单：
#   ○ 表头对应的每一条数据
```

