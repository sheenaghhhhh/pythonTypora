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
