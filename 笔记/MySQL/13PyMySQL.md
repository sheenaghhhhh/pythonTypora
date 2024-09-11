# 13 PyMySQL

## 1 pymysql模块

```python
# 1.什么是DB-API
# ● Python标准数据库规范为 DB-API， DB-API定义了一系列必须的对象和数据库操作方式，以便为各种数据库系统和数据库访问程序提供一致的访问接口。
#
# 2.数据库操作模块
# ● 开发人员将接口封装成不同的数据库操作模块，不同的数据库需要不同数据库操作模块
# ● 例如，MySQL数据库，它对应以下操作模块：https://wiki.python.org/moin/MySQL
 
# （1）MySQL for Python (MySQL官方给我们提供的模块 -- 但是我们不用)
# URL
# http://sourceforge.net/projects/mysql-python

# License
# GNU General Public License (GPL), Python License (CNRI Python License), Zope Public License
# Platforms
# OS Independent
# Python versions
# 2.3 - 2.6
# PyPI
# https://pypi.org/project/MySQL-python/

# MySQL on-line documentation, additional forums (maintainer does not currently read these)

# （2）mysqlclient （这个是一个补丁 -- 后面学Django的时候用）
# URL
# https://github.com/PyMySQL/mysqlclient-python

# License
# GPL
# Platforms
# OS Independent
# Python versions
# Python 2.7 and 3.4+
# PyPI
# https://pypi.org/project/mysqlclient/

# mysqlclient is a fork of MySQL-python. It adds Python 3 support and fixed many bugs. It is the MySQL library that is recommended by the Django documentation.

# （3）PyMySQL
# URL
# http://www.pymysql.org/

# License
# MIT
# Platforms
# OS Independent, CPython 2.x and 3.x, PyPy, Jython, IronPython

# Python versions
# 2.4 - 3.2
# PyPI
# https://pypi.org/project/PyMySQL/

# Pure-Python focused on simplicity and compatibility
# Virtually 100% compatible with MySQLdb
# Good performance
```



## 2 安装

```python
# 【一】安装 pymysql
# pip install PyMySQL

# 【二】导入模块
import pymysql
from pymysql.cursors import Cursor, DictCursor

# 【三】创建数据库链接
# 命名关键字 ---> * 号之前无所为 * 号之后必须按照关键字传递参数
conn = pymysql.connect(
    # 当前数据库的用户名
    user="root",
    # 当前数据库的密码
    password="1314521",
    # 连接到的数据库的名字
    database="emp_data",
    # 连接数据库的IP
    host="127.0.0.1",
    # 连接数据库的PORT
    port=3306,
    # 当前数据库的编码格式
    charset="utf8mb4",
    # cursorclass 对返回的数据进行进一步处理的方式
    # cursorclass=Cursor # 返回的数据是元组类型 ---> 不带字段名
    cursorclass=DictCursor  # 返回的数据是字典类型 ---> 带着字典名和字段值 之间映射拿出来
)
'''
user        当前MySQL的用户名
password    当前MySQL的密码
host        当前MySQL的IP 127.0.0.1 / 服务器公网IP
database    当前要操作的数据库的名字
unix_socket 不需要指定 本质上就是 TCP 的 AF_INIT + SOCKET_STREAM
port        当前MySQL的 port 端口 -- 3306 
charset     当前MySQL的 编码格式 （当前数据库的）
collation   不需要管
sql_mode    不需要管
read_default_file 不需要管
conv        不需要管
use_unicode 不需要管
client_flag 不需要管
cursorclass 默认对返回的数据的处理结果 ---> Cursor(返回的是列表+元组)  / DictCursor (字典)
init_command 不需要管
connect_timeout 不需要管，连接超时
read_default_group 不需要管
autocommit  需要了解 ---> 自动提交事务 
local_infile 不需要管
max_allowed_packet 不需要管
defer_connect 不需要管
auth_plugin_map 不需要管
read_timeout 不需要管
write_timeout 不需要管
bind_address 不需要管
binary_prefix 不需要管
program_name 不需要管
server_public_key 不需要管
ssl 不需要管
ssl_ca 不需要管
ssl_cert 不需要管
ssl_disabled 不需要管
ssl_key 不需要管
ssl_verify_cert 不需要管
ssl_verify_identity 不需要管
compress 不需要管
named_pipe 不需要管
passwd 当前MySQL的密码 如果配了上面的下面的就可以不用管
db  当前要操作的数据库的名字 如果配了上面的下面的就可以不用管
'''

# 【三】如何确认当前配置的参数没问题？
#   直接执行代码 如果不报错就表示参数没问题！

# 【四】操作数据库
# 【1】创建一个 cursor 对象来操作数据库
cursor = conn.cursor()
# print(cursor)  # <pymysql.cursors.Cursor object at 0x10cba5d50>

# 【2】书写SQL语句
sql = "select * from emp;"

# 【3】执行SQL语句
cursor.execute(sql)

# 【4】获取到SQL语句执行之后得到的结果
result = cursor.fetchone()  # limit 1 只拿第一条数据 --- 管道很像 每一次那一条 下一次基于上一次再向下去一条
# result = cursor.fetchall()  # 拿所有数据

print(result)
```



## 3 操作

```python
import pymysql
from pymysql.cursors import Cursor, DictCursor

'''
drop if exists database user_data;
create database user_data;
use user_data;
create table user(
    id int primary key auto_increment ,
    username varchar(32) unique not null, 
    password varchar(32)
);
insert into user(username,password) values("dream","521521"),("hope","369369");
'''


class MysqlHandler(object):
    def __init__(self):
        # 创建数据库链接
        self.conn = pymysql.connect(
            # 当前数据库的用户名
            user="root",
            # 当前数据库的密码
            password="1314521",
            # 连接到的数据库的名字
            database="user_data",
            # 连接数据库的IP
            host="127.0.0.1",
            # 连接数据库的PORT
            port=3306,
            # 当前数据库的编码格式
            charset="utf8mb4",
            # cursorclass 对返回的数据进行进一步处理的方式
            # cursorclass=Cursor # 返回的数据是元组类型 ---> 不带字段名
            cursorclass=DictCursor,  # 返回的数据是字典类型 ---> 带着字典名和字段值 之间映射拿出来
            autocommit=False,  # 默认是 False 是需要我们主动写 conn.commit()
            # 如果 autocommit=True 效果就是不需要我们主动提交事务 代码会自动帮我们提交事务
        )

        # 创建游标对象
        self.cursor = self.conn.cursor()

    # 数据库查询操作
    def search_database_option(self):
        # 【1】原生SQL
        sql = "select * from user;"
        # 执行原生SQL
        self.cursor.execute(sql)
        # 【2】查询方法
        # （1）查询单条数据 fetchone
        '''
        result = self.cursor.fetchone()
        # {'id': 1, 'username': 'dream', 'password': '521521'}
        result = self.cursor.fetchone()
        # {'id': 2, 'username': 'hope', 'password': '369369'}
        # fetchone : 每一次只获取一条数据 并且 是基于上一条数据获取到的 下一条 数据
        '''
        # （2）查询所有数据 fetchall
        '''
        result = self.cursor.fetchall()
        # fetchall : 一次性将表中的所有数据结果取出来 返回的结果是 根据配置的 cursorclass 决定
        # Cursor(返回的就是元组包裹着每一个元祖数据) : ((1, 'dream', '521521'), (2, 'hope', '369369'))
        # DictCursor(返回的是列表中包裹着每一个字典数据) :  [{'id': 1, 'username': 'dream', 'password': '521521'}, {'id': 2, 'username': 'hope', 'password': '369369'}]
        '''
        # （3）查询多条数据 fetchmany
        '''
        result = self.cursor.fetchmany(size=2)
        # [{'id': 1, 'username': 'dream', 'password': '521521'}, {'id': 2, 'username': 'hope', 'password': '369369'}]
        # fetchmany : 根据 size 的数量从结果中获取出指定数据量的结果 等价于 limit 
        '''
        # （4）移动光标 scroll
        # self.cursor.scroll(value, mode)
        # value : 移动多少个 索引
        # mode : 按照那种方式移动 relative 相对当前索引向后移动 absolute 相对于起始位置向后移动
        # [1]相对位置移动 relative
        '''
        result_one = self.cursor.fetchone()
        self.cursor.scroll(1, "relative")
        result_two = self.cursor.fetchone()
        print(result_one)  # {'id': 1, 'username': 'dream', 'password': '521521'}
        print(result_two)  # {'id': 3, 'username': 'opp', 'password': '369369'}
        '''
        '''
        # [2]以开头的绝对位置移动 absolute
        result_one = self.cursor.fetchone()
        self.cursor.scroll(0, "absolute")
        result_two = self.cursor.fetchone()
        print(result_one)  # {'id': 1, 'username': 'dream', 'password': '521521'}
        print(result_two)  # {'id': 1, 'username': 'dream', 'password': '521521'}
        '''

        return self.cursor.fetchall()

    # 插入数据操作
    def insert_data_option(self):
        # 【1】普通插入
        '''
        sql = f"insert into user(username,password) values('dream_one','369369');"
        # 正常执行SQL语句后会发现在数据库中并没有刚才插入的那条数据！
        self.cursor.execute(sql)
        '''
        # 【2】如果使用字符串的格式化输出语法一定记得 要加 ''
        '''
        username = "dream_two"
        password = "369369"
        # 如果用字符串的格式化输出语法的时候一定要 对 {} 加 ''
        # sql = f"insert into user(username,password) values({username},{password});"
        # print(sql)  # insert into user(username,password) values(dream_two,369369);
        # pymysql.err.OperationalError: (1054, "Unknown column 'dream_two' in 'field list'")

        sql = f"insert into user(username,password) values('{username}','{password}');"
        print(sql)  # insert into user(username,password) values('dream_two','369369');
        '''
        # 【3】为了解决上面的插入问题 ， pymysql 提供了 sql 的格式化输出语法
        username = "dream_4"
        password = "369369"
        # （1）按照位置 传入参数
        '''
        转换说明符	            解释
        %d、%i	        转换为带符号的十进制数
        %o	            转换为带符号的八进制数
        %x、%X	        转换为带符号的十六进制数
        %e	            转化为科学计数法表示的浮点数（e 小写）
        %E	            转化为科学计数法表示的浮点数（E 小写）
        %f、%F	        转化为十进制浮点数
        %g	            智能选择使用 %f 或 %e 格式
        %G	            智能选择使用 %F 或 %E 格式
        %c	            格式化字符及其ASCII码
        %r	            使用 repr() 函数将表达式转换为字符串
        %s	            使用 str() 函数将表达式转换为字符串
                '''
        '''
        sql = 'insert into user(username,password) values(%s,%s);'
        # 正常执行SQL语句后会发现在数据库中并没有刚才插入的那条数据！
        self.cursor.execute(sql, [username, password])
        # If args is a list or tuple, %s can be used as a placeholder in the query.
        '''
        # （2）按照关键字传入参数
        '''
        sql = 'insert into user(username,password) values(%(name)s,%(pwd)s);'
        # 正常执行SQL语句后会发现在数据库中并没有刚才插入的那条数据！
        self.cursor.execute(sql, {"name": username, "pwd": password})
        # If args is a dict, %(name)s can be used as a placeholder in the query.
        '''

        # 正常执行完 SQL 之后要提交事务
        self.conn.commit()

    # 更新数据操作
    def update_data_option(self):
        # 原生sql 更新
        # update 表名 set 字段名='新的值' where 条件;
        # sql = "update user set username='dream_1' where username='dream_one';"
        # self.cursor.execute(sql)

        # 上面插入数据中的按照位置和关键字同样在更新中适用
        sql = "update user set username=%(new_username)s where username=%(old_username)s;"
        self.cursor.execute(sql, {"new_username": "dream_2", "old_username": "dream_two"})
        self.conn.commit()

    # 删除数据
    def delete_data_option(self):
        # 原生sql
        # delete from 表名 where 条件
        sql = "delete from user where username=%(del_name)s;"
        self.cursor.execute(sql, {"del_name": "dream_4"})
        self.conn.commit()

    # 批量向数据库中插入数据
    def insert_many_data_option(self):
        # 【1】写原生的sql
        # insert into user(username,password) values(),(),();

        # 【2】按照关键字传数据
        '''
        sql = "insert into user(username,password) values(%(username)s,%(password)s);"
        self.cursor.executemany(sql, [
            {"username": "1", "password": "1"},
            {"username": "2", "password": "2"},
        ])
        '''

        # 【3】按照位置传入参数
        sql = "insert into user(username,password) values(%s,%s);"
        self.cursor.executemany(sql, [
            ("3", "3"),
            ("4", "4"),
        ])

        self.conn.commit()

    # SQL注入问题
    def sql_insert_problem(self):
        ...
        # 【一】模拟登陆 输入用户名和密码
        username = input("请输入用户名 :>>> ").strip()
        password = input("请输入密 码 :>>> ").strip()
        # 【二】去数据库中查询到当前的用户名和密码对应的数据
        sql = f"select * from user where username='{username}' and password='{password}';"
        print(f"sql :>>>>> {sql}")
        self.cursor.execute(sql)
        result = self.cursor.fetchall()
        print(f"result :>>>>> {result}")
        if result:
            print(f"登陆成功!")
        else:
            print(f"用户名和密码错误!登陆失败!")

        '''
        请输入用户名 :>>> dream
        请输入密 码 :>>> 521521
        sql :>>>>> select * from user where username='dream' and password='521521';
        result :>>>>> [{'id': 1, 'username': 'dream', 'password': '521521'}]
        登陆成功!
        '''

        # 当在SQL语句中出现 -- + 空格 的时候就表示当前是注释内容
        # select * from user where username='dream ';
        '''
        请输入用户名 :>>> dream '-- xxx'
        请输入密 码 :>>> xxx
        sql :>>>>> select * from user where username='dream '-- xxx' and password='xxx';
        result :>>>>> [{'id': 1, 'username': 'dream', 'password': '521521'}]
        登陆成功!
        '''

        # 总结：
        # SQL注入问题就是利用SQL语句的漏洞
        # 【1】比如我们 经常使用的用户名或密码 admin + 123456 / root + 123456 登入数据库 对数据库加密 ---> 再次登入数据库就会收到 勒索信息
        # 当前数据库已经被锁定 你需要 发送 多少钱到指定的用户获取到解锁码 解锁数据
        # 【2】我们在查询数据的时候 如果使用自己的字符串拼接语法 会将我们的 -- 作为注释内容 导致SQL失效 ---> 不需要知道密码 只需要用户名就可以操作数据

    # SQL问题解决方案
    def sql_insert_no_problem(self):
        ...
        # 【一】模拟登陆 输入用户名和密码
        username = input("请输入用户名 :>>> ").strip()
        password = input("请输入密 码 :>>> ").strip()
        # 【二】去数据库中查询到当前的用户名和密码对应的数据
        sql = "select * from user where username=%(username)s and password=%(password)s;"
        print(f"sql :>>>>> {sql}")
        self.cursor.execute(sql, {"username": username, "password": password})
        result = self.cursor.fetchall()
        print(f"result :>>>>> {result}")
        if result:
            print(f"登陆成功!")
        else:
            print(f"用户名和密码错误!登陆失败!")
        # 有问题就得有解决问题：所以出现了我们学到的SQL语句的 站位语法 %s %(name)s
        # 在使用 pymysql 提供给我们的格式化输出语法的时候 execute 方法会过滤掉 有漏洞的 语句内容 比如注释语法

    def __del__(self):
        self.cursor.close()
        self.conn.close()


if __name__ == '__main__':
    handle = MysqlHandler()
    # handle.insert_data_option()
    # handle.update_data_option()
    # handle.delete_data_option()
    # handle.insert_many_data_option()
    # handle.sql_insert_problem()
    handle.sql_insert_no_problem()
    # print(handle.search_database_option())
```





## X 大作业

```python
# 基于 pymysql 和 MySQL 数据库来写一个 员工管理系统
************** 功能菜单 ************** 
              1.用户注册
              2.用户登陆
              3.添加用户
              4.查看所有用户
              5.查看指定用户
              6.删除指定用户
              7.删除所有用户
              8.修改指定用户信息
              9.退出
************** 功能菜单 ************** 
```



