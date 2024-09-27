# 0 Web框架demo

- Django也是一个web框架 优秀在于 Django 已经经过了很多年的维护和修改
- web框架本质上可以看成是一个功能强大的socket服务端,用户的浏览器可以看成是拥有可视化界面的socket客户端。
- 两者通过网络请求实现数据交互，从架构层面上先简单地将Web框架看做是对前端、数据库的全方位整合

# 1 TCP服务端客户端模型

之前学过在Pycharm里写两个文件，一个服务端，一个客户端，绑定同一个IP+PORT，就可以进行连接。

形式是这样的：

```python
# 【Socket服务端】
import socket

# 1.创建一个 server
# 默认走的是 TCP 的流式协议
server = socket.socket()

# 2.绑定IP和PORT
addr = ("127.0.0.1", 9696)
server.bind(addr)

# 3.监听半连接池
server.listen(5)

while True:
    # 4.接受客户端对象
    conn, addr = server.accept()
    # 5.与客户端进行交互
    # 接收客户端信息
    data_from_client = conn.recv(1024)
    # 打印信息
    print(f"来自客户端的信息 :>>> {data_from_client.decode()}")
    # 返回数据
    data_to_client = "我已接收!"
    conn.send(data_to_client.encode())
```

```python
# 【Socket客户端】
# 1.导入模块
import socket

# 2.定义通信IP和端口
addr = ("127.0.0.1", 9696)

# 3.创建连接对象
client = socket.socket()
client.connect(addr)

while True:
    # 发送数据
    client.send(b"hello server")
    # 接收数据
    data = client.recv(1024)
    print(f"这是来自服务端的数据 :>>>> {data.decode('utf-8')}")
```

# 2 TCP服务端无法响应浏览器

- 现在可以把Web浏览器作为客户端，要注意Web浏览器遵循一系列的网络协议，如HTTP

```python
import socket

# 1.创建一个 server
# 默认走的是 TCP 的流式协议
server = socket.socket()

# 2.绑定IP 和 PORT
addr = ("127.0.0.1", 9696)
server.bind(addr)

# 3.监听半连接池
server.listen(5)

# 4.和客户端(现在是浏览器)交互
while True:
    # 接收客户端对象
    conn, addr = server.accept()

    #  1.接收到客户端信息
    data_from_client = conn.recv(1024)
    #  2.打印信息
    print(f"来自客户端的信息是 :>>>> \n {data_from_client.decode()}")
    #  3.反馈给客户端信息
    data_to_client = f"你好 {addr} 我是服务端!"
    #  4.发送数据
    conn.send(data_to_client.encode())
    
    # 运行服务端 在浏览器中输入http://127.0.0.1:9696/
    # 我们看到的效果是 服务端已经接收到了 客户端响应的数据 但客户端无法响应服务端的数据
    # 有一大段的信息在Pycharm中显示
    # 但是浏览器里是说服务器发送了无效的响应
    # 为什么？

    # 服务段的响应没有遵循 HTTP 协议的响应模型
    # 看返回的内容 可以发现格式为
    '''
    响应首行 响应状态码 协议版本
    响应头
    换行 \r\n
    响应体数据
    '''
```



# 3 TCP服务端响应浏览器客户端

- 当在浏览器中输入地址访问服务器时，浏览器期望服务器返回符合特定协议格式的响应。
- 尤其是 HTTP 响应格式，包括响应首行、响应头和响应体等。
- 如果服务器没有按照这些协议要求进行响应，浏览器可能无法正确解析和显示结果。

```python
import socket

# 1.创建一个 server
# 默认走的是 TCP 的流式协议
server = socket.socket()

# 2.绑定IP 和 PORT
# 这边和本来的写法有所区别
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
addr = ("127.0.0.1", 9696)
server.bind(addr)

# 3.监听半连接池
server.listen(5)

# 4.和客户端交互
while True:
    # 接收客户端对象
    conn, addr = server.accept()

    # 1.接收到客户端信息
    data_from_client = conn.recv(1024)
    # 2.打印信息
    print(f"来自客户端的信息是 :>>>> \n {data_from_client.decode()}")

    # 3.反馈给客户端信息
    # 发送数据
    
    # 1. 响应头
    response_line = 'HTTP1.1 200 OK'
    # 2. 也是响应头
    response_headers = 'Host: 127.0.0.1:9696'
    # 3. 换行
    response_next_line = "\r\n"
    # 4. 响应体数据
    response_body = f"你好 {addr} 我是服务端!"

    # 5. 拼接成完整的数据
    response_data = response_line + response_next_line + response_headers + response_next_line + response_next_line + response_body

    conn.send(response_data.encode("gbk"))

```

- 这一次，浏览器里看到了`你好 ('127.0.0.1', 57870) 我是服务端!`，这个57870是一个随机选择的端口
- 服务端接收两个信息 `GET / HTTP/1.1` 和 `GET /favicon.ico HTTP/1.1`



# 4 路由分发

```python
# 实现了一个简单的基于 TCP 的服务器，能够根据客户端的不同请求路径返回不同的响应内容。
import socket

# 1.创建一个 server
# 默认走的是 TCP 的流式协议
server = socket.socket()

# 2.绑定IP 和 PORT

server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
addr = ("127.0.0.1", 9696)
server.bind(addr)

# 3.监听半连接池
server.listen(5)


def response(data):
    # 4.发送数据

    # 1. 响应头
    response_line = 'HTTP1.1 200 OK'
    # 2. 响应头
    response_headers = 'Host: 127.0.0.1:9696'
    # 3. 换行
    response_next_line = "\r\n"
    # 4. 响应体数据
    response_body = data

    # 5. 拼接成完整的数据
    response_data = response_line + response_next_line + response_headers + response_next_line + response_next_line + response_body
    return response_data.encode("gbk")


# 访问首页 -> 返回首页的样式
def index():
    data = f"欢迎来到首页"
    return response(data=data)


# 访问登陆页面 -> 返回登陆页面的样式
def login():
    data = f"欢迎来到登录页面"
    return response(data=data)


def request(data: bytes):
    # 解析客户端发送的数据 根据请求的路径决定返回的相应内容
    original_data = data.decode("utf8")
    path_data = original_data.split("\n")[0]
    method, path, version = path_data.split(" ")

    if path == "/favicon.ico" or path == "/":
        return index()
    elif path == "/login":
        return login()


# 4.和客户端交互
while True:
    # 5.接收客户端对象
    conn, addr = server.accept()

    # 1.接收到客户端信息
    data_from_client = conn.recv(1024)
    request(data=data_from_client)
    # 2.打印信息
    print(f"来自客户端的信息是 :>>>> \n {data_from_client.decode()}")

    # 3.反馈给客户端信息
    # 写成了一个函数
    response_data = response(f'你好 {addr} 我是服务端!')
    conn.send(response_data)
```



# 5 分层开发

```python
# lib.py
# 借助 request 模块进行路由的分发
from url import url_patterns


def request(data: bytes):
    original_data = data.decode("utf8")
    path_data = original_data.split("\n")[0]
    method, path, version = path_data.split(" ")
    for path_info in url_patterns:
        if path in path_info:
            return path_info[1]()
    else:
        return response(data=f"非法的路径!")


def response(data):
    # （4）发送数据
    # 1. 响应头
    response_line = 'HTTP1.1 200 OK'
    # 2. 响应头
    response_headers = 'Host: 127.0.0.1:9696'
    # 3. 换行
    response_next_line = "\r\n"
    # 4. 响应体数据
    response_body = data

    # 5. 拼接成完整的数据
    response_data = response_line + response_next_line + response_headers + response_next_line + response_next_line + response_body
    return response_data.encode("gbk")
```

```python
# main.py
import socket
from lib import request

# 【1】创建一个 server
# 默认走的是 TCP 的流式协议
server = socket.socket()

# 【2】 绑定IP 和 PORT

server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
addr = ("127.0.0.1", 9696)
server.bind(addr)

# 【3】监听半连接池
server.listen(5)
# 【5】和客户端交互
while True:
    # 【4】接受客户端对象
    conn, addr = server.accept()

    # （1）接收到客户端信息
    data_from_client = conn.recv(1024)

    # （3）反馈给客户端信息
    response_data = request(data=data_from_client)

    conn.send(response_data)
```

```python
# url.py
from view import index, login, home

url_patterns = [
    ("/index", index),
    ("/", home),
    ("/login", login),
]
```

```python
# view.py
# 访问首页 -=--> 返回首页的样式
def index():
    from lib import response
    data = f"欢迎来到首页"
    return response(data=data)


# 访问登陆页面 ----> 返回登陆页面的样式
def login():
    from lib import response
    data = f"欢迎来到登录页面"
    return response(data=data)


def home():
    from lib import response
    data = f"欢迎来到根页面"
    return response(data=data)
```







# 6 分层开发wsgiref

```python
# lib.py
# 借助 request 模块进行路由的分发
from url import url_patterns


def request(path: str):
    for path_info in url_patterns:
        if path in path_info:
            return path_info[1]()
    else:
        return response(data=f"非法的路径!")


def response(data):
    return [data.encode("gbk")]
```

````python
# main.py
import socket
from lib import request as request_self
from wsgiref import simple_server


def run(request, response):
    # print(request)
    # request_method = request.get("REQUEST_METHOD")
    path = request.get("PATH_INFO")

    # 固定写参数 返回一个响应状态码为 200 OK 的响应 后面 [] 让你放数据的
    response("200 OK", [])
    return request_self(path=path)


# 写一个启动函数
def main():
    # 创建服务端对象
    server = simple_server.make_server(
        host="127.0.0.1",
        port=9696,
        app=run
    )

    # 启动服务端
    server.serve_forever()


if __name__ == '__main__':
    main()
````

```python
# url.py
from view import index, login, home

url_patterns = [
    ("/index", index),
    ("/", home),
    ("/login", login),
]
```

```python
# view.py
# 访问首页 -=--> 返回首页的样式
def index():
    from lib import response
    data = f"欢迎来到首页"
    return response(data=data)


# 访问登陆页面 ----> 返回登陆页面的样式
def login():
    from lib import response
    data = f"欢迎来到登录页面"
    return response(data=data)


def home():
    from lib import response
    data = f"欢迎来到根页面"
    return response(data=data)
```





# 7 wsgiref渲染数据
```python
# 导入模版对象
from jinja2 import Template

# 访问登陆页面 ----> 返回登陆页面的样式
def login():
    from lib import response, read_data
    user_info = {"username": "dream", "age": 18}
    # 读取到登陆页面的所有源码数据
    data = read_data(file_name="login.html")
    # 将源码数据交给 Template 对象实例化得到数据
    temp_obj = Template(data)
    # 调用对象的渲染语法 渲染数据
    result = temp_obj.render({"user_info":user_info})
    return response(data=result)

```

````python
# 【一】模版文件夹
# 创建一个 templates 文件夹 用来存储所有的 html 文件

# 【1】静态网页
# 是网页最基础的模板 ---- 所有的 代码和数据不会轻易发生改变 --- 死数据

# 【2】动态网页
# 只有最基础的框架 其他的数据都需要从后端传入然后进行渲染

# 【二】静态资源文件夹
# 创建一个 static 文件夹 用来存储基本不变的数据文件

# 【三】渲染网页 -- 借助第三方模块 Jinjia2
# 需要安装 pip install jinja2
'''
# 读取到登陆页面的所有源码数据
data = read_data(file_name="login.html")
# 将源码数据交给 Template 对象实例化得到数据
temp_obj = Template(data)
# 调用对象的渲染语法 渲染数据
result = temp_obj.render()
'''

# 【四】模版语法
# 在后端定义通过 render 方法 渲染给前端
# 前端需要用 {{}} 来渲染
'''
user_info = {"username": "dream", "age": 18}
# 读取到登陆页面的所有源码数据
data = read_data(file_name="login.html")
# 将源码数据交给 Template 对象实例化得到数据
temp_obj = Template(data)
# 调用对象的渲染语法 渲染数据
result = temp_obj.render({"user_info":user_info})
'''
'''
{{user_info.age}}
'''
````






# 8 web框架介绍

## 8.1 网络框架 及 MVC 框架

```python
# 1.网络框架
# 所谓网络框架是指这样的一组Python包，它能够使开发者专注于网站应用业务逻辑的开发，而无须处理网络应用底层的协议、线程、进程等方面。
# 这样能大大提高开发者的工作效率，同时提高网络应用程序的质量。
# 在目前Python语言的几十个开发框架中，几乎所有的全栈网络框架都强制或引导开发者使用MVC架构开发Web应用。
# 所谓全栈网络框架，是指除了封装网络和线程操作，还提供HTTP栈、数据库读写管理、HTML模板引擎等一系列功能的网络框架。
# 本文重点讲解的Django、Tornado和Flask是全栈网络框架的典型标杆；而Twisted更专注于网络底层的高性能封装而不提供HTML模板引擎等界面功能，所以不能称之为全栈框架。

# 2.MVC架构
# MVC(Model-View-Controller)模式最早由Trygve Reenskaug在1978年提出，在20世纪80年代是程序语言Smalltalk的一种内部架构。
# 后来MVC被其他语言所借鉴，成为了软件工程中的一种软件架构模式。
# MVC把Web应用系统分为3个基本部分。
# (1)模型(Model)
# 用于封装与应用程序的业务逻辑相关的数据及对数据的处理方法，是Web应用程序中用于处理应用程序的数据逻辑的部分，Model只提供功能性的接口，通过这些接口可以获取Model的所有功能。
# Model不依赖于View和Controller，它们可以在任何时候调用Model访问数据。
# 有些Model还提供了事件通知机制，为在其上注册过的View或Controller提供实时的数据更新。
# (2)视图(View)
# 负责数据的显示和呈现，View是对用户的直接输出。
# MVC中的一个Model通常为多个View提供服务。
# 为了获取Model的实时更新数据，View应该尽早地注册到Model中。
# (3)控制器(Controller)
# 负责从用户端收集用户的输入，可以看成提供View的反向功能。
# 当用户的输入导致View发生变化时，这种变化必须是通过Model反映给View的。
# 在MVC架构下，Controller一般不能与View直接通信，这样提高了业务数据的一致性，即以Model作为数据中心。

# 3.MVC架构图
# 这3个基本部分互相分离，使得在改进和升级界面及用户交互流程时，不需要重写业务逻辑及数据访问代码。
```

## 8.2 Python主流的后端框架

```python
# 1.Django
# (1)介绍
# 相对于Python的其他Web框架，Django的功能是最完整的，Django定义了服务发布、路由映射、模板编程、数据处理的一整套功能。
# 这也意味着Django模块之间紧密耦合，开发者需要学习Django自己定义的这一整套技术。
# (2)特点
# [1]完善的文档
# 经过10多年的发展和完善，Django有广泛的应用和完善的在线文档，开发者遇到问题时可以搜索在线文档寻求解决方案。
# [2]集成数据访问组件
# Django的Model层自带数据库ORM组件，使开发者无须学习其他数据库访问技术(dbi、SQLAlchemy等)。
# [3]强大的URL映射技术
# Django使用正则表达式管理URL映射，因此给开发者带来了极高的灵活性。
# [4]后台管理系统自动生成
# 开发者只需通过简单的几行配置和代码就可以实现完整的后台数据管理Web控制台。
# [5]错误信息非常完整
# 在开发调试过程中如果出现运行异常，则Django可以提供非常完整的错误信息帮助开发者定位问题，比如缺少xxx组件的配置引用等，这样可以使开发者马上改正错误。
# (3)组成结构
# Django是遵循MVC架构的Web开发框架，其主要由以下几部分组成。
# 管理工具(Management)：一套内置的创建站点、迁移数据、维护静态文件的命令工具。
# 模型(Model)：提供数据访问接口和模块，包括数据字段、元数据、数据关系等的定义及操作。
# 视图(View)：Django的视图层封装了HTTP Request和Response的一系列操作和数据流，其主要功能包括URL映射机制、绑定模板等。
# 模板(Template)：是一套Django自己的页面渲染模板语言，用若干内置的tags和filters定义页面的生成方式。
# 表单(Form)：通过内置的数据类型和控件生成HTML表单。
# 管理站(Admin)：通过声明需要管理的Model，快速生成后台数据管理网站。
# (4)总结
# 大而全
#   ○ 自带的功能非常的多
#   ○ 但是有时候会略显笨重
# 类似于'航空母舰'


# 2.Flask
# (1)介绍
# Flask是Python Web框架族里比较年轻的一个，于2010年出现，这使得它吸收了其他框架的优点，并且把自己的主要领域定义在了微小项目上。
# 同时，它是可扩展的，Flask让开发者自己选择用什么数据库插件存储他们的数据。
# 很多功能简单但性能卓越的网站就是基于Flask框架而搭建的，比如httpbin.org就是一个功能简单但性能强大的HTTP测试项目。
# Flask是一个面向简单需求和小型应用的微框架。
# (2)特点
# [1]内置开发服务器和调试器
# 网络程序调试是在将编制好的网站投入实际运行前，用手工或编译程序等方法进行测试，修正语法错误和逻辑错误的过程。
#   ○ 有经验的开发者都知道，这是保证网站系统能够正式应用的必要步骤。
# Flask 自带的开发服务器使开发者在调试程序时无须再安装其他任何网络服务器，比如Tomcat、JBoss、Apache等。
# Flask默认处于调试状态，使得运行中的任何错误会同时向两个目标发送信息：
#   ○ 一个是Python Console，即启动Python程序的控制台；
#   ○ 另一个是HTTP客户端，即Flask开发服务器将调试信息传递给了客户端。
# [2]与Python单元测试功能无缝衔接
# 单元测试是对最小软件开发单元的测试，其重点测试程序的内部结构，主要采用白盒测试方法，由开发人员负责。
# 单元测试的主要目标是保证函数在给定的输入状态下，能够得到预想的输出，在不符合要求时能够提醒开发人员进行检查。
# Flask提供了一个与Python自带的单元测试框架unitest无缝衔接的测试接口，即Flask对象的test_client()函数。
# 通过test_client()函数，测试程序可以模拟进行HTTP访问的客户端来调用Flask路由处理函数，并且获取函数的输出来进行自定义的验证。
# [3]使用Jinja2模板
# 将HTML页面与后台应用程序联系起来一直是网站程序框架的一个重要目标。
# Flask通过使用Jinja2模板技术解决了这个问题。
# Jinja2是一个非常灵活的HTML模板技术，它是从Django模板发展而来的，但是比Django模板使用起来更加自由且更加高效。
# Jinja2模板使用配制的语义系统，提供灵活的模板继承技术，自动抗击XSS跨站攻击并且易于调试。
# [5]完全兼容WSGI 1.0标准
# WSGI(Web Server Gateway Interface)具有很强的伸缩性且能运行于多线程或多进程环境下，因为Python线程全局锁的存在，使得WSGI的这个特性至关重要。
# WSGI已经是Python界的一个主要标准，各种大型网路服务器对其都有良好的支持。
# WSGI位于Web应用程序与Web服务器之间，与WSGI完全兼容使得Flask能够配置到各种大型网络服务器中。
# [6]基于Unicode编码
# Flask是完全基于Unicode的。这对制作非纯ASCII字符集的网站来说非常方便。
# HTTP本身是基于字节的，也就是说任何编码格式都可以在HTTP中传输。
# 但是，HTTP要求在HTTP Head中显式地声明在本次传输中所应用的编码格式。
# 在默认情况下，Flask会自动添加一个UTF-8编码格式的HTTP Head，使程序员无须担心编码的问题。
# (3)总结
# 小而精
#   ○ 自带的功能非常的少
#   ○ 但是第三方模块非常的多
# 类似于'游骑兵'
# flask的第三方模块加到一起甚至比django还多
#   ○ 并且也越来越像django
# flask由于过多的依赖于第三方模块
#   ○ 有时候也会受制于第三方模块


# 3.Tornado
# (1)介绍
# Tornado是使用Python编写的一个强大的可扩展的Web服务器。
# 它在处理高网络流量时表现得足够强健，却在创建和编写时有着足够的轻量级，并能够被用在大量的应用和工具中。
# Tornado作为FriendFeed网站的基础框架，于2009年9月10日发布，目前已经获得了很多社区的支持，并且在一系列不同的场合中得到应用。
# 除FriendFeed和Facebook外，还有很多公司在生产上转向Tornado，包括Quora、Turntable.fm、Bit.ly、Hipmunk及MyYearbook等。
# (2)特点
# 完备的Web框架：与Django、Flask等一样，Tornado也提供了URL路由映射、Request上下文、基于模板的页面渲染技术等开发Web应用的必备工具。
# 是一个高效的网络库，性能与Twisted、Gevent等底层Python框架相媲美：提供了异步I/O支持、超时事件处理。这使得Tornado除了可以作为Web应用服务器框架，还可以用来做爬虫应用、物联网关、游戏服务器等后台应用。
# 提供高效HTTPClient：除了服务器端框架，Tornado还提供了基于异步框架的HTTP客户端。
# 提供高效的内部HTTP服务器：虽然其他Python网络框架(Django、Flask)也提供了内部HTTP服务器，但它们的HTTP服务器由于性能原因只能用于测试环境。
# 而Tornado的HTTP服务器与Tornado异步调用紧密结合，可以直接用于生产环境。
# 完备的WebSocket支持：WebSocket是HTML5的一种新标准，实现了浏览器与服务器之间的双向实时通信。
# (3)总结
# 异步非阻塞框架
#   ○ 速度极快
#   ○ 常被用作大型站点的接口服务框架
#   ○ 甚至可以用于充当游戏服务器


# 4.Fastapi
# (1)介绍
# FastAPI 是一个现代 Web 框架，速度相对较快，用于基于标准 Python 类型提示使用 Python 3.7+ 构建 API。
# FastAPI还帮助我们自动为我们的Web服务生成文档，以便其他开发人员可以快速了解如何使用它。
# FastAPI 具有许多功能，例如它可以显着提高开发速度，还可以减少代码中的人为错误。
# 它很容易学习并且完全可以用于生产。
# FastAPI 与众所周知的 API 标准(即OpenAPI 和JSON schema)完全兼容。
# (2)特点
# [1]自动文档
# FastAPI 使用 OpenAPI 标准自动生成交互式 API 文档。
# 可以通过访问应用程序中的特定端点来访问此文档，这使得理解和测试 API 变得非常容易，而无需手动编写大量文档。
# [2]Python 类型提示
# FastAPI 的突出功能之一是它使用 Python 类型提示。
# 通过使用类型提示注释函数参数和返回类型，不仅可以提高代码可读性，还可以使 FastAPI 自动验证传入数据并生成准确的 API 文档。
# 此功能使我们的代码不易出错并且更加自我记录。
# [3]数据验证
# FastAPI 使用 Pydantic 模型进行数据验证。
# 可以使用 Pydantic 的架构和验证功能定义数据模型。
# 这可确保传入数据自动验证、序列化和反序列化，从而降低在应用程序中处理无效数据的风险。
# [4]异步支持
# 随着Python异步编程的兴起，FastAPI完全拥抱异步操作。
# 可以使用Python的async和await关键字来编写异步端点，使其非常适合处理I/O密集型任务并提高应用程序的整体响应能力。
# [5]依赖注入
# FastAPI 支持依赖注入，允许声明端点的依赖关系。
# 这有助于保持代码模块化、可测试和可维护。
# 我们可以将数据库连接、身份验证等依赖项无缝地注入到的路由中。
# [6]安全功能
# FastAPI 包含各种开箱即用的安全功能，例如对 OAuth2、JWT(JSON Web 令牌)的支持以及请求数据的自动验证，以防止 SQL 注入和跨站点脚本 (XSS) 攻击等常见安全漏洞。
```

## 8.3 Python框架官网(部分)

```python
# 框架的核心逻辑几乎是一致的 我们在学习的时候只需要先学会一种之后就可以触类旁通
# Django框架官网：https://www.djangoproject.com/
# Flask框架官网：https://flask.palletsprojects.com/en/3.0.x/
# Fastapi框架官网：https://fastapi.tiangolo.com/
# Pyramind框架官网：https://trypyramid.com/
# Tornado框架官网：https://www.tornadoweb.org/en/stable/
# Sanic框架官网：https://github.com/sanic-org/sanic
# Fastapi框架官网：https://fastapi.tiangolo.com/
# Aiohttp框架官网：https://docs.aiohttp.org/en/stable/
```





