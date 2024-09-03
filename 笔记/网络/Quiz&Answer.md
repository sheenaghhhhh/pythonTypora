## 1Quiz：

1. 什么是编程，什么是网络编程
2. 什么是Mac地址，什么是广播，什么是ethernet
3. 常见的网络协议有哪些
4. osi有哪几层，tcp有哪几层
5. 什么是端口，什么是进程号
6. 如何确定世界上唯一的一台计算机





## 1Answer：

1. 将人类语言转化成计算机可以识别的方式

   编程语言在计算机之间通信的一种方式

2.  Mac地址：每块网卡在被出厂的时候烧制的唯一一串Mac地址，用12位16进制数进行表示，前6位是厂商编号 后6位是流水线号

   广播：广播是一种将消息发送给同一网络中所有设备的方式，所有设备都会收到并决定是否处理该消息

   ethernet：以太网协议。是大家处在同一个局域网的分组方式标准。

3. 常见的网络协议

   TCP/IP协议

   HTTP协议

   FTP协议

4. 七层： 

   应用层 表示层 会话层 传输层 网络层 数据链路层 物理层

   五层：

   应用层 传输层 网络层 数据链路层 物理层

5. 192.168.2.11:1001

   这个1001就是端口 区分同一台设备上不同的程序或服务

   进程号 区分同一台设备之间不同的应用程序

6. 通过TCP协议可以知道 IP地址 用来表示唯一主机

   ip+端口 区分唯一程序

   ip+mac地址 区分唯一主机





## 2Quiz：

1. 什么是粘包问题，解决思路，复现代码
2. tcp服务端客户端的交互过程
3. udp服务端和客户端的交互过程
4. 什么是tcp协议
5. 什么是udp协议，二者有什么区别





## 2Answer:

1. 粘包在TCP协议中会出现

   因为一般服务端返回客户端数据的时候 每次有接受的数据大小限制 比如1024个字节 而有些数据大于这个大小 没有接收完的数据会随着下一次客户端的输入发送过来

   解决思路

   1. 把本来的大小暴力提升 希望发送过来的数据不要比这个值大
   2. 让客户端知道整个数据的大小 先知道大小 在进行接收

   复现代码

   ```python
   # 用 struct 模块将上面的 字节总数 打包成四个字节的二进制数据
               pack_data = struct.pack('i', len(file_data_dict_bytes))
               # 再去发送数据
               # 发送字典转成的二进制数据打包后的长度
               client_socket.send(pack_data)
               # 发送字典的二进制数据
               client_socket.send(file_data_dict_bytes)
               # 发送当前的文件数据
               client_socket.send(file_data)
               
   
               
    # 最开始接收到的数据一定是四个字节的二进制数据
       pack_data = client.recv(4)
       # 让 struct 模块将打包号的四个字节的二进制数据解包
       print(pack_data)
       json_data_length_all = struct.unpack("i", pack_data)[0]
       # 接收到的结果应该是当前字典转为json字符串后再转为二进制数据的长度
       file_data_dict = json.loads(client.recv(json_data_length_all))
       # 提取出当前文件数据的总长度
       data_length_all = file_data_dict.get('data_all')
       print(f"data_length_all :>>>> {data_length_all}")
       count_data = 0
       data = b""
       buffer_size = 1024
       while count_data < data_length_all:
           # 每次提取固定大小的数据 拼接到总数据中
           data += client.recv(buffer_size)
           # 计算已经提取了多少数据
           count_data += buffer_size
       # 计算当前总数据的哈希值
       md5 = hashlib.md5()
       md5.update(data)
   ```

   

2. tcp服务端客户端的交互过程

   ```python
   import socket
   
   # 【1】服务端
   # 1.创建socket对象
   server = socket.socket(family=socket.AF_INET, type=socket.SOCK_STREAM, proto=0)
   
   
   # 2.绑定地址和端口
   addr = "127.0.0.1" 
   prot = 9696
   
   server.bind((addr, prot))
   
   
   # 3.监听连接请求
   server.listen(5)
   
   
   # 4.接受连接
   client_socket, client_addr = server.accept()
   print(f"client_socket :>>>> {client_socket}")
   print(f"client_addr :>>>> {client_addr}")
   
   # 5.接收数据
   data_from_client = client_socket.recv(1024)
   print(f"data_from_client :>>>> {data_from_client.decode()}")
   
   # 6.发送数据
   data_to_client = f"你好!我是服务端!{client_addr}"
   client_socket.send(data_to_client.encode("utf-8"))
   
   # 7.关闭连接
   client_socket.close()
   server.close()
   
   
   
   
   
   # 借助第三方模块
   import socket
   
   # 【2】客户端
   # 1.创建socket对象
   client = socket.socket()
   
   # 2.连接到服务器：connect() 方法将客户端 socket 连接到指定的服务器地址和端口（127.0.0.1:9696）。这相当于“拨号”到服务器。
   addr = "127.0.0.1"
   prot = 9696
   client.connect((addr, prot))
   
   # 3.发送数据：客户端通过 send() 方法将数据发送给服务器。数据在发送前需要编码为字节形式。
   data_to_server = f"你好!我是客户端!"
   client.send(data_to_server.encode("utf-8"))
   
   # 4.接收数据：recv(1024) 方法从服务器接收数据。
   data_from_server = client.recv(1024)
   
   # 5.关闭连接：通信完成后，客户端关闭与服务器的连接。
   client.close()
   ```
   
3. UDP服务端客户端的交互过程

   ```python
   # 借助第三方的模块
   from socket import socket, AF_INET, SOCK_DGRAM
   
   # 创建 服务端对象
   server = socket(family=AF_INET, type=SOCK_DGRAM)
   
   # 监听IP 和 端口
   addr = "127.0.0.1"
   prot = 9696
   server.bind((addr, prot))
   data_from_client, client_address = server.recvfrom(1024)
   # TCP要使用accept UDP通过recvfrom和sendto就可以了
   
   data_to_client = data_from_client.decode().upper()
   server.sendto(data_to_client.encode("utf-8"), client_address)
   
   
   
   
   # 借助第三方模块
   from socket import socket, AF_INET, SOCK_DGRAM
   
   client = socket(family=AF_INET, type=SOCK_DGRAM)
   addr = "127.0.0.1"
   prot = 9696
   data_to_server = input("请输入给服务端的数据 :>>>> ").strip()
   client.sendto(data_to_server.encode("utf-8"), (addr, prot))
   data_from_server = client.recvfrom(1024)
   ```

   

4. 可靠传输协议 流式协议

5. 不可靠传输协议 报式协议

   UDP的数据包之间没有关联 独立传输 没有顺序 每次发送的数据只发送一次；TCP是封装成包进行传输的 包和包之间相互独立 可以有序发送





## 3Quiz：

1. 什么是闭包函数
2. 什么是进程，什么是程序
3. 什么是粘包问题，解决思路，实现方式
4. 什么是Tcp协议，如何实现
5. 什么是udp协议，如何实现
6. 什么是面向对象，什么是面向过程
7. 你都知道哪些魔法方法？都有什么作用？
8. 简述三次握手和四次挥手的流程





## 3Answer：

1. 闭包函数：内嵌函数对外部作用域有引用的函数

2. 进程：运行起来的硬盘中的数据

   程序：堆在硬盘中的数据

3. 粘包问题

   因为一般服务端返回客户端数据的时候 每次有接受的数据大小限制 比如1024个字节 而有些数据大于这个大小 没有接收完的数据会随着下一次客户端的输入发送过来

   解决思路

   1. 把本来的大小暴力提升 希望发送过来的数据不要比这个值大
   2. 让客户端知道整个数据的大小 先知道大小 在进行接收

   复现代码

4. TCP协议

   可靠传输协议 流式套接字

   ```python
   import socket
   
   # 【1】服务端
   # 1.创建socket对象
   server = socket.socket(family=socket.AF_INET, type=socket.SOCK_STREAM, proto=0)
   
   
   # 2.绑定地址和端口
   addr = "127.0.0.1" 
   prot = 9696
   
   server.bind((addr, prot))
   
   
   # 3.监听连接请求
   server.listen(5)
   
   
   # 4.接受连接
   client_socket, client_addr = server.accept()
   print(f"client_socket :>>>> {client_socket}")
   print(f"client_addr :>>>> {client_addr}")
   
   # 5.接收数据
   data_from_client = client_socket.recv(1024)
   print(f"data_from_client :>>>> {data_from_client.decode()}")
   
   # 6.发送数据
   data_to_client = f"你好!我是服务端!{client_addr}"
   client_socket.send(data_to_client.encode("utf-8"))
   
   # 7.关闭连接
   client_socket.close()
   server.close()
   
   
   # 借助第三方模块
   import socket
   
   # 【2】客户端
   # 1.创建socket对象
   client = socket.socket()
   
   # 2.连接到服务器：connect() 方法将客户端 socket 连接到指定的服务器地址和端口（127.0.0.1:9696）。这相当于“拨号”到服务器。
   addr = "127.0.0.1"
   prot = 9696
   client.connect((addr, prot))
   
   # 3.发送数据：客户端通过 send() 方法将数据发送给服务器。数据在发送前需要编码为字节形式。
   data_to_server = f"你好!我是客户端!"
   client.send(data_to_server.encode("utf-8"))
   
   # 4.接收数据：recv(1024) 方法从服务器接收数据。
   data_from_server = client.recv(1024)
   
   # 5.关闭连接：通信完成后，客户端关闭与服务器的连接。
   client.close()
   ```

5. UDP协议

   不可靠传输协议 报式套接字

   ```python
   # 借助第三方的模块
   from socket import socket, AF_INET, SOCK_DGRAM
   
   # 创建 服务端对象
   server = socket(family=AF_INET, type=SOCK_DGRAM)
   
   # 监听IP 和 端口
   addr = "127.0.0.1"
   prot = 9696
   server.bind((addr, prot))
   data_from_client, client_address = server.recvfrom(1024)
   # TCP要使用accept UDP通过recvfrom和sendto就可以了
   
   data_to_client = data_from_client.decode().upper()
   server.sendto(data_to_client.encode("utf-8"), client_address)
   
   
   # 借助第三方模块
   from socket import socket, AF_INET, SOCK_DGRAM
   
   client = socket(family=AF_INET, type=SOCK_DGRAM)
   addr = "127.0.0.1"
   prot = 9696
   data_to_server = input("请输入给服务端的数据 :>>>> ").strip()
   client.sendto(data_to_server.encode("utf-8"), (addr, prot))
   data_from_server = client.recvfrom(1024)
   ```
   
6. 面向对象 对象是某个整体 盛放需要的数据+功能

   面向国产 将程序流程化

7. ```python
   # 最常用
   # __init__      ：初始化类时触发
   # __del__      ：删除类时触发
   # __new__      ：构造类时触发
   # __str__      ：str函数或者print函数触发
   # __repr__      ：repr或者交互式解释器触发
   # __doc__      ：打印类内的注释内容
   # __enter__     ：打开文档触发
   # __exit__      ：关闭文档触发
   # __getattr__   ：访问不存在的属性时调用
   # __setattr__   ：设置实例对象的一个新的属性时调用
   # __delattr__   ：删除一个实例对象的属性时调用
   # __setitem__   ：列表添加值
   # __getitem__   ：将对象当作list使用
   # __delitem__   ：列表删除值
   
   # __call__      ：对象后面加括号，触发执行
   # __iter__      ：迭代器
   ```

8. 三次握手四次挥手

   三次握手

   第一次握手：

   SYN=1 seq=x

   第二次握手：

   SYN=1 ACK=1 seq=y ack=x+1

   第三次握手：

   ACK=1 seq=x+1 ack=y+1

   四次挥手

   第一次挥手：

   FIN=1 seq=u

   第二次挥手

   ACK=1 seq=v ack=u+1

   第三次挥手

   FIN=1 seq=w ack=u+1

   第四次挥手

   ACK=1 seq=u+1 ack=w+1

   



## 4Quiz:

1. 同步，异步，阻塞，非阻塞
2. 进程，程序
3. 开设多进程的两种方式



## 4Answer:

1. 同步：在执行一个程序的时候，没有得到结果之前不会执行第二个程序

   异步：在执行一个程序的时候，不需要等待当前结果，就可以执行第二个程序

   阻塞：在进程返回之前，当前进程被挂起

   非阻塞：不需要等待程序结果直接运行下一个

2. 进程：运行起来的硬盘中的数据

   程序：堆在硬盘中的数据

3. Process类创建子进程对象启动

   ```python
   # 【一】创建多进程方式一
   # 直接使用Process类创建 子进程对象然后启动
   # 1.定义子进程函数
   def work(name):
       sleep_time = random.randint(1, 3)
       print(f"{name} is sleeping for {sleep_time} seconds...")
       time.sleep(sleep_time)
       print(f"{name} is finished ...")
   
   
   # 2.获得子进程对象并启动子进程
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
       process_one.start()
       process_two.start()
       # 1
       # 2
       # 1 is sleeping for 3 seconds...
       # 2 is sleeping for 1 seconds...
       # 2 is finished ...
       # 1 is finished ...
       
       # 开启多进程的目的是为了让多几个进程同时运行
   
   if __name__ == '__main__':
       print("1")
       process_normal()
       print("2")
   # 得到一个进程对象
   ```

   继承Process进行重写

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

   



## 5Quiz：

1. 什么是进程，程序
2. 三次握手和四次挥手
3. 多进程实现服务端并发代码
4. 什么是守护进程，如何做
5. 什么是生产者模型和消费者模型
6. 什么是ipc，如何进程ipc





## 5Answer：

1. 进程是正在运行的程序 运行在内存中

   程序是静态的存储在硬盘中的数据

2. 三次握手和四次挥手：

   第一次握手：

   SYN=1 seq=x

   第二次握手：

   SYN=1 ACK=1 seq=y ack=x+1

   第三次握手：

   ACK=1 seq=x+1 ack=y+1

   四次挥手

   第一次挥手：

   FIN=1 seq=u

   第二次挥手

   ACK=1 seq=v ack=u+1

   第三次挥手

   FIN=1 seq=w ack=u+1

   第四次挥手

   ACK=1 seq=u+1 ack=w+1

3. 多进程服务器并发

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

4. 守护进程

   daemon，在计算机系统启动时就已经运行，并一直在后台运行的一类特殊进程

   ```python
   # 【三】主进程死亡之后子进程随之死亡
   def process_daemon():
       # 方式一：在创建当前子进程的时候就在子进程中增加 守护进程
   
       # 方式二：先创建一个子进程对象，然后给子进程对象加一个守护进程
       process_dream = Process(target=task, args=("def",))
       process_dream.daemon = True
   
       process_dream.start()
   
       # process_dream.daemon = True
       # AssertionError: process has already started
   ```

5. 生产者模型与消费者模型

   生产者的主要任务是将数据或任务添加到队列中

   消费者的主要任务是从队列中取出数据或任务

   ```python
   def producer(name, food, queue):
       """
       生产者生产数据
       :param name:生产者名字
       :param food: 生产者做出来的食物
       :param queue: 队列
       :return:
       """
       for i in range(2):
           # 生产数据
           data = f"当前大厨 {name} 生产出了第 {i} 份 {food}!"
           # 模拟延迟
           time.sleep(random.randint(1,4))
           # 添加数据
           queue.put(food)
           print(f"生产者 {name} :>>>>  {data}")
   
       # *****
       # 直接使用 joinablequeue 内置的方法增加结束标志
       queue.join()
   
   
   def customer(name, queue):
       """
       消费者消费数据
       :param name:消费者名字
       :param queue: 队列
       :return:
       """
       while True:
           # 取出食物
           food = queue.get()
           # 模拟延迟
           time.sleep(random.randint(1, 4))
           print(f"{name} 正在消费 {food}")
           # *****
           queue.task_done()
   
   
   def process_one():
       from multiprocessing import JoinableQueue
       # 1.建立媒介 - 创建队列对象
       # *****
       queue = JoinableQueue()
   
       # 2.创建生产者模型
       producer_one = Process(target=producer, args=("onef", "ice", queue))
       producer_two = Process(target=producer, args=("twof", "cream", queue))
   
       # 3.创建消费者模型
       customer_one = Process(target=customer, args=("onee", queue))
       customer_two = Process(target=customer, args=("twoe", queue))
   
       # 给消费者加上守护进程 主进程存在就会一直存活
       # 只要主进程一死，守护进程立马跟着结束
       customer_one.daemon = True
       customer_two.daemon = True
   
       process_list = [producer_one, producer_two, customer_one, customer_two]
   
       # 4.启动生产者和消费者
       [task.start() for task in process_list]
   
       # 5.阻塞等待所有生产者和消费者结束
       producer_one.join()
       producer_two.join()
   
       """
       生产者 onef :>>>>  当前大厨 onef 生产出了第 0 份 ice!
       生产者 twof :>>>>  当前大厨 twof 生产出了第 0 份 cream!
       onee 正在消费 ice
       生产者 onef :>>>>  当前大厨 onef 生产出了第 1 份 ice!
       twoe 正在消费 cream
       生产者 twof :>>>>  当前大厨 twof 生产出了第 1 份 cream!
       twoe 正在消费 cream
       onee 正在消费 ice
       """
       # 在生产者生产完所有数据的结尾增加一个结束生产的标志
       # 当消费者最后获取到结束标志的时候就将整个子进程结束掉
   
   
   if __name__ == '__main__':
       process_one()
   ```

6. IPC是inter process communication 进程间通信

   通过一个中间介质就可以实现进程间的通信

   multiprocessing 队列&管道

   ```python
   from multiprocessing import Process, Queue
   
   
   # 【一】子进程与主进程之间进行通信
   def producer(queue):
       print(f"来自主进程的数据 :>>> {queue.get()}")
       queue.put(f"dau process")
   
   
   def process():
       # 1.创建队列用于存储数据
       queue = Queue()
       # 2.传入数据
       queue.put(f"main process")
       # 3.创建子进程
       process_daughter = Process(
           target=producer,
           args=(queue,)
       )
       # 4.启动子进程
       process_daughter.start()
       # 如果这里get 会发现取不到数据 会被阻塞 是因为子进程时间太短还没有结束
       # 5.等待主进程结束前结束子进程
       process_daughter.join()
       print(f"来自子进程的数据 :>>> {queue.get()}")
   """
   来自主进程的数据 :>>> main process
   来自子进程的数据 :>>> dau process
   """
   
   
   def producer_one(queue, name):
       # 负责放数据
       print(f"{name} put")
       queue.put(f"dau process one put")
   
   
   def producer_two(queue, name):
       # 负责取数据
       print(f"{name} get:>>> {queue.get()}")
   
   
   # 子进程与子进程之间实现通信
   def process_dtod():
       # 1.生成Queue对象
       queue = Queue()
       # 2.创建两个子进程
       a = Process(target=producer_one, args=(queue, 1))
       b = Process(target=producer_two, args=(queue, 2))
   
       # 3.挨个启动子进程
       b.start()
       a.start()
   
       # 4.阻塞进程
       b.join()
       a.join()
   """
   1 put
   2 get:>>> dau process one put
   """
   
   
   if __name__ == '__main__':
       process()
       process_dtod()
   ```

   ```python
   import random
   import time
   from multiprocessing import Pipe, Process
   
   
   def producer(name, conn):
       """
       生产者生产数据
       :param name: 生产者名字
       :param conn: 管道对象
       :return:
       """
       left_conn, right_conn = conn
       # 把右侧管道关闭
       right_conn.close()
       for i in range(5):
           # 生产数据
           data = f"当前大厨 {name} 生产出了第{i}份!"
   
           # 从左侧管道向管道中添加数据
           left_conn.send(data)
           print(f"生产者 {name} :>>>>  {data}")
           # # 模拟延迟
           # time.sleep(random.randint(1, 4))
       left_conn.close()
   
   
   def customer(name, conn):
       """
       消费者消费数据
       :param name: 消费者名字
       :param conn: 队列
       :return:
       """
       left_conn, right_conn = conn
       # 把左侧管道关闭
       left_conn.close()
       while True:
           try:
               # 取出数据
               food = right_conn.recv()
               # 模拟延迟
               time.sleep(random.randint(1, 4))
               # 打印数据
               print(f"消费者 {name} :>>>>  {food}")
           except:
               right_conn.close()
               break
   
   
   def process_one():
       # 【1】建立媒介 --- 创建管道
       left_conn, _right_conn = Pipe()
       '''
       (<multiprocessing.connection.Connection object at 0x12533fee0>, <multiprocessing.connection.Connection object at 0x12533feb0>)
       '''
   
       customer_smile = Process(target=customer, args=("abc", (left_conn, _right_conn)))
       customer_smile.start()
   
       # 生产数据
       producer(name="zyx", conn=(left_conn, _right_conn))
   
       left_conn.close()
       _right_conn.close()
   
       customer_smile.join()
   
   
   """
   生产者 zyx :>>>>  当前大厨 zyx 生产出了第0份!
   生产者 zyx :>>>>  当前大厨 zyx 生产出了第1份!
   生产者 zyx :>>>>  当前大厨 zyx 生产出了第2份!
   生产者 zyx :>>>>  当前大厨 zyx 生产出了第3份!
   生产者 zyx :>>>>  当前大厨 zyx 生产出了第4份!
   消费者 abc :>>>>  当前大厨 zyx 生产出了第0份!
   消费者 abc :>>>>  当前大厨 zyx 生产出了第1份!
   消费者 abc :>>>>  当前大厨 zyx 生产出了第2份!
   消费者 abc :>>>>  当前大厨 zyx 生产出了第3份!
   消费者 abc :>>>>  当前大厨 zyx 生产出了第4份!
   """
   
   if __name__ == '__main__':
       process_one()
   ```

   

## 6Quiz：

1. 什么是进程。什么是线程
2. 三次握手和四次挥手
3. 什么是可迭代对象，什么是迭代器对象
4. 什么是粘包问题，解决思路，实现方式
5. 你都知道哪些魔法方法？都有什么作用？



## 6Answer：

1. 进程：操作系统的资源分配的最小单位

   线程：CPU调度执行的最小单位

   在一个进程下开设多个线程

2. ```python
   # 三次握手
   # 发生在TCP建立连接的过程中
   
   # 第一次握手: 由客户端发起(携带SYN=1, seq=x), 发送给服务端
   # 第二次握手: 由服务端发起(携带SYN=1, ACK=1, seq=y, ack=x+1), 发送给客户端
   # 第三次握手: 由客户端发起(携带ACK=1, seq=x+1, ack=y+1), 发送给服务端
   # 服务端同意建立连接
   
   
   # 四次挥手
   # 发生在TCP断开连接的过程中
   
   # 第一次挥手：由客户端发起(携带FIN=1, seq=u) ，发送给服务端
   # 第二次挥手：由服务端发起(携带ACK=1, seq=v, ack=u+1) ，发送给客户端 --- 我可能还有数据没有传输完成不允许断开
   # 第三次挥手：由服务端发起(携带FIN=1, seq=w, ack=u+1) ，发送给客户端 --- 数据传输完成了 允许断开连接
   # 第四次挥手：由客户端发起(携带ACK=1, seq=u+1, ack=w+1) ，发送给服务端
   ```

3. 可迭代对象：有``__iter__``方法的对象

   迭代器对象：有``__iter__``和``__next__``方法的对象

4. 粘包问题：

   一般服务端返回客户端数据的时候 每次有接受的数据大小限制 比如1024个字节 而有些数据大于这个大小 没有接收完的数据会随着下一次客户端的输入发送过来

   解决思路

   1. 把本来的大小暴力提升 希望发送过来的数据不要比这个值大
   2. 让客户端知道整个数据的大小 先知道大小 再进行接收	

​	实现方式：json+struct

5. 魔法方法及作用

   ```python
   # 最常用
   # __init__      ：初始化对象时触发
   # __del__      ：销毁类时触发
   # __new__      ：构造类时触发
   # __str__      ：str函数或者print函数触发
   # __repr__      ：repr或者交互式解释器触发
   # __doc__      ：打印类内的注释内容
   # __enter__     ：打开文档触发
   # __exit__      ：关闭文档触发
   
   # __getattr__   ：访问不存在的属性时调用
   # __setattr__   ：设置实例对象的一个新的属性时调用
   # __delattr__   ：删除一个实例对象的属性时调用
   
   # __setitem__   ：列表添加值
   # __getitem__   ：将对象当作list使用
   # __delitem__   ：列表删除值
   
   # __call__      ：对象后面加括号，触发执行
   # __iter__      ：迭代器
   ```

   