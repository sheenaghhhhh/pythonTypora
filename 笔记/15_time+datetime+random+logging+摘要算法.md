# 0 内容回顾

## 0.1 内置模块

```python
# 内置模块是Python解释器自带的模块
# 只需要使用 语法 导入使用即可
```

## 0.2 序列化和反序列化模块

```python
# 0.序列化和反序列化
# 序列化：将Python对象转换为JSON字符串
# 反序列化：将JSON字符串转换为Python对象

# 1.Json模块
# a.json模块操作Python对象
# 将Python对象 转换为 json格式的字符串
user_data = {"username": "sheenagh"}
user_data_str = json.dumps(user_data)
# 将 json格式的字符串/二进制数据 转换为Python对象
user_data_dict = json.loads(user_data_str)

# b.json模块操作文件数据
# 将Python对象转换为 json 格式的字符串写入到 json文件中
with open('user_data.json', 'w', encoding="utf-8") as fp:
    json.dump(user_data, fp)
    # ensure_ascii: 是否启用ASCII编码
    # indent: 缩进格式如果是 0 将json数据的每一行贴左侧
    # separators: 调整每个元素之间的分隔符及键值对的分隔符 默认;

# 将文件中的符合json格式的字符串数据转换为Python对象
with open("user_data.json", "r", encoding="utf-8") as fp:
    data = json.load(fp)
print(data)
```

## 0.3 pickle模块

```python
# pickle只能操作python对象
# json不能把函数 类转换 但是pickle可以
# python对象转为 二进制数据
# 二进制数据转回 python对象

# 1.pickle 模块操作Python对象
# 将Python对象转换为 json 格式的字符串
user_data = {"username": "sheenagh"}
user_data_bytes = pickle.dumps(user_data)
# b'\x80\x04\x95\x1a\x00\x00\x00\x00\x00\x00\x00}\x94\x8c\x08username\x94\x8c\x08sheenagh\x94s.'
print(user_data_bytes)

# 将 json 格式的字符串 / json 格式的二进制数据 转换为Python对象
user_data_dict = pickle.loads(user_data_bytes)
print(user_data_dict)

# 2.json 模块操作文件数据
user_data = {"username": "sheenagh"}
# 将Python对象转换为 json 格式的字符串写入到 json文件中
with open("user_data", "wb") as fp:
    pickle.dump(user_data, fp)
# 将文件中的符合json格式的字符串数据转换为Python对象
with open("user_data", "rb") as fp:
    data = pickle.load(fp)
print(data)
```

## 0.4 os模块

```python
# 1.os.path :查看文件或文件信息
# os.path.abspath(__file__)  # 获取到当前文件所在的绝对路径
# os.path.dirname()  # 获取到当前路径的上一级文件夹路径
# os.path.exists()  # 判断当前文件路径是否存在
# os.path.join()  # 拼接文件路径
# os.path.getsize()  # 获取到当前文件的文件大小
# os.path.isdir()  # 判断当前路径是否是文件夹
# os.path.isabs()  # 判断当前路径是否是绝对路径
# os.path.isfile()  # 判断当前路径是否是文件
# os.path.getatime()  # 获取当前文件的最后访问时间 access
# os.path.getctime()  # 获取当前文件的创建时间 create
# os.path.getmtime()  # 获取当前文件的最后修改时间 modify
# os.path.split()  # 获取到当前文件的文件名和文件夹路径 split 切分后的结果是元组 (前面的绝对路径,文件路径的最后文件名/文件夹名)
# os.path.basename()  # 获取到当前文件路径的最后一个文件名
# os.path.sep  # 获取当前系统的路径分隔符

# 2.os. :操作文件或文件夹
# os.mkdir()  # 创建单级文件夹
# os.makedirs(exist_ok=True)  # 创建多级文件夹 exist_ok=True 当前路径不存在则自动创建 存在则忽略
# os.rmdir()  # 删除单级文件夹 当前文件夹必须是空文件夹才能被删除
# os.removedirs()  # 删除多级文件夹 当前文件夹必须是空文件夹才能被删除
# os.rename()  # 对原本的文件或者文件夹名字重名
# os.chdir()  # 切换工作目录
# os.getcwd()  # 获取当前的工作目录
# os.sep  # 获取当前系统的路径分隔符
# os.environ  # 获取当前系统的环境变量
# os.stat()  # 获取当前文件的元信息
# os.listdir()  # 获取当前文件夹下的所有文件和文件夹名字
# os.name  # 获取当前操作系统对应的标志名
# os.system()  # 执行系统命令，直接在控制台展示输出结果
# os.popen()  # 执行系统命令 但是需要自己主动提取结果
```

# 1 time和datetime模块

## 1.1 time模块

```python
# 时间表示方式
# 方式一：时间戳 timestamp
# 从1970年1月1日00:00:00 UTC到某个时间点的秒数。
# 方式二：格式化时间字符串
# %Y-%m-%d %H:%M:%S
# 方式三：时间元组 struct_time
# 一个包含9个元素的元组：年、月、日、时、分、秒、一周的第几天、今年的第几天和夏令时标志。
import time
```

```python
# 1.获取到当前对于的时间戳
# 时间戳：时间戳，是从1970年1月1日（UTC/GMT的午夜）开始所经过的秒数（不考虑闰秒），用于表示一个时间点。
# 然而，这种格式对人类阅读并不友好，因此需要转换成可读的日期和时间格式。
# 这个工具能够将时间戳快速转换为人类可读的日期时间格式，同时也支持反向转换，即将日期时间转换为时间戳。
time_stamp = time.time()
print(time_stamp)
# 1722579987.4812746
```

```python
# 2.时间戳转换为时间元组（UTC时间）-------*****-------
# 时间戳默认转换后的时间元素是以伦敦的UTC时间进行转换
# gm是 Greenwich Mean, GMT 即格林尼治标准时间 即转换成UTC Coordinated Universal Time 协调世界时
time_tuple = time.gmtime(time_stamp)
print(time_tuple)
# 这里也可以不写 time_stamp 因为这个函数的默认值就是当前的时间戳 即time.time()
# time.struct_time(tm_year=2024, tm_mon=8, tm_mday=2,
# tm_hour=6, tm_min=26, tm_sec=27, tm_wday=4, tm_yday=215, tm_isdst=0)
```

```python
# 3.时间戳转换为时间元组（中国时间）-------*****-------
# local是根据电脑系统设置的时区返回的
time_tuple = time.localtime(time_stamp)
print(time_tuple)
# 这里也可以不写 time_stamp 因为这个函数的默认值就是当前的时间戳 即time.time()
# time.struct_time(tm_year=2024, tm_mon=8, tm_mday=2,
# tm_hour=14, tm_min=26, tm_sec=27, tm_wday=4, tm_yday=215, tm_isdst=0)
```

```python
# 4.格式化输出
# 格式化包括：
# %y  年份两位数 [00-99]
# %Y  年份四位数 2024
# %w  星期中的第几天 [0-6] 0周日
# %m  月份 [01,12]
# %d  日 [01,31]
# %H  小时 [00,23]
# %M  分钟  [00,59]
# %S  秒数  [00,59]
# %f  微秒  [000000,999999]
# %z  UTC偏移 +0000
# #Z  时区名称 UTC
# %a  星期名称简写 Mon
# %A  星期名称全写 Monday
# %b  月份名称简写 Jan
# %B  月份名称全写 January
# %I  12小时制 [01,12]
# %p  本地 am / pm 上下午标志
# %j  一年中的第几天（001-366）
# %U  一年中的第几周（00-53，星期天作为一周的开始）
# %W  一年中的第几周（00-53，星期一作为一周的开始）
# %c  本地相应的日期和时间表示（如 Tue Aug 02 12:34:56 2024）
# %x  本地相应的日期表示（如 08/02/24）
# %X  本地相应的时间表示（如 12:34:56）

# 输入时间元组 输出格式化时间字符串
time_strip = time.strftime("%Y-%m-%d %H:%M:%S", time_tuple)
print(time_strip)
# 2024-08-02 14:53:25

time_strip = time.strftime("%c")
# 本地日期和时间格式
# Fri Aug  2 15:15:47 2024
print(time_strip)
```

```python
# 5.转化时间元组 结构化时间
# (1)将时间元组转换为 struct_time 类型的时间元组
# 为什么报警告 因为 是把时间元组转换为 时间元组 重复了
# 所以 我也不太明白 为何这么做
print(time.struct_time(time.localtime()))
# time.struct_time(tm_year=2024, tm_mon=8, tm_mday=2,
# tm_hour=15, tm_min=20, tm_sec=25, tm_wday=4, tm_yday=215, tm_isdst=0)

# (2)时间元组转换为时间戳
# make time
print(time.mktime(time.localtime()))
# 1722583225.0

# (3)时间字符串转换为时间元组
print(time.strptime("2024-08-02 15:32:25", "%Y-%m-%d %H:%M:%S"))
# time.struct_time(tm_year=2024, tm_mon=8, tm_mday=2,
# tm_hour=15, tm_min=32, tm_sec=25, tm_wday=4, tm_yday=215, tm_isdst=-1)
# strptime string parse time 字符串 转换 时间

# (4)时间元组转换为标准的时间格式
print(time.asctime(time.localtime()))
# Fri Aug  2 15:33:02 2024
# ascii time 表示将时间信息以ASCII字符的形式（可读的文本格式）呈现

# (5)时间戳转换为标准的时间格式
print(time.ctime(time.time()))
# Fri Aug  2 15:33:02 2024
# current time 当前时间的字符串形式

# (6)转换关系
# 字符串变格式化时间 strptime str parse time-------*****-------
time_struct_time = time.strptime("2024-08-02 15:37:44", "%Y-%m-%d %H:%M:%S")
print(time_struct_time)
# time.struct_time(tm_year=2024, tm_mon=8, tm_mday=2,
# tm_hour=15, tm_min=37, tm_sec=44, tm_wday=4, tm_yday=215, tm_isdst=-1)

# 格式化时间变字符串 strftime string format time 字符串格式的时间-------*****-------
time_strftime = time.strftime("%Y-%m-%d %H:%M:%S", time_struct_time)
print(time_strftime)
# 2024-08-02 15:37:44

# 时间戳 转 格式化时间字符串-------*****-------
formatted_time = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
print(formatted_time)
# 2024-08-02 15:57:32

# 格式化时间字符串 转 时间戳
time_string = "2024-08-02 12:34:56"
struct_time = time.strptime(time_string, "%Y-%m-%d %H:%M:%S")
timestamp = time.mktime(struct_time)
print(timestamp)
# 1722573296.0

# struct time格式化时间变时间戳 -------*****-------
time_mktime = time.mktime(time_struct_time)
print(time_mktime)
# 时间戳变格式化时间 -------*****-------
print(time.gmtime(time_mktime))
print(time.localtime(time_mktime))
```

## 1.2 datetime模块

```python
# datetime
import datetime

# 1.自定义时间日期并做格式化输出
print(datetime.date(2024, 8, 2))
# 2024-08-02

# 2.获取本地时间
# 生成一个时间日期对象
date_obj = datetime.date.today()
date_obj_2 = datetime.datetime.today()
print(date_obj)
# 2024-08-02
print(date_obj_2)
# 2024-08-02 15:59:17.702698

# 获取当前的年月日时分秒
print(date_obj.year)  # 2024
print(date_obj.month)  # 8
print(date_obj.day)  # 2
print(date_obj_2.hour)  # 15
print(date_obj_2.minute)  # 59
print(date_obj_2.second)  # 17

# 生成时间日期的增减对象
t_day = datetime.timedelta(days=7)
date_obj2 = date_obj + t_day
print(date_obj2)
# 2024-08-09
tt_day = date_obj2 - date_obj
print(tt_day)
# 7 days, 0:00:00

# 日期对象 = 日期对象 +- timedelta对象
# 用来加减日期 算ddl 也可以用来算时间差 timedelta对象
```

# 2 random模块

```python
import random
# 1. 随机生成小数 0 - 1 之间的小数
print(random.random())

# 2.生成指定区间小数
print(random.uniform(5, 6))

# 3.随机整数 两侧都能取到
print(random.randint(1, 10))

# 4.随机返回区间内的奇偶数
# randrange 从start 开始 每次加step 直到stop的前一个数 从这些数里随机去生成
# 所以才可以用这个来随机返回奇偶数
print(random.randrange(1, 10, 2))

# 5.随机返回列表元素
name_list = [f"张三_{i}" for i in range(20)]
print(random.choice(name_list))

# 6.随机从列表中返回制定个数的元素
print(random.sample(name_list, 3))

# 7.随机打乱列表中的元素 -- 影响到的是原来的列表
print(name_list)
random.shuffle(name_list)
print(name_list)

# 【random模块的应用场景】
# chr() ascii
# A65 Z90 a97 z122
# 通过随机生成范围内的数字 来生成大小写字母
num_big = [chr(i) for i in range(65, 91)]
num_small = [chr(i) for i in range(97, 123)]
code = random.choice(num_big)
print(code)
print(random.choice(num_small))

def get_code(n):
    code = ""
    for i in range(n):
        num_big = chr(random.choice([i for i in range(65, 91)]))
        num_small = chr(random.choice([i for i in range(97, 123)]))
        num_int = random.choice([str(i) for i in range(0, 10)])
        code += random.choice([num_big, num_small, num_int])
    return code

# n位数随机验证码
print(get_code(6))
```

# 3 摘要算法

## 3.1 什么是摘要算法

```python
# 又称为哈希算法，散列算法
# 通过一个函数将任意长度的数据转换为一个长度固定的字符串
# 16进制 32 长度 的字符串
# 为了验证数据传输前后的数据完整性
```

## 3.2 引入模块

```python
import hashlib

# 1.加密
def encrypted(data):
    # （1）生成一个 md5 对象
    md5 = hashlib.md5()
    # （2）有一个原始数据
    data = "你真坏！123456"
    # （3）原始数据转二进制数据
    data = data.encode()
    # （4）对原始数据进行摘要加密
    md5.update(data)
    # （5）获取加密后的 16 进制串
    print(md5.hexdigest())
    # e10adc3949ba59abbe56e057f20f883e
    # b'\xe1\n\xdc9I\xbaY\xab\xbeV\xe0W\xf2\x0f\x88>'


encrypted("你真坏！123456")

# 【补充】
# 我们在将加密后的加密串找到页面上的网址进行解密的时候
# 会发现 如果是简单的数据 123456 在网页上可以解密出来
# 但一旦数据太多/太复杂
# 会解密不成功
# 只能单向加密 不能反向解密

# 能解出原来数据的原因：
# 有人 随机生成指定字符串 对他进行摘要算法 生成加密串
# 将这些加密串放到自己的服务器上面的数据库中
# 输入加密串的时候 去数据库中直接找 找到返回 找不到就告诉你在努力

# 这种通过尝试得到加密结果的方法交撞库
```

## 3.3 为了提高数据的安全性

```python
# 盐：给当前数据加一个随机的字符串 让原始数据 和随机字符串一起加密
# 在解密的时候必须有原始数据 + 加密串 才能完成加密操作
# 给一个data 一个salt


def encrypted(data, salt):
    # （1）生成一个 md5 对象
    md5 = hashlib.md5()
    # （2）有一个原始数据
    original_data = data + salt
    # （3）原始数据转二进制数据
    original_data = original_data.encode()
    # （4）对原始数据进行摘要加密
    md5.update(original_data)
    # （5）获取加密后的 16 进制串
    print(md5.hexdigest())


encrypted(data="123456", salt="6666")
```

## 3.4 摘要算法的加密场景

```python
# 摘要算法不是加密算法
# 加密算法的含义是 可以对元数据进行加密 并且 可以对加密后的数据进行解密
# 摘要算法的含义是 单向加密 没办法反向解密

# 1.数据签名
# 一般是用在证书上面
# 提供一个原始的数据 ---> 加密出来一个串 放到你的证书上面
# 如果想验证你这个证书的真伪 0---> 某些数据是只有你才知道的数据 身份证号 / 自己设置的密码
# 去他的平台 输入你的信息 在后台做一些摘要操作 生成一个串

# 2.验证数据的完整性
# 在传输文件数据 ---> 二进制数据
# 将一部分二进制数据拦截到然后在其中添加了一点病毒
# 你会将文件下载到本地然后运行 ---> 被病毒入侵电脑

# 为了避免别人篡改数据
# 在传输文件数据 ---> 二进制数据 --> 用 md5 加密
# 把这个串一并发给你
# 接受到文件数据 ---> 二进制数据 ---> 自动启动验证程序 ---> 将接收到的所有二进制数据进行 md5 加密
# 前后两次加密串进行比对 如果一直 别人没有篡改过
```

# 4 logging模块

```python
# logging模块
# 记录日志 ： 当我们的程序中遇到错误或者有一些比较重要的操作的时候

# 配置日志配置文件

import logging
import logging.config
import os
import sys

try:
    # 想要给日志上色就安装这个模块
    import coloredlogs
except Exception as e:
    if str(e) == "No module named 'coloredlogs'":
        pass

# 自定义日志级别
CONSOLE_LOG_LEVEL = "INFO"
FILE_LOG_LEVEL = "DEBUG"

# 自定义日志格式
# 打印在文件里的格式：时间戳 + 线程名 + 线程ID + 任务ID + 发出日志调用的源文件名 + 发出日志调用的源代码行号 + 日志级别 + 日志消息正文
# [2023-06-04 15:16:05][MainThread:22896][task_id:root][调用.py:12][INFO][这是注册功能]
STANDARD_FORMAT = '[%(asctime)s][%(threadName)s:%(thread)d][task_id:%(name)s][%(filename)s:%(lineno)d][%(levelname)s][%(message)s]'
# 打印在控制台的格式：日志级别 + 时间戳 + 发出日志调用的源文件名 + 发出日志调用的源代码行号 + 日志消息正文
# [INFO][2023-06-04 15:37:28,019][调用.py:12]这是注册功能

SIMPLE_FORMAT = '[%(levelname)s][%(asctime)s][%(filename)s:%(lineno)d]%(message)s'
'''
参数详解:
-1.%(asctime)s: 时间戳，表示记录时间
-2.%(threadName)s: 线程名称
-3.%(thread)d: 线程ID
-4.task_id:%(name)s: 任务ID，即日志记录器的名称
-5.%(filename)s: 发出日志调用的源文件名
-6.%(lineno)d: 发出日志调用的源代码行号
-7.%(levelname)s: 日志级别，如DEBUG、INFO、WARNING、ERROR、CRITICAL等
-8.%(message)s: 日志消息正文
'''

# 日志文件路径
# os.getcwd() : 获取当前工作目录，即当前python脚本工作的目录路径
# 拼接日志文件路径 : 当前工作路径 + “logs”(日志文件路径)
LOG_PATH = os.path.join(os.getcwd(), "Activity_logs")
# OS模块 ： 创建多层文件夹
# exist_ok=True 的意思是如果该目录已经存在，则不会抛出异常。
os.makedirs(LOG_PATH, exist_ok=True)
# log日志文件路径 ： 路径文件夹路径 + 日志文件
LOG_FILE_PATH = os.path.join(LOG_PATH, 'Logs.log')

# 日志配置字典
LOGGING_DIC = {
    # 日志版本
    'version': 1,
    # 表示是否要禁用已经存在的日志记录器（loggers）。
    # 如果设为 False，则已经存在的日志记录器将不会被禁用，而是可以继续使用。
    # 如果设为 True，则已经存在的日志记录器将会被禁用，不能再被使用。
    'disable_existing_loggers': False,
    # 格式化程序：用于将日志记录转换为字符串以便于处理和存储。
    # 格式化程序定义了每个日志记录的输出格式，并可以包括日期、时间、日志级别和其他自定义信息。
    'formatters': {
        # 自定义格式一：年-月-日 时:分:秒
        'standard': {
            # 自定义日志格式 ：时间戳 + 线程名 + 线程ID + 任务ID + 发出日志调用的源文件名 + 发出日志调用的源代码行号 + 日志级别 + 日志消息正文
            # 这里要对应全局的 STANDARD_FORMAT 配置
            'format': STANDARD_FORMAT,
            # 时间戳格式：年-月-日 时:分:秒
            'datefmt': '%Y-%m-%d %H:%M:%S'  # 时间戳格式
        },
        # 自定义格式二：
        'simple': {
            # 自定义日志格式：# 日志级别 + 时间戳 + 发出日志调用的源文件名 + 发出日志调用的源代码行号 + 日志消息正文
            # 这里要对应全局的 SIMPLE_FORMAT 配置
            'format': SIMPLE_FORMAT
        },
    },
    # 过滤器
    'filters': {},
    # 日志处理器
    'handlers': {
        # 自定义处理器名称 - 输出到控制台屏幕
        'console': {
            # NOTSET < DEBUG < INFO < WARNING < ERROR
            # 设置日志等级 为INFO
            'level': CONSOLE_LOG_LEVEL,
            # 表示该处理器将输出日志到流（stream）：日志打印到控制台
            'class': 'logging.StreamHandler',
            # 日志打印格式：日志级别 + 时间戳 + 发出日志调用的源文件名 + 发出日志调用的源代码行号 + 日志消息正文
            # 这里的配置要对应 formatters 中的 simple 配置
            'formatter': 'simple'
        },
        # 自定义处理器名称 - 输出到文件
        'default': {
            # 自定义日志等级
            'level': FILE_LOG_LEVEL,
            # 标准输出到文件
            'class': 'logging.handlers.RotatingFileHandler',
            # 日志打印格式：年-月-日 时:分:秒
            # 这里的配置要对应 formatters 中的 standard 配置
            'formatter': 'standard',
            # 这里 要注意声明配置文件输出端的文件路径
            'filename': LOG_FILE_PATH,
            # 限制文件大小：1024 * 1024 * 5 = 5242880，意味着这个变量的值是 5MB（兆字节）
            'maxBytes': 1024 * 1024 * 5,
            # 表示保留最近的5个日志文件备份。
            # 当日志文件达到最大大小限制时，将会自动轮转并且保留最新的5个备份文件，以便查看先前的日志记录。
            # 当日志文件达到最大大小限制时，会自动进行轮转，后续的文件名将会以数字进行命名，
            # 例如，第一个备份文件将被重命名为原始日志文件名加上".1"的后缀，
            # 第二个备份文件将被重命名为原始日志文件名加上“.2”的后缀，
            # 以此类推，直到保留的备份数量达到设定的最大值。
            'backupCount': 5,
            # 日志存储文件格式
            'encoding': 'utf-8',
        },
    },
    # 日志记录器，用于记录应用程序的运行状态和错误信息。
    'loggers': {
        # 空字符串作为键 能够兼容所有的日志(当没有找到对应的日志记录器时默认使用此配置)
        # 默认日志配置
        '': {
            # 日志处理器 类型：打印到控制台输出 + 写入本地日志文件
            'handlers': ['default', 'console'],
            # 日志等级 ： DEBUG(测试环节会产生的所有信息)  INFO(正常执行产生的信息)  WARNING(警告 提示你这样做不对)  ERROR(程序运行遇到错误)  CRITICAL(事故最大的级别)
            'level': 'WARNING',
            # 默认情况下，当一个日志消息被发送到一个Logger对象且没有被处理时，该消息会被传递给它的父Logger对象，以便在更高层次上进行处理。
            # 这个传递过程称为“传播(propagation)”，而propagate参数指定了是否要使日志消息向上传播。
            # 将其设置为True表示应该传播消息到上一级的Logger对象；如果设置为False则不传播。
            # 表示异常将会在程序中继续传播
            # 也就是说，如果一个异常在当前的代码块中没有被处理，它将会在上级代码块或调用函数中继续向上传递，直到被某个代码块捕获或者程序退出。
            # 这是 Python 中异常处理机制的默认行为。
            # 如果将 'propagate' 设置为 False，则异常不会被传播，即使在上级代码块中没有处理异常的语句，程序也会忽略异常并继续执行。
            'propagate': True,
        },
    },
}


def set_logging_color(name='', ):
    # 初始化日志处理器 - 使用配置字典初始化日志处理器(将自定义配置加载到日志处理器中)
    # logging.basicConfig(level=logging.WARNING)
    logging.config.dictConfig(LOGGING_DIC)
    # 实例化日志处理器对象 - 并赋予日志处理器等级
    logger = logging.getLogger(name)
    # 将logger对象传递给coloredlogs.install()函数，并执行该函数以安装彩色日志记录器，使日志信息在控制台上呈现为带有颜色的格式。
    # 具体来说，该函数会使用ANSI转义序列在终端上输出日志级别、时间戳和消息等信息，并按日志级别使用不同的颜色来区分它们。这可以让日志信息更易于阅读和理解。
    coloredlogs.install(logger=logger)
    # 禁止日志消息向更高级别的父记录器(如果存在)传递。
    # 通常情况下，当一个记录器发送一条日志消息时，该消息会被传递给其所有祖先记录器，直到传递到根记录器为止。但是，通过将logger.propagate设置为False，就可以阻止该记录器的消息向上层传递。
    # 换句话说，该记录器的消息只会被发送到该记录器的处理程序（或子记录器）中，而不会传递给祖先记录器，即使祖先记录器的日志级别比该记录器要低。
    # 这种方法通常适用于需要对特定记录器进行控制，并希望完全独立于其祖先记录器的情况。
    # 确保 coloredlogs 不会将我们的日志事件传递给根 logger，这可以防止我们重复记录每个事件
    logger.propagate = False
    # 配置 日志颜色
    # 这段代码定义了一个名为coloredFormatter的变量，并将其赋值为coloredlogs.ColoredFormatter。
    # 这是一个Python库中的类，用于创建带有彩色日志级别和消息的格式化器。
    # 该变量可以用作日志记录器或处理程序的格式化程序，以使日志输出更易于阅读和理解。
    coloredFormatter = coloredlogs.ColoredFormatter(
        # fmt表示格式字符串，它包含了一些占位符，用于在记录日志时动态地填充相关信息。
        # [%(name)s]表示打印日志时将记录器的名称放在方括号内，其中name是一个变量名，将被记录器名称所替换。
        # %(asctime)s表示打印日志时将时间戳（格式化为字符串）插入到消息中，其中asctime是时间的字符串表示形式。
        # %(funcName)s表示打印日志时将函数名插入到消息中，其中funcName是函数的名称。
        # %(lineno)-3d表示打印日志时将行号插入到消息中，并且将其格式化为3位数字，其中lineno表示行号。
        # %(message)s表示打印日志时将消息本身插入到消息中。
        # 综合起来，这个格式字符串将在记录日志时输出以下信息：记录器名称、时间戳、函数名称、行号和日志消息。
        # 记录器名称 + 时间戳 + 函数名称 + 行号 + 日志消息
        # [root] 2023-06-04 16:00:57 register 15   this is an info message
        fmt='[%(name)s] %(asctime)s %(funcName)s %(lineno)-3d  %(message)s',
        # 级别颜色字典
        level_styles=dict(
            # debug 颜色：白色
            debug=dict(color='white'),
            # info 颜色：蓝色
            info=dict(color='blue'),
            # warning 颜色：黄色 且 高亮
            warning=dict(color='yellow', bright=True),
            # error 颜色：红色 且 高亮 且 加粗
            error=dict(color='red', bold=True, bright=True),
            # critical 颜色：灰色 且 高亮 且 背景色为红色
            critical=dict(color='black', bold=True, background='red'),
        ),
        # 这段代码定义了一个名为field_styles的字典 , 其中包含四个键值对。
        # 每个键代表日志记录中的不同字段，而每个值是一个字典，它指定了与该字段相关联的样式选项。
        # 具体来说，这些字段和样式选项如下：
        # name：指定记录器的名称，将使用白色颜色。
        # asctime：指定日志记录的时间戳，将使用白色颜色。
        # funcName：指定记录消息的函数名称，将使用白色颜色。
        # lineno：指定记录消息的源代码行号，将使用白色颜色。
        field_styles=dict(
            name=dict(color='white'),
            asctime=dict(color='white'),
            funcName=dict(color='white'),
            lineno=dict(color='white'),
        )
    )

    ## 配置 StreamHandler：终端打印界面
    # 这行代码定义了一个名为ch的日志处理器。
    # 具体来说，它是logging.StreamHandler类的一个实例，用于将日志输出到标准输出流（即控制台）中。
    # 在创建StreamHandler对象时，需要指定要使用的输出流
    # 因此stream=sys.stdout参数指定了该对象将把日志写入到标准输出流中.
    ch = logging.StreamHandler(stream=sys.stdout)
    # 这段代码是 Python 中用于设置日志输出格式的语句。
    # 它使用了一个名为 coloredFormatter 的格式化器对象，并将其传递给 ch.setFormatter() 方法来指定输出日志的样式。
    # 具体来说，ch 是一个 Logger 对象，它代表了整个日志系统中的一个记录器。
    # 可以通过该对象来控制日志的级别、输出位置等行为。而 setFormatter() 方法则用于设置该 Logger 输出日志的格式化方式
    # 这里传递的是 coloredFormatter 格式化器对象。
    # 在实际应用中，coloredFormatter 可以是自定义的 Formatter 类或者已经存在的 Formatter 对象。
    # 这里 将 coloredFormatter - 彩色输出日志信息的格式化器 传入进去。
    ch.setFormatter(fmt=coloredFormatter)
    # 这段代码是在Python中添加一个日志记录器（logger）的处理器（handler）。其中，hdlr参数是指定要添加到记录器(logger)中的处理器(handler)对象，ch是一个代表控制台输出的处理器对象。
    # 这行代码的作用是将控制台输出的信息添加到日志记录器中。
    logger.addHandler(hdlr=ch)
    # 这段代码用于设置日志级别为DEBUG，也就是最低级别的日志记录。
    # 意思是在程序运行时，只有DEBUG级别及以上的日志信息才会被记录并输出，而比DEBUG级别更低的日志信息则不会被记录或输出。
    # DEBUG（调试）、INFO（信息）、WARNING（警告）、ERROR（错误）和CRITICAL（严重错误）。
    logger.setLevel(level=logging.DEBUG)
    # 返回日志生成对象
    return logger


def get_logger(name='', ):
    '''
    :param name: 日志等级
    :return:
    '''
    # 初始化日志处理器 - 使用配置字典初始化日志处理器(将自定义配置加载到日志处理器中)
    logging.config.dictConfig(LOGGING_DIC)
    # 实例化日志处理器对象 - 并赋予日志处理器等级
    logger = logging.getLogger(name)
    # 返回日志生成对象
    return logger


if __name__ == "__main__":
    # # 示例：
    # # （1）初始化日志处理器 - 使用配置字典初始化日志处理器(将自定义配置加载到日志处理器中)
    # logging.config.dictConfig(LOGGING_DIC)
    # # （2）实例化日志处理器对象 - 并赋予日志处理器等级
    # # # logger1 = logging.getLogger('') # 默认为 '' 即以默认配置进行实例化
    # logger1 = logging.getLogger('')
    # # （3）当日志发生时，打印的提示语
    # logger1.debug('这是日志生成语句')
    # logger_nor = get_logger(name="admin")
    # logger_nor.info(msg="this is a info message")
    # logger_nor.debug(msg="this is a debug message")
    # logger_nor.warning(msg="this is a warning message")
    # logger_nor.critical(msg="this is a critical message")
    # logger_nor.error(msg="this is a error message")
    logger_col = set_logging_color()
    logger_col.info(msg="this is a info message")
    logger_col.debug(msg="this is a debug message")
    logger_col.warning(msg="this is a warning message")
    logger_col.critical(msg="this is a critical message")
    logger_col.error(msg="this is a error message")
```



TASK：

1. 这部分的代码√笔记√

2. 熟悉全部内容 过一遍

3. 写系统 第一次写登陆注册 看文件夹是如何构造起来 顺序怎样 然后转为ATM系统 自己想思路 再写一写