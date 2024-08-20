# 知识Test&Answer

## 01Quiz：

1. 计算机的五大组成部分，各部分的功能
2. 计算机的三大核心硬件有哪些，主要功能是
3. 我们使用的Python解释器的版本



## 01Answer：

1. 控制器：控制整个计算机的硬件协调工作

   运算器：执行计算机中各种算数和逻辑运算

   存储器：存储临时或永久性的数据

   输入设备：向计算机中输入信息（程序、数据、声音、文字、图形、图像等）的设备

   输出设备：硬件系统的终端设备，将计算机计算出的内容展示给用户

2. CPU：计算机的控制中心，负责调度和运算

   硬盘：存储永久数据

   内存：存储临时的数据

3. CPython--->3.10.x



## 02Quiz：

1. 如何实现多版本Python解释器共存
2. 如何修改默认Python解释器版本
3. 什么是变量，什么是常量。各有什么特点。
4. 变量的三大特性有哪些，如何查看
5. 八大基本数据类型有哪些各有什么特点



## 02Answer：

1. 不同版本Python解释器 对应安装目录的Python.exe，但是解释器地址不同，将各自重命名为python+版本号.exe

2. 打开环境变量，将想要的python解释器版本放到最上面

3. 变量是用于存储数据值的标识符，可以通过变量名访问和操作这些数据。变量就是可以变化的量，量指的是事物的状态。

   更灵活地处理数据。实现动态的状态和行为。

   常量就是指程序运行过程中值不会发生改变的值。

4. （1）变量内存地址 : 每一次运行代码的时候都会重新开辟内存空间 存储变量值 print(id(username))

   （2）变量类型 : 知道某个变量类型，通过type

   （3）变量值：直接打印变量名就能看到变量值 print(username)

5. 数字类型

   1. 整数类型int
   2. 浮点类型float

   字符串类型str

   列表类型list

   字典类型dict

   布尔类型bool

   元组类型tuple

   集合类型set




## 03Quiz：

1. 流程控制语句有哪些，语法格式
2. 如何向程序输入内容，程序如何输出内容
3. 尽可能多的写出，字符串的内置方法
4. 进制转换有哪几种方式
5. 三元运算的基本格式，比较两个数的大小有几种方式
6. ==和is的区别



## 03Answer：

1. 最常见的【顺序结构】

   【分支结构】

   if

   (1)单分支

   ​	if 条件:

   ​		条件为真执行

   (2)双分支

   ​	if 条件:

   ​		条件为真执行

   ​	else:

   ​		条件为假执行

   (3)多分支

   ​	if 条件1:

   ​		条件1为真执行

   ​	elif 条件2:

   ​		条件2为真执行

   ​	else:

   ​		都不符合则执行

   【循环结构】

   (1)while

   ​	while 终止条件:

   ​		条件为真则执行

   (2)range

   ​	list(range(1, 9, 2))

   (3)for

   ​	可迭代类型的数据

   ​	for i in data:

   ​		print(i)

   (4)continue

   (5)break

   (6)死循环

   (7)标志位

   (8)while~else

2. input print

3. 字符串的内置

   "-".join("gohome")

   "dream"[0]

   "dream"[1:4:2]

   len(a)

   "a" in "dream"

   "draeam".strip("a")	"draeam".lstrip("d")	 "draeam".rstrip("m")

   "dream|a|sd|opp".split("|")

   for while

   字符串*数字

   .upper() 、 .lower()

   [0]=="", [-1]= = & .startwith() .endswith()

   $s .format() f"{}"

   .reaplace("a", "b")

   .isdigit() .isdecimal

4. 二进制、八进制、十进制、十六进制

   bin oct dec hex

   int("字符串",字符串几进制)->把这个字符串从几进制转为10进制

5. print(a if 条件 else b)

   if 真 a 否则 b

   print((b, a)[条件])

   if 真 a 否则 b

6. ==和is都是去判断两个变量是否相等，区别在于= =判断值是否相等，而is判断值和内存地址是否一致



## 04Quiz：

1. 尽可能多的写出你所知道的数据类型的内置方法



## 04Answer：

1. 整数

   类型转换 int()

   进制转换bin oct hex

   int去转换

   

   浮点数

   float()

   .isdigit()

   .isdecimal()

   

   字符串

   .join()

   "str"[0];"string"[-1]

   "string"[0:3:2]

   len()

   in/not in

   .strip()

   .lstrip()/.rstrip()

   .split()

   for/while

   string * n

   .upper() .lower()

   .startwith() .endwith()

   %s

   '{}'.format()

   f'{a}'

   .replace()

   .isdigit()/.isdecimal

   

   列表

   list()

   [0:1:1]

   

## 05Quiz：

1. 打开文件的两种方式
2. 操作文件的几种方式
3. 操作文件的几种方法
4. 什么是字符编码
5. 字符编码的发展过程
6. 如何进行编码和解码

## 05Answer：

1. 第一种

   fp = open (,,)

   操作语句

   fp.close()

   第二种

   with open(,,) as fp:

   ​	操作语句

2. r w a + b

3. fp.read() : 一次性全部将内容读取出来

   data = fp.readline() : 只读取一行的数据

   fp.readlines():读取所有数据但是每一行数据是列表中的每一个元素

   for i in fp : 句柄对象可以被遍历 遍历得到的每一个元素是当前行

   fp.readable() : 判断当前句柄对象是否可读

   fp.read() 一次性全部将内容读取出来

   fp.write("hello world") ： 一次性将所有内容全部写入

   fp.writelines(列表) : 将列表中的每一个元素首尾拼接后再一次性写入

   fp.writable() : 判断当前句柄对象是否可写

4. 翻译的过程 就是将字符和模板中的对应关系一一找到的过程

   编码就是指向我们的中文转换成计算机可以识别的二进制数据 或者将计算机可以识别

5. 第一阶段 一家独大 ascii

   第二阶段 诸侯割据 映射表 ascii/shift_jis/gbk

   第三阶段 大一统 utf-8 万国符

6. x.encode()

   x.decode()



## 06Quiz：

1. 文件操作模式有哪些，各有什么特点
2. 文件操作方法有哪些，各有什么特点
3. 什么是异常
4. 异常捕获有哪些关键字。语法
5. 如何主动抛出异常

## 06Answer：

1. r 读 如果不存在会报错

   w 写 重覆盖

   a 在原本文件的末尾去添加

   +既能读又能写

   b 二进制

2. fp.read() : 一次性全部将内容读取出来

   data = fp.readline() : 只读取一行的数据

   fp.readlines():读取所有数据但是每一行数据是列表中的每一个元素

   for i in fp : 句柄对象可以被遍历 遍历得到的每一个元素是当前行

   fp.readable() : 判断当前句柄对象是否可读

   

   

   fp.write("hello world") ： 一次性将所有内容全部写入

   fp.writelines(列表) : 将列表中的每一个元素首尾拼接后再一次性写入

   fp.writable() : 判断当前句柄对象是否可写

3. 程序在执行过程中遇到的报错，导致程序崩溃的代码就是异常

4. 关键字+语法

   raise

   assert

```python
'''
try:
    # 正常执行可能会发生错误的代码
    # print(1 / 0)
    int("a")
    # print(1)
# Exception 捕获所有异常类型
except Exception as e:
    # 在处理异常的时候可以通过变量e查看到当前的异常类型和异常信息
    # 异常处理机制
    print(f"发生异常了 {e}")
    ...
# 当代码没有抛出异常时 ， 结束正常代码会自动进入到 else 语句中
else:
    print(f"当前走入到 else 中")
# 不管代码抛出还是不抛出异常都会走入到 finally 当中
finally:
    print(f"当前走入到 finally 中")
'''
```

5. 主动抛出

```python
# 借助关键字 raise
# 语法 raise [Exception [, args [, traceback]]]

"""
for i in range(10):
    if i == 2:
        raise ValueError("参数不能为2")
"""
```



## 07Quiz：

1. 什么是函数
2. 函数的参数分类
3. 函数的调用方式

## 07Answer：

1. 代码的集合体可以写成函数，帮助我们实现重复代码的输出

2. 分类1：形参和实参

   def fun(a, b) 形参：在定义的时候放在函数名后面的参数

   ​	...

   fun(x, y) 实参：在调用的时候实际传入的值

   

   分类2：关键字参数和位置参数

   def fun(username, password)

   ​	...

   fun("sheenagh", "526")

   fun(password="526", username="sheenagh")

   混用的时候，关键字需要放到后面

   fun("sheenagh", password="526")

   

   其他：默认参数

   def fun(username, password, age="20")

   

   其他：可变长参数

   可变长位置参数

   可变长关键字参数

   def fun(a, b, *args):

   def add(x, y, *args, **kwargs):

3. 直接调用

   def fun():

   ​	...

   fun()

   间接调用

   def fun():

   ​	...

   haha = fun

   haha()

   表达式调用

   和返回值有关 对元组进行解包赋值

   def fun():

   ​	...

   ​	return a, b

   x, y = fun()

   

## 08Quiz：

1. 什么是闭包函数

2. 什么是作用域，查找顺序和加载顺序

3. 什么是名称空间。查找顺序和加载顺序

4. 如何局部修改全局可变数据类型和全局不可变数据类型

5. 如何在局部的局部修改可变和不可变数据类型

   

## 08Answer：

1. 内嵌函数对外部函数作用域有引用的函数就叫闭包函数

2. 作用域就是变量名和变量值可以被访问的范围

   查找顺序：内嵌-局部-全局-内建

   加载顺序：内建-全局-局部-内嵌

3. 名称空间就是存放变量名和变量值映射关系的地方

   查找顺序：局部-全局-内建

   加载顺序：内建-全局-局部

4. 可变类型

   li = [1, 2, 3]

   def outer():

   ​	li.append(4)

   不可变类型

   a = 18

   def outer():

   ​	global a

   ​	a = 20

5. 可变

   def outer():

   ​	li = [1, 2, 5]

   ​	def inner()

   不可变

   def outer():

   ​	age = 19

   ​	def inner():

   ​	nonlocal age

   ​	age = 28



## 09Quiz：

1. 函数的参数有哪些
2. 什么是装饰器
3. 什么是闭包函数
4. 装饰器的分类
5. 装饰器的模板
6. 写一个取款函数，取款之前验证用户名和密码，写两个装饰器分别验证用户名和密码

## 09Answer：

1. 按照函数定义时位置的不同

   形参

   实参

   

   按照传值放的不同分

   位置参数

   关键字参数

   

   可变长位置参数 *args

   可变长关键字参数 *kwargs

   

   默认参数 定义时函数名后面的参数有默认值的形参

   

   命名关键字参数

   def add(x, y, *, z, e)

2. 装饰器

   在不修改被装饰对象源代码和调用方式的前提下为被装饰对象添加额外的功能

3. 闭包函数

   函数内部再定义一个函数 并且这个函数用到了外边函数的变量

4. 分类

   无参装饰器

   有参装饰器

5. 模板-无参装饰器

```python
def outer(func):
    def inner():
        # 在真正地进入到上面的 func 之前进行认证
        # 执行真正的函数
        result = func()
        # 处理执行后的结果
        # 对返回结果进行定制化返回
        return result
    return inner
```

模板-有参装饰器

```python
def outer(func):
    def inner(*args, **kwargs):
        # 在真正地进入到上面的 func 之前进行认证
        # 执行真正的函数
        result = func(*args, **kwargs)
        return result
    return inner
```

6. 写一个取款函数，取款之前验证用户名和密码，写两个装饰器分别验证用户名和密码



## 10Quiz：

1. 什么是模块
2. 什么是包
3. 模块的加载顺序和查找顺序
4. json模块如何保存和读取数据
5. 生成器的创建方式，特点
6. 什么是可迭代对象，什么是迭代器对象



## 10Answer：

1. 一个py文件其实就是一个模块 模块名=文件名
2. 多个py文件+一个 `__init__.py`文件的文件集合体

3. 内建->py

   py文件->内建

4. import jason

   jason.dumps

   jason.dump

   jason.loads

   jason.load

5. g = (x * 2 for x in range(5))

   def my_generator():
       yield 1
       yield 2
       yield 3

   节省内存 惰性计算 无限序列

5. 具有 `__iter__.py`方法的对象

   包括str list dict tuple set；

   具有 `__iter__.py`和 `__next__.py`方法的对象

   可以用可迭代对象 `.__iter__()`去生成

## 11Quiz：

1. os 模块至少写出 5个常用的方法，尽可能多的写
2. 什么是json 格式的数据，有何特点，json 模块的几个方法是什么，有什么作用

## 11Answer：

1. os.path.abspath() 文件绝对路径

   os.path.dirname() 文件夹绝对路径

   os.path.exist() 判断路径是否存在

   os.path.join() 拼接文件路径

   os.path.split() 切割文件路径 第2部分文件名or最后的文件夹名

   os.path.basename() 获取结尾文件名or最后的文件夹名

   os.path.isfile() 判断是否为文件

   os.path.isabs() 判断是否为绝对路径

   os.path.isdir() 判断是否为文件夹路径

   os.path.getatime() 获取文件/文件夹的最后访问时间

   os.path.getctime() 获取文件/文件夹的创建时间

   os.path.getMtime() 获取文件/文件夹的最后修改时间

   os.path.getsize() 获取文件大小(单位是字节)

   os.path.sep() 获取系统的路径分隔符

   

   os.mkdir() 创建单级文件夹

   os.makedirs() 创建多级文件夹

   os.rmdir() 删除单级文件夹

   os.removedirs() 创建多级文件夹

   os.listdir() 列出所有文件名

   os.rename() 重命名文件or文件夹名字

   os.stat() 列出元信息

   os.getcwd() 获取到当前所在的工作目录

   os.chdir() 切换目录

   



2. jason数据做好了序列化和反序列化的相关操作

   jason.dumps()

   jason.loads()

   jason形式的str和python数据类型obj的互转

   jason.dump(x, fp)

   jason.load(fp=fp)

   jason文件和python数据类型obj的互转
   
   

## 12Quiz：

1. 什么是字符组，字符组如何用
2. 什么是量词，都有哪些量词
3. 什么是贪婪模式，如何取消贪婪模式
4. 如何执行系统命令
5. 默写手机格式匹配的正则表达式



## 12Answer：

1. 同一个位置可能出现的字符组成 [字符组]

   用法： [0123] [0-9] [a-z] [a-zA-Z]

2. 可以指定前面模式的重复次数

   *0to多

   +1to多

   ? 0to1

   {n} n

   {n,} nto多

   {n,m} ntom

3. 贪婪模式 尽可能多拿

   取消贪婪模式：非贪婪模式 用?跟在量词后面 尽可能少匹配字符

4. os

5. 手机号码：

   ^(13[09]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$





## 13Quiz：

1. 如何将数字和字符之间进行互转，
2. 一共有9篇文章，用代码算一算需要多少页，每页4篇





## 13Answer：

1. print(ord('A'))

   65

   print(chr(65))

   A

2. divmod(9, 4)

   (2, 1)

   3页





## 14Quiz: 

1. 什么是装饰器
2. 装饰器的模板
3. 文件打开的方式
4. 什么是可迭代对象，迭代器对象
5. 什么是闭包函数
6. 默写随机验证码生成器



## 14Answer:

1. 在不修改被装饰对象源代码和调用方式的前提下

   为被装饰对象添加额外的功能

2. ```python
   def outer(func):
       def inner():
           # 在正式执行真正的func前做一些校验
           if ...:
               ...
           # 执行 func 真正的函数 获取到函数的返回值
           result = func()
           # 对真正执行后的函数的结果进行定制化输出
           if ...:
               ...
           return result
       return inner
   ```

3. ```python
   # 1.手动打开
   fp = open(file_path, "w", encoding="utf-8")
   fp.write()
   fp.close()
   # 要手动关闭
   
   # 2.自动回收资源
   with open(file_path, "w", encoding="utf-8") as fp:
       fp.write()
   # 会自动关闭
   ```

4. 可迭代对象：有`__iter__`方法的对象

   包括str dict list tuple set

   迭代器对象：有`__iter__`和`__next__`方法的对象

5. 内嵌函数对外部作用域有引用的函数就叫闭包函数

6. ```python
   # 默写随机验证码
   # 大小数字随机
   def verify(n):
       code = ""
       for i in range(n):
           small = chr(random.choice([i for i in range(ord("a"), ord("z")+1)]))
           big = chr(random.choice([i for i in range(ord("A"), ord("Z")+1)]))
           number = str(random.randiant(0, 9))
           code += random.choice(big, small, number)
   ```



## 15Quiz: 

1. 什么是迭代器，什么是可迭代对象
2. 什么是闭包函数，有哪些应用场景
3. 遍历列表时，既能获取到当前元素的索引又可以获取到索引对应的值你有哪些思路
4. 生成器的创建方式
5. 什么是垃圾回收机制
6. Python是值传递还是引用传递



## 15Answer:

1. 迭代取值的工具 既有`__iter__`也有`__next__`方法的对象

   具有`__iter__`方法的对象

2. 内嵌函数对外部作用域有引用的函数就叫闭包函数

   使用场景：计时器 验证密码/是否登录/是否激活等操作

3. 索引 i 0->len(list)-1 list[i]

   enumerate()

   for index in range(len(list)):

4. ```python
   g = (x for x in range(5))
   # 元组生成式
   
   def generator():
       yield 1
       yield 2
   # 关键字卡住
   ```

5. 用来回收不可用的变量值所占用的内存空间

   三个过程

   引用计数->标记清除->分代回收

6. Python既不是值传递 也不是引用传递 他有自己的传递方式 既有可变变量也有不可变变量





## 16Quiz:

1. 什么是面向过程，什么是面向对象
2. 类语法
3. 类里面的属性分类有哪些
4. 类中的self是谁
5. 分析初始化方法的推导步骤





## 16Answer:

1. 面向过程 将程序流程化

   面向对象 对象是某个整体 盛放需要的数据和功能

2. ```python
   class 类(参数):
       代码体
   ```

3. 数据属性

   功能属性

4. 是对象本身

5. 本来初始化数据比较麻烦 所以在类内部进行名称空间函数更新 写init_obj_cls or init_obj_obj

   但这样也不够好 进一步提取功能赋给类 init_obj函数传入的变成了self

   最后变成 ``__init__``方法 在内部写更新

   终极版->不用update了 就是self.属性名=属性值





## 17Quiz:

1. 什么是封装
2. 封装的好处
3. 如何将函数属性包装成数据属性
4. 封装会发生哪些事
5. 你都用过os模块的那些方法，举例说明





## 17Answer:

1. 把某些数据保护和隐藏起来

2. 可以防止外部代码随意修改对象内部的代码

3. 在函数定义前面加@property

4. ```python
   首先类内部定义的时候把变量名用双下划线去声明
   
   这样初始化的时候 从最开始的变量名 变成了 __变量名
   
   进一步变成了 _类__变量名
   
   在类内部时 直接 类.__变量名可以调用
   但是在类外部 需要对象._类__变量名才可以
   
   这种变形只会发生一次
   ```

5. os.path.x 和 os.x

   ```python
   # path
   os.path.abspath()
   os.path.dirname() 文件夹
   os.path.exists()
   
   os.path.join()
   os.path.split()
   
   os.path.basename() 结尾
   os.path.isfile()
   os.path.isabs()
   os.path.isdir()
   
   os.path.atime()
   os.path.ctime()
   os.path.mtime()
   
   os.path.getsize()
   os.path.sep() 分隔符
   
   
   # os.x
   os.mkdir()
   os.makedirs()
   os.rmdir()
   os.removedirs()
   
   os.listdir()
   os.rename()
   os.stat() 元信息
   
   os.getcwd()
   os.chdir()
   os.system()
   os.popen()
   
   
   
   ```

   



## 18Quiz:

1. 什么是继承
2. 什么是菱形继承
3. 菱形继承的属性查找顺序
4. 封装属性和不封装属性的查找顺序
5. 什么是抽象，什么是继承
6. 抽象类的特点，如何实现？





## 18Answer:

1. 创造一个新的类，会继承母类的所有属性，并可以自己添加自己的属性
2. 在多重继承中，一个类同时继承两个不同的类，而这两个类又继承自同一个母类，导致继承结构形成一个菱形的形状。

3. 新式类是广度优先，经典类是深度优先

4. 封装属性 从自己执行该方法的类中去找 如果没有就报错了

   不封装属性 是从自己本身开始的 实例化类里找 找不到的话去母类里找

5. 抽象：将某一类事物或者东西 公共的部分提取出来

   继承：将某一些公共的部分提取出来，让某一些类去继承这些公共的部分

6. 抽象类只能被继承不能被实例化，实际不存在而是基于类抽象化而来的一种类。而且子类必须实现抽象方法

   借助ABC模块 用@abstractmethod

