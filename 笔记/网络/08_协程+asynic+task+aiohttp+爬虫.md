# 1 内容回顾

```python
# 【一】GIL锁 - 全局解释器锁
# CPython解释器自带的锁
# 为了保证线程的安全
# 在一个线程下开启多线程的时候，同一时刻只能有一个线程在运行。

# 【二】Python的GIL锁导致无法并发
# 1.计算密集型
# 需要持续地计算
# 需要占用大量的CPU资源进行任务，这种情况下多线程就没有太大优势了

# 2.IO密集型
# 涉及到大量的输入输出、网络波动
# 一旦遇到IO阻塞 CPU就会释放权限和资源
# 多个任务，有某个任务阻塞的时候 将CPU权限放出来，下一个任务就可以使用CPU资源

# 【三】死锁
# Lock
# 死锁是指两个或多个进程，在执行过程中因为抢夺资源造成互相等待的现象
# 死锁有解决方法吗？ - 别造成死锁

# 【四】递归锁
# RLock
# 递归锁也叫可重入锁，允许一个或者多个进程抢夺同一把锁
# 在某一个线程抢夺到当前锁之后，会对当前锁的计数增加
# 当前线程释放锁之后，会对当前锁的计数减少
# 这样就可以保证同一时刻在持有锁的时候再次获取锁也不用被自己的锁阻塞

# 【五】信号量
# 信号量本质上其实也是一把锁，但是比互斥锁更加灵活

# 信号量可以控制同一时刻拿到锁的进程数或线程数

# 厕所有三坑
# 三坑满了，加锁不让第四个人进去 -> 信号量来控制当前锁的个数
# 一个人一个坑，加锁不让其他人拿这个坑 -> 互斥锁
# from threading import Semaphore
# from multiprocessing import Semaphore

# 【六】事件
# 事件是进程或线程之间通信的一种方式
# 存储当前事件的状态 -> True / False
# wait() - 等待当前事件的状态切换
# set() - 设置当前事件的状态False - True
# clear() - 清除当前事件的状态True - False

# 【七】进程池和线程池
# 1.导入
from concurrent.futures import ProcessPoolExecutor, ThreadPoolExecutor

# 2.创建一个带有指定容量的池子
# 创建指定容量的池子的时候就已经在内存空间中开辟出指定数量的资源了
# 减少了开辟资源的消耗
# max_workers 创建的池子数量
pool = ProcessPoolExecutor(max_workers=4)
pool2 = ThreadPoolExecutor(max_workers=4)


# 3.写异步的任务 - 子进程/子线程 函数
def work():
    ...


# 3.5.补充 异步回调函数
# 在执行完上面的异步任务之后直接进入到回到函数中并且拿着异步任务的结果
def callback(future_obj):
    result = future_obj.result()
    ...


# 4.提交任务
for i in range(5):
    # 第一个参数是需要执行的异步函数地址，函数的参数直接在后边按照位置传入即可
    pool.submit(work,).add_done_callback(callback)

# 5.主进程等待所有子进程结束后结束
pool.shutdown()
```



# 2 协程-理论

```python
# 协程
# 进程是在操作系统中执行的程序
# 线程是在进程内部执行的程序
# 协程是在线程内部执行的程序

# 【一】yield 关键字
# 创建生成器对象的时候用到过
# 可以保持当前的状态
# send 关键字：向一个函数内部传入当前的值并且让异步函数向下执行

# 【二】协程介绍
# 1.什么是协程
# 协程是一种用户态的轻量级线程

# 2.优点
# 开销更小 属于程序级别的切换 操作系统完全感知不到
# 因为GIL锁导致同一时刻只能运行一个线程 但在一个线程内不会限制协程数 最大程度地利用CPU

# 3.缺点
# 协程的本质是单线程下的多任务处理 也没办法利用多核优势
# 一旦协程遇到阻塞 就会阻塞整个线程

# 【三】Greenlet
# 在以前开启协程时使用的模块
# 但现在基本不用了

# 【四】asyic模块
```



# 3 协程-操作

```python
# 【一】asyncio模块
# asyncio模块是Python中实现异步的一个模块，该模块在Python3.4的时候发布
# async和await关键字在Python3.5中引入。
# 因此，想要使用asyncio模块，建议Python解释器的版本不要低于Python3.5。

# 【二】协程函数和协程对象
# 1.协程函数
# 加上 asynic 关键字的函数 就是协程函数
# 2.协程对象
# 协程函数被调用后的返回值 就是协程对象
def work_old():
    print(f"22")


async def work():
    print(f"11")

print(work)
print(work_old)
# <function work at 0x00000150DE3663B0>
# <function work_old at 0x00000150DDED3F40>

result = work()
# sys:1: RuntimeWarning: coroutine 'work' was never awaited
# 正常调用异步的写成函数会直接报错
result_old = work_old()
# 22
print(result)
print(result_old)
# <coroutine object work at 0x00000150DE316960>
# None

# 【三】调用协程函数
# 创建事件循环 - 启动
import asyncio

loop = asyncio.run(work())
# 11
```



# 4 asynic模块的使用

```python
import asyncio
import time


# 【一】基本使用
async def work():
    print(f"当前是协程函数!")

    return f"work return"


# 执行协程函数方式一
def main_one():
    # 1.调用协程函数 -> 协程对象
    obj = work()
    # print(f"obj :>>> {obj}")
    # obj :>>>> <coroutine object work at 0x0000028D353E7060>

    # 2.创建一个事件循环对象
    loop = asyncio.get_event_loop()
    # print(f"loop :>>> {loop}")
    # loop :>>>> <ProactorEventLoop running=False closed=False debug=False>

    # 3.将协程对象注册到事件循环对象中
    loop.run_until_complete(obj)


# 执行协程函数方式二
def main_two(func):
    # 1.得到一个异步协程对象
    obj = func()

    # 2.直接使用 内置的 run 方法启动协程函数
    # 本质上的内置原理和第一种方式是一样的：
    # 先创建事件循环对象
    # 然后注册当前异步协程对象到事件循环中
    # 注意：run方法是 python3.7+ 才有的
    asyncio.run(obj)


# 【二】异步关键字 await
# 当我们 **协程函数中** 调用另一个协程函数的时候，有返回值
# 在调用另一个函数的时候对于协程来说其实是一个 IO 阻塞的状态
# 为了能够异步地完成自己的任务 ，就不能 直接 等待 --- > 异步变成了同步
async def work_one():
    print(f"这是协程函数内部!")
    print(f"IO阻塞开始")

    '''
    # time.sleep(2)  # 使用内置模块的 sleep 由原本的异步变成了同步
    # 只要遇到IO阻塞就必须加 await --- 调用函数 / 睡眠
    # await asyncio.sleep(2)
    '''

    '''
    response =  work()
    # 不加 await 就会直接抛出异常
    # RuntimeWarning: Enable tracemalloc to get the object allocation traceback
    '''

    response = await work()
    # 加了 await 之后就可以获取到异步函数的返回值

    print(f"IO阻塞完成 {response}")


async def work_two(i):
    print("start ... ")
    await asyncio.sleep(2)
    print("end ... ")

    return f"{i}的返回值 {i * i}"


async def work_three():
    print(f"协程函数内部开始")
    # 调用上面的异步函数 work_two 并且获取到 work_two 的返回值

    response_one = await work_two(2)
    print(f"IO阻塞结束 :>>>> {response_one}")

    response_two = await work_two(4)
    print(f"IO阻塞结束 :>>>> {response_two}")


if __name__ == '__main__':
    # main_one()
    # 当前是协程函数!

    # main_two(func=work)
    # 当前是协程函数!

    # main_two(func=work_one)
    # 这是协程函数内部!
    # IO阻塞开始
    # 当前是协程函数!
    # IO阻塞完成 work return

    main_two(func=work_three)

    # 协程函数内部开始
    # start ...
    # end ...
    # IO阻塞结束 :>>>> 2的返回值 4
    # start ...
    # end ...
    # IO阻塞结束 :>>>> 4的返回值 16
```



# 5 task对象

```python
import asyncio


async def work(i):
    print(f"start .... ")
    await asyncio.sleep(2)
    print("end .... ")

    return i * i


async def run():
    print(f"协程函数内部开始")

    # await ---> 如果给函数用 是为了获取到当前异步函数的返回值的

    """
    # 单任务：
    # 获取当前的协程对象
    obj = work()

    # 创建协程对象的任务对象 TASK
    task = asyncio.create_task(obj)
    # <Task pending name='Task-2' coro=<work() running at /Users/dream/Documents/PythonProjects/Python30/day38/06task对象.py:8>>

    # 异步等待当前任务完成
    res = await task
    print(res)  # 81
    """

    # 多任务：
    # 创建多个协程对象然后一起等待结果
    # work(i) : 异步的协程对象
    task_list = [asyncio.create_task(work(i)) for i in range(5)]
    # 创建了 5 个任务对象并存储在 task_list 中，每个任务将执行 work(i)，i 的值分别为 0 到 4

    # task_list 里面存的是所有创建好的协程的 task 任务对象

    # 获取到 task 任务执行后的结果
    # res_list = [await task for task in task_list]
    # print(res_list) # [0, 1, 4, 9, 16]

    # 获取到 task 任务执行后的结果 --- 第一个值是 set() 往后是一个集合 集合中存着每一个异步的结果
    done, pending = await asyncio.wait(task_list, timeout=2)

    # 我们想要的是回调回来的结果
    response = await asyncio.gather(*task_list)

    print(response)
    # [0, 1, 4, 9, 16]


if __name__ == '__main__':
    asyncio.run(run())
```



# 6 aiohttp

```python
# ● 我们之前学习过爬虫最重要的模块requests，但它是阻塞式的发起请求，每次请求发起后需阻塞等待其返回响应，不能做其他的事情。
#   ○ 本文要介绍的aiohttp可以理解成是和requests对应Python异步网络请求库，
#   它是基于 asyncio 的异步模块，可用于实现异步爬虫，有点就是更快于 requests 的同步爬虫。
#   ○ 安装方式，pip install aiohttp。
# ● aiohttp是一个为Python提供异步HTTP 客户端/服务端编程，基于asyncio的异步库。
#   ○ asyncio可以实现单线程并发IO操作，其实现了TCP、UDP、SSL等协议，
#   ○ aiohttp就是基于asyncio实现的http框架。

import aiohttp
import asyncio


async def main():
    async with aiohttp.ClientSession() as session:
        async with session.get("http://httpbin.org/headers") as response:
            print(await response.text())

asyncio.run(main())

'''
{
    "headers": {
        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
        "Accept-Encoding": "gzip, deflate",
        "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6",
        "Host": "httpbin.org",
        "Upgrade-Insecure-Requests": "1",
        "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36 Edg/128.0.0.0",
        "X-Amzn-Trace-Id": "Root=1-66d7c952-14e252d64c287cd06c9a203b"
    }
}
'''
```



# 7 爬虫

## 7.1 爬虫基础

```python
# 【一】分析页面构成
# 1.地址栏输入 https://pic.netbian.com/
# 回车就能够看到当前的页面

# 2.页面是从哪里来的？
# TCP客户端的服务端
# 服务端是可以根据客户端的请求返回指定数据的
# 浏览器其实本质上就是一个很大的客户端

# 我们在地址栏输入地址其实本质上就是在向服务端请求资源
# 本质上还是向某一个IP和端口发送请求获取响应数据

# 3.服务端接收到请求
# 返回当前请求对应的资源--->我们看到的页面的HTML源码

# 4.如果做爬虫
# 我们的目的就是为了 将 HTML 源码下载到本地中
# 再从HTML 代码中将需要的资源提出来

# 将页面源码直接复制出来
# 问题：上百页 人力完成这个任务很浪费时间

# 于是诞生爬虫
# 自动化地帮助我们拉取我们想要的数据到本地

# 【二】浏览器去访问服务端数据
# 正常提交请求给任务栏即可
# 我们通过代码能够完成吗
# 只要是在地址栏输入地址回车的请求都是属于 get 请求

# 有人写好了这样操作的模块
# pip install requests
# import requests
# fake
# 帮助我们完成 浏览器向指定地址发送请求返回数据的操作

import requests

# pip install fake-useragent
from fake_useragent import UserAgent

print(UserAgent().random)

target_url = 'https://www.baidu.com/'

headers = {
    'User-agent': "Mozilla/5.0 (iPhone; CPU iPhone OS 17_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) EdgiOS/120.0.2210.141 Version/17.0 Mobile/15E148 Safari/604.1"
}

response = requests.get(target_url, headers=headers)

# print(response, type(response))
# <Response [200]> <class 'requests.models.Response'>
# 想要的数据应该是页面源码对应的 HTML 代码

# 两个方法
# response.text 获取文本字符串形式的源码数据
# response.content 获取二进制形式的源码数据 图片/视频/...

response.encoding = 'utf-8'
# 有乱码 就试一下utf-8 jbk
# print(response.text)

"""
target_url = 'https://pic.netbian.com/uploads/allimg/240904/003722-17253814421937.jpg'

response = requests.get(url=target_url)

print(response.content)
"""
```

## 7.2 多进程多线程爬虫

```python
# pip install requests
import json
import os.path
import time
from multiprocessing import Process
from threading import Thread

import requests
# pip install fake-useragent
from fake_useragent import UserAgent
# pip install lxml
from lxml import etree


class SpiderImage(object):
    def __init__(self):
        self.area = "https://pic.netbian.com"
        self.headers = {
            "User-Agent": UserAgent().random
        }
        self.BASE_DIR = os.path.dirname(os.path.abspath(__file__))
        self.SOURCE_DIR = os.path.join(self.BASE_DIR, "source")
        self.PAGE_DIR = os.path.join(self.SOURCE_DIR, "page")
        self.IMAGE_DIR = os.path.join(self.SOURCE_DIR, "image")
        self.IMAGE_DATA_DIR = os.path.join(self.SOURCE_DIR, "image_json")
        self.IMAGE_PNG_DIR = os.path.join(self.SOURCE_DIR, "image_png")

    @staticmethod
    def read(file_path, tag=None):
        with open(file=file_path, mode="r", encoding="utf-8") as fp:
            if tag == "json":
                data = json.load(fp=fp)
            else:
                data = fp.read()
        return data

    @staticmethod
    def save(file_path, data, mode="w", encoding=None, tag=None):
        if mode == "w":
            encoding = "utf-8"
        with open(file=file_path, mode=mode, encoding=encoding) as fp:
            if tag == "json":
                json.dump(fp=fp, obj=data, ensure_ascii=False)
            else:
                fp.write(data)

    def create_tree(self, target_url):
        file_name = '_'.join(target_url.split('/')[-2:])
        if ".html" not in file_name:
            file_name += ".html"
        os.makedirs(self.SOURCE_DIR, exist_ok=True)
        os.makedirs(self.PAGE_DIR, exist_ok=True)
        os.makedirs(self.IMAGE_DIR, exist_ok=True)
        os.makedirs(self.IMAGE_DATA_DIR, exist_ok=True)
        os.makedirs(self.IMAGE_PNG_DIR, exist_ok=True)
        # 拼接文件路径
        if "tupian" in file_name:
            file_path = os.path.join(self.IMAGE_DIR, file_name)
        else:
            file_path = os.path.join(self.PAGE_DIR, file_name)
        # 如果是第一次请求则文件不存在请求并保存一份文件
        if not os.path.exists(file_path):
            response = requests.get(url=target_url, headers=self.headers)
            response.encoding = "gbk"
            self.save(file_path=file_path, data=response.text)
            print(f"当前地址 {target_url} 第一次请求，保存源码完成!")
        # 把保存的文件读出来
        data = self.read(file_path=file_path)
        # 生成 tree 对象
        tree = etree.HTML(data)
        return tree

    def spider_category(self):
        tree = self.create_tree(self.area)
        a_list = tree.xpath('//*[@id="main"]/div[2]/a')
        # 提取 标签对象中的 属性值 @属性名 / 文本值 text()
        category_dict = {}
        for a in a_list:
            # //*[@id="main"]/div[2]/a[1]
            # ./@href
            a_href = self.area + a.xpath("./@href")[0]
            a_title = a.xpath("./text()")[0]
            # a_title = a.xpath("./@title")[0]
            category_dict[a_title] = a_href
        return category_dict

    @staticmethod
    def format_img_title(img_title):
        not_user_type = ['?', '*', ':', '"', '<', '>', '\\', '/', '|']
        for item in not_user_type:
            if item in img_title:
                img_title = img_title.replace(item, 'x')
        return img_title

    def spider_image_data(self, target_url):
        tree = self.create_tree(target_url=target_url)
        file_name = '_'.join(target_url.split('/')[-2:])
        file_path = os.path.join(self.IMAGE_DATA_DIR, file_name + ".json")
        img_data_dict = {}
        if not os.path.exists(file_path):
            li_list = tree.xpath('//*[@id="main"]/div[3]/ul/li')
            for li in li_list:
                # //*[@id="main"]/div[3]/ul/li[1]/a
                # ./a/@href
                detail_url = self.area + li.xpath("./a/@href")[0]
                try:
                    img_tree = self.create_tree(target_url=detail_url)
                    # //*[@id="img"]/img
                    img_url = self.area + img_tree.xpath('//*[@id="img"]/img/@src')[0]
                    img_title = img_tree.xpath('//*[@id="img"]/img/@title')[0]
                    img_data_dict[self.format_img_title(img_title=img_title)] = img_url
                except Exception as e:
                    print(f"当前地址 {detail_url} 出错，已跳过！")
            self.save(file_path=file_path, data=img_data_dict, tag="json")
            print(f"当前第一次请求地址 {target_url} 保存本页数据完成！")
        img_data_dict = self.read(file_path=file_path, tag="json")
        print(f"读取当前地址 {target_url} 图片数据完成!")
        return img_data_dict

    @staticmethod
    def create_target_url_list(target_url, start_page, end_page):
        target_url_list = []
        for i in range(start_page, end_page + 1):
            if i == 1:
                target_url_list.append(target_url)
            else:
                _target_url = target_url + f'index_{i}.html'
                target_url_list.append(_target_url)
        return target_url_list

    def download_img(self, img_url, img_title):
        # 请求图片地址 获取图片的二进制数据
        response = requests.get(url=img_url, headers=self.headers)
        # 转为二进制数据
        img_data = response.content
        # 拼接文件保存路径 并且保存文件数据
        file_path = os.path.join(self.IMAGE_PNG_DIR, f'{img_title}.png')
        # 保存文件数据
        self.save(file_path=file_path, data=img_data, mode="wb", tag="png")
        print(f"当前文件 {img_title} 下载完成!")

    def main_spider_img_data_dict(self):
        while True:
            category_dict = self.spider_category()
            category_name_dict = {}
            for index, category_name in enumerate(category_dict.keys(), start=1):
                print(f"当前编号 :>>>> {str(index).zfill(5)} 当前分类名称 :>>>> {category_name}")
                category_name_dict[str(index).zfill(5)] = category_name
            category_id = input("请输入分类ID :>>>> ").strip()
            if category_id not in category_name_dict.keys():
                print(f"当前分类ID异常!")
                continue
            category_name = category_name_dict[category_id]
            category_url = category_dict.get(category_name)
            # 输入起始页码和终止页码
            start_page = input("请输入起始页码 :>>>> ").strip()
            end_page = input("请输入终止页码 :>>>> ").strip()
            if not start_page.isdigit() or not end_page.isdigit():
                print(f"当前页码必须是数字")
                continue
            start_page = int(start_page)
            end_page = int(end_page)
            if start_page > end_page or start_page < 0 or end_page < 0:
                print(f"当前页码非法!")
                continue
            # 创建目标URL列表
            target_url_list = self.create_target_url_list(target_url=category_url, start_page=start_page,
                                                          end_page=end_page)
            # 有了目标地址 提取当前页的 图片数据
            img_data_dict = {}
            for target_url in target_url_list:
                img_data_dict.update(self.spider_image_data(target_url=target_url))
            return img_data_dict

    @staticmethod
    def timer(func):
        def inner(*args, **kwargs):
            start_time = time.time()
            result = func(*args, **kwargs)
            end_time = time.time()
            print(f"{func.__name__} 总耗时 :>>>> {end_time - start_time} s")
            return result

        return inner

    @timer
    def download_normal(self):
        # 正常下载方式
        img_data_dict = self.main_spider_img_data_dict()
        # 解包下载
        for img_title, img_url in img_data_dict.items():
            self.download_img(img_url=img_url, img_title=img_title)

    # 多进程和多线程
    @timer
    def process_download(self, cls):
        img_data_dict = self.main_spider_img_data_dict()
        task_list = [cls(target=self.download_img, args=(img_url, img_title)) for img_title, img_url in
                     img_data_dict.items()]
        [task.start() for task in task_list]
        [task.join() for task in task_list]

    # 多进程
    def process(self):
        self.process_download(cls=Process)

    # 多线程
    def thread(self):
        self.process_download(cls=Thread)


if __name__ == '__main__':
    s = SpiderImage()
    # print(s.spider_category())
    # target_url = 'https://pic.netbian.com/4kdongman/'
    # print(s.create_target_url_list(target_url, 1, 9))
    s.main_spider_img_data_dict()
    # print(s.spider_image_data(target_url="https://pic.netbian.com/4kdongman/index_9.html"))
```

还缺一个多协程 因为爬虫有一些实施问题 先放着 过几天解决