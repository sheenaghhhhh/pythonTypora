# 1 内容回顾

```python
# 【一】进程/程序/线程
# 程序是存在硬盘中的数据和代码
# 进程是程序的运行实例
# 线程是进程内的运行实例

# 每一个进程都会有一个默认的主线程

# 进程是资源分配的最小单位
# 线程是CPU调度的最小单位

# 【二】为何要开设线程
# 开设进程会频繁地开辟内存空间 分配资源 占用系统的资源并且耗时较高
# 开设线程是在进程的内部 进程已经开辟了内存空间 线程可以共享进程的内存空间

# 【三】多线程的创建方式
# 1.利用类得到线程对象然后跑线程对象
# 2.继承原本的类 重写类中的方法 run

# 和多进程的创建方式思路一致 其中方法和属性基本通用
"""
# 进程名字获取
from multiprocessing import current_process, Process
# 现成名字获取
from threading import current_thread, Thread


def work():
    ...


def process(cls):
    task_list = [
        cls(target=work, arg=()) for i in range(10)
    ]
    # 启动所有子进程/子线程
    # task_list = [task.start() for task in task_list]
    # 进程start返回的是None start生成的列表 是多个None的列表 这样下面就无法join 会出错
    # task_list = [task.join() for task in task_list]
    [task.start() for task in task_list]
    [task.join() for task in task_list]


class MyThread(Thread):
    def run(self):
        # 为什么要重写的是run
        # 线程.start 会启动run
        # 而我们执行run方法就行了
        ...


if __name__ == '__main__':
    process(cls=Process)
    process(cls=Thread)
"""

# 为什么查看线程存活数的时候明明只有一个启动的子线程但是存活数是2
# 进程默认有一个主线程存在

# 守护线程
# 互斥锁
# 互斥锁主要用来管理线程之间数据共享产生的访问错乱问题
# 在同一个进程下所有的子线程都会共享同一个进程下的数据 如果多个线程同时使用同一个数据就会产生问题
# 为了控制对共享资源的访问和修改于是就产生了互斥锁

# 1.进程间原本是数据隔离的
# 但是多个进程操作同一个内存数据的时候也容易产生错乱问题
# 以文件存储数据，多个进程对数据同时读写就容易产生数据错乱问题

# 2.加锁的时机
# 加了锁就等于将并行改为串行，原本高效的并行执行模式就变成了低效的串行执行模式
# 加锁要在数据发生改变之前进行加锁
# 要在数据改完之后释放当前锁
```



# 2 GLI全局解释器锁

```python
# 【一】什么是GIL锁
# 又叫全局解释器锁
"""
In CPython, the global interpreter lock, or GIL, is a mutex that prevents multiple
native threads from executing Python bytecodes at once. This lock is necessary mainly

because CPython’s memory management is not thread-safe. (However, since the GIL
exists, other features have grown to depend on the guarantees that it enforces.)

结论：在Cpython解释器中，同一个进程下开启的多线程，同一时刻只能有一个线程执行，无法利用多核优势
"""

import time
# 【二】全局解释器锁和普通锁有什么区别
from threading import Thread, Lock

money = 99


# 1.不加锁
def work_no_lock(i):
    global money
    temp = money
    time.sleep(0.1)
    money = temp - 1


def main_no_lock():
    print(f"修改前 :>>> {money}")
    task_list = [Thread(target=work_no_lock, args=(i,)) for i in range(100)]
    [task.start() for task in task_list]
    [task.join() for task in task_list]
    print(f"修改后 :>>> {money}")

    # 修改前 :>>> 99
    # 修改后 :>>> 98
    # 多线程在处理当前进程下的共享数据的时候应该是所有子线程共享同一个数据
    # 当一个子线程启动之后会去 抢 GIL 锁 进入到IO阻塞状态，自己的锁没有放开之前其他人都拿不到这个锁，改不了数据


# 2.加锁
def work_lock(i, lock):
    global money

    lock.acquire()
    temp = money
    time.sleep(0.1)
    money = temp - 1

    lock.release()


def main_lock():
    lock = Lock()
    print(f"修改前 :>>> {money}")
    task_list = [Thread(target=work_lock, args=(i, lock)) for i in range(100)]
    [task.start() for task in task_list]
    [task.join() for task in task_list]
    print(f"修改后 :>>> {money}")
    # 修改前 :>>> 99
    # 修改后 :>>> -1


# 3.加自动锁
lock = Lock()


def work_auto_lock():
    global money
    with lock:
        temp = money
        time.sleep(0.1)
        money = temp - 1


def main_auto_lock():
    print(f"修改前 :>>> {money}")
    task_list = [Thread(target=work_auto_lock, args=()) for i in range(100)]
    [task.start() for task in task_list]
    [task.join() for task in task_list]
    print(f"修改后 :>>> {money}")
    # 修改前 :>>> 99
    # 修改后 :>>> -1


if __name__ == '__main__':
    # main_no_lock()
    # main_lock()
    main_auto_lock()
```



# 3 GLI作用

```python
import os
import random
import time
from multiprocessing import Process
from threading import Thread


# 【一】 Cpython 解释器中 GIL
# ● 在 Cpython 解释器中 GIL 是一把互斥锁，用来阻止同一个进程下的多个线程的同时进行
#   ○ 同一个进程下的多个线程无法利用这一优势？
#   ○ Python的多线程是不是一点用都没有？
# ● 因为在 Cpython 中的内存管理不是线程安全的
#   ○ ps:内存管理（垃圾回收机制）
#     ■ 应用计数
#     ■ 标记清除
#     ■ 分代回收
# 【二】Python的多线程是不是一点用都没有？
# ● 同一个进程下的多线程无法利用多核优势，是不是就没用了
# ● 多线程是否有用要看情况
#   ○ 单核
#     ■ 四个任务（IO密集型/计算密集型）
#   ○ 多核
#     ■ 四个任务（IO密集型/计算密集型）
# （1）计算密集型
# 一直处在计算运行中
# ● 每个任务都需要 10s
#   ○ 单核
#     ■ 多进程：额外消耗资源
#     ■ 多线程：减少开销
#   ○ 多核
#     ■ 多进程：总耗时 10s
#     ■ 多线程：总耗时 40s+


def calculation(n=1000):
    result = 1
    for i in range(1, n + 1):
        result *= i
    # 为了节约内存 使用计算器
    # _ = sum(int(digit) for digit in str(result))
    print('计算结束,结果为 :>>>> ', result)


def io_condition():
    print(f"start sleeping ")
    time.sleep(random.randint(1, 3))
    print(f"end sleeping ")


def timer(func):
    def inner(*args, **kwargs):
        start_time = time.time()

        res = func(*args, **kwargs)

        end_time = time.time()
        print(f'函数 {func.__name__} 运行时间：{end_time - start_time}')
        return res

    return inner


@timer
def process_calculate(cls):
    # 获取当前正在运行的CPU个数
    print(f"当前正在运行的CPU是 :>>>> {os.cpu_count()}")
    task_list = [cls(target=calculation, args=(1000,)) for i in range(400)]
    [task.start() for task in task_list]
    [task.join() for task in task_list]


@timer
def process_io(cls):
    # 获取当前正在运行的CPU个数
    print(f"当前正在运行的CPU是 :>>>> {os.cpu_count()}")
    task_list = [cls(target=io_condition, args=()) for i in range(400)]
    [task.start() for task in task_list]
    [task.join() for task in task_list]


def condition_one():
    # 多进程下的耗时
    # process_calculate(cls=Process)
    # 函数 process 运行时间：16.424844980239868

    # 多线程下的耗时
    process_calculate(cls=Thread)
    # 函数 process 运行时间：0.20564889907836914


def condition_two():
    # 多进程下的耗时
    # process_io(cls=Process)
    # 函数 process_io 运行时间：19.173663854599

    # 多线程下的耗时
    process_io(cls=Thread)
    # 函数 process_io 运行时间：3.0549049377441406


if __name__ == '__main__':
    condition_one()
    # condition_two()


# 1.计算密集型任务(多进程)
# ● 计算密集型任务主要是指需要大量的CPU计算资源的任务，其中包括执行代码、进行算术运算、循环等。
#   ○ 在这种情况下，使用多线程并没有太大的优势。
#   ○ 由于Python具有全局解释器锁（Global Interpreter Lock，GIL），在同一时刻只能有一条线程执行代码，这意味着在多线程的情况下，同一时刻只有一个线程在执行计算密集型任务。
# ● 但是，如果使用多进程，则可以充分利用多核CPU的优势。
#   ○ 每个进程都有自己独立的GIL锁，因此多个进程可以同时执行计算密集型任务，充分发挥多核CPU的能力。
#   ○ 通过开启多个进程，我们可以将计算密集的任务分配给每个进程，让每个进程都独自执行任务，从而提高整体的计算效率。
# 2.IO密集型任务(多线程)
# ● IO密集型任务主要是指涉及大量输入输出操作（如打开文件、写入文件、网络操作等）的任务。
#   ○ 在这种情况下，线程往往会因为等待IO操作而释放CPU执行权限，不会造成太多的CPU资源浪费。
#   ○ 因此，使用多线程能够更好地处理IO密集型任务，避免了频繁切换进程的开销。
# ● 当我们在一个进程中开启多个IO密集型线程时，大部分线程都处于等待状态，开启多个进程却不能提高效率，反而会消耗更多的系统资源。
#   ○ 因此，在IO密集型任务中，使用多线程即可满足需求，无需开启多个进程。
# 3.总结
# ● 计算密集型任务使用多进程可以充分利用多核CPU的优势，而IO密集型任务使用多线程能够更好地处理IO操作，避免频繁的进程切换开销。
#   ○ 根据任务的特性选择合适的并发方式可以有效提高任务的执行效率。
```



# 4 死锁和递归锁

## 4.1 死锁

```python
# 【一】死锁
# 1.介绍
# ● 死锁是指两个或多个进程，在执行过程中，因争夺资源而造成了互相等待的一种现象。
# ● 即两个或多个进程持有各自的锁并试图获取对方持有的锁，从而导致被阻塞，不能向前执行，最终形成僵局。
# ● 在这种情况下，系统资源利用率极低，系统处于一种死循环状态。
# 2.例子
# ● 要吃饭，必须具备盘子和筷子
# ● 但是一个人拿着盘子等筷子。
# ● 另一个人拿着筷子等盘子
# 3.解决办法
# ● 锁不要有多个，一个足够
# ● 如果真的发生了死锁的问题，必须迫使一方先交出锁

import random
import time
from threading import Thread, Lock

lock_a = Lock()
lock_b = Lock()


class MyThread(Thread):
    def run(self):
        # 执行我的子线程函数
        self.func_one()
        self.func_two()

    def func_one(self):
        # 先拿锁A
        lock_a.acquire()
        print(f"{self.name} 拿到了锁A")
        # 然后拿锁B
        lock_b.acquire()
        print(f"{self.name} 拿到了锁B")
        # 先释放锁B
        lock_b.release()
        print(f"{self.name} 释放了锁B")
        # 再先释放锁A
        lock_a.release()
        print(f"{self.name} 释放了锁A")

    def func_two(self):
        # 先拿锁 B
        lock_b.acquire()
        sleep_time = random.randint(1, 3)
        print(f"{self.name} 拿到了锁B 开始睡觉 :>>> {sleep_time}")
        # 模拟阻塞 睡一会
        time.sleep(sleep_time)
        print(f"{self.name} 睡觉结束 :>>> {sleep_time}")
        # 拿锁A
        lock_a.acquire()
        print(f"{self.name} 拿到了锁A")
        # 释放锁A
        lock_a.release()
        print(f"{self.name} 释放了锁A")
        # 释放锁B
        lock_b.release()
        print(f"{self.name} 释放了锁B")


def main():
    task_list = [MyThread() for i in range(5)]
    [task.start() for task in task_list]

    '''
    Thread-1 拿到了锁A
    Thread-1 拿到了锁B
    Thread-1 释放了锁B
    Thread-1 释放了锁A
    Thread-1 拿到了锁B 开始睡觉 :>>> 2
    Thread-2 拿到了锁A
    Thread-1 睡觉结束 :>>> 2
    '''


if __name__ == '__main__':
    main()
```



## 4.2 递归锁

```python
# 【二】递归锁
# 1.介绍
# ● 递归锁（也叫可重入锁）是一种特殊的锁，它允许一个线程多次请求同一个锁，称为“递归地”请求锁
# ● 在该线程释放锁之前，会对锁计数器进行累加操作，线程每成功获得一次锁时，都要进行相应的解锁操作，直到锁计数器清零才能完全释放该锁。
# ● 递归锁能够保证同一线程在持有锁时能够再次获取该锁，而不被自己所持有的锁所阻塞，从而避免死锁的发生。
# ● 但是注意要正常使用递归锁，避免过多地获取锁导致性能下降。
# 2.示例
# ● 可以被连续的 acquire 和 release
# ● 但是只能被第一个抢到这把锁上执行上述操作
# ● 他的内部有一个计数器，每acquire一次计数 +1 每release一次 计数-1
# ● 只要计数不为0，那么其他人都无法抢到该锁

import random
import time
from threading import Thread, Lock, RLock

# 就相当于前面说的解决方案 使用同一把锁
lock_a = lock_b = RLock()


class MyThread(Thread):
    def run(self):
        # 执行我的子线程函数
        self.func_one()
        self.func_two()

    def func_one(self):
        # 先拿锁A
        lock_a.acquire()  # 锁的计数器 + 1 = 1
        print(f"{self.name} 拿到了锁A")
        # 然后拿锁B
        lock_b.acquire()  # 锁的计数器 + 1 = 2
        print(f"{self.name} 拿到了锁B")
        # 先释放锁B
        lock_b.release()  # 锁的计数器 - 1 = 1
        print(f"{self.name} 释放了锁B")
        # 再先释放锁A
        lock_a.release()  # 锁的计数器 - 1 = 0
        print(f"{self.name} 释放了锁A")

    def func_two(self):
        # 先拿锁 B
        lock_b.acquire()
        sleep_time = random.randint(1, 3)
        print(f"{self.name} 拿到了锁B 开始睡觉 :>>> {sleep_time}")
        # 模拟阻塞 睡一会
        time.sleep(sleep_time)
        print(f"{self.name} 睡觉结束 :>>> {sleep_time}")
        # 拿锁A
        lock_a.acquire()
        print(f"{self.name} 拿到了锁A")
        # 释放锁A
        lock_a.release()
        print(f"{self.name} 释放了锁A")
        # 释放锁B
        lock_b.release()
        print(f"{self.name} 释放了锁B")


def main():
    task_list = [MyThread() for i in range(5)]
    [task.start() for task in task_list]


if __name__ == '__main__':
    main()
```



# 5 信号量

```python
# 【一】信号量(了解)
# ● 信号量Semahpore(同线程一样)
# 1.引入
# ● 互斥锁 同时只允许一个线程更改数据，而Semaphore是同时允许一定数量的线程更改数据
# 2.例子
# ● 比如厕所有3个坑，那最多只允许3个人上厕所，后面的人只能等里面有人出来了才能再进去
# ● Lock 锁住一个马桶，同时只能有一个人
# ● Semaphore 锁住的是公共厕所，同时可以来一堆人
# 3.示例
# ● 如果指定信号量为3，那么来一个人获得一把锁，计数加1，当计数等于3时，后面的人均需要等待。
# ● 一旦释放，就有人可以获得一把锁
# 信号量与进程池的概念很像，但是要区分开，信号量涉及到加锁的概念

import os
import random
import time
from threading import Thread, Semaphore as Thread_Semaphore
from multiprocessing import Process, Semaphore as Process_Semaphore


def go_wc(sem, user):
    # 进来占坑 加锁
    sem.acquire()
    sleep_time = random.randint(1, 3)

    print(f"{user} 占用一个坑 开始使用 :>>>> {sleep_time}")

    # 模拟延迟 当前这个人正在阻塞
    time.sleep(sleep_time)

    print(f"{user} 结束一个坑")

    # 使用完离开 要把锁打开
    sem.release()


def process(cls, sem):
    # 获取当前正在运行的CPU个数
    print(f"当前正在运行的CPU是 :>>>> {os.cpu_count()}")
    task_list = [cls(target=go_wc, args=(sem, f"用户 {i}")) for i in range(30)]
    [task.start() for task in task_list]
    [task.join() for task in task_list]

    '''
    用户 10 占用一个坑 开始使用 :>>>> 3
    用户 22 占用一个坑 开始使用 :>>>> 3
    用户 4 占用一个坑 开始使用 :>>>> 2

    用户 4 结束一个坑
    用户 16 占用一个坑 开始使用 :>>>> 2

    用户 10 结束一个坑
    用户 7 占用一个坑 开始使用 :>>>> 3
    '''


if __name__ == '__main__':
    # process_sem = Process_Semaphore(3)
    # process(cls=Process, sem=process_sem)

    thread_sem = Thread_Semaphore(3)
    process(cls=Thread, sem=thread_sem)

    '''
    用户 0 占用一个坑 开始使用 :>>>> 2
    用户 1 占用一个坑 开始使用 :>>>> 1
    用户 2 占用一个坑 开始使用 :>>>> 3

    用户 1 结束一个坑
    用户 3 占用一个坑 开始使用 :>>>> 1
    '''
```



# 6 事件(了解)

```python
# 1.什么是事件
# ● Event（同线程一样）
# 2.事件处理方法
# ● python线程的事件用于主线程控制其他线程的执行，事件主要提供了三个方法 set、wait、clear。
# ● 事件处理的机制：
#   ○ 全局定义了一个“Flag”
#   ○ 如果“Flag”值为 False，那么当程序执行 event.wait 方法时就会阻塞
#   ○ 如果“Flag”值为True，那么event.wait 方法时便不再阻塞。
#   ○ clear：
#     ■ 将“Flag”设置为False
#   ○ set：
#     ■ 将“Flag”设置为True


from multiprocessing import Process, Event, Manager, set_start_method
import time
import random

color_red = '\033[31m'  #
color_reset = '\033[0m'  #
color_green = '\033[32m'  #


# ● 在Python中，可以使用ANSI转义码来设置打印到控制台的文本颜色。
# ● \033[是一个开始 ANSI 序列的转义字符，之后的数字和字母组合定义了具体的样式改变。
# ● 以下是对\033[设置]的一个详细解释及示例：

# ● 前景色：30-37 代表不同的颜色，加上40可以使颜色变为亮色系。
#   ○ 30: 黑色
#   ○ 31: 红色
#   ○ 32: 绿色
#   ○ 33: 黄色
#   ○ 34: 蓝色
#   ○ 35: 紫红色（洋红）
#   ○ 36: 青蓝色（青色）
#   ○ 37: 白色


# 定义车辆行为函数，它会根据事件对象(event)的状态来决定是否等待或通过路口
def car(event, n):
    """
    模拟汽车行为
    :param event: 事件对象，用于同步操作，红绿灯状态的标识
    :param n: 第几辆车的标识
    :return: 无返回值
    """

    # 创建一个无限循环直到车辆离开
    while True:
        # 如果事件未设置，表示红灯 False
        if not event.is_set():
            # 红灯亮时，车辆等待，并输出提示信息
            print(f'{color_red}红灯亮{color_reset}，car{n} :>>> 正在等待 ... ')
            # 阻塞当前进程，等待event被设置（绿灯）
            event.wait()
            # 当event被设置后，打印绿灯信息
            print(f'{color_green}车 {n} :>>>> 看见绿灯亮了{color_reset}')
            # 模拟通过路口需要的时间
            time.sleep(random.randint(3, 6))
            # 防止在sleep期间event状态改变，再次检查状态
            if not event.is_set():
                continue
            # 通过路口
            print(f'car {n} :>>>> 安全通行')
            # 退出循环
            break


# 定义 警车 行为函数， 警车 在红灯时等待一秒后直接通过
def police_car(event, n):
    """
    模拟 警车 行为
    :param event: 事件对象，用于同步操作，红绿灯状态的标识
    :param n: 第几辆车的标识
    :return: 无返回值
    """
    while True:
        # 判断是否为红灯 False
        if not event.is_set():
            print(f'{color_red}红灯亮 {event.is_set()} {color_reset}，car{n} :>>> 正在等待 ... ')
            #  等待1秒，不完全遵守交通规则
            event.wait(1)
            print(f'灯的是{event.is_set()}，警车走了,car{n}')
            # 通过后立即结束循环
            break


# 定义交通灯控制函数，周期性切换红绿灯状态
def traffic_lights(event, interval):
    """
    模拟 交通灯 行为
    :param event: 事件对象，用于同步操作，红绿灯状态的标识
    :param interval: 间隔（比如10秒）改变信号灯
    :return: 无返回值
    """
    # 无限循环，持续控制交通灯状态
    while True:
        # 按照给定间隔（比如10秒）改变信号灯
        time.sleep(interval)
        # 如果当前是绿灯
        if event.is_set():
            # 切换到红灯状态
            # event.is_set() ---->False
            event.clear()
        else:
            # 如果当前是红灯，则切换到绿灯状态
            # event.is_set() ----> True
            event.set()


def main():
    # 打印分割线，表明程序运行开始
    print(' ------------交通开始------------- ')
    set_start_method("spawn")
    # win里面是spawn

    # 初始化事件对象，初始状态为清除（即红灯）
    event = Event()
    # 使用Manager创建一个可跨进程共享的Event对象
    # manager = Manager()
    # event = manager.Event()

    # 创建并启动多个 警车 进程
    # police_car_list = [Process(target=police_car, args=(event, i)) for i in range(5)]
    # [task.start() for task in police_car_list]

    police_car_list = [Process(target=car, args=(event, i)) for i in range(5)]
    [task.start() for task in police_car_list]
    # 启动交通灯控制进程
    # 交通灯变化周期为10秒
    traffic_lights_process = Process(target=traffic_lights, args=(event, 10))
    traffic_lights_process.start()


if __name__ == '__main__':
    main()
    # ------------交通开始-------------
    # 红灯亮，car0 :>>> 正在等待 ...
    # 红灯亮，car1 :>>> 正在等待 ...
    # 红灯亮，car3 :>>> 正在等待 ...
    # 红灯亮，car4 :>>> 正在等待 ...
    # 红灯亮，car2 :>>> 正在等待 ...
    # 车 0 :>>>> 看见绿灯亮了
    # 车 3 :>>>> 看见绿灯亮了
    # 车 1 :>>>> 看见绿灯亮了
    # 车 4 :>>>> 看见绿灯亮了
    # 车 2 :>>>> 看见绿灯亮了
    # car 4 :>>>> 安全通行
    # car 1 :>>>> 安全通行
    # car 0 :>>>> 安全通行
    # car 3 :>>>> 安全通行
    # car 2 :>>>> 安全通行

    # print("\033[31m这是红色文本\033[0m")  # 红色前景色
    # print("\033[32m这是绿色文本\033[0m")  # 绿色前景色
    # print("\033[1;33m这是加粗的黄色文本\033[0m")  # 加粗并设置黄色前景色
    # print("\033[44m这是蓝色背景文本\033[0m")  # 蓝色背景色
    # print("\033[34;47m这是蓝色前景色和白色背景色的文本\033[0m")  # 蓝色前景和白色背景
```



# 7 进程池和线程池

```python
import random
import time
# 开辟一个池子 在一个池子内只能容纳指定个数的进程和线程启动
# 跟信号量其实差不多

# 【二】构造 进程池 ProcessPoolExecutor/ 线程池 ThreadPoolExecutor
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

# 1.创建一个池子
pool = ThreadPoolExecutor(max_workers=5)


# 2.使用
def work(n, d):
    print(f"{n, d} 进来了")
    time.sleep(random.randint(1, 3))
    print(f"{n, d} 走了")

    return n + d


def push_task():
    for i in range(5):
        # 提交任务 后会获取到一个返回值 这个返回值是一个 Future 对象
        res = pool.submit(work, i, i * i)
        print(f"{i} :>>>> {res}")
        # <Future at 0x11fa4d7e0 state=running>
        # <Future at 0x11fa4e680 state=pending>

        # 通过 Future 对象 的 result() 方法可以将 子进程返回的结果拿回来
        print(f"{i} :>>>> 结果是 {res.result()}")


def push_task_one():
    print(f"main process start --- ")
    task_list = [pool.submit(work, i, i * i) for i in range(5)]
    print(task_list)
    # [<Future at 0x1211c5b40 state=running>, <Future at 0x1211c6890 state=running>, <Future at 0x1211c6a40 state=pending>, <Future at 0x1211c6680 state=pending>, <Future at 0x1211c6da0 state=pending>]

    # 在运行之后获取每一个子进程的结果
    result_list = [task.result() for task in task_list]
    print(result_list)
    # [0, 2, 6, 12, 20]

    print(f"main process end --- ")


def push_task_two():
    print(f"main process start --- ")
    task_list = [pool.submit(work, i, i * i) for i in range(5)]
    print(task_list)
    # [<Future at 0x1211c5b40 state=running>, <Future at 0x1211c6890 state=running>, <Future at 0x1211c6a40 state=pending>, <Future at 0x1211c6680 state=pending>, <Future at 0x1211c6da0 state=pending>]

    # 在运行之后获取每一个子进程的结果

    pool.shutdown()
    # wait=True 等价于 我们 在多进程中用到 join 主进程等待所有子进程后再结束

    result_list = [task.result() for task in task_list]
    print(result_list)
    # [0, 2, 6, 12, 20]

    print(f"main process end --- ")


def callback(future_obj):
    print(f"callback res :>>> {future_obj.result()}")


def work_back(n, d):
    print(f"{n, d} 进来了")
    time.sleep(random.randint(1, 3))
    print(f"{n, d} 走了")

    return n + d


def push_task_three():
    print(f"main process start --- ")
    # 给当前的子线程添加一个回调函数 add_done_callback(函数名字)
    task_list = []
    for i in range(5):
        res = pool.submit(work_back, i, i * i).add_done_callback(callback)
        print(f" {i} res :>>> {res}")

    print(f"main process end --- ")


def main():
    # 1.创建进程池 / 线程池 并指定池子的数量
    pool = ThreadPoolExecutor(5)

    # 2.书写自己的 异步函数
    def work(*args, **kwargs):
        ...
        # 异步函数逻辑

    # 3.写一个异步回调函数用来处理子线程函数的结果
    def callback(future_obj):
        result = future_obj.result()
        # 对结果进行处理

    # 4.提交线程任务 / 进程任务
    pool.submit(work).add_done_callback(callback)

    # 5.阻塞 等待所有子进程结束后结束
    pool.shutdown()


if __name__ == '__main__':
    push_task_three()
```



# 8 多进程和多线程的爬虫案例

```python
import json
import os.path
import time
from multiprocessing import Process
from threading import Thread

import requests
from lxml import etree


class Spider(object):
    def __init__(self):
        self.area = "https://pic.netbian.com"
        self.headers = {
            "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36"
        }
        self.count = 0
        if not os.path.exists(os.path.join(os.path.dirname(__file__), "img_data.json")):
            self.save_img_data(data=self.spider_img_data())
        else:
            pass

    def create_target_url(self):
        # https://pic.netbian.com/4kdongman/index_1.html # 访问失败
        # 第一页的地址 ： https://pic.netbian.com/4kdongman/
        # 第二页的地址 ： https://pic.netbian.com/4kdongman/index_2.html
        # 第二页的地址 ： https://pic.netbian.com/4kdongman/index_3.html
        target_url_list = []
        for i in range(1, 6):
            if i == 1:
                target_url = self.area + "/4kdongman/"
                target_url_list.append(target_url)
            else:
                target_url = self.area + f"/4kdongman/index_{str(i)}.html"
                target_url_list.append(target_url)
        return target_url_list

    def get_tree(self, target_url):
        response = requests.get(url=target_url, headers=self.headers)
        response.encoding = 'gbk'
        # 将源码转为字符串类型的数据
        tree = etree.HTML(response.text)
        return tree

    def spider_img_detail_data(self, target_url):
        tree = self.get_tree(target_url=target_url)
        # //*[@id="img"]/img
        img_url = self.area + tree.xpath('//*[@id="img"]/img/@src')[0]
        img_title = tree.xpath('//*[@id="img"]/img/@title')[0]
        return img_title[0:len(img_title) // 2], img_url

    def spider_page_text(self, target_url):
        tree = self.get_tree(target_url=target_url)
        # 获取到每一张图片的总列表
        li_list = tree.xpath('//*[@id="main"]/div[3]/ul/li')
        img_data = {}
        for li in li_list:
            # //*[@id="main"]/div[3]/ul/li[13]/a/img
            # ./a/img/@src
            detail_url = self.area + li.xpath('./a/@href')[0]
            img_title, img_url = self.spider_img_detail_data(target_url=detail_url)
            img_data[img_title] = img_url
        return img_data

    def save_img_data(self, data):
        with open(os.path.join(os.path.dirname(__file__), "img_data.json"), "w") as fp:
            json.dump(obj=data, fp=fp)

    def read_data(self):
        with open(os.path.join(os.path.dirname(__file__), "img_data.json"), "r") as fp:
            data = json.load(fp)
        return data

    def spider_img_data(self):
        img_data = {}
        target_url_list = self.create_target_url()
        for target_url in target_url_list:
            # 抓取每一页的图片数据 更新到大字典中
            print(f"当前正在抓取 :>>>> {target_url}")
            img_data.update(self.spider_page_text(target_url=target_url))
        return img_data

    def download_img(self, img_title, img_url):
        self.count += 1
        img_dir = os.path.join(os.path.dirname(__file__), "img")
        os.makedirs(img_dir, exist_ok=True)
        # 从网络中获取图片的二进制数据
        response = requests.get(url=img_url, headers=self.headers)
        # 将响应数据转为二进制类型的数据 =-> 只能针对二进制数据的链接
        img_data = response.content
        # 先写文件名
        img_path = os.path.join(img_dir, f"{img_title}.png")
        # 保存数据
        with open(img_path, mode="wb") as fp:
            fp.write(img_data)
        print(f"第 {self.count} 张图片 :>>>> {img_title} 下载完成")

    @staticmethod
    def timer(func):
        def inner(*args, **kwargs):
            start_time = time.time()
            res = func(*args, **kwargs)
            end_time = time.time()
            print(f"耗时 :>>>> {end_time - start_time}")
            return res

        return inner

    @timer
    def download_normal(self):
        img_data = self.read_data()
        for img_title, img_url in img_data.items():
            self.download_img(img_title, img_url)

        # 耗时 :>>>> 34.61733078956604

    @timer
    def download_process(self, cls):
        img_data = self.read_data()
        task_list = [cls(target=self.download_img, args=(img_title, img_url)) for img_title, img_url in
                     img_data.items()]
        [task.start() for task in task_list]
        [task.join() for task in task_list]

    def download_process_process(self):
        self.download_process(cls=Process)
        # 耗时 :>>>> 6.42774224281311

    def download_process_thread(self):
        self.download_process(cls=Thread)
        # 耗时 :>>>> 5.637322902679443


if __name__ == '__main__':
    spider = Spider()
    spider.download_normal()
    # spider.download_process_process()
    # spider.download_process_thread()

    # spider = Spider()
    # target_url = 'https://pic.netbian.com/4kdongman/'
    # res = spider.download_img(img_title="1",
    #                           img_url="https://pic.netbian.com/uploads/allimg/240807/080123-17229888838040.jpg")
    # print(res, len(res))
    # ['https://pic.netbian.com/4kdongman/',
    # 'https://pic.netbian.com/4kdongman/index_2.html',
    # 'https://pic.netbian.com/4kdongman/index_3.html',
    # 'https://pic.netbian.com/4kdongman/index_4.html',
    # 'https://pic.netbian.com/4kdongman/index_5.html']
```