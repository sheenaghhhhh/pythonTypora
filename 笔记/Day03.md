# 昨日内容回顾

```python
# 【零】多版本Python解释器共存
# 电脑上安装了多个版本的 Python解释器
# 想要使用特定版本的解释器的时候
# 解决方案：复制原本的Python执行文件--->改个名字

# 如何确定当前系统默认的Python解释器版本
# 看自己的系统环境变量中不同版本解释器的环境顺序

# 【零】Python知识补充
# 【1】pip换源
# 我们使用的解释器是国外大佬写的，有很多第三方模块，安装很慢
# 于是各大名校机构就在自己的场地内建立了一个公共的平台，让我们能快速下载相应模块
# 全局换 ： pip set global_index 源地址
# 局部换 ： pip install 模块名 -i 源地址

# 【2】系统环境和虚拟环境
# 系统环境就是安装在我们电脑上面的解释器环境
# 虚拟环境是单独为某个项目创建的系统解释器的副本

# 为什么要使用虚拟环境
# 为了让不同项目之间的环境进行隔离

# 【3】如何创建虚拟环境
# (1) 方案一：通过Python自带的虚拟环境工具 venv
# python -m venv 虚拟环境名称
# 激活虚拟环境：进入到创建的虚拟环境的文件夹中--->Scripts文件夹--->activate.bat
# 退出虚拟环境：在虚拟环境中打开cmd--->输入deactivate
# (2) 方案二：借助第三方模块 virtualenv # pip install virtualenv
# 创建虚拟环境：virtualenv 虚拟环境名称 默认是根据计算机的默认解释器版本
# 创建不同解释器版本的虚拟环境，就要加上解析器版本
# virtualenv -p python310 虚拟环境名称
# 激活虚拟环境：进入到创建的虚拟环境的文件夹中--->Scripts文件夹--->activate.bat
# 退出虚拟环境：在虚拟环境中打开cmd--->输入deactivate
# (3) 方案三：直接使用Pycharm

# 【零】Pycharm安装
# 打开官网下载安装包
# 路径不选C盘
# 专业版

# 【一】注释
# 为什么要添加注释：为了解释代码的含义
# 注释语法
# 单行注释：# + 注释内容
# 多行注释：三个单引号或者三个双引号包裹起来的内容

# 【二】变量
# 变量就是存储计算机运行过程中产生的动态变化的量
# 【1】变量命名规则：
# 字母+数字+下划线任意组合
# 数字不能作为变量名开头
# 变量名不能是关键字 打出来橙色

# 【2】变量的命名风格
# 驼峰体
# 大驼峰 每个单词首字母都大写
# 小驼峰 第一个单词首字母小写，其他单词首字母大写
# 字母+下划线组合 ：字母+数字+下划线任意组合

# 【3】变量的定义和调用
# 定义：变量名 = 变量值
# = ： 赋值符号

# 调用：直接使用变量名即可找到对应的变量值
# 原理：开辟一块内存空间，在这块空间存储变量值，用变量名指向变量值

# 【4】变量的三大特性
# 1.变量值 print(a)
# 2.变量内存空间地址 print(id(a)) 开辟内存空间存储变量值的位置
# 3.变量值的类型 print(type(a))

# 【三】常量
# 是一种特殊的变量（在python里！！！）
# 常量就是在程序运行过程中不会轻易发生改变的量
# 在python中定义后可以被改变，但其他大多数语言是不允许的

# 【四】PEP8编码规范
# 对python相关的代码进行了约束
```

# 1 八大基本数据类型

```py
# 【一】学习变量的目的
# 在程序运行过程中，能够保存一些数据供后续代码使用

# 【二】学习基本数据类型的目的
# 最基本的

# 【三】八大基本数据类型
# 【1】数字类型
# 整数类型 int
# 浮点数类型 float
# 【2】字符串类型 str
# 【3】列表类型 list
# 【4】字典类型 dict
# 【5】元组类型 tuple
# 【6】布尔类型 bool
# 【7】集合类型 set

# 【一】数字类型
# 【1】整数类型 int
# 语法 变量名 = 整数值
age = 18
print(age, type(age))
# 18 <class 'int'>
# 主要功能：进行算数运算

# 【2】浮点类型 float
# 浮点数和整数可以运算但是精确度会发生变化
# print(3.11+1)
# 4.109999999999999

# 【二】字符串类型
# 用来表示一些常用的文本信息
# 名字住址
name = "dream"
print(name, type(name))  # dream <class 'str'>

# 语法：四种
# 一 单引号包裹的字符
# 二 双引号包裹的字符
# 三 三个双引号包裹的字符
# 四 三个单引号包裹的字符

# [1]索引取值
# 字符串的每一个元素都有自己的索引下标
# 正向 从0开始
# 反向 从-1开始逆序

# 字符串[索引下标]

# [2]数学运算
# 字符串 + 字符串 = 字符串首尾拼接
# 字符串 * 数字 = 字符串重复出现数字次数

# [3]字符串的格式化输出语法
# (1) %s站位
# "My name is %s" % ("dream")
# %s 任意文本类型 不是文本类型也可以被转换为文本
# %d表示站位的是整数
# %f表示站位的是浮点数
# %x表示站位的是十六进制

# (2) format屙屎化输出
# "My name is {}" .format("dream")
# 上面只能按照位置传递参数
# 可以按照关键字传递
# "My name is {name}" .format(name="dream")

# (3) f"name"
# 在字符串的最前面 + f 在字符串的里面 用大括号站住位置

# 【三】列表类型
# 存多个学生名字，如果用字符串，不好取
# 存多个相同类型的变量值或者不同类型也可以

# 语法：变量名=[元素1，元素2，元素3，·..]
# 【1】索引取值
# 列表的每一个元素都有自己的索引下标
# 正向索引：从0开始从左向右
# 反向索引：从-1开始从右向左

# 列表[索引下标]

# 如果嵌套很多层，只需要很清楚地知道每一个元素的下标，按下标取值

# 【四】字典类型
# 上面的列表 可以放多个值 但是这些值 没有标志
# 为每一个值增加上必要的说明
# 语法 在大括号{} 中以键值对的方式存储的变量名和变量值

# {"key":value}
# key:一般是对 value的描述性信息 建议都使用字符串类型 或者 数字类型
# value:是对键的补充 真正的数据

# {"name":"Toby","age":18}

# 字典可以索引取值

num_list = [1, 2, 3, 4, 5]
print(num_list[0])
data = {
    "name": "dream",
    "age": 18
}

# 字典[值] 按照键取字典中取值 字典中如果没有这个键就会报错

# 字典.get() 没有的话返回None

# 列表和字典 组合嵌套
info = {
    'name': 'Dream',
    'addr': {
        '国家': '中国',
        'info': [666, 999, {'编号': 466722, 'hobby': ['read', 'study', 'music']}]
    }
}
# 格式化输出：my name is name , my age is age , my 国家 is 国家 , my 编号 is 编号
# hobby : my hobby is read , study and music

# print(f"试一试字典用key名{info['name']}和试一试字典用位置{info[0]}")
# 所以 字典 这种有 key的 就要用key去指定；像列表那种 有数字顺序的 用数字去指定。两者可以互相嵌套，但取值时必须遵从规则。

print(f'''
    My name is {info['name']}, 
    my 国家 is {info['addr']['国家']} , 
    my 编号 is {info['addr']['info'][2]['编号']}
''')
print(f'''
My hobby is {info['addr']['info'][2]['hobby'][0]},
 {info['addr']['info'][2]['hobby'][1]} 
 and {info['addr']['info'][2]['hobby'][2]}.
 ''')

print(f'''
    my name is {info.get("name")}
    my 国家 is {info.get("addr").get("国家")}
    my luck_num is {info.get("addr").get("info")[0], info.get("addr").get("info")[1]}

''')

# 【五】布尔类型
# 布尔类型 只有两个值 True 和 False
# 就是用来表示逻辑的对与错的两个值
# 用于判断 / 循环 ...
# 语法 变量名 = True / False

# 在Python中为真 True
# 除了假的情况以外的其他情况都是真的
# 1.数字1
# 2.True
# 3.非空的字符串
print(bool(1))
print(bool(True))
print(bool(" "))

# 在Python中为假 False
# 1.False
# 2.0
# 3.空的变量(空的字符串 / 空的列表 / 空的字典 ...)

print(bool(False))
print(bool(0))
print(bool(""))
print(bool([]))
print(bool({}))

# 字符串中有空格 也是真
print(bool(" "))

# 【六】元组类型
num_list = [1, 2, 3, 4, 5]
print(num_list[0])
num_list[0] = 999
print(num_list[0])

# 元组(tuple)是一种不可变的序列类型，类似于列表，用于存储多个有序元素。
# 元组与列表的主要区别在于元组的元素不能被修改、删除或添加，是不可变的数据
# 元组通常用于存储相关联的数据，保持数据的完整性

# num_tuple = (1, 2, 3, 4, 5)
# num_tuple[0] = 6 # TypeError: 'tuple' object does not support item assignment

# 字符串 如果后面加了一个,

name = "dream",
name_tuple = ("dream")
print(name, type(name))
print(name_tuple, type(name_tuple))

# 总结
# 当元组中只有一个元素的时候要加,
# 如果是字符串或者整数类型的时候加上,就变成了元组类型

# 元组按照位置解包
name, age = ("dream", 18)
print(name, age)

# 【七】集合类型
# 集合(set)是一种无序且不重复的数据类型
# 用于存储多个无序且不重复的元素。
# 语法：以单个元素的形式在大括号中存储数据

num_set = {2, 3, 1, 1, 1, 2}
print(num_set, type(num_set))  # {1, 2, 3} <class 'set'>

dream_set = {"d", "r", "e", "a", "m"}
print(dream_set, type(dream_set))

# 当集合中的类型是数字类型的时候顺序一般不会被打乱
# 但是如果集合中的类型是字符串 就会被打乱

test_set = {2, "a", "n", 3, False}
print(test_set, type(test_set))

# 集合里面不能放 列表 或者元组 和字典

# 集合运算：并交差补

# 并集：两个集合中的元素都放到一个集合中
# 交集：两个集合共有的元素放到一个集合中
# 差集：集合1有而集合2没有的元素放到一个集合中
# 补集：集合1包含集合2 1-2即为补集
# 严谨来说：数学意义上应做到包含 而在python领域中没有那么严谨，未设置条件，因此差（补）就成了同一个东西
# 对称差集： 两个集合中不重复的元素都放到一个集合中

# 【1】向集合中添加元素
print("-----")
name_set = {"dream", "opp"}
name_set.add("oppp")

print(name_set)

# 【2】集合中删除元素
# 按照指定的值删除指定元素
name_set.remove("opp")
print(name_set)

# 【3】集合运算
set_a = {1, 2, 3, 4, 5, 6}
set_b = {4, 5, 6, 7, 8, 9}

# （1）并集
print(set_a.union(set_b))
# （2）交集
print(set_a.intersection(set_b))
# （3）差集
print(set_a.difference(set_b))
# （4）对称差集
print(set_a.symmetric_difference(set_b))
```

# 2 程序与用户交互

## 2.1 什么是程序与用户交互

```
# 就是人向代码中输入内容
# 代码运行结果输出内容
```

## 2.2 为什么要交互

```
# 登录某个网站的时候需要输入用户名和密码
# 代码如何知道你的用户名和密码正确与否 ---> 经过逻辑认证
# 程序向我们反馈登录的结果
```

## 2.3 如何交互

```
# 【1】用户输入内容
# input : 在输入框内提示需要用户输入的信息
# user_name = input("Enter your name: ")
# password = input("Enter your password: ")
#
# result = user_name + password
# print(user_name, type(user_name))
# print(password, type(password))
# print(result, type(result))  # 1 + 2 = 12

# 通过input语句输入的内容获取到的结果是字符串类型

# 将字符串类型的 1 转换成数字类型的 1

# result = int(user_name) + int(password)
# print(result, type(result))  # 1 + 2 = 3
# 只能转换符合整数类型的字符串

# 【2】输出
# 就是 print 语句
print("Hello World")
# 打印变量
name = "dream"
print(name)
# 有一个默认参数 end="\n"
print("dre\nam")
print(1,end="*")
print(2)
print(3)
```

# 3 基本运算符

```py
# 算数运算符
# 逻辑运算符
# 比较运算符
# 增量运算符

# 【一】算术运算符
# 进行算术运算的运算符
# + - * / % ** //
a = 10
b = 3

print(a + b)
print(a - b)
print(a * b)
print(a / b)
print(a % b)
print(a ** b)  # 幂次方
print(a // b)  # 取模 整除

# 【二】比较运算符
# > >= < <= == !=
print(a > b)
print(a >= b)
print(a < b)
print(a <= b)
print(a == b)
print(a != b)

# 【三】赋值运算符
# = += -= *= /= %= //= **=
# 10 3
print("----")
a += b
print(a)
a -= b
print(a)

a *= b
print(a)
a /= b
print(a)

a %= b
print(a)
a //= b
print(a)

a **= b
print(a)

# 【1】链式赋值
a = b = c = d = 999
# 仅python这样用

# 【2】交叉赋值
a = 10
b = 3
a, b = b, a  # 3, 10

# 【3】解压赋值
# 将列表/元组/集合 能够索引取值的类型中的每一个元素
num_list = [1, 2, 3]
a, b, c = num_list
# a, b, c = [1, 2, 3]
print(a)
print(b)
print(c)

# 列表中的元素个数不够前面变量解压的时候就会报错
# a, b, c, d = num_list
# not enough

# 如果列表中的元素多余位置变量的时候也会报错
# 用下划线来存储不需要的变量值
a, b, _ = num_list
print(a)
print(b)

# 【四】逻辑运算符
# 与或非
# 与：两者都必须 and
# 或：两者任一 or
# 非：取反 not

a = 10
b = -10

print(a>0 and b<0)
print(a>0 and b>0)

print(a>0 or b<0)
print(a>0 or b>0)

# 连续多个and
print(10>0 and 5>3 and 3<0)
print(10>0 and 5>3 or 3<0)
print(10>0 or 5>3 or 3<0)

# 提高运算的优先级 对优先的部分加小括号
# not > and > or
print(3 > 4 and 4 > 3 or 1 == 3 and 'x' == 'x' or 3 > 3)
# 0 and 1 or 0 and 1 or 0
# 0 or 0 or 0
# 0

print(3 > 4 and (4 > 3 or 1 == 3) and ('x' == 'x' or 3 > 3))
# 0 and (1 or 0) and (1 or 0)
# 0 and 1 and 1
# 0

# 【五】成员运算
# 判断某个成员在某个成员里面
# in / not in
num_list = [1, 2, 3]
print(1 in num_list)
print(6 in num_list)
print(6 not in num_list)

# 【六】身份运算
# 判断这个值是否是另一个值
a = 1
b = "1"
print(a is b)
print(a is not b)
print(a == b)
print(a != b)

# == 和 is 的区别
# == 比较的是值 两个值是否相等
# is 比较的是 值和内存地址是否相等

name = "dream"
name_one = "dream"
print(name == name_one)
print(name is name_one)
# 研究下地址怎么不一样
# 关于变量对象的内存存储地址
# 以前大一些、复杂一些的整型和字符串就会内容一样地址不同
# 随着python版本的更新，现在随便打一些内容地址都一样，但仍可能出现这种情况，所以要记得is到底是什么意思
```

# 4 流程控制语句

## 4.1 什么是流程控制语句

```py
# 就是程序在运行过程中控制流程的语句
# 程序到了 10：00 发邮件通知上班
# 17：30 发邮件通知下班
```

## 4.2 流程控制的三种方式

```python
# 顺序结构 ---> 按照顺序执行代码
# 分支结构 ---> 有条件约束，达到了某个条件才会执行代码
# 循环结构 ---> 循环执行代码
```

## 4.3 顺序结构

```python
# 程序按照顺序依次执行代码，一句接一句地执行
# 平常写的代码都是顺序结构
print("a")
print("b")
print("c")
```

## 4.4 分支结构

```python
# 【1】分支结构是程序在运行过程中达到某一条件才会执行代码的结果

# 【2】单分支结果
# 程序在运行过程中，如果达到了某个条件，就会执行该条件下的代码
score = 98
if score >= 90:
    print(f"优秀")

# 【3】双分支结构
# 程序在运行过程中有两个条件 要么达到条件一 执行条件二的代码
# 要么达到条件二 执行条件二的代码
'''
if 条件1:
    ...
else:
    ...
'''
score = 98
if score >= 90:
    print(f"优秀")
else:
    print(f"还需努力")

# 【4】多分支结构
# 程序在运行过程中有多个条件进行约束和判断
# 当程序执行遇到某一个条件就执行当前条件下的代码
age = 60
if 28 >= age >= 18:
    print(f"无语1")
elif 38 >= age > 28:
    print(f"无语2")
elif 48 >= age >= 38:
    print(f"无语3")
elif 58 >= age >= 48:
    print(f"巨无语")
else:
    print(f"快点走吧")

'''
if 条件一:
    条件1执行的代码
elif条件二:
    条件2执行的代码
elif条件三:
    条件3执行的代码
else:
    呵呵哒
'''

# 登录注册练习
# 让用户输入 用户名和密码
# 比对用户名和密码是否正确
# 正确打印登录成功
# 失败就打印登录失败 并且告诉用户是用户名错误还是密码错误

true_username = "sheenagh"
true_password = "20240717"

username = input("Enter your username:")
password = input("Enter your password:")

if username == true_username and password == true_password:
    print(f"登陆成功")
elif username != true_username and password != true_password:
    print(f"用户名和密码都错误")
elif username != true_username:
    print(f"用户名错误")
else:
    print(f"密码错误")
```

## 4.5 循环结构

```py
# 【五】循环结构：重复执行代码直至代码达到某个条件才会终止

# 循环结构 while
'''
while 终止条件：
    代码
'''
# 让while里面的代码一直执行 执行到 符合 终止条件的之后自动终止

count = 0

while count < 5:
    count += 1
    print(count)

# 【1】关键字 range
# int 转换符合数字类型字符串
# (1)生成一个 执行区间的数字列表
print(range(1, 5))  # range(1, 5)
print(list(range(1, 5)))  # [1, 2, 3, 4]
print(tuple(range(1, 5)))  # (1, 2, 3, 4)
print(set(range(1, 5)))  # {1, 2, 3, 4}

# (2)重复执行代码执行次数
for i in range(0, 3):
    print(i)
    print("hi")

# range(起始数字，结束数字，步长(可省略))
# 结束的数字是取不到的 顾头不顾尾 [start, end)

# range 如果第一个位置的参数不写 默认就是0
for i in range(3):
    print(i)

# 【2】continue
# 搭配 while 循环使用
# 在while循环内部 如果当前条件遇到满足的时候 不需要执行 跳过即可
count = 0
while count < 5:
    count += 1
    if count == 3:
        print(f"已跳出本次循环 {count}")
        continue
    else:
        print(count)
    # 当 count = 3的时候不打印3 其他的不打印
    # continue: 让当前while循环跳出去

# 【3】break关键字
# 当程序循环执行达到某一条件的时候，就想结束整个程序

tag = True # 标志位
count = 0
while tag:
    count += 1
    if count == 3:
        print(f"已跳出本次循环 {count}")
        continue
    elif count == 4:
        print(f"已结束本次循环 {count}")
        tag = False
        break
    else:
        print(count)

# 【死循环】
# while 的循环条件一直为真的时候就会一直循环执行
# while 内部的代码
# 平时写代码的时候应当避免出现死循环
# while True:
#   print(1)
```

# 5 练习和作业

```py
# 猜年龄
# 提前给定一个确认的年龄 18
# 让用户输入年龄 ---> 和真的年龄比较 如果大了就提示大了
# 小了就提示小了
# 错误的时候让用户重复尝试3次
# 3次全部错误以后让用户选择是否再继续猜
# 继续猜再给他3次机会 否则结束

true_age = 29
number = 0
while number < 4:
    if number == 3:
        no = input("你已经猜了3次，继续请输入1，退出请输入0:")
        if no == "0":
            break
        else:
            number = 0
    age = input("猜一下年龄吧！")
    age = int(age)
    if age > true_age:
        print("猜大了！")
    elif age < true_age:
        print("猜小了！")
    else:
        print("你猜对了！")
        break
    number += 1
```



TASK:

1. 整理一遍代码&笔记√
2. 作业√
3. 不懂的点查√
4. 多学两天的内容