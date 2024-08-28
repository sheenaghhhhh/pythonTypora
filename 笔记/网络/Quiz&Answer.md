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