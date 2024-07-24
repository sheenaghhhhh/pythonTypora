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

   

## 05Quiz

1. 打开文件的两种方式
2. 操作文件的几种方式
3. 操作文件的几种方法
4. 什么是字符编码
5. 字符编码的发展过程
6. 如何进行编码和解码

## 05Answer

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
