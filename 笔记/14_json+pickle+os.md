# 0 内容回顾

## 0.1 装饰器的修复技术

```python
# 如果装饰器的内嵌函数中有参数注释和相关的关键字解释
# 那么可以通过 help 函数查看到内嵌函数的注释内容
# 如果注释内容中有核心的数据，就会导致敏感信息泄露

# 装饰器是在不改变原函数代码和调用方式的基础上增加额外的新功能
# 但是也不能暴露新功能的相关逻辑

# 装饰器的修复技术 对装饰器的内嵌函数的注释进行修改和替换
from functools import wraps


def outer(func):
    @wraps(func)
    def inner(*args, **kwargs):
        result = func(*args, **kwargs)
        return result
    return inner

# 包装后的效果就是再用 help 查看当前函数注解的时候只能看到当前函数的注解
# 而看不到装饰器内嵌函数的注解 这样可以保护需要隐藏的信息
```

## 0.2 可迭代对象和迭代器对象

```python
# 1.可迭代对象
# 具有 __iter__() 方法的对象就是可迭代对象
# str list dict tuple set

# 2.迭代器对象
# 具有 __iter__() 和 __next__() 方法的对象就是可迭代对象
# str list dict tuple set

# 3.生成迭代器对象
# 可迭代对象 = 对象.__iter__()
# 可迭代对象 = iter(对象)

# 4.迭代器取值
# 可迭代对象.__next__() (前提是已经生成了 生成后才可以进行 next操作)
# next(可迭代对象)
```

## 0.3 生成器

```python
# 生成器是一种特殊的迭代器，在我们需要数据的时候生成数据并且能够保存状态
# 1.生成器的创建方式 有两种
# a.元组生成式
# g = (i for i in range(10))

# b.借助 yield 关键字
# 写一个函数 在函数中用 yield 作为返回值
# def add():
#   yield
# res = add()
# res 就是生成器对象


# 2.如何向生成器中传值
"""
def eater():
    print(f"开始做饭")
    while True:
        food = yield
        print(f"拿到食物 : >>> {food}")


eater_obj = eater()
print(eater_obj)
# 启动生成器对象
next(eater_obj)
# 向生成器中传值
# 要先启动 才可以传值
eater_obj.send("布丁")
eater_obj.send("三文鱼")
"""

# 3.借助装饰器启动 生成器对象
"""
def init(func):
    def inner(*args, **kwargs):
        obj = func(*args, **kwargs)
        # 在装饰器里添加启动语句
        next(obj)
        return obj
    return inner


@init
def eater():
    print(f"开始做饭")
    while True:
        food = yield
        print(f"拿到食物 : >>> {food}")


eater_obj = eater()
eater_obj.send("布丁")
eater_obj.send("三文鱼")
# 省了自己启动
"""

# 3.for循环内部原理
# 借助生成器原理不断地 yield 迭代取值
# 如果取完所有值再继续向下取值 会报错
# 于是就有异常捕获机制来帮助我们处理异常信息
# for循环取完所有值后不报错的原因是 内部有异常处理机制
```

## 0.4 模块与包

```python
# 1.什么是模块
# 模块就是一个包含所有你定义的函数的变量的文件 其后缀名是 .py
# 一个 py 文件就是一个模块 文件名就是模块名
# 模块就是当前功能的统一接口

# 2.模块的来源
# 内建：Python解释器自带的模块 os / jason / time ...
# 第三方：别人开发 拿过来用 必须安装 django / requests ...
# 自己写的：自己写模块

# 3.模块的导入语法
# a.import 语法
# import 模块名
# 可以导入指定的模块
# 问题是无法导入模块中指定的变量名
# import 可以连续导入多个模块

# b.from ... import ... 语法
# from 路径 import 模块
# from 模块 import 变量
# 详细导入语法

# 4.循环导入问题
# 从A导入B的时候 在B中也在导入A
# A ---> B ---> A ---> B ---> A ---> ...
# 解决办法：
# 方案1：将最后循环导入的语法放在整个文件的结尾（不建议使用）
# 方案2：我们使用变量大多数实在函数中使用，在哪里使用就在哪里导入，将循环导入的语句放在函数内部执行

# 5.包
# 包就是一个包含多个模块和__init__.py文件的文件夹
# a.创建方式
# 方案1：直接在 pycharm 文件夹右键 选择 python package
# 方案2：自己创建一个文件夹 然后在该文件夹中创建一个 __init__.py文件

# b.制作包
# 在没有制作包之前导入语法是从当前包下面的模块中导入相关的变量
# 制作包就是在包的 __init__.py 中注册当前包可以导出的变量名
# 制作完包之后就可以直接从包名导入相关的变量

# 6.绝对路径和相对路径
# 绝对路径：从盘符开始写路径
# 相对路径：从当前文件夹开始写路径

# . :代表当前路径开始找路径
# .. :代表上一级路径开始找路径

# 7.主文件入口
# 每一个 py 文件都有一个文件入口
print(globals())
# '__name__': '__main__'
# 当前文件是主文件
# 在某些情况下我们需要对本文件中的某些接口进行测试而作为模块的导入的时候不需要执行这部分测试代码
# 所以应该把这部分代码隐藏起来
# if __name__ == '__main__'
# 声明当前 模块在被作为主文件运行的时候 会触发 if 条件下的代码
# 而作为其他模块导入的时候不会触发 if 条件下的代码

# 8.当一个模块在被当做模块导入的时候发生了哪些事
# import a
# 先找到 a 模块所在的文件位置
# 然后从上至下依次加载 a 模块中的代码
# 然后加载完 a 模块后回到 原本的文件中
# 然后将a模块中的所有名称空间 添加到当前文件所在的名称空间内 并且对a模块的名称空间进行命名
# a.x 才能看到 a 模块中的x
```

# 1 json模块

## 1.1 序列化和反序列化

```python
# 1.序列化
# 将原本的 Python 对象 dict/list/tuple 转换成 str 的过程 就叫序列化

# 2.反序列化
# 将原本的 str 转换成 Python 对象 dict/list/tuple 的过程 就叫反序列化

# 3.为什么要进行序列化
# 比如说 写程序的时候 一个程序产生的数据要传递给另一个数据
# 我们能想到的办法就是将产生的数据存储到文件中 由另一个程序将数据读出来再使用

# 但是对于计算机来说 文件里面是没有 dict 这个概念的
# 所以没有办法将字典放入文件中
# 在进行登录注册的时候 是把字典中的数据拼接为字符串 再保存到文件中

# 将字典中的数据转换成字符串中的数据就成为序列化
# 这样的序列化方式
# 有人帮助做好了 序列化和反序列化相关的操作
# 那么就只需要使用 不需要再写具体转换过程了
```

## 1.2 jason模块

```python
# 比较通用的一个模块 Python Go C Java JavaScript都有
# 区别在于调用方式不太一样
# 总之不同的语言和程序之间进行交互数据的时候 json 格式的数据是一个通用的数据形式

# 1.处理Python对象(python对象 和 json格式的字符串 互转)
# 有一个dict格式或者其他格式的数据
# 转化为json格式的字符串
# 一定要导入这个模块，才可以用里面的方法
import json

"""
user_data = {
    "sheenagh": {
        "username": "sheenagh",
        "password": 526,
        "age": 23,
        "ready": True,
        "hobby": ["music", "love"]
    }
}
"""

# (1)将Python对象转换为JSON格式的字符串数据
# 方法：jason.dumps(x)
# user_data_jason_str = json.dumps(obj=user_data)
# print(user_data_jason_str, type(user_data_jason_str))
# 尽管转换后的数据是JSON格式
# 但在Python中，它仍然被视作一个str（字符串）类型。
# 这是因为JSON本质上是文本格式，不管它表示什么数据结构，
# 最终在Python中都被视为字符串。

# (2)将JSON格式的字符串转换为Python对象
# 方法：jason.loads(x)
# user_data_jason_obj = json.loads(user_data_jason_str)
# print(user_data_jason_obj, type(user_data_jason_obj))
# 看差别
# user_data_jason_str 是 str 类型 保存时是 双引号
# user_data_jason_obj 转回来的时候变成了 dict 类型 都变成了单引号

# 2.处理json文件(json文件和)
user_data = {
    "sheenagh": {
        "username": "sheenagh",
        "password": 526,
        "age": 23,
        "ready": True,
        "hobby": ["music", "love", "yqj"]
    }
}

# 将Python中的 list 存到 json 文件中 是list
# data1 = ["music", "love", "yqj"]

# 将Python中的 元组 转换为 list 存到json文件中
# user_data = ("music", "love", "yqj")
# 这个tuple进去之后 就会变成[]这种 再出来就是list了
# ['music', 'love', 'yqj'] <class 'list'>

# (1)将Python对象转换为json字符串并保存到文件中
# 文件拓展名应该是 .json
# 先将 Python对象转换为json字符串
with open(file='user_data.json', mode='w', encoding="utf-8") as fp:
    # json.dump(x, fp) dump是写进去 格式也转换了
    json.dump(obj=user_data, fp=fp)
    # 这个命令将user_data对象转换为JSON格式，并直接写入到fp（文件对象）中。
    # json.dump()类似于json.dumps()，但是它将结果写入到指定的文件，而不是返回一个字符串。

# (2)将json文件中的数据读取出来并转换成python对象
with open(file='user_data.json', mode='r', encoding="utf-8") as fp:
    data = json.load(fp=fp)

print(data, type(data))

from json.encoder import JSONEncoder
from json.decoder import JSONDecodeError

# 使用 JSONEncoder 和 JSONDecodeError
# 可以帮助你在处理 json 数据时进行自定义和错误处理。


# 这是 Python 数据类型与 JSON 数据类型之间的映射关系
'''
    +-------------------+---------------+
    | Python            | JSON          |
    +===================+===============+
    | dict              | object        |
    +-------------------+---------------+
    | list, tuple       | array         |
    +-------------------+---------------+
    | str               | string        |
    +-------------------+---------------+
    | int, float        | number        |
    +-------------------+---------------+
    | True              | true          |
    +-------------------+---------------+
    | False             | false         |
    +-------------------+---------------+
    | None              | null          |
    +-------------------+---------------+
'''
# 将 Python中的 字典 转换为JSON格式的字典
# 将 Python中的 元组和列表 转换为 JSON格式的列表
# 将 Python中的 字符串 转换为 JSON格式的字符串
# 将 Python中的 布尔值 True / False 转换为 JSON格式的 true / false
# 将 Python中的 None 转换为 JSON格式的 null
# 将 Python中的 整数 转换为 JSON格式的数字

# 如：
# user_data = None
# with open(file="user_data.json", mode="w", encoding="utf-8") as fp:
#     json.dump(fp=fp, obj=user_data)
# 里面写着null
```

## 1.3 补充

```python
"""
dump用法的详细说明
obj : 需要写入到文件中的 Python 对象
fp : 文件句柄
* : * 后面的所有参数都必须按照关键字传参
skipkeys=False : 默认是 False 如果dict的keys内的数据不是python的基本类型(str,unicode,int,long,float,bool,None) 设置为False时，就会报TypeError的错误。
ensure_ascii=True : 是否启用默认的编码格式 如果是 True 则默认使用 ASCII 码格式，如果是 False 则使用 Unicode 格式
check_circular=True : 如果“check_circular”为false，则将跳过容器类型的循环引用检查，循环引用将导致“RecursionError”（或更糟）。
allow_nan=True : 如果`allow_nan`为false，那么严格按照JSON规范序列化超出范围的`float`值（`nan`、`inf`、`-inf`）将是一个`ValueError`，而不是使用JavaScript等效值（`nan`、`Infinity`、`-Infinity`）。
cls=None ： 如果你自定义了转化的数据格式 那就指定你自己写的类的转化规则 否则就用默认的 JSONEncoder 去转换
indent=None :   应该是一个非负的整型 如果是0就是顶格分行显示  如果为空就是一行最紧凑显示  否则会换行且按照indent的数值显示前面的空白分行显示 这样打印出来的json数据也叫pretty-printed json 
separators=None ： 分隔符
default=None
sort_keys=False 将数据根据keys的值进行排序。 
**kw
"""

from json.encoder import JSONEncoder

# json模块默认的编码格式是 ASCII 码
# 写数据的时候再 open 参数中指定了 utf-8 是不生效的
# 于是就需要对 参数进行调整
# ensure_ascii=False 确保了内容的正确保存 如果是True 数据hobby里有中文 会多出额外的符号
with open(file="user_data.json", mode="w", encoding="utf-8") as fp:
    json.dump(fp=fp, obj=user_data, skipkeys=False, ensure_ascii=False)
```

# 2 pickle模块

```python
# pickle 模块 和 json 模块都是用用来序列化和反序列化 Python 对象的

# 不同点在于 如果是json处理不了Python中的 函数和类
# 为了解决 Python对象可以被保存的问题 于是有了 pickle

import json
import pickle
```

## 2.1 处理 Python对象转换成 二进制数据格式

```python
user_data = {
    "sheenagh": {
        "username": "sheenagh",
        "password": 526,
        "age": 23,
        "ready": True,
        "hobby": ["music", "love", "yqj"]
    }
}


def add(x, y):
    return x + y


# 将函数对象序列化为字节流
user_data_pickle_bytes = pickle.dumps(obj=add)
print(user_data_pickle_bytes, type(user_data_pickle_bytes))
# b'\x80\x04\x95\x14\x00\x00\x00\x00\x00\x00\x00\x8c\x08__main__\x94\x8c\x03add\x94\x93\x94.' <class 'bytes'>


# 将字节流反序列化为函数对象
user_data_pickle_dict = pickle.loads(user_data_pickle_bytes)
print(user_data_pickle_dict, type(user_data_pickle_dict))
# <function add at 0x0000023BE19D3F40> <class 'function'>

# 调用反序列化后的函数
print(user_data_pickle_dict(1, 5))
```

## 2.2 处理Python对象转换为 文件格式

```python
# 写入文件 记住是 二进制 所以 wb rb
with open("user_data","wb") as fp:
    pickle.dump(add, fp)

# 读取文件
with open("user_data", "rb") as fp:
    obj = pickle.load(fp)
print(obj(3, 6))


def login():
    ...


def register():
    ...


func_dict = {
    "1": register,
    "2": login
}

with open("func_dict", "wb") as fp:
    pickle.dump(func_dict, fp)
```

# 3 os模块

```python
import datetime
# os模块：一般是用来处理文件路径和文件夹

import os

# __file__: 代表的是当前文件
```

## 3.1 文件路径相关 os.path.*

```python
# 1.获取当前文件所在路径 绝对路径
file_path_abs = os.path.abspath(__file__)
print(file_path_abs)
# D:\Program Files\JetBrains\PycharmProjects\Day14\04os模块.py

# 2.获取当前文件所在的文件夹路径 绝对路径
file_dir_abs = os.path.dirname(__file__)
print(file_dir_abs)
# D:\Program Files\JetBrains\PycharmProjects\Day14

# 3.判断当前文件路径是否存在
file_path_exist = r"D:\Program Files\JetBrains\PycharmProjects\Day14"
print(os.path.exists(file_path_exist))
# 这里有警告 W605 invalid escape sequence 是因为\是转义字符
# 解决方法有很多 其中比较简单的是在前面加一个r 使 \ 在这里被当成普通字符

# 4.拼接文件路径
img_dir_path = os.path.join(file_dir_abs, "img")
# D:\Program Files\JetBrains\PycharmProjects\Day14 + img
print(img_dir_path)
# D:\Program Files\JetBrains\PycharmProjects\Day14\img
print(os.path.exists(img_dir_path))
# 没有创建 所以这里是 False

# 5.切割文件路径 : 将当前路径切割为两部分 后部分为最后文件名or最后文件夹名 + 前部分 前面的绝对路径
file_path_split = os.path.split(file_path_abs)
print(file_path_split)
# D:\Program Files\JetBrains\PycharmProjects\Day14\04os模块.py
# ('D:\\Program Files\\JetBrains\\PycharmProjects\\Day14', '04os模块.py')

file_dir_split = os.path.split(file_dir_abs)
print(file_dir_split)
# D:\Program Files\JetBrains\PycharmProjects\Day14
# ('D:\\Program Files\\JetBrains\\PycharmProjects', 'Day14')

# 6.获取结尾文件/文件夹名 总之返回的是结尾文件或者路径
file_base_name_path = os.path.basename(file_path_abs)
print(file_base_name_path)  # 05os模块.py
file_base_name_dir = os.path.basename(file_dir_abs)
print(file_base_name_dir)  # Day14

# 7.判断当前路径是否是一个文件路径
print(os.path.isfile(file_path_abs))  # True
print(os.path.isfile(file_dir_abs))  # False
# file_dir_abs不是文件路径 而是一个目录路径

# 8.判断当前路径是否是绝对路径
file_path = "./04os模块.py"  # 这样写就是False
print(os.path.isabs(file_path_abs))  # True
print(os.path.isabs(file_path))  # False

# 9.判断当前路径是否是一个文件夹路径
print(os.path.isdir(file_path_abs))  # False
print(os.path.isdir(file_dir_abs))  # True

# 10.获取当前文件或目录的最后访问时间atime --- access time
print(os.path.getatime(file_path_abs))  # 1722483262.016646
print(datetime.datetime.fromtimestamp(os.path.getatime(file_dir_abs)))  # 2024-08-01 11:35:41.141825
# 11.获取当前文件或目录的创建时间ctime -- create time
print(os.path.getctime(file_path_abs))  # 1722483261.826308
print(datetime.datetime.fromtimestamp(os.path.getctime(file_dir_abs)))  # 2024-08-01 11:35:40.920485
# 12.获取当前文件或目录的修改时间 # mtime -- modify time
print(os.path.getmtime(file_path_abs))  # 1722483261.826308
print(datetime.datetime.fromtimestamp(os.path.getmtime(file_dir_abs)))  # 2024-08-01 11:35:40.920485

# 13.返回当前文件的大小
print(os.path.getsize(file_path_abs))
# 4129 字节

# 14.获取当前系统对应的路径分隔符
# MacOS/linux : /
# Windows : \
print(os.path.sep)
```

## 3.2 路径操作相关 os.*

```python
BASE_DIR = os.path.dirname(os.path.abspath(__file__))
print(BASE_DIR)
# D:\Program Files\JetBrains\PycharmProjects\Day14

# 1.创建单级文件夹
img_file_dir = os.path.join(BASE_DIR, "img")
print(img_file_dir)
# D:\Program Files\JetBrains\PycharmProjects\Day14\img
print(os.path.exists(img_file_dir))
# 还没有创建 所以是False
# os.mkdir(img_file_dir)
# 创建文件夹
print(os.path.exists(img_file_dir))
# 现在是True了

# 2.创建多级文件夹
img_file_dir = os.path.join(BASE_DIR, "img", "girl")
print(img_file_dir)
# D:\Program Files\JetBrains\PycharmProjects\Day14\img\girl
print(os.path.exists(img_file_dir))
# False
os.makedirs(img_file_dir)
print(os.path.exists(img_file_dir))
# True

img_file_dir = os.path.join(BASE_DIR, "img", "girl")
print(img_file_dir)
# D:\Program Files\JetBrains\PycharmProjects\Day14\img\girl
print(os.path.exists(img_file_dir))
# True
# exist_ok=True : 自动判断当前文件路径是否存在 如果不存在则主动创建 如果存在则忽略
os.makedirs(img_file_dir, exist_ok=True)
print(os.path.exists(img_file_dir))
# True

# 3.删除单级文件夹
img_file_dir = os.path.join(BASE_DIR, "img", "girl")
print(img_file_dir)
# D:\Program Files\JetBrains\PycharmProjects\Day14\img\girl
print(os.path.exists(img_file_dir))
# True
# os.rmdir : 只能删除单级文件夹且单级文件夹为空
# os.rmdir(img_file_dir)
print(os.path.exists(img_file_dir))
# False 因为下面要删除多级的 这边 注释掉 没有进行删除 就变成True了

# 4.删除多级文件夹

img_file_dir = os.path.join(BASE_DIR, "img", "girl")
print(img_file_dir)
# D:\Program Files\JetBrains\PycharmProjects\Day14\img\girl
print(os.path.exists(img_file_dir))
# True
# os.rmdir : 能删除多级文件夹且多级文件夹为空 如果是多级文件夹但是最深层的文件夹为空也会主动删除为空的文件路径
os.removedirs(img_file_dir)
print(os.path.exists(img_file_dir))
# False

# 5.列出当前文件路径下的所有文件名
# BASE_DIR = "D:\Program Files\JetBrains\PycharmProjects\Day14"
print(os.listdir(BASE_DIR))

# 6.重命名当前文件或者文件夹名字
img_path = os.path.join(BASE_DIR, 'img')
os.makedirs(img_path, exist_ok=True)
old_name = img_path
new_name = os.path.join(BASE_DIR, 'new_img')
# os.rename(old_name, new_name)

img_path = os.path.join(BASE_DIR, 'add.json')
old_name = img_path
new_name = os.path.join(BASE_DIR, 'new_add.json')
# os.rename(old_name, new_name)

# 7.列出当前文件的元信息
print(os.stat(BASE_DIR))
# os.stat_result(st_mode=16895, st_ino=5629499534458662, st_dev=1183741232, st_nlink=1, st_uid=0, st_gid=0,
# st_size=4096, st_atime=1722502256, st_mtime=1722502256, st_ctime=1722473654)
# st_mode: inode 保护模式
# st_ino: inode 节点号。
# st_dev: inode 驻留的设备。
# st_nlink: inode 的链接数。
# st_uid: 所有者的用户ID。
# st_gid: 所有者的组ID。
# st_size: 普通文件以字节为单位的大小；包含等待某些特殊文件的数据。
# st_atime: 上次访问的时间。
# st_mtime: 最后一次修改的时间。
# st_ctime: 由操作系统报告的"ctime"。在某些系统上（如Unix）是最新的元数据更改的时间，在其它系统上（如Windows）是创建时间（详细信息参见平台的文档）。
file_path = os.path.join(BASE_DIR, 'user_data.json')
print(os.stat(file_path))
# s.stat_result(st_mode=33206, st_ino=1407374883798829, st_dev=1183741232, st_nlink=1, st_uid=0, st_gid=0, st_size=180,
# st_atime=1722498067, st_mtime=1722498067, st_ctime=1722478346)

# 9.获取到当前所在的工作目录
print(os.getcwd())
# D:\Program Files\JetBrains\PycharmProjects\Day14

# 10.切换工作目录
day13_path = os.path.join(os.path.dirname(BASE_DIR), 'day13')
print(day13_path)
# D:\Program Files\JetBrains\PycharmProjects\day13
os.chdir(day13_path)
print(os.getcwd())
# D:\Program Files\JetBrains\PycharmProjects\day13


# 11.执行shell 命令 ---> Windows上执行不出来
# windows ---> dir
# macOS/linux ---> ls
os.system("dir")
# 11、12win好多问题

# 12.执行命令获取结果
res = os.popen("dir").read()
print(res)
```



# 系统

```python
# os + json
# 写一个文件处理系统
# json模块学了 用json模块存储用户信息
# 做一个登录注册文件系统
# 查看当前文件夹下的所有文件名
# 创建文件夹
# 删除文件夹
# 重命名文件夹
# 文件拷贝

# 注册 register
# 登录 login
# 查看当前文件夹下的所有文件名
# 创建文件夹
# 删除文件夹
# 重命名文件夹
# 文件拷贝


import json
import os
import shutil  # 添加文件拷贝功能所需的模块

# 全局登陆字典
login_dict = {
    "username": None
}

# 用户数据存储
user_data_all = {}

# 数据处理函数
def handle(file='user_data.json', mode='r', encoding="utf-8", data=None):
    if mode == "r":
        user_data_all = {}
        if os.path.exists(file):
            try:
                with open(file=file, mode=mode, encoding=encoding) as fp:
                    content = fp.read().strip()
                    if content:
                        user_data_all = json.loads(content)
                    else:
                        print("文件内容为空，初始化空用户数据。")
            except json.JSONDecodeError:
                print("JSON 解析错误，文件格式不正确。")
        else:
            print("用户数据文件不存在，将创建新文件。")
        return user_data_all
    elif mode == "w" and data is not None:
        with open(file=file, mode='w', encoding=encoding) as fp:
            json.dump(data, fp, ensure_ascii=False, indent=4)
def get_username_password():
    username = input("请输入用户名 :>>>> ").strip()
    password = input("请输入密码 :>>>> ").strip()
    return username, password

def input_hobby():
    hobby_list = []
    while True:
        hobby = input("请输入爱好(q退出) :>>>> ").strip()
        if hobby.lower() == "q":
            print("当前输入爱好结束!")
            break
        if hobby and hobby not in hobby_list:
            hobby_list.append(hobby)
            print(f"当前输入爱好 {hobby} 已添加!当前已有爱好 :>>>> {', '.join(hobby_list)}")
    return hobby_list

def input_age():
    age = input("请输入年龄 :>>>> ").strip()
    if not age.isdigit():
        return False, "当前年龄非法!不是数字!"
    age = int(age)
    if age < 0 or age > 150:
        return False, "当前年龄已超出正常人类范畴!"
    return True, age

def register():
    print("欢迎来到注册功能!")
    username, password = get_username_password()
    flag, age = input_age()
    if not flag:
        return flag, age
    hobby_list = input_hobby()
    role = "admin" if username == "dream" and password == "521" else "user"
    user_data_all = handle()
    if username in user_data_all:
        return False, f"当前用户 {username} 已存在!请去登录!"
    user_data_all[username] = {
        "username": username,
        "password": password,
        "age": age,
        "hobby": hobby_list,
        "role": role
    }
    handle(mode="w", data=user_data_all)
    return True, f"用户 {username} 注册成功!"

def login():
    print("欢迎来到登陆功能!")
    username, password = get_username_password()
    user_data_all = handle()
    user_info = user_data_all.get(username)
    if not user_info:
        return False, f"当前用户 {username} 不存在 请先注册!"
    if user_info.get("password") != password:
        return False, f"当前用户 {username} 用户名或密码错误 请重新输入!"
    login_dict.update({
        "username": user_info.get("username"),
        "role": user_info.get("role")
    })
    return True, f"当前用户 {username} 登陆成功!"

# 文件系统功能

def list_files():
    files = os.listdir(".")
    print(f"当前目录下的文件和文件夹有: {', '.join(files)}")
    return True, "文件列表显示成功"

def create_directory():
    dir_name = input("请输入要创建的文件夹名称 :>>>> ").strip()
    if not dir_name:
        return False, "文件夹名称不能为空!"
    try:
        os.makedirs(dir_name)
        return True, f"文件夹 {dir_name} 创建成功!"
    except FileExistsError:
        return False, f"文件夹 {dir_name} 已存在!"
    except Exception as e:
        return False, str(e)

def delete_directory():
    dir_name = input("请输入要删除的文件夹名称 :>>>> ").strip()
    if not dir_name:
        return False, "文件夹名称不能为空!"
    try:
        os.rmdir(dir_name)
        return True, f"文件夹 {dir_name} 删除成功!"
    except FileNotFoundError:
        return False, f"文件夹 {dir_name} 不存在!"
    except OSError:
        return False, f"文件夹 {dir_name} 不是空的，无法删除!"
    except Exception as e:
        return False, str(e)

def rename_directory():
    old_name = input("请输入要重命名的文件夹名称 :>>>> ").strip()
    new_name = input("请输入新的文件夹名称 :>>>> ").strip()
    if not old_name or not new_name:
        return False, "文件夹名称不能为空!"
    try:
        os.rename(old_name, new_name)
        return True, f"文件夹 {old_name} 重命名为 {new_name} 成功!"
    except FileNotFoundError:
        return False, f"文件夹 {old_name} 不存在!"
    except Exception as e:
        return False, str(e)

def copy_file():
    src_file = input("请输入要拷贝的文件路径 :>>>> ").strip()
    dest_file = input("请输入目标文件路径 :>>>> ").strip()
    if not src_file or not dest_file:
        return False, "文件路径不能为空!"
    try:
        shutil.copy(src_file, dest_file)
        return True, f"文件 {src_file} 拷贝到 {dest_file} 成功!"
    except FileNotFoundError:
        return False, f"文件 {src_file} 不存在!"
    except Exception as e:
        return False, str(e)

# 主功能菜单
func_menu = '''
***************** 文件系统 ***************** 
                    1. 注册
                    2. 登陆
                    3. 查看当前文件夹下的所有文件
                    4. 创建文件夹
                    5. 删除文件夹
                    6. 重命名文件夹
                    7. 文件拷贝
                    q. 退出
***************** 文件系统 ***************** 
'''

func_dict = {
    "1": register,
    "2": login,
    "3": list_files,
    "4": create_directory,
    "5": delete_directory,
    "6": rename_directory,
    "7": copy_file
}

def main():
    while True:
        print(func_menu)
        func_id = input("请输入功能ID :>>>> ").strip()
        if func_id == "q":
            print("欢迎下次使用!")
            break
        func = func_dict.get(func_id)
        if not func:
            print("当前功能ID不存在!")
            continue
        flag, msg = func()
        print(msg)

if __name__ == "__main__":
    main()
```



TASK：

1. 手打一遍所有代码 看自己电脑上各个输出 记录下来 对os jason两个模块好好研究下 √

2. 笔记 √

3. 使用到 os + jason 写系统

   文件处理系统 用jason模块存储用户信息

   写一个文件系统，具有以下功能：

   注册登录

   查看当前文件夹下的所有文件名

   创建文件夹

   删除文件夹

   重命名文件夹
   
   文件拷贝