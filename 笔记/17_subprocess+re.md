# 1 subprocess模块

```python
# subprocess模块
# 能够帮助我们启动一个新的进程
# 然后输入和输出错误信息

# 简单理解：能够帮助我们连接别人的电脑
```

## 1.1 导入模块

```python
import subprocess
import os
```

## 1.2 执行系统命令 Popen

```python
# args 执行的是操作系统命令 
# 如果是win系统就是windows命令 
# dir可以显示目录和文件的列表 在Linux或mac是ls

# shell=False 命令作为列表传递给模块 这样更安全 避免了shell注入攻击的风险 适用于简单的 不需要shell特性的情况
# shell=True 命令通过shell来执行 作为一个字符串传递 shell会解析和执行 可以利用shell特性 如管道 重定向 环境变量的情况

# stdout=None 存储正确结果的管道->捕获标准输出
# stderr=None 存储错误结果的管道->捕获标准错误
response = subprocess.Popen(
    "dir",  # ls 是 linux/mac; dir 是 win
    shell=True,
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE
)

# 进口 ---------- 出口
# 将结果从管道中取出后 管道中就没有结果了
# 打印Popen对象
print(response)  # <Popen: returncode: None args: 'ls'>

# 我们需要将 执行的结果从管道中取出来
# 获取并打印标准输出
print(response.stdout)  # <_io.BufferedReader name=3>
# 直接read就是没有解码的内容
# print(response.stdout.read())
# b' \xc7\xfd\xb6\xaf\xc6\xf7 D ……
# 所以需要解码并打印标准内容
print(response.stdout.read().decode("gbk"))
#  win gbk;mac utf8
#  驱动器 D 中的卷是 新加卷
#  卷的序列号是 468E-7530
#  ……

# 同理 这边是输出标准错误 所以解码打印没有内容
print(response.stderr)  # <_io.BufferedReader name=4>
# print(response.stderr.read())
print(response.stderr.read().decode("gbk"))
```

## 1.3 执行系统命令 run

```python
# *popenargs : *解包 popenargs 可以解包的类型 ---> 传列表["cd","目录"]
# 就是args 包含要执行的命令及其参数 shell=True的时候可以是字符串 或者 由一个程序名称和参数组成的列表

# timeout : 超时时间，执行某个命令 在指定时间内没有执行成功将引发 TimeoutExpired 异常
# check : 退出状态码 当设置为True的时候 进程退出状态码不是0 就弹出CalledProcessError异常 默认是False
# shell=False 同上 True的时候命令在shell中执行 适合需要一些shell特定命令的时候 而False时不通过shell 更加安全 默认为False
# stdout=None 存储正确结果的管道 捕获标准输出
# stderr=None 存储错误结果的管道 捕获标准错误输出
# encoding


def run_command(command_list):
    response = subprocess.run(
        command_list,
        shell=True,
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE,
        # 指定编码格式 在管道中提取出来的结果就是已经被解码后的结果 否则是二进制数据
        encoding="gbk"
        # 如果这里为 True 就不需要执行上面的 stdout 和 stderr
        # 但是如果命令是错误的 会直接报错
        # capture_output=True,
    )
    if response.returncode == 0:
        return response.stdout
    else:
        return response.stderr


base_dir = os.path.dirname(__file__)
result = run_command(["dir", base_dir])
print(result)


# run是一个高级接口 封装了popen的一些常见用法 简化了子进程调用和管理 更方便
# popen底层接口 进程启动后还能继续交互 更灵活 操作起来更复杂
# popen更灵活，更有交互性，返回对象popen可交互，run返回的对象包含了很多信息，run会自动处理管道
# 需要实时交互 逐行读取选popen；执行简单命令任务选run
```

## 1.4 执行系统命令call方法

```python
# 使用 subprocess.call 执行命令

# subprocess.call 返回命令的退出状态码，而不捕获命令的输出。
subprocess.call(['python', '--version'])
# Python 3.10.11
```



# 2 re模块

## 2.1 引入

```python
# re：写正则表达式
# 根据指定的规则在指定的文本中进行匹配和提取相应的内容

# 以前经常用在 爬虫 中
# 但是后来随着 css 语法 和 xpath 语法的兴起 正则表达式不再被常用

import re
# 正则匹配的目标内容必须是字符串
# 使用我们自己的匹配方式 校验用户输入的手机号


def check_phone(phone):
    # 1.全是数字
    if not phone.isdigit():
        return f"格式不正确 必须是数字"
    # 2.11位
    if len(phone) != 11:
        return f"格式不正确 必须11位"
    # 3.符合国内运营商
    if phone[0:2] not in ["13", "15", "17", "18"]:
        return f"格式不正确 必须13，15，17，18开头"
    return f"格式正确: >>> {phone}"


def check_phone_re(phone):
    pattern = "^1[3456789]\d{9}$"
    if not re.match(pattern, phone):
        return f"格式不正确"
    return f"格式正确: >>> {phone}"


phone = input("输入电话号: >>> ").strip()
print(check_phone(phone))
print(check_phone_re(phone))
```

## 2.2 语法

```python
import re
# 【一】字符组
# ● 字符组 ：[字符组] 在同一个位置可能出现的各种字符组成了一个字符组
# ● 在正则表达式中用[]表示
# ● 字符分为很多类
#   ○ 比如数字、字母、标点等等。
# ● 假如你现在要求一个位置"只能出现一个数字"
# ● 那么这个位置上的字符只能是0、1、2...9这10个数之一。

# 正则    待匹配字符  匹配结果   说明
# [0123456789]  8  True   在一个字符组里枚举合法的所有字符，字符组里的任意一个字符和"待匹配字符"相同都视为可以匹配
# [0123456789]  a  False  由于字符组中没有"a"字符，所以不能匹配
# [0-9] 7  True   也可以用-表示范围,[0-9]就和[0123456789]是一个意思
# [a-z] s  True   同样的如果要匹配所有的小写字母，直接用[a-z]就可以表示
# [A-Z] B  True   [A-Z]就表示所有的大写字母
# [0-9a-fA-F]   e  True   可以匹配数字，大小写形式的a～f，用来验证十六进制字符

# findall 去指定文本中查找符合正则表达式的内容

data = "023"
pattern = "[1]"
# 去 data 里面找 符合 [1] 的内容 [1] 去文本中匹配 有数字 1 的内容
# 如果有就返回匹配到的内容 如果没有 则返回 空
print(re.findall(pattern, data))  # []

data = "0123"
pattern = "[1]"
print(re.findall(pattern, data))  # ['1']


# 匹配 0 - 9 的数字
data = "0123"
pattern = "[0123456789]"
print(re.findall(pattern, data))  # ['0', '1', '2', '3']

# 匹配 0 - 9 的数字 简写
data = "0123"
pattern = "[0-9]"
print(re.findall(pattern, data))  # ['0', '1', '2', '3']


# # 匹配 a - z 的 字符 简写
data = "0123a"
pattern = "[a-z]"
print(re.findall(pattern, data))  # ['a']

# # 匹配 A - Z 的 字符 简写
data = "0123aA"
pattern = "[A-Z]"
print(re.findall(pattern, data))  # ['A']


# 匹配 A - Z + 0 - 9 + a - z 的 字符 简写
data = "0123aA"
pattern = "[0-9a-zA-Z]"
print(re.findall(pattern, data))  # ['0', '1', '2', '3', 'a', 'A']


# 【二】元字符
# 元字符   匹配内容
# . 匹配除换行符以外的任意字符
# \w    匹配字母或数字或下划线
# \s    匹配任意的空白符
# \d    匹配数字
# \n    匹配一个换行符
# \t    匹配一个制表符
# \b    匹配一个单词的结尾
# ^ 匹配字符串的开始
# $ 匹配字符串的结尾
# \W    匹配非字母或数字或下划线
# \D    匹配非数字
# \S    匹配非空白符
# a|b   匹配字符a或字符b
# ()    匹配括号内的表达式，也表示一个组
# [...] 匹配字符组中的字符
# [^...]    匹配除了字符组中字符的所有字符

# . 匹配除换行符以外的任意字符
data = "0123a \n A"
pattern = "."
print(re.findall(pattern, data))  # ['0', '1', '2', '3', 'a', ' ', ' ', 'A']

# \w 匹配字母或数字或下划线
data = "0123a \n _A$%#"
pattern = "\w"
print(re.findall(pattern, data))  # ['0', '1', '2', '3', 'a', '_', 'A']

# \s 匹配任意的空白符
data = "0123a \n _\tA$%#"
pattern = "\s"
print(re.findall(pattern, data))  # [' ', '\n', ' ', '\t']

# \d 匹配数字
data = "0123a \n _A$%#"
pattern = "\d"
print(re.findall(pattern, data))  # ['0', '1', '2', '3']

# \n 匹配一个换行符
data = "0123a \n \n_A$%#"
pattern = "\n"
print(re.findall(pattern, data))  # ['\n', '\n']

# \t 匹配一个制表符(就是一个 tab 键 = 四个空格)
data = "0123a \n \n_ \t A$%#"
pattern = "\t"
print(re.findall(pattern, data))  # ['\t']

# \b 匹配一个单词的结尾 找的是结尾
# 匹配单词边界的位置，但它本身不匹配任何实际字符
data = "0123a \n \n_ \t A$%#"
pattern = "\b"
print(re.findall(pattern, data))  # []

# ^ 匹配字符串的开始
data = "0123a \n \n_ \t A$%#"
pattern = "^[0-9]"
print(re.findall(pattern, data))  # ['0']

data = "0123a \n \n_ \t A$%#"
pattern = "^[a-z]"
print(re.findall(pattern, data))  # []

# $ 匹配字符串的结尾
data = "0123a \n \n_ \t A$%#"
pattern = "[0-9]$"
print(re.findall(pattern, data))  # []

data = "0123a \n \n_ \t A$%#"
pattern = "#$"
print(re.findall(pattern, data))  # ['#']

# \W 匹配非字母或数字或下划线
data = "0123a \n \n_ \t A$%#"
pattern = "\W"
print(re.findall(pattern, data))  # [' ', '\n', ' ', '\n', ' ', '\t', ' ', '$', '%', '#']

# \D 匹配非数字
data = "0123a \n \n_ \t A$%#"
pattern = "\D"
print(re.findall(pattern, data))  # ['a', ' ', '\n', ' ', '\n', '_', ' ', '\t', ' ', 'A', '$', '%', '#']

# \S 匹配非空白符
data = "0123a \n \n_ \t A$%#"
pattern = "\S"
print(re.findall(pattern, data))  # ['0', '1', '2', '3', 'a', '_', 'A', '$', '%', '#']

# a|b 匹配字符a或字符b
data = "0123a \n \n_ \t A$%#"
pattern = "[0-9]|[a-z]"
print(re.findall(pattern, data))  # ['0', '1', '2', '3', 'a']

# () 匹配括号内的表达式，也表示一个组
#  01([0-9])3 会匹配 0123，并将 2 作为捕获组的内容返回
data = "0123a \n \n_ \t A$%#"
pattern = "01([0-9])3"  #
print(re.findall(pattern, data))  # ['2']

# [...] 匹配字符组中的字符
data = "0123a \n \n_ \t A$%#"
pattern = "01([0-9])3"  #
print(re.findall(pattern, data))  # ['2']

# [^...] 匹配除了字符组中字符的所有字符
data = "0123a \n \n_ \t A$%#"
pattern = "[^0-9]"  #
print(re.findall(pattern, data))  # ['a', ' ', '\n', ' ', '\n', '_', ' ', '\t', ' ', 'A', '$', '%', '#']


# 【三】量词
# 重复 多少次

# 量词默认是贪婪匹配：尽可能多的拿！
# 它们会尝试找到最长的匹配 找不到再找第二长

# 量词	用法说明
# 量词代表 前面的模式 的次数！
# *	重复零次或更多次
# +	重复一次或更多次
# ?	重复零次或一次
# {n}	重复n次
# {n,}	重复n次或更多次
# {n,m}	重复n到m次


# *	重复零次或更多次
data = "01223a \n \n_ \t A$%#"
pattern = "01([0-9][0-9])3"  # 一个字符组只能匹配一个字符 如果想匹配两个就需要 两个字符组
print(re.findall(pattern, data))  # ['22']

data = "012222223a \n \n_ \t A$%#"
pattern = "01([0-9]*)3"  # 有几个拿几个
print(re.findall(pattern, data))  # ['222222']

# +	重复一次或更多次
data = "012222223a \n \n_ \t A$%#"
pattern = "01([0-9]+)3"  # 有几个拿几个
print(re.findall(pattern, data))  # ['222222']

# ?	重复零次或一次
data = "012222223a \n \n_ \t A$%#"
pattern = "01([0-9]?)3"  # 可以拿零或者1个
print(re.findall(pattern, data))  # []

data = "0123a \n \n_ \t A$%#"
pattern = "01([0-9]?)3"  # 可以拿零或者1个
print(re.findall(pattern, data))  # ['2']

# {n}	重复n次
data = "012223a \n \n_ \t A$%#"
pattern = "01([0-9]{3})3"  # 写几个拿几个
print(re.findall(pattern, data))  # ['222']

# {n,}	重复n次或更多次
data = "01222222223a \n \n_ \t A$%#"
pattern = "01([0-9]{3,})3"  # 必须是3起步 包括3
print(re.findall(pattern, data))  # ['22222222']

# {n,m}	重复n到m次
data = "012222223a \n \n_ \t A$%#"
pattern = "01([0-9]{3,6})3"  # 必须是3起步 包括3 最多能取 6 个
print(re.findall(pattern, data))  # ['222222']

# 关于{}模式的疑惑解答：
# 捕获组 () 中的内容会捕获括号内的匹配内容。
# 贪婪匹配会尽可能多地匹配字符，这会影响捕获组的内容。
# 当使用像 [0-9]{3} 这样的量词时，它匹配的是三个数字，并且只匹配三个字符。
# 量词 {n} 指定了前面模式的重复次数!!!

# 【四】位置元字符
# . 任意字符
# ^ : 以某个字符开头
# $ : 以某个字符结尾

# . 代表的含义是匹配除换行符之外的任意单个字符。
# 海. ---> 在文本中匹配 两个字符 并且 海 + 任意一个字符
data = "海盐海燕海娇海冬梅"
pattern = "海."
print(re.findall(pattern, data))
# ['海盐', '海燕', '海娇', '海冬']


# ^ : 以某个字符开头
data = "海盐海燕海娇海冬梅"
pattern = "^海."
print(re.findall(pattern, data))
# ['海盐']


# $ : 以某个字符结尾
# 海. ---> 在文本中匹配 两个字符 并且 海 + 任意一个字符 且是结尾
#
data = "海盐海燕海娇海冬梅"
pattern = "海..$"
print(re.findall(pattern, data))
# ['海冬梅']

# {5} : 重复出现 5 次
# \w : 匹配字母或者数字或者下划线
# ^ : 以某个字符开头
data = "apple,banana,peach,orange,melon"
pattern = "^\w{5}"
# ^ 开头找 5个字符的字符串 字符为字母/数字/下划线
res_one = re.findall(pattern, data)
print(res_one)  # ['apple']

# {5} : 重复出现 5 次
# \w : 匹配字母或者数字或者下划线
# $ : 以某个字符结尾
data = "apple,banana,peach,orange,melon"
pattern = "\w{5}$"
res_two = re.findall(pattern, data)
print(res_two)  # ['melon']

# {5} : 重复出现 5 次
# \w : 匹配字母或者数字或者下划线
# ^ : 以某个字符开头
# $ : 以某个字符结尾
data = "apple,banana,peach,orange,melon"
pattern = "^\w{5}$"
# 要既是开头又是末尾的5个符合要求字符的字符串 所以不行
res_three = re.findall(pattern, data)
print(res_three)  # []

# 限制当前匹配的字符长度必须是 5 位 并且 以 字母或者数字或者下划线 开头 + 以 字母或者数字或者下划线 结尾
data = "apple"
pattern = "^\w{5}$"
res_three = re.findall(pattern, data)
print(res_three)  # ["apple"]

# 不是以 字母或者数字或者下划线 结尾
data = "appl￥"
pattern = "^\w{5}$"
res_three = re.findall(pattern, data)
print(res_three)  # []

# 不是以 字母或者数字或者下划线 开头
data = "#pple"
pattern = "^\w{5}$"
res_three = re.findall(pattern, data)
print(res_three)  # []

# 【五】重复匹配
# . ： 任意一个字符
# ? : 匹配 0 次 或 1 次
data = "李杰和李莲英和李二棍子和李"
pattern = "李.?"
# ? 控制的是 . 这个模式 匹配0或1次
# 所以变成了 李 接 0 或 1 个字符 且尽量是1个
print(re.findall(pattern, data))
# ['李杰', '李莲', '李二', '李']

# . ： 任意一个字符
# * : 匹配 0 次 或 无数 次
data = "李杰和李莲英和李二棍子和李"
pattern = "李.*"
# . 可以 0次或无数次 而且找最大
print(re.findall(pattern, data))
# ['李杰和李莲英和李二棍子和李']

# . ： 任意一个字符
# + : 匹配 1 次 或 无数 次
data = "李杰和李莲英和李二棍子和李"
pattern = "李.+"
# . 可以 1次或无数次 而且找最大
print(re.findall(pattern, data))
# ['李杰和李莲英和李二棍子和李']

# . ： 任意一个字符
# {1} : 匹配 指定 次数
data = "李杰和李莲英和李二棍子和李"
pattern = "李.{1}"
# 李+一个字符
print(re.findall(pattern, data))
# ['李杰', '李莲', '李二']

data = "李杰和李莲英和李二棍子和李"
pattern = "李.{1,}"
# 李+ 一到多个 任意字符
print(re.findall(pattern, data))
# ['李杰和李莲英和李二棍子和李']

data = "李杰和李莲英和李二棍子和李"
pattern = "李.{1,3}"
# 李+ 1~3个 任意字符 3个优先
print(re.findall(pattern, data))
# ['李杰和李', '李二棍子']


# 【六】字符集
# [] : 匹配字符组中的任意一个字符
# [^] : 匹配除了字符组以外的任意一个字符
# ^[] : 以字符组中的任意一个字符开头


data = "李杰和李莲英和李二棍子和李"
pattern = "李[杰莲英二棍子]"
print(re.findall(pattern, data))
# ['李杰', '李莲', '李二']


data = "李杰和李莲英和李二棍子和李"
pattern = "李[杰莲英二棍子]*"
# 李 + []中的一个字符 * 0~多次
print(re.findall(pattern, data))
# ['李杰', '李莲英', '李二棍子', '李']

data = "李杰和李莲英和李二棍子和李"
pattern = "李[^和]*"
# 李 + 除了和的其他任意字符 * 0~多次
print(re.findall(pattern, data))
# ['李杰', '李莲英', '李二棍子', '李']


# 【七】分组匹配
data = "sad4a56ds4a65d4s6a54d6a5sd1a56s1da6s1dsa536"
# \d : 匹配任意数字
# + : 1 次或更多次
pattern = "\d+"
print(re.findall(pattern, data))
# 把所有连续的数字都提取出来了
# ['4', '56', '4', '65', '4', '6', '54', '6', '5', '1', '56', '1', '6', '1', '536']

# 分组匹配 : () 提升优先级
# | 两个条件任一
# [^] 排除指定以外的规则


# 【八】转义符
# \ 在Python \ 具有特殊的含义 能够 和某些字符构成指定的含义 \n 换行

# 在正则表达式中 \ 同样具有特殊的含义

# \n 换行符
data = "\n"
pattern = "\n"
print(re.findall(pattern, data))
# ['\n']

# 取消转义的两种方式
# 1.方式一
# \ + \ 取消 后面的 \ 的特殊含义
data = "\n"
pattern = "\\n"
print(re.findall(pattern, data))
# ['\n']

# 2.方式二：
# 在整个字符匹配的开头 + r 取消字符串的转义
# f "" 格式化输出 r "" 取消 \ 的转义
data = "\n"
pattern = r"\n"
print(re.findall(pattern, data))
# ['\n']

# Pattern1: \\n
# \\ 在 Python 中被解析为单个 \，所以 \\n 变成了 \n，即换行符。
# 匹配到的结果是包含换行符的列表：['\n']。

# Pattern2: r"\n"
# r"\n" 直接表示换行符。
# 匹配到的结果同样是包含换行符的列表：['\n']。
# 正因为在正则表达式中，\n 表示换行符，而在 Python 字符串中，双反斜杠 \\ 也被解析为换行符，
# 所以两者在匹配换行符时行为相同


# 【九】贪婪匹配
# 主要是针对量词来说的
# * : 匹配 0 次 或更多次 默认是更多次 而不是 了 0 次
# ? : 匹配 0 次 或 1 次
# + : 匹配 1 次 或更多次
# . : 匹配任意字符

# ?
# 这是一个修饰符，用来改变前一个量词（在这里是 *）的匹配方式。
# 当它跟在量词（如 *, +, {}）后面时，表示非贪婪匹配，即尽可能少地匹配字符。
# 在满足条件时 量词的匹配方式是按照最小的来匹配
# 量词 + ?

data = "李杰和李莲英和李二棍子和李"
pattern = "李.*"
# 李 + 任意字符*0~最多次
print(re.findall(pattern, data))
# ['李杰和李莲英和李二棍子和李']


# 是非贪婪匹配的典型例子
data = "李杰和李莲英和李二棍子和李"
pattern = "李.*?"
# 李 + 任意字符*0~最多次 + ?
# 非贪婪匹配 转一转 尽可能少
print(re.findall(pattern, data))
# ['李', '李', '李', '李']


# 【十】模式修正符/正则修饰符
# 在某些情况下对正则表达式的某部分含义不一样

# 值	说明
# re.I	是匹配对大小写不敏感
# re.L	做本地化识别匹配
# re.M	多行匹配，影响到^和$
# re.S	使.匹配包括换行符在内的所有字符
# re.U	根据Unicode字符集解析字符，影响\w、\W、\b、\B
# re.X	通过给予我们功能灵活的格式以便更好的理解正则表达式

data = "abcdABCD"
pattern = "[a-z]"
print(re.findall(pattern, data))
# ['a', 'b', 'c', 'd']

data = "abcdABCD"
pattern = "[a-z]"
print(re.findall(pattern, data, re.I))
# ['a', 'b', 'c', 'd', 'A', 'B', 'C', 'D']

```

## 2.3 常用方法

```python
import re
import time

# 1.查找结果 findall
# 返回满足条件的所有结果并且结果是放在列表中的
data = "李杰和李莲英和李二棍子和李"
pattern = "李.+?"
print(re.findall(pattern, data))
# ['李杰', '李莲', '李二']

# 2.查找结果search
# 按照指定的表达式在文本中查找如果匹配到信息就返回一个 匹配信息的对象
# 只能查找到符合条件的第一个结果
# 如果查询不到指定的结果返回的就是None
data = "李杰和李莲英和李二棍子和李"
pattern = "李.+?"
result = re.search(pattern, data)
print(result)
# <re.Match object; span=(0, 2), match='李杰'>

# 获取 search 查询到的对象中的数据
print(result.group())  # 李杰

data = "李杰和李莲英和李二棍子和李"
pattern = "赵.+?"
result = re.search(pattern, data)
print(result)
# None

# 3.查找结果 match
data = "李杰和李莲英和李二棍子和李"
pattern = "李.+?"
result = re.match(pattern, data)
print(result)  # <re.Match object; span=(0, 2), match='李杰'>
print(result.group())  # 李杰

data = "李杰和李莲英和李二棍子和李"
pattern = "z.+?"
result = re.match(pattern, data)
print(result)  # None

# 4.切割 split
# 按照正则表达式对字符串进行切割
# 返回值是切割后的；列表 切割符会随之消失
data = "李杰和李莲英和李二棍子和李"
pattern = "和"
result = re.split(pattern, data)
print(result)  # ['李杰', '李莲英', '李二棍子', '李']

# 5.替换 sub(正则表达式,替换的字符,原始数据,替换次数)
# sub 的返回值是替换后的结果
data = "李杰和李莲英和李二棍子和李"
pattern = "和"
result = re.sub(pattern, '|', data)
print(result)  # 李杰|李莲英|李二棍子|李

data = "李杰和李莲英和李二棍子和李"
pattern = "和"
result = re.sub(pattern, '|', data, 1)
print(result)  # 李杰|李莲英和李二棍子和李

# 6.替换 subn
# 返回值是一个元祖 (替换后的结果,替换次数)
data = "李杰和李莲英和李二棍子和李"
pattern = "和"
result = re.subn(pattern, '|', data, )
print(result)  # ('李杰|李莲英|李二棍子|李', 3)

data = "李杰和李莲英和李二棍子和李"
pattern = "和"
result = re.subn(pattern, '|', data, 1)
print(result)  # ('李杰|李莲英和李二棍子和李', 1)

# 7.编译正则表达式 compile 先编译正则表达式然后使用
data = "李杰和李莲英和李二棍子和李"
pattern = "和"
pattern_compile = re.compile(pattern=pattern, flags=re.S)
print(pattern_compile)  # re.compile('和', re.DOTALL)
result = re.subn(pattern_compile, '|', data)
print(result)  # ('李杰|李莲英|李二棍子|李', 3)


def timer(func):
    def inner():
        start_time = time.time()
        res = func()
        end_time = time.time()
        print(f"当前总耗时 :>>>> {end_time - start_time} s")
        return res

    return inner


# 每一次在查询数据的时候其实都是先编译再运行
@timer
def re_check_normal():
    data = "李杰和李莲英和李二棍子和李"
    pattern = "和"
    for i in range(1000000):
        result = re.subn(pattern, '|', data)
        print(result)


# 每一次在查询数据的时候其实都是先编译再运行
@timer
def re_check_compile():
    data = "李杰和李莲英和李二棍子和李"
    pattern_compile = re.compile(pattern=pattern, flags=re.S)
    for i in range(1000000):
        result = re.subn(pattern_compile, '|', data)
        print(result)


# re_check_normal()
# 当前总耗时 :>>>> 11.16263747215271 s

# re_check_compile()
# 当前总耗时 :>>>> 10.401513576507568 s

# 结论：每次使用正则表达式匹配的时候先编译再执行
# 实际上，Python 的 re 模块在处理简单的正则表达式时已经做了大量优化，
# 因此在简单模式下每次使用正则表达式编译和不编译之间的差异并不明显。
# 如果用复杂的正则表达式 编译会带来显著性的提升

print(f"--------- ")

# 8.查询结果 finditer
# 获得到的结果是一个可迭代对象，可以使用 for 循环来遍历
data = "李杰和李莲英和李二棍子和李"
pattern = "李.+?"
pattern_compile = re.compile(pattern=pattern, flags=re.S)
result = re.finditer(pattern_compile, data)

print(result)  # <callable_iterator object at 0x10af03eb0>
print(next(result))  # <re.Match object; span=(2, 3), match='和'>
print(next(result).group())  # 李杰
num_list = [1, 2, 3]
num_list_iter = iter(num_list)
print(num_list_iter)  # <list_iterator object at 0x1493f3d60>
print(next(num_list_iter))
print(next(num_list_iter))
print(next(num_list_iter))
num_list_iter = iter(num_list)
print(next(num_list_iter))
print([i for i in result])  # [<re.Match object; span=(7, 9), match='李二'>]
print([i.group() for i in result])  # ['李杰', '李莲', '李二']
```

## 2.4 查询优先级

```python
import re

# 原始数据
data = "www.baidu.com|www.bilibili.com"
# 编译正则表达式
pattern = re.compile("www.baidu.com")
print(re.findall(pattern, data))  # ['www.baidu.com']

# 编译正则表达式
pattern = re.compile("www.(?:baidu|bilibili).com")
# 解释: www.(?:baidu|bilibili).com
# 使用了非捕获组 (?:...)，
# 它匹配 www. 后跟 baidu 或 bilibili，然后是 .com。
# re.findall 返回所有匹配的结果，
# 包含 www.baidu.com 和 www.bilibili.com，
# 因此结果是 ['www.baidu.com', 'www.bilibili.com']。
print(re.findall(pattern, data))  # ['www.baidu.com', 'www.bilibili.com']

# 【切分优先级】
# 原始数据
data = "dream1dream2dream3dream4"
# 编译正则表达式
pattern = re.compile("\d+")
# 用所有数字去切割 空字符串是因为data用4来结尾的
print(re.split(pattern, data))
# ['dream', 'dream', 'dream', 'dream', '']

# 原始数据
data = "dream1dream2dream3dream4"
# 编译正则表达式
pattern = re.compile("(\d+)")
# 解释: (\d+) 匹配一个或多个数字，并且捕获这些数字。
# re.split 使用这个正则表达式进行切割时，捕获的组也会出现在结果中。
# 结果是 ['dream', '1', 'dream', '2', 'dream', '3', 'dream', '4', '']，
# 因为数字被分隔并被保留在结果中。
print(re.split(pattern, data))
# ['dream', '1', 'dream', '2', 'dream', '3', 'dream', '4', '']
```

## 2.5 常见的正则表达式

```python
import re


def check_phone_re(phone):
    # ^ 以 1
    # [3456789] : 匹配字符组中的任意一个
    # \d ： 任意数字
    # {9} ： 重复 9 次
    # $ ： 以 数字 结尾
    pattern = "^1[3456789]\d{9}$"
    if not re.match(pattern, phone):
        return f"当前手机号格式不正确!"
    return f"当前手机号格式正确 :>>>> {phone}"

# \w:匹配任意的字母+数字+下划线
# \w+ : 1 次 或更多次
# 15222222@qq.com / @163.com / @ gmail.com
# Email地址：^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$

# 域名：[a-zA-Z0-9][-a-zA-Z0-9]{0,62}(/.[a-zA-Z0-9][-a-zA-Z0-9]{0,62})+/.?

# InternetURL：[a-zA-z]+://[^\s]* 或 ^http://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$

# 手机号码：^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$
#
# 电话号码("XXX-XXXXXXX"、"XXXX-XXXXXXXX"、"XXX-XXXXXXX"、"XXX-XXXXXXXX"、"XXXXXXX"和"XXXXXXXX)：^(\(\d{3,4}-)|\d{3.4}-)?\d{7,8}$

# 国内电话号码(0511-4405222、021-87888822)：\d{3}-\d{8}|\d{4}-\d{7}

# 身份证号(15位、18位数字)：^\d{15}|\d{18}$

# 短身份证号码(数字、字母x结尾)：^([0-9]){7,18}(x|X)?$ 或 ^\d{8,18}|[0-9x]{8,18}|[0-9X]{8,18}?$

# 帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)：^[a-zA-Z][a-zA-Z0-9_]{4,15}$

# 密码(以字母开头，长度在6~18之间，只能包含字母、数字和下划线)：^[a-zA-Z]\w{5,17}$

# 强密码(必须包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间)：^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$

# 日期格式：^\d{4}-\d{1,2}-\d{1,2}

# 一年的12个月(01～09和1～12)：^(0?[1-9]|1[0-2])$

# 一个月的31天(01～09和1～31)：^((0?[1-9])|((1|2)[0-9])|30|31)$
```





TASK：

1. 过subprocess模块√+re模块√
1. 过之前的很多代码√
1. 研究分层目录结构
1. 老毛病 研究单层和双层的装饰器场景 目前来看单层很适合用在系统各个功能的检验登录功能+计时器功能 双层再研究 再研究多层
1. 找题目做随机出看哪里薄弱