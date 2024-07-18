# 1 昨日内容回顾

```python
# 【一】八大基本数据类型
# 【1】数字类型
# (1)整数类型
# 语法 变量名 = 整数值
# 做算术运算
# (2)浮点数类型
# 做算术运算

# 【2】字符串类型
# 四种语法 （单双引号 三个单双引号）
# (1)索引取值
# 正向 0开始
# 负向 -1开始从右逆序

# (2)字符串格式化输出语法
# %s 站位
# {}站位 .format{}
# f"{变量名}"

# 【3】列表
# 变量名 = [1, 2, 3, 4]
# 可以存储多个相同类型的变量值
# 允许索引修改值

# 【4】元素
# 变量名 = (1, 2, 3, 4)
# 一个元素的时候加, 不加就是原本类型
# 字符串在声明时不要加,
# 不能被索引修改
# 支持索引取值

# 【5】字典
# 键值对
# 变量名 = {key1 : value1, key2 : value2}
# 键建议用字符串or数字
# 底层 基于哈希的值去排列

# 【6】布尔
# True/False
# 假：0,空,False
# 真：其余

# 【7】集合
# 无序 不重复
# 变量名 = {a, b, c}

# 集合无序 ---> 针对数字以外的字符
# 不重复 ---> 自动去除重复项 保留一个

# 并交差
# union
# intersection
# difference

# 【二】程序与用户进行交互
# input
# print
# end \n

# 【三】基本运算符
# 【1】算术运算符
# + - * / % // **
# 【2】比较运算符
# > >= < <= == !=
# 【3】赋值运算符
# += -= *= /= %= //= **=
# 链式赋值 a= b= c= 99
# 交叉赋值 x, y = y, x
# 解包赋值 a, b = [1,2] # 既不能多也不能少
# 【4】逻辑运算符
# and or not
# not > and > or
# 【5】身份运算符
# is
# is not
# 【6】成员运算符
# in
# not in
# 【7】 == 和 is 的区别
# == 比较两者的值
# is 比较值和id

# 【四】流程控制语句
# 【1】顺序结构
# 从上至下依次执行

# 【2】分支结构
# 1.单分支
# if 条件:
#   代码

# 2.双分支
# if 条件:
#   代码
# else:
#   代码

# 3.多分支
# if 条件:
#   代码
# elif:
#   代码
# else:
#   代码

# 【3】循环结构
# 1.while语法
# 重复执行某部分代码 直到符合终止条件为止
'''
while 终止条件:
    代码
'''

# 2.range关键字
print(range(1,6),type(range(1,6)))
# python 2.x 优化

# range(起始数字，终止数字，步长)
print(list(range(1, 9)))
# 步长:隔几个索引取一次
print(list(range(0, 9, 2)))

# (3)for关键字
# 遍历可迭代类型的数据
# 可迭代 含义就是能被索引取值
# 遍历 每一个元素都拿一遍
'''
data_str = "dream"
data_str = {"1":"3","2":"4"}
# for 循环在遍历字典的时候默认拿到的事当前字典的键
for i in data_str:
    print(i)
'''

# (4)continue
# 主要搭配逻辑语法使用
# 能够让循环退出本次循环继续下一次循环
# 当达到某个条件continue

# （5）break 关键字
# 主要搭配逻辑语法使用
# 能够让循环终止
#  当达到某个条件 break 以后 当前打印就直接退出循环

# （6）死循环
# 当前 while 循环语句没有终止条件代码一直运行
# 我们在写代码的时候应该避免出现死循环

# （7）标志位
# 主要是用在  while 循环条件上面
# 当while循环条件为 False的时候自动终止后续循环
# 将 条件做成一个标志 当达到某个条件的时候直接将 标志位设置为 False

# （8）while ~ else
'''
count = 0

while count < 6:
    count += 1
    print(count)
# 当while循环结束的时候会自动进入到 while 循环
# 除非主动结束当前循环
else:
    print(count)
'''

'''
for i in range(0,6):
    print(i)

count = 0
while count < 6:
    print(count)
    count += 1
'''

'''
data_str = [12, 2, 3, 45, 6, 4]
for i in data_str:
    print(i)
'''

'''
count = 0
# len ： 计算当前类型中的元素个数
# print(len(data_str))
while count < len(data_str):
    print(data_str[count])
    count += 1
'''
```

# 2 三元表达式

```py
# 三元运算
# ●三元表达式（三目运算符）
# 能够简洁我们的代码
# 三元表达式其实是将if...else...判断语句的简化表达，代替很多if else
# ●和if-else一样，只有一个表达式会被执行。
# 因此，三元表达式中的if和else可以包含大量的计算，但只有True的分支会被执行
# ●在Java、C、JavaScript等语言中，他们的格式为：a?b:c

# 优化代码
# 真的结果 if 条件 else 条件为假的结构

a = 10
b = 3
print(a if a > b else b)

# 用到元组或者字典上
print((b, a)[a < b])
# (20,10)[1]--->10
# (20,10)[0]--->20

print({True: a, False: b}[a < b])

# 只适用于简单的逻辑判断 不适用于复杂的逻辑判断
```

# 3 整点数和浮点数的内置方法

- 数据类型的内置方法
- 当我们在操作 字符串 / 列表
- 想到对字符串or列表做一些高级的操作
- 字符串 判断这个字符是否以某个字符开头
- 列表 添加元素 删除元素 修改元素
- 官方根据上面的功能给我们提供了一些公共的接口(方法)

## 3.1 整数类型

```
# 语法 变量名=整数值

num = 42

# 1.强制类型转换
# 可以将符合整数类型的字符串强制转换成整数类型
num_str = "55"
num_str_int = int(num_str)
print(num_str, type(num_str))
print(num_str_int, type(num_str_int))

# 2.进制转换
# 十进制 decimal dec
# 二进制 每一个元素组成都小于2 且以0b开头
print(bin(999))  # 0b1111100111
# 八进制 每一个元素组成都小于8 且以0o开头
print(oct(999))  # 0o1747
# 十六进制 每一个元素组成都小于16 且以0x开头
print(hex(999))  # 0x3e7

# 3.int也能做类型转换
print(int("0b1111100111", 2))
print(int("0o1747", 8))
print(int("0x3e7", 16))
```

## 3.2 浮点数类型

```python
# 语法 变量名=浮点数值
# 1.强制类型转换
# 将符合浮点数格式的字符串转换为浮点数
print(float("1.11"))

# 【整数类型和浮点数类型方法补充】
# 1.判断当前字符串是否符合整数类型格式
num1 = b'4'
print(num1, type(num1))
num2 = '4'
num3 = '四'
num4 = 'V'  # 罗马数字

# 2.判断当前数字是否符合数字类型
print(num1.isdigit())
print(num2.isdigit())
print(num3.isdigit())
print(num4.isdigit())
# 二进制形式和字符串形式的整数类型都是整数
# 中文汉字和罗马数字不符合整数

# 3.判断当前数字是否符合数字类型
# print(num1.isdecimal())
print(num2.isdecimal())
print(num3.isdecimal())
print(num4.isdecimal())

# 小结
# 在python中没有判断当前字符串是否是浮点数格式的方法

age = input("请输入年龄：")
if age.isdigit():
    age = int(age)
else:
    print(f"当前 {age} 不是合法的数字")
```

# 4 字符串的内置方法

## 4.1 字符串的语法

```python
# 变量名 = "字符"
```

## 4.2 优先记住的内置方法

```python
# 1.字符串的拼接
# + 号
print("1" + "2")
# 扩充 ''.join(可迭代类型) # 借助列表或者元组
print(''.join(["1", "2", "3"]))
# 字符
print('|'.join(["1", "2", "3"]))
print('*'.join(["1", "2", "3"]))
print('-'.join("gohome"))

# 2.字符串索引取值
# 正向索引：从0开始
# 负向索引：从-1开始从右往左
print("dream"[0])
print("dream"[-1])
# 字符串可以索引取值但是不支持索引改值
# name = "dream"
# name[0] = "s" # 'str' object does not support item assignment

# 3.切片
# 按照指定位置将某部分隔离出来
# dream -> re
# 字符串[索引] -> 索引取值
# 字符串[索引1:索引2] -> 根据区间将整体某部分切离出来
# [a:b)
print("dream"[1:3])
# [a:b:c) c是步长
print("dream"[1:4:2])
print("dream"[-3:-1])

# 字符串[::-1] 将整个字符串进行翻转
print("dream"[::-1])

# 4.计算长度
# len(变量名)
print(len("dream"))

# 5.成员运算
# 判断某个字符是否在某个成员内
# "dream" "d"
print("d" in "dream")

# 6.去除特殊字符
# 一个字符串 "$dream$" 首尾的$不要 仅限首尾
print("$dream$".strip("$"))
# 默认值是空格或换行
data_str = "     dream   "
data_str_one = '''

dream
dream
dream
'''
print(data_str)
print(data_str.strip())
print(data_str_one)
print(data_str_one.strip())

# 控制左右去除的位置
# 去除左面的特殊字符
print("$dream$".lstrip("$"))
# 去除右面的特殊字符
print("$dream$".rstrip("$"))

# 7.切分字符串
# 按照指定的分隔符将字符串进行切割 并且分隔符会消失
names = "dream|opp|oppp|hope"
print(names.split("|"))

user_pwd = "abc:123"
username, password = user_pwd.split(":")
print(username, password)

# 8.遍历字符串
# for
# while 用索引

# 9.字符串重复
# 字符串 * 数字

# 10.大小写转换
name = "UserName"
# 将上面的单词转换为全大写
print(name.upper())
# 将上面的单词转换为全小写
print(name.lower())

# 11.首尾字符判断
# "dream" 判断以d开头 以m结尾
print("dream"[0] == "d")
print("dream"[-1] == "m")
print("dream".startswith("d"))
print("dream".endswith("m"))

# 12.格式化输出
# %s
# '{}'.format()
# f"{a}"

# 13.替换指定字符
name = "dream"
# dream -> d 改成 a
# _old, new
print(name.replace("d", "a"))

# 14.判断是否符合数字类型
print("1".isdigit())
print("1".isdecimal())
```

## 4.3 了解的内置方法

```py
# 1.查找
# 在字符串中查找某个字符所在的索引位置
# (1)find
# 从左向右 返回第一个位置
print("draeam".find("a"))

# 从后往前
print("draeam".rfind("a"))
# 不存在的字符 返回-1

# (2)index
print("draeam".index("a"))

# 从后往前
print("draeam".rindex("a"))
# 不存在的字符 直接报错(区别)

# 2.统计字符在字符串中出现的次数
print("draeam".count("a"))

# 3.填充
# (1)两侧
# --dream--
# 长度, 字符
print("dream".center(len("dream") + 2, "*"))

# 奇数, 优先右侧
print("dream".center(len("dream") + 3, "*"))

name = "dream"
# (2)左对齐
print(name.rjust(len(name) + 3, "-"))

# (3)右对齐
print(name.ljust(len(name) + 3, "-"))

# 把name想成目标，center放中间，r放右边，l放左边，所以才是反的

# (4)填充 0
# 默认使用 0 填充至指定的长度 从左向右
print(name.zfill(len(name) + 3))

# 4.首字母大写
sentence = "my name is dream."
print(sentence.capitalize())

sentence = sentence.capitalize()
# 5.大小写翻转
# upper lower
print(sentence.swapcase())

# 6.每个单词首字母大写
# 要空格才生效
print(sentence.title())
```



## 【登陆注册】

````python
● 设定好用户名和密码，用户通过输入指定的用户名和密码进行登陆
● 最大尝试次数：用户最多尝试猜测3次
● 最大尝试次数后：如3次后，问用户是否继续登陆 
  ○ 如果回答Y或y，就再给3次机会，提示【还剩最后三次机会】
  ○ 3次都猜错的话登录结束
  ○ 如果回答N或n，登陆结束！
  ○ 如果格式输入错误，提示【输入格式错误，请重新输入：】
● 如果猜对了，登陆结束！
````



TASK：

1. 代码、笔记都过一遍√

2. 作业√ 写了 有点问题 记录一下

3. 研究下在手机/平板上看md笔记适合的平台以及操作方式

   选定：Github 理由：既能多端看笔记 也能传项目 也能用于找工作贴链接 也是学习进程的记录和证明

   比其他平台操作难度高 因此 周末时间 搞定三件事

   - bilibili上看一个完整的github平台的使用介绍
   - 电脑已有git 搞定将 typora笔记同步到github上的操作
   - 弄一个 github的主页 模板 用于介绍自己

1. 随着知识的增多，要调整一下学习策略
