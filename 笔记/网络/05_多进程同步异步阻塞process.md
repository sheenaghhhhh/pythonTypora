# 1 内容回顾

```python
# 【一】程序和进程
# 程序其实就是执行的代码 是静态的 安装到硬盘中
# 进程就是程序的运行实例 是动态的 是正在运行的程序 它运行在内存中

# 程序执行的过程： 硬盘中的数据 - 读取到内存 - CPU去内存中读取执行

# 【二】操作系统
# 操作系统就是用来管理计算机硬件设备和软件程序的系统软件
# 协调、管理和控制计算机硬件资源和软件资源

# 【三】为什么要有操作系统
# 操作系统的底层是一大堆硬件 CPU/主板/内存条/硬盘
# 为了更加方便地管理这些硬件资源

# 1.提高开发效率
# 操作系统将底层的接口进行封装 让开发人员直接使用接口开发即可 而无需关注底层硬件
# 2.优化操作行为
# 将复杂繁琐的操作流程封装成一个简单的操作
# 3.隐藏硬件接口
# 将某部分的硬件进行整合统一暴露一个接口给用户使用 USB/显卡口
# 4.资源有序化
# 通过操作系统可以统一分配和管理系统资源 让每一个进程之间相互隔离和有序进行

# 【四】多路复用实现的两种方式
# 0.产生背景
# 出现了单核CPU 只能执行一个进程 无法实现并发

# 1.空间上的复用
# 进程是操作系统的分配最小分配单位
# 为了可以高效地利用内存资源于是就出现了空间上的复用，谁用的资源多就给谁多分配资源，用完了以后将资源回收

# 2.时间上的复用
# 同一台机器上有多个进程在跑，在同一个时刻只能执行一个进程，为了更加高效地利用CPU资源
# 就出现了时间上的复用，当某个进程遇到 IO阻塞的时候就临时释放 CPU资源，由其他进程先使用，使用完了以后归还资源

# 3.多道技术
# 多道技术就是指 时间上的复用和空间上的复用

# 【五】操作系统和普通软件的区别
# 操作系统是用来管理和控制计算机资源和软件程序的系统软件
# 普通软件是运行在操作系统之上的具有单一功能的程序

# 【六】操作系统发展史
# 第一代计算机：真空管和穿孔卡片，用卡片代表某一组代码的含义，然后读取部分卡片进行操作
# 第二代计算机：晶体管和批处理系统，将部分卡片放到一个统一的盒子中，让专业人员来操作，只需要等待结果即可
# 第三代计算机：集成电路芯片和多道程序设计，用电路芯片来存储部分代码的含义，执行某部分功能即可，出现了单核CPU的概念
# 第四代计算机：超大规模集成电路和多道程序设计，出现了多核CPU的概念，出现了多线程的概念
```



# 2 多进程理论

```python
# 多进程理论 - 实现
# 多线程理论 - 实现
# 协程理论 - 实现

# 【一】什么是进程
# 进程就是一个正在执行的任务或程序
# 负责执行进程的是CPU

# 1.单任务
# 单核CPU+多道技术 -> 实现多个进程的伪并发

# 2.多任务
# 多个任务并发进行

# 【二】进程和程序
# 程序就是一堆代码 做菜的菜谱
# 进程就是程序的执行过程 按照菜谱做菜的过程

# 【三】进程的调度算法问题
# 不同进程之间如何进行调度

# 1.先来先服务算法
# 哪个进程先被执行起来，就先被CPU执行
# 最基础的调度算法
# 适合大型且繁琐的任务，不利于IO繁忙的任务

# 2.短作业优先调度算法
# 在进程执行之前先进行预估，找到短作业或短进程然后按照短作业和短进程的优先级执行进程
# 适合短作业任务 不适合长作业任务

# 3.时间片轮转法
# 在每个进程执行之前进入到一个队列调度中
# 将CPU 的权限划分成 几个时间段，可能是几秒/十几秒
# 进程执行的时候会分配给当前进程一个时间片，让进程在这个时间片之内独享CPU资源
#   在这个时间片内进程执行完成 这个时间片剩下的资源释放掉给下一个进程使用
#   在这个时间片内进程没有执行完成 回到进程的调度队列中的末尾再继续等待资源的分配

# 4.多级反馈队列
# 为进程设置优先级，优先级高的先执行，优先级低的后执行
# 让优先级高的进程去独享CPU资源执行进程
# 进程执行完成之后，进程的优先级降低
# 没有结束的进程，继续按照优先级高的进程执行
# ● 仅当第一队列空闲时，调度程序才调度第二队列中的进程运行；
#   ○ 仅当第1～(i-1)队列均空时，才会调度第i队列中的进程运行。
# ● 如果处理机正在第i队列中为某进程服务时，又有新进程进入优先权较高的队列(第1～(i-1)中的任何一个队列)
#   ○ 则此时新进程将抢占正在运行进程的处理机
#   ○ 即由调度程序把正在运行的进程放回到第i队列的末尾
#   ○ 把处理机分配给新到的高优先权进程。
```



# 3 并发与并行

```python
# 并发与并行
# 无论是并发还是并行 在我们看来状态是一致的，效果就是多个任务在同时运行

# 1.并发
# 伪并行，在一个时间段内，多个任务交替进行，宏观上是同时进行的，微观上并不是同时进行

# 2.并行
# 前提条件是 必须具备多个CPU
# 单核下只能借助多道技术才能实现伪并行
# 多核下才能实现多任务同时进行
```



# 4 同步异步阻塞非阻塞

```python
# 【一】同步
# 指在执行一个程序的时候，没有得到结果之前不会执行第二个程序

# 【二】异步
# 在执行一个程序的时候，不需要等待当前结果，就可以执行第二个程序

# 同步和异步是指在其他进程或者需要在某一段时间内执行任务的情况

# 【三】阻塞
# 在进程返回之前，当前进程被挂起

# 【四】非阻塞
# 不需要等待程序结果直接运行下一个

# 【五】同步调用
# 当一个进程执行多次的任务的时候，该调用就是一直等待直到所有结果结束，但是并未阻塞

# 【六】阻塞调用
# 当我们在使用 socket 建立 tcp 连接后 只要recv没有接收到数据 就会一直处于阻塞的状态 直到有数据为止

# 【小结】
# 同步和异步针对的是函数/任务的调用方式
# 同步指一个进程必须得到当前进程的结果才能执行下一个进程
# 异步指当前进程不需要等待当前进程结果就可以执行下一个进程

# 阻塞/非阻塞针对的是进程or线程
# 阻塞当前进程不满足的时候将程序挂起的状态
# 非阻塞当前进程不满足的时候不会阻塞程序的执行
```



# 5 进程的创建和状态

```python
# 【一】进程的创建
# 在我们的计算机操作系统中，操作系统管理的是底层的硬件
# 只要有操作系统，进程的概念就存在了
# 就需要了解一下进程的创建方式
#   微波炉，由开关可以控制当前微波炉 当你打开开关的时候其实就是启动了一个进程

# 【二】通用系统中进程的创建方式
# 1.系统初始化
# 在系统启动的时候，操作系统会自动启动一些内置的进程
# 比如windows系统中的任务管理器、资源管理器、系统监视器等

# 2.进程中开启子进程
# 一个进程中又开启了一些子进程
# 比如我们开启 Pycharm 但是执行 py 文件的时候

# 3.交互式请求包
# 比如你双击启动一个应用程序

# 4.批处理作业的初始化
# 大型的机器中。有一些批处理的作业会连串的执行

# 【三】在不同的操作系统中新进程的创建方式
# 1.在unix/linux - macos/linux
# fork 创建一个和父进程一样的副本

# 2.在windows
# 通过内置的进程调度 CreatProcess 创建一个进程

# 【四】进程的终止(了解)
# 1.正常退出
# 自己关闭软件
# 2.出错退出
# 在启动阶段发现文件不存在
# 3.严重错误退出
# 在执行非法的指令的时候被迫退出
# int("a")
# 4.被其他进程杀死
# linux: kill
# windows: taskkill


# 【五】进程的状态
# 1.五态模型
# 创建态：进程正在被创建，但是还没有完成创建的过程 双击应用程序图标正在读取内存数据的状态
# 就绪态：进程具备运行的条件，但是还没有分配到CPU，处于等待CPU分配的时间段
# 阻塞态：进程正在执行某个操作，但是需要等待某个事件的完成，比如读写文件
# 执行态：进程正在执行，此时CPU正在执行该进程的指令
# 终止态：进程正在被销毁，此时操作系统会回收该进程所占用的资源

# 2.三态模型
# 运行态：当前进行在运行过程中的状态
# 就绪态：当前进程已经具备运行的条件，但是还没有分配到CPU，处于等待CPU分配的时间段
# 阻塞态：当前进程正在等待某个事件的完成，比如等待用户输入数据 / 读写文件
```



# 6 多进程的实现

```python
# 我们需要借助内置的模块来帮助我们创建进程
# Python 本身是不支持多核的
# multiprocessing 模块可以让我们创建多进程

# 【一】导入模块
import multiprocessing

# 【二】Process类的参数
# process 创建的子进程对象
process = multiprocessing.Process()
"""
group:当前的进程组，默认是None 我们也不会改 始终是 None
target：表示当前需要创建子进程的函数对象
name：当前子进程的名字，默认不会改
args：在调用上面子进程函数的时候需要传递进去的参数 按照位置传递
kwargs：在调用上面子进程函数的时候需要传递进去的参数 按照关键字传递
daemon：守护进程是否开启
"""

# 【三】子进程的启动
process.start()  # 启动子进程
process.run()  # 启动子进程 - 启动 start 之后会触发 run 的运行
process.join()  # 主进程等待所有子进程结束后在结束
process.is_alive()  # 判断当前子进程的存活状态
process.terminate()  # 杀死当前子进程
process.close()  # 终止当前子进程

# 【四】子进程的属性
process.daemon  # 默认是False 当前进程是否开启守护进程
# 只要系统关机你的任务管理器也会随之杀死，任务管理器也会随着系统启动而启动
process.name  # 获取到当前子进程的名字
process.pid  # 查看当前子进程的进程ID
process.exitcode  # 退出状态码
process.authkey # 认证件 默认是一串 32位的16进制数
```



# 7 创建多进程

## 7.1 前情提要

```python
# 【前情提要】
# 注意：在启动子进程的时候一定要在main入口下启动
# 官方提示：
"""
Since Windows has no fork, the multiprocessing module starts a new Python process **and** imports the calling module.
If Process() gets called upon import, then this sets off an infinite succession of new processes (**or** until your machine runs out of resources).
This **is** the reason **for** hiding calls to Process() inside

**if name == "main"**
since statements inside this **if**-statement will **not** get called upon import.
"""

"""
由于Windows没有fork，多处理模块启动一个新的Python进程并导入调用模块。 
如果在导入时调用Process（），那么这将启动无限继承的新进程（或直到机器耗尽资源）。 
这是隐藏对Process（）内部调用的原，使用if **name** == “**main** ”，这个if语句中的语句将不会在导入时被调用。
"""
```

## 7.2 方式一：Process类

```python
# 【一】创建多进程方式一
# 直接使用Process类创建 子进程对象然后启动
# 1.定义子进程函数
def work(name):
    sleep_time = random.randint(1, 3)
    print(f"{name} is sleeping for {sleep_time} seconds...")
    time.sleep(sleep_time)
    print(f"{name} is finished ...")


# 2.获得子进程对象并启动紫禁城
def process_normal():
    # (1)得到一个进程对象
    process_one = Process(
        # target : 目标子进程函数 ， 记住给的是内存地址
        target=work,
        # args : 按照位置向目标函数传递参数，是一个元祖 即使一个元素也要加,
        args=(1,)
    )
    process_two = Process(
        target=work,
        args=(2,)
    )
    
    # (2)启动子进程
    # process_one.start()
    # process_two.start()
    # 1
    # 2
    # 1 is sleeping for 3 seconds...
    # 2 is sleeping for 1 seconds...
    # 2 is finished ...
    # 1 is finished ...
    
    process_one.run()
    process_two.run()
    # 1
    # 1 is sleeping for 3 seconds...
    # 1 is finished ...
    # 2 is sleeping for 2 seconds...
    # 2 is finished ...
    # 2 
    
    # 开启多进程的目的是为了让多几个进程同时运行
    # run 之后看到的效果是多个程 串行


if __name__ == '__main__':
    print("1")
    process_normal()
    print("2")
# 得到一个进程对象
```

## 7.3 方式二：继承

```python
# 【二】创建多进程方式二
# 继承父类process重写run方法
class MyProcess(Process):
    def __init__(self, name):
        super().__init__()
        self.name = name

    def run(self):
        sleep_time = random.randint(1, 4)
        print(f"{self.name} is start sleeping {sleep_time} ... ")
        time.sleep(sleep_time)
        print(f"{self.name} is end sleeping {sleep_time} ... ")


def process_myprocess():
    # (1)得到进程对象
    process_one = MyProcess(name='1')
    process_two = MyProcess(name='2')

    # (2)启动子进程
    process_one.start()
    process_two.start()
    '''
    main process start ... 
    main process end ... 
    1 is start sleeping 1 ... 
    2 is start sleeping 3 ... 
    1 is end sleeping 1 ... 
    2 is end sleeping 3 ... 
    '''


if __name__ == '__main__':
    print(f"main process start ... ")
    process_myprocess()
    print(f"main process end ... ")
```



# 8 进程之间的数据相互隔离

```python
# 进程和进程之间数据是相互隔离的
# 开不同的进程 数据并不会共享

from multiprocessing import Process

money = 99


def change_money():
    global money
    print(f"原来 {money}")
    money += 1
    print(f"现在 {money}")


def change_normal():
    change_money()  # 100
    change_money()  # 101


def change_process():
    task_list = []
    for i in range(3):
        process = Process(target=change_money)
        task_list.append(process)
    [task.start() for task in task_list]

    # 每个人改之后都是 100
    # 是因为每一个子进程都会将主进程的所有数据拷贝一份变成自己的
    # 修改数据只修改自己拷贝出来的那份 而原本的全局不变


if __name__ == '__main__':
    change_normal()
    change_process()
    print(f"修改后的money :>>> {money}")
    # 101
    # 如果没有normal只有process 那就是99
```



# 9 多进程并发TCP服务端

## 9.1 串行

```python
# server
from socket import socket, AF_INET, SOCK_STREAM

server = socket(family=AF_INET, type=SOCK_STREAM)

server.bind(('127.0.0.1', 9696))

server.listen(5)

while True:
    conn, addr = server.accept()
    while True:
        data_from_client = conn.recv(1024)
        if not data_from_client:
            conn.close()
            break
        print(f"来自于客户端的数据 :>>>> {data_from_client.decode()}")
        data_from_client_upper = data_from_client.decode().upper()
        conn.send(data_from_client_upper.encode())
```

```python
# client
from socket import socket, AF_INET, SOCK_STREAM

client = socket(family=AF_INET, type=SOCK_STREAM)

client.connect(('127.0.0.1', 9696))

while True:
    msg = input('请输入要发送的消息：')
    if not msg:
        continue
    client.send(msg.encode('utf-8'))
    data_from_server = client.recv(1024)
    print(f"服务器返回的数据：{data_from_server.decode('utf-8')}")
```

## 9.2 并发

```python
# server
import multiprocessing
from socket import socket, AF_INET, SOCK_STREAM, SOL_SOCKET, SO_REUSEADDR


def work(conn, addr):
    while True:
        data_from_client = conn.recv(1024)
        if not data_from_client:
            conn.close()
            break
        print(f"来自于客户端 {addr} 的数据 :>>>> {data_from_client.decode()}")
        data_from_client_upper = data_from_client.decode().upper()
        conn.send(data_from_client_upper.encode())


def main():
    server = socket(family=AF_INET, type=SOCK_STREAM)
    server.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
    server.bind(('127.0.0.1', 9696))

    server.listen(5)

    while True:
        # 负责接收每一个客户端的链接对象
        conn, addr = server.accept()
        # 将当前的链接对象创建程一个子进程
        task = multiprocessing.Process(
            target=work, args=(conn, addr)
        )
        # 让当前子进程启动
        task.start()


if __name__ == '__main__':
    main()
```

```python
# client
from socket import socket, AF_INET, SOCK_STREAM

client = socket(family=AF_INET, type=SOCK_STREAM)

client.connect(('127.0.0.1', 9696))

while True:
    msg = input('请输入要发送的消息：')
    if not msg:
        continue
    client.send(msg.encode('utf-8'))
    data_from_server = client.recv(1024)
    print(f"服务器返回的数据：{data_from_server.decode('utf-8')}")
```



# 10 主进程等待所有子进程结束而结束

```python
import random
import time
from multiprocessing import Process


def timer(func):
    def inner(*args, **kwargs):
        start_time = time.time()
        res = func(*args, **kwargs)
        end_time = time.time()
        print(f"函数 {func.__name__} 运行时间：{end_time - start_time} 秒")
        return res

    return inner


def work(name):
    sleep_time = random.randint(1, 4)
    print(f"{name} is starting sleeping {sleep_time}")
    time.sleep(sleep_time)
    print(f"{name} is ending sleeping {sleep_time}")


@timer
def process_work():
    task_list = [Process(
        target=work,
        # 按照位置传递参数
        # args=(f"Process-{i}",)
        # 按照关键字传递参数
        kwargs={"name": f"Process-{i}"}
    ) for i in range(5)]
    [task.start() for task in task_list]

    '''
    main process is starting 
    main process is ending 
    Process-1 is starting sleeping 3
    Process-0 is starting sleeping 4
    Process-4 is starting sleeping 2
    Process-3 is starting sleeping 4
    Process-2 is starting sleeping 2
    Process-4 is ending sleeping 2
    Process-2 is ending sleeping 2
    Process-1 is ending sleeping 3
    Process-0 is ending sleeping 4
    Process-3 is ending sleeping 4
    '''
    # 主进程不会等待所有子进程结束后再结束
    # 并行


@timer
def process_work_wait():
    task_list = [Process(
        target=work,
        # 按照位置传递参数
        # args=(f"Process-{i}",)
        # 按照关键字传递参数
        kwargs={"name": f"Process-{i}"}
    ) for i in range(5)]
    for task in task_list:
        task.start()
        task.join()
    '''
    main process is starting 
    Process-0 is starting sleeping 3
    Process-0 is ending sleeping 3
    Process-1 is starting sleeping 2
    Process-1 is ending sleeping 2
    Process-2 is starting sleeping 3
    Process-2 is ending sleeping 3
    Process-3 is starting sleeping 3
    Process-3 is ending sleeping 3
    Process-4 is starting sleeping 1
    Process-4 is ending sleeping 1
    main process is ending 
    '''
    # 主进程和子进程挨个执行，只有拿到上一次的结果后才会执行相爱一个进程
    # 串行
    # 最后的执行时间事所有子进程和主进程的时间综合


@timer
def process_work_wait_main():
    task_list = [Process(
        target=work,
        # 按照位置传递参数
        # args=(f"Process-{i}",)
        # 按照关键字传递参数
        kwargs={"name": f"Process-{i}"}
    ) for i in range(5)]
    [task.start() for task in task_list]  # 将当前所有的子进程的状态改为就绪态
    [task.join() for task in task_list]  # 整合所有子进程 进行进程间的调度

    # 并行就是所有进程同时执行 --> 结束时间取决于? 取决于时间最长的那个进程!
    # 时间应该是取决于？


if __name__ == '__main__':
    print(f"main process is starting ")
    # 只有 start 没有 join --- 并行
    # process_work()
    # 函数 process_work 运行时间：0.010191679000854492 秒

    # start 之后 直接 join -- 串行
    # process_work_wait()
    # 函数 process_work_wait 运行时间：15.083608150482178 秒

    # 全部 start 之后 再 挨个 join --- 并行
    process_work_wait_main()
    # 函数 process_work_wait_main 运行时间：4.562238931655884 秒

    print(f"main process is ending ")

'''
【6】为什么集体join变并行
● start()方法与join()方法调用顺序对程序执行流程影响的一种理解尝试。
（1）多进程 start() 后紧跟 join() 是“串行”行为的理解
● 在Python中，如果你在一个循环中创建了多个子进程，并且每个子进程创建后立即调用了join()方法，这实际上并不是典型的并行执行模式。
● 这里的“串行”是一种相对的说法，指的是逻辑上的执行流程给人的直观感受，并不是指CPU调度层面真的变为串行。

● 在这个例子中，每个子进程确实是在其前一个子进程结束（join()完成）后才开始。 # 同步
● 这是因为join()会阻塞主进程，直到相应的子进程执行完毕。
● 因此，尽管多个子进程可能在不同CPU核心上并行执行，但从主进程的角度来看，它们是按顺序等待的，形成了“串行”的效果。
（2）先start()后添加到列表中再挨个join()是“并行”行为的理解
● 如果改变策略，先启动所有子进程，然后在外部循环中逐个调用join()，情况就有所不同：

● 在这种情况下，所有子进程几乎同时启动（受到系统资源限制），然后每个进程并行执行。
● 当到达第一个join()循环时，主进程会等待每个子进程完成，但它并不阻止其他子进程的同时执行。
● 也就是说，虽然主进程自身仍然是串行等待每个子进程结束，但实际上所有子进程是并行执行的。
● 这种方式最大化了并行潜力，因为子进程之间的执行不依赖于彼此完成的顺序。
（3）结论
● “串行”和“并行”的概念在这里更多地是从程序逻辑控制流的角度来区分的。
● 直接在每个start()之后调用join()看起来像是串行，因为主进程的执行路径是按顺序等每个任务完成。 # 同步执行
● 而先全部start()再集体join()则更接近并行处理，因为所有子任务被同时发起，尽管最终的join()调用还是按照顺序进行。 # 异步执行
● 真正的并行性体现在子进程层面，它们可以在不同的处理器上同时运行，不受主进程控制流程的影响。
'''
```



# 11 子进程的其他属性

```python
import os
import random
import time
from multiprocessing import Process, current_process


def timer(func):
    def inner(*args, **kwargs):
        start_time = time.time()
        res = func(*args, **kwargs)
        end_time = time.time()
        print(f"函数 {func.__name__} 运行时间：{end_time - start_time} 秒")
        return res

    return inner


def work(name):
    sleep_time = random.randint(1, 4)
    # 查看当前子进程的 pid
    # print(f"{name} pid is {current_process().pid}")
    # 查看当前子进程的 pid
    print(f"{name} pid is {os.getpid()}")  # 每一个子进程的 pid 不一样
    # 查看当前父进程的 pid
    print(f"{name} pid is {os.getppid()}")  # 但是每一个子进程的父 pid 是一样的

    print(f"{name}  is starting sleeping {sleep_time}")

    time.sleep(sleep_time)
    print(f"{name} is ending sleeping {sleep_time}")


@timer
def process_work_wait_main():
    task_list = []
    for i in range(5):
        task = Process(
            target=work,
            # 按照位置传递参数
            # args=(f"Process-{i}",)
            # 按照关键字传递参数
            kwargs={"name": f"Process-{i}"}
        )
        # print(f"task :>>>> {task.pid}")
        task.start()  # 将当前所有的子进程的状态改为就绪态
        if i == 3:
            print(f"task :>>>> {task.pid} is alive {task.is_alive()}")
            # task :>>>> 29744 is alive True
            task.terminate()
            time.sleep(2)
            print(f"task :>>>> {task.pid} is alive {task.is_alive()}")
            # task :>>>> 29744 is alive False
        # print(f"task :>>>> {task.pid}")
        task_list.append(task)

    [task.join() for task in task_list]  # 整合所有子进程 进行进程间的调度


if __name__ == '__main__':
    print(f"main process is starting ")

    # 全部 start 之后 再 挨个 join --- 并行
    process_work_wait_main()
    # 函数 process_work_wait_main 运行时间：4.562238931655884 秒

    print(f"main process is ending ")
```