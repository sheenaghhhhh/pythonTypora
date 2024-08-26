# 0 内容回顾

## 0.1 函数的参数

```python
# 1.分为两大类
# 形参: 定义在函数名后面的变量名
# 实参: 实际调用函数的时候传递进去的指定值

# 2.位置参数和关键字参数: 按照传值方式的不同来区分
# 位置参数: 按照位置传递参数 add(1, 2)
# 关键字参数: 按照关键字传递参数 add(x=1, y=2)

# 3.可变长位置参数和可变长关键字参数
# 可变长位置参数: *args 定义在函数名后面的参数 可以接受多余的位置参数
# 可变长关键字参数: **kwargs 定义在函数名后面的参数 可以接受多余的关键字参数

# 4.默认参数
# 在函数定义的时候，函数名后面的参数有默认值的形参叫默认参数
# 在函数调用的时候有值传入就用传入的值 如果没有值传入就用函数定义的默认值

# 5.命名关键字参数
# 在函数定义阶段限制 参数只能按照位置传递 add(x, y, *, z, e)
# 调用add的时候，x和y可以按位置也可以按关键字，z和e只能按关键字传
```

## 0.2 函数的类型注释

```python
# 在函数定义阶段注释当前参数的类型
# 这是一个弱约束 ---> 不遵守不会报错 会有警告
# 强约束 ---> 不遵守会直接抛出异常
# from typing import List, Tuple, Dict, Union,Optional
```

## 0.3 名称空间

```python
# 用来存放变量名和变量值映射关系的地方
# 内建名称空间 Python解释器自带的名称空间关键字 def / if / else / class ...
# 局部名称空间 定义在函数或者类内部的名称空间函数内部的变量名
# 全局名称空间 定义在Python文件里面的所有变量名和函数名以及类名

# 加载顺序：内建--全局--局部
# 查找顺序：局部--全局--内建
```

## 0.4 作用域

```python
# 用来约束当前变量的作用区域 限制名称空间的查找范围
# LEGB
# L-Local 嵌套作用域
# E-Enclosing 局部作用域
# G-Global 全局作用域
# B-Built-in 内建作用域

# 嵌套作用域 定义在函数的函数内部的作用域
# 局部作用域 定义在函数内部的作用域
# 全局作用域 定义在函数外部的所有作用域
# 内建作用域 Python解释器自带的作用域

# 加载顺序：内建--全局--局部--嵌套
# 查找顺序：嵌套--局部--全局--内建
```

## 0.5 局部修改全局不可变数据类型

```python
# 局部修改全局可变数据类型 ---> 不需要任何处理 ---> 可变数据类型内存地址不会被改变
# 修改全局不可变数据类型 ---> global
```

## 0.6 局部的局部修改全局不可变数据类型

```python
# 局部的局部修改局部可变数据类型 ---> 直接改 ---> 就是同一个
# 修改局部不可变数据类型 ---> enclosing

# 修改全局不可变数据类型 ---> 见代码
"""
def change_age():
    age = 18
    print(age,id(age))#18 4328620816
    def inner():
        global age
        age = 28
        print('inner',age,id(age))#inner 28 4328621136
    inner()
    print('outer', age, id(age))
change_age()
print(age,id(age))  #28 4328621136
"""

"""
age = 38
print(age, id(age))
def change_age():
    age = 18
    print(age, id(age))  # 18 4328620816

    def inner():
        global age
        print('inner global',age, id(age)) # inner global 38 4375692688
        age = 28
        print('inner', age, id(age)) # inner 28 4328621136
    inner()
change_age()
print(age, id(age))  # 28 4328621136
"""
```

## 0.7 小结 global 和 nonlocal

```python
# global 是声明全局变量 加载的是全局的不可变数据类型
# nonlocal 声明嵌套函数的局部变量 加载的是嵌套函数的局部变量
```



# 1 闭包函数

# 1.1 函数的调用方式

```python
# 1.函数可以被引用
"""
def add(x, y):
    return x + y

res = add
res(1, 2)
"""

# 2.函数可以被作为容器类型的元素
"""
def add(x, y):
    return x + y
func_dict = {
    'add': add
}
"""

# 3.函数作为参数传递给另一个函数使用
"""
def add(x, y):
    return x + y

def pdd(x, y, func):
    return func(x, y) + x + y

print(pdd(1, 3, add))
"""

# 4.函数作为参数返回
"""
def add(x, y):
    return x + y

def pdd(x, y, func):
    return func(x, y) + x + y

def odd():
    # 函数在定义的时候其实是开辟了一块内存空间 存放函数的代码体
    # 将这个函数的内存地址赋值给函数名 再通过函数名进行调用
    
    # 总之就是要知道 如果def的odd这个函数 不管里面写了什么 
    # 如果最后 return 了 pdd
    # 那如果在外面用一个a = odd() 这个a就相当于pdd了
    # 因为return的就是pdd 后面每一次使用 都是 a() --> pdd()
    # 还有注意 既然调用的时候 是 a() 有括号的
    # 那return的时候 这个return pdd 肯定是不带括号的 不然就报错了
    
    return pdd

print(odd)
"""
```

## 1.2 闭包函数

```python
# 闭包函数: 函数内部再定义一个函数 并且这个函数用到了外边函数的变量
# 函数的返回值是可以任意的值 包括函数本身的内存地址
# 作用域是定义完函数之后就已经被确定的关系

# 自己的理解:
# 三个条件
# 1.有outer和inner两个嵌套的函数
# 2.inner里要使用outer的变量
# 3.outer的return是inner


"""
def add():
    age = 18
    # locals() 函数会以字典类型返回当前位置的全部局部变量
    print(locals())  # {'age': 18}
    def inner():
        print(age)
        print(locals())  # {'age': 18}
    inner()

add()
print(locals())
# {'__name__': '__main__',
#  '__doc__': '\ndef add(x, y):\n    return x + y\n\nres = add\nres(1, 2)\n', '__package__': None,
#  '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x000002116C418820>,
# '__spec__': None,
# '__annotations__': {},
# '__builtins__': <module 'builtins' (built-in)>,
# '__file__': 'D:\\Program Files\\JetBrains\\PycharmProjects\\Day12\\02闭包函数.py',
# '__cached__': None,
# 'add': <function add at 0x000002116C353F40>}
print(globals())
# locals() 实际上没有返回局部名字空间，它返回的是一个拷贝。
# 所以对它进行修改，修改的是拷贝，而对实际的局部名字空间中的变量值并无影响。
# globals() 返回的是实际的全局名字空间，而不是一个拷贝与 locals 的行为完全相反。
"""
```

## 1.3 闭包函数的应用场景

```python
# 保持状态 可以将一个函数传给另一个参数 也可以将另一函数的返回值放出来
"""
# 方式1下载同一页面
import requests


# 将值以参数的形式传入
def get(url):
    return requests.get(url).text


get('https://www.python.org')
get('https://www.python.org')
get('https://www.python.org')
"""

"""
# 方式2
import requests
def page(url):
    print(url)
    print(locals())

    def get():
        print('get', url)
        print('get', locals())
        return requests.get(url).text
    return get

python = page('http://python.org')
print(python)  # 因为python = page(url) 而 page这边的写法是闭包函数
# page return的是 get 那python --> get() 所以 print 它就是 get这个方法
print(python)  # <function page.<locals>.get at 0x000001D9CF2263B0>
python()  # 因此后面每次调用 都相当于 get() 会打印 get xxxx
python()
python()
"""


# 2.装饰器
# 在不改变源代码和原调用方式的基础上进行增加额外的新功能
```

# 2 装饰器

## 2.1 什么是装饰器

```python
# ● 装饰 代指为被装饰对象添加新的功能，器 代指器具/工具，装饰器与被装饰的对象均可以是任意可调用对象。
# ● 概括地讲，装饰器的作用就是在不修改被装饰对象源代码和调用方式的前提下为被装饰对象添加额外的功能。
# ● 装饰器经常用于有切面需求的场景
#   ○ 插入日志、性能测试、事务处理、缓存、权限校验等应用场景
#   ○ 装饰器是解决这类问题的绝佳设计
#   ○ 有了装饰器，就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。
# 提示：可调用对象有函数，方法或者类，此处我们单以本章主题函数为例，来介绍函数装饰器，并且被装饰的对象也是函数。
```

## 2.2 装饰器的用途

```python
# ● 软件的设计应该遵循开放封闭原则，即对扩展是开放的，而对修改是封闭的。
#   ○ 对扩展开放，意味着有新的需求或变化时，可以对现有代码进行扩展，以适应新的情况。
#   ○ 对修改封闭，意味着对象一旦设计完成，就可以独立完成其工作，而不要对其进行修改。
# ● 软件包含的所有功能的源代码以及调用方式，都应该避免修改，否则一旦改错，则极有可能产生连锁反应，最终导致程序崩溃
#   ○ 而对于上线后的软件，新需求或者变化又层出不穷，我们必须为程序提供扩展的可能性，这就用到了装饰器。
```

## 2.3 装饰器的分类

```python
# ● 函数装饰器分为：无参装饰器和有参装饰两种
# ● 二者的实现原理一样，都是’函数嵌套+闭包+函数对象’的组合使用的产物。
```

# 3 无参装饰器

```python
"""
# 给一个函数增加一个时间计数器

# print(time.time()) # 1722305790.024003
# 时间戳，是从1970年1月1日（UTC/GMT的午夜）开始所经过的秒数（不考虑闰秒），用于表示一个时间点。
# 然而，这种格式对于人类阅读并不友好，因此需要转换成可读的日期和时间格式。
# 这个工具能够将时间戳快速转换为人类可读的日期时间格式，同时也支持反向转换，即将日期时间转换为时间戳。

print(time.time())
# 时间戳 秒
start_time = time.time()
time.sleep(2)
end_time = time.time()

print(end_time - start_time)
"""
```

```python
# 1.写一个函数 给这个函数增加一个计时功能
"""
def index():
    start_time = time.time()
    time.sleep(2)
    end_time = time.time()
    print(f"总耗时: >>> {end_time - start_time}")

index()
"""
# 问题：在函数内部计算耗时 每一次都要写一个计时代码
```

```python
"""
def index():
    print('index 运行')
    time.sleep(2)
def func():
    print('func 运行')
    time.sleep(2)

start_time = time.time()
# index()
func()
end_time = time.time()
print(f"总耗时 :>>>> {end_time - start_time}s")
"""
# 问题：每一次都要将上一个函数注释掉然后启动新的函数去计算耗时
```

```python
"""
def index():
    print('index 运行')
    time.sleep(2)


def home():
    print('home 运行')
    time.sleep(3)


def timer(func):
    def inner():
        start = time.time()
        func()
        end = time.time()
        print(f"总耗时: >>> {end - start}")
        # return res
    return inner


res = timer(func=index)
print(res)
print(res())
res()
res = timer(func=home)
# print(res)
print(res())
# res()
"""
```

```python
"""
def index():
    print(f"index starting  .... ")
    time.sleep(2)
    return 'index 运行结束'


# 想给原本的函数 index 加上一个 计时功能并且不要改变原来的调用方式
# index()
def timer(func):
    # func ---> index 真正的函数内存地址
    def inner():
        result = func()
        # 原本的 index 函数 可能会有返回值
        return result

    return inner


# timer() ---> 返回值是 inner 的函数地址 ---> 重命名函数地址
index = timer(func=index)  # index 就是 timer 内的 inner 的内存地址
result = index()  # index() ---> timer 内的 inner 执行 ---> func() --->  index 真正的函数内存地址 ---> index()
# result 是谁？ 是你真正的index 函数执行后返回的返回值
"""
```

```python
# Practice ：有一个取款的函数
def withdraw():
    print(f"取款 starting  .... ")
    time.sleep(2)
    return True, '取款 运行结束'


def insert():
    print(f"insert starting  .... ")
    time.sleep(2)
    return True, 'insert 运行结束'


# 验证当前用户登录过后才能进行取款
# 如果没有登陆过要让用户去登录
# 并且 不要改变原本的 取款的调用方式
# print(withdraw())

'''
login_dict = {
    "username": 'None'
}
def login_auth(func):
    # func ---> withdraw 真正的函数内存地址
    def inner():
        # 在进入 withdraw 功能之前 进行验证登陆
        if login_dict.get("username"):
            res = func()
            return res
        else:
            return False, '请先登陆!'

    return inner


withdraw = login_auth(func=withdraw)
res = withdraw()
print(res)

insert = login_auth(func=insert)
res = insert()
print(res)
'''

"""
# Practice
# 登陆认证
# 取款
# 验证用户名 验证密码
# 先验证用户名 再验证密码
def withdraw():
    print(f"取款 starting  .... ")
    time.sleep(2)
    return True, '取款 运行结束'


login_dict = {
    "username": 'sheenagh',
    "password": "526"
}


def login_auth(func):
    # func ---> withdraw 真正的函数内存地址
    def inner():
        # 在进入 withdraw 功能之前 进行验证登陆
        if login_dict.get("username") != "dream":
            return False, f'用户名错误!'
        elif login_dict.get("password") != "521":
            return False, '密码错误!'
        result = func()
        return result

    return inner

withdraw = login_auth(withdraw)
print(withdraw())
"""
```

```python
# 【无参装饰器的模板】
def outer(func):
    """
    :param func: 真正的函数内存地址
    :return:
    """
    def inner():
        # 在真正地进入到上面的 func 之前进行认真
        # 执行真正的函数
        result = func()
        # 处理执行后的结果
        # 对返回结果进行定制化返回
        return result
    return inner
```

# 4 有参装饰器

```python
login_dict = {
    'username': 'sheenagh',
    'password': '526'
}


def withdraw(username, money):
    print(f"当前用户 {username} 正在提现 {money} 元")


def transform(username, to_username, money):
    print(f"当前用户 {username} 给 {to_username} 转账 {money} 元")


def login_auth(func):
    def inner(username, to_username, money):
        if login_dict.get("username"):
            return func(username, money)  # withdraw(参数)
        else:
            return False, f'当前请先登陆!'
    return inner

# 可变长参数
def login_auth(func):
    def inner(*args, **kwargs):
        print(args)  # ('dream', 'opp', 888)
        print(kwargs)  # {'username': 'dream', 'money': 999}
        if login_dict.get("username"):
            return func(*args, **kwargs)  # withdraw(参数)
        else:
            return False, f'当前请先登陆!'
    return inner


withdraw = login_auth(withdraw)
# withdraw(username, money)
res = withdraw(username="sheenagh",
               money=2000)  # withdraw --> login_auth 内 的 inner ---> inner() ---> func() ---> withdraw()
print(res)

transform = login_auth(transform)
# transform(username, to_username, money)
res = transform("sheenagh", "noone", 888)
```

```python
# 有参装饰器模板
def outer(func):
    def inner(*args, **kwargs):
        # 第一部分 在正式进入func之前进行校验
        # 第二部分 正式执行 原本的函数
        result = func(*args, **kwargs)
        # 第三部分 对返回的结果进行继续处理
        return result
    return inner
```

# 5 语法糖

```python
"""
def withdraw(username, money):
    return f"当前 {username} 取款 {money} 元!"


def transform(username, to_username, money):
    return f'当前 {username} 给 {to_username}转账 {money} 元!'


def login_auth(func):
    def inner(*args, **kwargs):
        if not login_dict.get("username"):
            return "当前未登录请先登陆!"
        return func(*args, **kwargs)

    return inner
# login_auth(withdraw) 返回值是 login_auth 里面的 inner 函数的内存地址
# 给 login_auth 里面的 inner 函数的内存地址 重新赋值 withdraw
withdraw = login_auth(withdraw)
# withdraw() ---> login_auth 里面的 inner() ---> inner 里面的func() ---> withdraw() ---> 原本真正的函数()
withdraw("sheenagh", 1000)
"""
```

```python
 Python开发者为了更加方便的给当前函数加上指定的装饰器 于是就有了一种新语法
# 语法糖 ： 语法糖就是一种便捷的语法 本质还是原来的语法

"""
login_dict = {
    "username": "sheenagh",
    "password": "526"
}


def login_auth(func):
    def inner(*args, **kwargs):
        if not login_dict.get("username"):
            return "当前未登录请先登陆!"
        return func(*args, **kwargs)

    return inner


@login_auth  # 相当于 withdraw = login_auth(withdraw)
def withdraw(username, money):
    return f"当前 {username} 取款 {money} 元!"


print(withdraw("sheenagh",526))

@login_auth
def transform(username, to_username, money):
    return f'当前 {username} 给 {to_username}转账 {money} 元!'

print(transform("sheenagh","hope",526))
"""

"""
login_dict = {
    "username": "sheenagh",
    "role": "admin"
}


def login_decorator(func):
    # func ---> withdraw 函数的内存地址
    def inner(*args, **kwargs):
        print("login_decorator")
        if not login_dict.get("username"):
            return "当前未登录请先登陆!"
        return func(*args, **kwargs)

    return inner


def permission_decorator(func):
    # func ---> login_decorator 的 inner 函数地址
    def inner(*args, **kwargs):
        print("permission_decorator")
        if login_dict.get("role") != "admin":
            return "当前没有权限访问当前功能!"
        return func(*args, **kwargs)

    return inner


@login_decorator  # withdraw = login_decorator(permission_decorator 里面的 inner) =  login_decorator 里面的 inner 函数地址
@permission_decorator  # withdraw = permission_decorator(withdraw真正的) = permission_decorator 里面的 inner
def withdraw(username, money):
    return f"当前 {username} 取款 {money} 元!"


# 这里的 withdraw 函数的内存地址是 login_decorator 里面的 inner 函数地址
res = withdraw("sheenagh", "9999")
print(res)

# 多层语法糖的包装顺序是从下向上包装
# 执行顺序是从上向下执行

# 必须是登录状态且是管理员权限的情况下才能取款
# withdraw = login_decorator(withdraw) 的返回值  = login_decorator 的 inner 函数地址
withdraw = login_decorator(withdraw)
# withdraw = permission_decorator(login_decorator 的 inner 函数地址)的返回值  = permission_decorator 的 inner 函数地址
withdraw = permission_decorator(withdraw)
# permission_decorator  的 inner 函数
withdraw("sheenagh", "9999")
"""
```

```python
"""
# # withdraw = login_decorator(withdraw) 的返回值  = login_decorator 的 inner 函数地址
# withdraw = permission_decorator(withdraw)
# # withdraw = permission_decorator(login_decorator 的 inner 函数地址)的返回值  = permission_decorator 的 inner 函数地址
# withdraw = login_decorator(withdraw)
# # permission_decorator  的 inner 函数
# withdraw("sheenagh", "9999")

def transform(username, to_username, money):
    return f'当前 {username} 给 {to_username}转账 {money} 元!'
"""
```

```python
"""
login_dict = {
    "username": "sheenagh",
    "role": "admin"
}


def decorator(tag):
    # print(tag)  # <function withdraw at 0x1023c6050>
    if tag == "login":
        def login_decorator(func):
            # func ---> withdraw 函数的内存地址
            def inner(*args, **kwargs):
                print("login_decorator")
                if not login_dict.get("username"):
                    return "当前未登录请先登陆!"
                return func(*args, **kwargs)

            return inner

        return login_decorator
    elif tag == "permission":
        def permission_decorator(func):
            # func ---> login_decorator 的 inner 函数地址
            def inner(*args, **kwargs):
                print("permission_decorator")
                if login_dict.get("role") != "admin":
                    return "当前没有权限访问当前功能!"
                return func(*args, **kwargs)

            return inner

        return permission_decorator


@decorator(tag='login')  # @login_decorator
@decorator(tag='permission')  # @permission_decorator
def withdraw(username, money):
    return f"当前 {username} 取款 {money} 元!"


print(withdraw("sheenagh", 999))
"""
```

```python
"""
# withdraw = login_decorator(withdraw) 的返回值  = login_decorator 的 inner 函数地址
login_auth = decorator(tag="login")
permission = decorator(tag="permission")
# <function decorator.<locals>.login_decorator at 0x117f111b0>
withdraw = permission(withdraw)  # withdraw = permission_decorator(withdraw)
withdraw = login_auth(withdraw)  # withdraw = login_decorator(withdraw)
print(withdraw("sheenagh", 999))

# withdraw = permission_decorator(login_decorator 的 inner 函数地址)的返回值  = permission_decorator 的 inner 函数地址
# withdraw = login_decorator(withdraw)
# permission_decorator  的 inner 函数
# withdraw("sheenagh", "9999")


def transform(username, to_username, money):
    return f'当前 {username} 给 {to_username}转账 {money} 元!'
"""
```



# 6 ATM 粗糙

```python
# ATM 与 购物车
# 1.注册
# 2.登陆
# 3.激活银行卡
# 4.取款
# 5.转账
# 6.充值余额
# 7.查看流水
# 8.查看银行信息(查看自己的卡号、余额、流水等信息)

# 激活银行卡逻辑：
# 在注册的时候给一个初始的银行余额 和银行卡号 但是不给他银行密码 ---> 让他自己去激活银行卡
# 取款的时候 先验证当前用户已经登陆了 并且 银行卡被激活过了

# 注册 ： 存储到文件中 用户名 - 登录密码 - 年龄 - 银行卡号(1314) - 取款密码 - 余额(1000)
# 登录 ： 直接将用户信息从文件中取出，然后进行比对 用户名 - 密码
# 取款 :  验证你的取款密码，更改余额   余额(1000) ，记录你的提款信息 -- 文件里 - 加时间
# 转账 ： 验证你的取款密码，更改余额 目标银行卡号去转 记录你的提款信息 -- 文件里

"""
user_data_all[username] = {
    "username": username,
    "password": password,
    "age": age,
    "card_number": card_number,
    "withdraw_word": withdraw_word,
    "balance": balance
}
"""




# 今天任务
"""
# 1.注册
# 2.登陆
# 3.激活银行卡
# 4.取款
"""

# 按照前几天的 while 循环 + 功能菜单  + 功能字典


import random
# 全局登陆字典
login_dict = {
    "username": None
}


# 全局卡号列表
card_list = []


def handle(file_path="user_info.txt", mode="r", encoding="utf-8", data=None):
    if mode == "r":
        user_data_all = {}
        try:
            with open(file=file_path, mode=mode, encoding=encoding) as fp:
                for line in fp:
                    username, password, age, card_number, withdraw_word, balance = line.strip().split("|")
                    user_data_all[username] = {
                        "username": username,
                        "password": password,
                        "age": age,
                        "card_number": card_number,
                        "withdraw_word": withdraw_word,
                        "balance": balance
                    }
        except Exception as e:
            print(f"欢迎第一次注册!")
        return user_data_all
    elif mode == "w":
        user_info_data_list = []
        for user_info_small_dict in data.values():
            user_info_data = '|'.join([str(item) for item in user_info_small_dict.values()])
            user_info_data_list.append(user_info_data + "\n")
        with open(file=file_path, mode=mode, encoding=encoding) as fp:
            fp.writelines(user_info_data_list)


def get_username_password():
    username = input("请输入用户名 :>>>> ").strip()
    password = input("请输入登录密码 :>>>> ").strip()
    return username, password


# 1.注册
def register():
    print(f"欢迎来到注册功能!")
    # 1.让用户输入 用户名 - 登录密码 | 年龄 - 取款密码 | 余额(1000) - 银行卡号(1314)
    # 用户名和密码 年龄 注册时输入
    # 余额=默认值 卡号生成 取款密码先none
    username, password = get_username_password()

    age = input(f"请输入年龄 :>>>> ").strip()
    if not age.isdigit():
        return False, f"当前年龄非法!不是数字!"
    age = int(age)
    if age < 0 or age > 150:
        return False, f"当前年龄已超出正常人类范畴!"

    balance = 1000

    while True:
        card_number = f"{random.randint(1, 9999):04}"
        if card_number not in card_list:
            card_list.append(card_number)
            break
    # 2.判断一下当前用户是否存在
    user_data_all = handle()
    user_info = user_data_all.get(username)
    # 3.如果用户存在让用户去登录
    if user_info:
        return False, f"当前用户 {username} 已存在!请去登录!"
    # 4.如果用户不存在 让用户注册
    user_data_all[username] = {
        "username": username,
        "password": password,
        "age": age,
        "card_number": card_number,
        "withdraw_word": None,
        "balance": balance
    }
    handle(mode="w", data=user_data_all)
    # 5.注册成功返回注册成功信息
    return True, f"用户 {username} 注册成功!"


# 2.登录 ligin
def login():
    print(f"欢迎来到登陆功能!")
    # 1.让用户输入用户名和密码
    username, password = get_username_password()
    # 2.查询到所有人的用户信息
    user_data_all = handle()
    # 3.查询到当前登录这个人的登录信息
    user_info = user_data_all.get(username)
    # 4.校验当前用户是否存在
    if not user_info:
        return False, f'当前用户 {username} 不存在 请先注册!'
    # 5.校验当前登录的用户名和密码是否正确
    if user_info.get("password") != password or username != user_info.get("username"):
        return False, f'当前用户 {username} 用户名或密码错误 请重新输入!'
    # 6.正确登录成功保存用户状态
    login_dict.update({
        "username": user_info.get("username")
    })
    # 7.错误登录返回错误信息
    return True, f'当前用户 {username} 登陆成功!'


# 3.激活
# 激活的时候 让设置取款密码
def motivate():
    print(f"欢迎来到激活功能!")
    # 1.检验当前是否登录
    if not login_dict.get("username"):
        return False, f'当前用户未登录!请先登录！'
    # 2.获取所有信息 取出目前登录员工的信息
    user_data_all = handle()
    # 3.设置取款密码
    withdraw_word = input(f"请输入四位数字取款密码 :>>>> ").strip()
    if not withdraw_word.isdigit():
        return False, f"当前密码非法!不是数字!"
    withdraw_word = int(withdraw_word)
    print(withdraw_word)
    if withdraw_word < 0 or withdraw_word > 9999:
        return False, f"当前密码超出范围!"
    username = login_dict.get("username")
    user_data_all[username]["withdraw_word"] = withdraw_word
    # 4.存入文件并返回激活成功信息
    handle(mode="w", data=user_data_all)
    return True, f"用户 {username} 激活成功!"


# 4.取款
# 验证你的取款密码，更改余额   余额(1000) ，记录你的提款信息 -- 文件里 - 加时间 -----没有写记录信息的模块
def withdrawal():
    print(f"欢迎来到取款功能!")
    # 1.判断是否登录
    if not login_dict.get("username"):
        return False, f'当前用户未登录!请先登录！'
    # 2.获取所有信息 取出目前登录员工的信息
    user_data_all = handle()
    username = login_dict.get("username")
    # 3.验证取款密码是否正确
    temp = input(f"请输入四位数字取款密码 :>>>> ").strip()
    if temp != user_data_all[username]["withdraw_word"]:
        return False, f'当前用户取款密码错误！'
    print("进入取款模式")
    money_temp = input(f"请输入您要取出的金额 :>>>> ").strip()
    if not money_temp.isdigit():
        return False, f"当前金额非法!不是数字!"
    money_temp = int(money_temp)
    if money_temp > int(user_data_all[username]["balance"]):
        return False, f"超出余额范围!"
    user_data_all[username]["balance"] = int(user_data_all[username]["balance"]) - money_temp
    handle(mode="w", data=user_data_all)
    return True, f"取款成功"


func_menu = '''
***************** ATM系统 ***************** 
                    1.注册
                    2.登陆
                    3.激活银行卡
                    4.取款
                    5.转账
                    6.充值余额
                    7.查看流水
                    8.查看银行信息(查看自己的卡号、余额、流水等信息)
                    9.退出
***************** ATM系统 ***************** 
'''


func_dict = {
    "1": register,
    "2": login,
    "3": motivate,
    "4": withdrawal,
    "9": "q"
}


def main():
    while True:
        print(func_menu)
        func_id = input("请输入功能ID :>>>> ").strip()
        if func_id not in func_dict:
            print(f"当前功能ID不存在!")
            continue
        func = func_dict.get(func_id)
        if func == "q":
            print(f"欢迎下次使用!")
            break
        flag, msg = func()
        if flag:
            print(msg)
        else:
            print(msg)
            continue


main()
```



TASK:

1. 过一遍代码√ 梳理笔记√
2. ATM前四个功能√
3. 好好思考装饰器 整理逻辑
4. 再多看两遍类型注释