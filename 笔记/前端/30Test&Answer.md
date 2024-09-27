## 01

1. 什么是http协议

   超文本传输协议 主要用于传输与超文本相关的资源文件

2. http协议的四大特点

   基于请求和响应的模型

   基于TCP和IP之上的应用层协议

   无状态

   短连接

3. 什么是html

   超文本标记语言 包含标记的文本文件

4. tcp三次握手四次挥手过程

   ```python
   # 【一】三次握手
   # 发生在TCP建立连接的过程中
   
   # 第一次握手: 由客户端发起(携带SYN=1, seq=x), 发送给服务端
   # 第二次握手: 由服务端发起(携带SYN=1, ACK=1, seq=y, ack=x+1), 发送给客户端
   # 当前服务端已经收到了连接请求并同意当前的链接请求
   # 第三次握手: 由客户端发起(携带ACK=1, seq=x+1, ack=y+1), 发送给服务端
   # 建立连接
   
   
   # 【二】四次挥手
   # 发生在TCP断开连接的过程中
   
   # 第一次挥手：由客户端发起(携带FIN=1, seq=u) ，发送给服务端
   # 第二次挥手：由服务端发起(携带ACK=1, seq=v, ack=u+1) ，发送给客户端 --- 我可能还有数据没有传输完成不允许断开
   # 第三次挥手：由服务端发起(携带FIN=1, seq=w, ack=u+1) ，发送给客户端 --- 数据传输完成了 允许断开连接
   # 第四次挥手：由客户端发起(携带ACK=1, seq=u+1, ack=w+1) ，发送给服务端
   ```

5. 进程间通信的方式有哪些

   管道or队列 multiprocessing模块

6. ·什么是并行，并发。串行

   并发是伪并行 多个任务交替进行 在宏观上同时

   并行是多任务同时进行 需要具备多个CPU

   串行是主进程的执行路径是按顺序等每个任务完成

7. 块级标签和行内标签的区别

   块级标签从新的一行开始 自己独自占据一行

   行内标签在一行内显示 会不停打断当前行并且只占据自己所需的内容宽度

8. 你知道那些魔法方法，有什么作用

   ```python
   # __init__      ：初始化对象时触发
   # __del__      ：删除对象时触发
   # __new__      ：构造对象时触发
   # __str__      ：对象str函数或者print函数触发
   # __repr__      ：repr或者交互式解释器触发
   # __doc__      ：打印类内的注释内容
   # __enter__     ：打开文档触发
   # __exit__      ：关闭文档触发
   # __getattr__   ：对象访问不存在的属性时调用
   # __setattr__   ：设置实例对象的一个新的属性时调用
   # __delattr__   ：删除一个实例对象的属性时调用
   # __setitem__   ：使用索引操作符为对象的字段赋值时触发
   # __getitem__   ：使用索引操作符获取对象的字段值时触发
   # __delitem__   ：使用索引操作符删除对象的字段时触发
   ```






## 02

1. 什么是事务

   事务是指一系列相关操作的集合，这些操作被视为一个不可分割的工作单元。

2. 事务有哪些特性

   ACID 原子性 一致性 隔离性 持久性

3. 事务的使用场景

   检查库存 扣减库存 生成订单 计算总价 更新账户 生成支付信息等

4. 什么是索引，在MySQL中有哪些索引

   索引是存储引擎用于快速找到记录的一种数据结构

   主键索引 辅助索引 唯一索引 全文索引

5. 如果一句SQL查询数据慢你有哪些解决方案

   创建或优化索引

   优化查询语句 避免没有必要的复杂写法 

6. 什么是http协议，http协议的特点

   超文本传输协议 用于传输与超文本相关的资源文件 是一种无状态网络应用协议

   基于请求和响应的模型

   基于TCP和IP之上的应用层协议

   无状态

   短连接

7. 列出常用的html标签并解释其作用和属性

   img标签 渲染图片 src网络链接或本地地址；alt图片加载失败的提示信息；title图片悬浮时显示的标题信息；height width 图片高度宽度

   a标签 跳转指定地址 href指定的地址

   input标签 表单输入的控件标签 type文本输入框类型；password密码输入框；checkbox多选框；radio单选框；...

   select标签 name传递给后端的键；id通过id获取到选择的值；multiple单选变多选；...





## 03

1. TCP三次握手和四次挥手流程

   三次握手：

   第一次握手：由客户端发起，发送给服务端，请求连接

   第二次挥手：由服务端发起，发送给客户端，同意客户端的连接请求

   第三次挥手：由客户端发起，发送给服务端，双方进行连接

   四次挥手：

   第一次挥手：由客户端发起，发送给服务端，请求断开连接

   第二次挥手：由服务端发起，发送给客户端，收到客户端的断开连接请求，但还有数据没有传输完成

   第三次挥手：由服务端发起，发送给客户端，数据传输完成，同意断开连接

   第四次挥手：由客户端发起，发送给服务端，双方断开连接

2. 静态方法和动态方法的区别

   分类 定义 用法

   静态方法 不绑定给目标的方法

   需要用@staticmethod在函数前面装饰 obj.func() / cls.func() 

   动态方法 绑定给类的方法和绑定给对象的方法

   给类 需要用@classmethod在函数前面装饰 obj.func() / cls.func() 都不需要传入额外的参数

   给对象 obj.func() 会自动把这个obj作为self传入 / cls.func(obj) 需要主动传入对象

3. 你知道哪些魔法方法，各自的应用场景

   ```python
   # __init__      ：初始化对象时触发
   # __del__      ：删除对象时触发
   # __new__      ：构造对象时触发
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
   ```
   
4. 方法属性变数据属性的方式

   使用@property

5. 什么是进程什么是线程什么是协程

   进程是在操作系统中执行的程序

   线程是在进程内部执行的程序

   协程是在线程内部执行的程序

6. 什么是IPC，IPC的实现方式有哪些

   IPC是inter process communication 进程间通信

   通过一个中间介质就可以实现进程间的通信

   multiprocessing 队列process&管道queue



获取对象属性的方法：

1. 点操作符  对象.属性
2. getattr函数  getattr(对象, "属性")
3. `__dict__`属性   `对象.__dict__`输出所有属性的字典
4. 在类的方法中访问属性  self.属性





## 04

1. 什么是事务，事务的四大特点，如何开启事务

   一系列操作的集合

   ACID 原子性 一致性 隔离性 持久性

   start transaction

2. SQL语句的注意事项有哪些

   关键字不区分大小写

   以;英文分号结尾

   关键词不能跨行或简写

   如果数据库名 表名 字段名 与 关键字冲突 用反引号圈起来

3. 什么是协程对象和协程函数

   协程对象 是 协程函数被调用后的返回值

   协程函数是 加上 asynic 关键字的函数

4. 拼表的方式有哪些，默写SQL

   ```sql
   # inner join
   # left join
   # right join
   # union join
   
   select *
   from emp
   xx join dep on emp.dep_id = dep.id
   
   # union
   select *
   from emp
   left join dep on emp.dep_id = dep.id
   union
   select *
   from emp
   right join dep on emp.dep_id = dep.id
   ```

5. 什么是子查询，什么是联表查询

   子查询：把一个查询的结果作为另一个查询的查询条件

   联表查询：把多张表的数据拼接到一张表中 在这张表中查询想要的数据

6. 什么是闭包函数

   函数内部再定义一个函数 并且这个函数用到了外部函数的变量





## 05

1. js函数有可变长位置参数吗，如何处理多余的位置参数

   某种程度上来说 js中没有可变长位置参数。 在js中并不区分位置参数和关键字参数。

   在js中可变长参数有两种写法

   1.可以使用arguments 会接受所有实参

   因此可以接收到多余的位置参数 并不进行多余处理

   2....rest 这是ES6的语法 区别大致在于 argument不是真正的数组 不能和箭头函数结合使用；而...rest参数支持所有数组方法 可以在箭头使用，并且最重要的是，他是最后一个参数，只能捕获多余的、没有被定义的参数，而arguments捕获所有参数

2. http协议的特点，什么是http协议

   超文本传输协议 特点包括：

   基于请求和响应的模型

   基于TCP和IP之上的应用层协议

   无状态

   短连接

3. tcp服务端如何响应浏览器的请求，写出示例代码

   ```python
   import socket
   
   # 服务端
   server = socket.socket(family=socket.AF_INET, type=socket.SOCK_STREAM)
   addr = "127.0.0.1"
   port = 9696
   server.bind((addr, port))
   server.listen(5)
   client_socket, client_addr = server.accept()
   data_from_client = client_socket.recv(1024)
   
   # 响应
   data_to_client = f"你好!我是服务端!{client_addr}"
   client_socket.send(data_to_client.encode("utf-8"))
   
   client_socket.close()
   server.close()
   ```

4. Python查询MySQL数据的方式有哪些

   f"" 字符串格式化

   % 也是字符串格式化

   %s 占位符

   直接写

5. 什么是SQL注入问题，如何解决

   利用SQL的漏洞

   查询数据的时候 使用了自己字符串拼接语法 把 -- 作为注释内容 导致SQL失效 无需用户名就可以对数据进行操作

   解决方案

   SQL语句的站位语法

   ```python
   sql = "select * from user where username=%(username)s and password=%(password)s;"





## 06

1. 面向对象三大特性

   封装

   多态

   继承

2. 做一个红色3像素的直线边框如何处理

   ```css
   border: 3px solid red;
   ```

3. 想要对当前窗口的网址进行判断，如果是百度就提示欢迎，如果不是就提示输入正确网址

   ```js
   function checkHerf() {
       if (location.href === "https://www.baidu.com/") {
   		alert("欢迎");
   	}
   	else {
   		resultWeb=prompt("请输入正确网址 :>>> ", 0);
   	}
   }
   ```

4. 做一个浏览器交互操作，获取到用户输入的网址并进行跳转

   ```js
   function goUrl() {
       tagUrl = prompt("请输入跳转网址 :>>> ");
       if (tagUrl.indexOf("http") === -1 || tagUrl.indexOf("https") === -1) {
           alert("请输入正确的网址");
       } else {
           window.open(tagUrl, "blank")
       }
   }
   
   
   
   
   resultWeb=prompt("请输入网址 :>>> ", 0);
   location.assign(resultWeb);
   ```

   



## 07

1. 设计一个页面交互程序，让用户点击页面上的登录按钮，弹出输入框，输入用户名跟密码，设定一个自己定义的用户名和密码，如果正确就跳转到百度，不正确弹出警告框

   ```html
   // 点击按钮就能弹出输入框
   // .onclick
   // 用户名和密码两个东西都要输入 2个prompt
   // 判定 正确-window.location.href
   // 不正确-alert
   
   
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
       <!--    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.min.js"></script>-->
       <!--    <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">-->
       <!--    <script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>-->
   
   </head>
   <body>
   
   <!-- 定义一个 button 按钮 -->
   <!--<button id="btn">点我登陆</button>-->
   
   <div id="d1">
       <p id="p1">
   
       </p>
   </div>
   <input type="button" id="btn" value="等">
   <script>
       // 获取到当前的按钮标签
       var btnEle = document.getElementById("btn")
       // 点击页面上的登录按钮
       btnEle.addEventListener("click", () => {
           username = window.prompt("请输入用户名 ")
           password = window.prompt("请输入密码 ")
   
           if (username === "aaa" && password === "bbb") {
               window.open("https://www.baidu.com")
           } else {
               window.alert("666")
           }
       })
   
   </script>
   </body>
   </html>
   ```

2. js中对象的构建方式有哪些

   ```js
   // 1.直接构建
   var obj = {}
   var obj = {name: "sheenagh", age:23}
   
   // 2.构造函数来生成对象
   var obj = new Object()
   
   // 3.自定义对象
   function Person(name, age) {
       this.name = name
       this.age = age
   }
   var obj = Person("sheenagh", 23)
   
   // 4.ES6中的新语法
   class Student {
       constructor(name, gender) {
           this.name = name
           this.gender = gender
       }
   }
   
   var obj = new Student("sheenagh", "female")
   ```

3. js中对象的特性有哪些

   无序的属性集合

   键值对

   值可以是数据或函数

   可以枚举 可以封装 可以转换为JSON格式

4. 操作标签属性的操作有哪些

   ```js
   d=document.getElementById("d")
   // 1
   // 获取属性值
   d.getAttribute("class")
   // 设置
   d.setAttribute("attributename", "value")
   // 删除
   d.removeAttribute("attributename")
   // 访问
   d.attributeName
   d.hasAttribure("attributename")
   
   // 2
   // 属性值数组
   list=d.classList
   // 添加属性
   list.add("new")
   // 删除
   list.remove("new")
   // 切换 如果有就删除 就添加
   list.toggle("haha")
   ```

5. 如何获取标签的所有class值

   ```js
   d.classList
   d.getAttribute("class").split(" ")
   ```

   



## 08

1. 默写开关灯事件(jq或js都行)

   ```js
   btnEle = document.getElementById("btn")
   divEle = document.getElementById("change")
   btnEle.onclick = function() {
   	divEle.classList.toggle("change_red")
   }
   
   // css
   .change_red {
       background-color: red;
   }
   ```

2. js绑定事件的方式有哪些，jq绑定事件的方式有哪些

   ```js
   divEle = document.getElementById("change")
   // 1.传统
   divEle.addEventListener(
       "", function () {
           //
       }
   )
   
   // 2.提前定义 绑定
   function abcMethod() {
       divEle.setAttribute(
           "style", "background-color: red"
       )
   }
   
   // 1. .on
   $(selector).on(eventName, eventHandler);
   
   // 2.快捷
   $('#myButton').click(function() {
       // 处理点击事件的代码
   });
   ```

3. 给输入框绑定时间，聚焦的时候增加一个3像素红色的直线，失去焦点时，输入框变为绿色的背景色

   ```js
   inputEle = document.getElementById("go")
   
   inputEle.onfocus = function() {
       inputEle
   }
   
   inputEle.onblur = function() {
       inputEle.style.backGroundColor = "green"
   }
   ```

   

4. 给输入框的提交按钮绑定事件，阻止提交按钮的自动刷新并校验用户名和密码是否和自定义的用户名和密码一致

   ```js
   $("#submit").click(function (e) {
       // 阻止当前标签的默认行为
       e.preventDefault()
       
       // 组织默认行为和校验是两块 无关的 
   })
   ```

   
