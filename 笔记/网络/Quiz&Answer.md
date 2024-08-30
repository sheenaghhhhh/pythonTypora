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

   