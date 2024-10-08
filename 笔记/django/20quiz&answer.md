## 01

1. 默写二级菜单事件

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
       <script src="../static/plugins/jQuery/jquery.min.js"></script>
       <link href="../static/plugins/bootstrap/bootstrap.min.css" rel="stylesheet">
       <script src="../static/plugins/bootstrap/bootstrap.min.js"></script>
       <style>
           .hide {
               display: none;
           }
       </style>
   </head>
   <body>
   <div class="title">
       一级菜单
       <p class="item"> 1.1</p>
       <p class="item">1.2</p>
       <p class="item">1.3</p>
   </div>
   <div class="title">
       二级菜单
       <p class="item">2.1</p>
       <p class="item">2.2</p>
       <p class="item">2.3</p>
   </div>
   <div class="title">
       三级菜单
       <p class="item">3.1</p>
       <p class="item">3.2</p>
       <p class="item">3.3</p>
   </div>
   
   <script>
       $(".title").on(
           "click", function () {
               // 先给 所有的 带 item 的标签加 hide
               $(".item").addClass("hide")
               // 把自己的儿子的全部移除掉
               $(this).children().removeClass("hide")
           }
       )
   </script>
   </body>
   </html>
   ```

2. 创建Django项目的注意事项

   ```python
   # 计算机名称不要出现中文
   # python解释器版本不同可能会出现启动报错
   # 项目中所有的文件名称不要出现中文
   # 多个项目文件尽量不要嵌套,做到一项一夹
   ```

3. 打开Django的注意事项

   ```python
   # 打开的项目有两层
   # 如果报错了启动Django项目支持 settings-languagesxxx-django
   ```

4. 如何创建项目，启动项目

   ```python
   # 创建
   # 1.使用命令
   django-admin startproject demo01
   # 2.Pycharm new project 创建
   
   # 启动
   # 1.指令 打开目录终端
   python manage.py runserver [IP:PORT]
   # 2.Pycharm启动 按按钮
   ```

5. 如何做解释器的多版本共存

   ```python
   # Edit configuration 里 可以修改当前项目使用的解释器
   ```

   




## 02

1. 演示什么是冒泡事件。如何阻止事件冒泡

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
       <script src="query.min.js"></script>
   </head>
   <body>
   <div id="d1">
       我是 div
       <p id="p1">
           我是 p
           <span id="s1">
               我是 span
           </span>
       </p>
   </div>
   
   <form action="">
       <input type="submit" id="submit">
   </form>
   
   <script>
       // 阻止事件后续执行
       $("#submit").click(function (e) {
           // return false
   
           // 阻止当前标签的默认行为
           e.preventDefault()
       })
   
       // 阻止事件冒泡
       // 在不断嵌套的标签内 会发现点击内部标签会持续触发后续的 逻辑代码
       $("#d1").click(
           function (e) {
               alert(`我出来了 ${$(this).text()}`)
               console.log(e)
           }
       )
       $("#p1").click(
           function (e) {
               alert(`我出来了 ${$(this).text()}`)
               e.stopPropagation()
           }
       )
       $("#s1").click(
           function (e) {
               alert(`我出来了 ${$(this).text()}`)
               e.stopPropagation()
           }
       )
   
       // 方案一：阻止后续代码执行 在函数内部 用 return false 来告诉 js 代码后续不需要执行
   
       // 方案二：内置了一个方法帮助我们阻止事件后续执行
       // 在函数的参数上 有一个对象e 执行  e.stopPropagation()
   
   </script>
   </body>
   </html>
   ```

   

2. 如何阻止事件的默认行为

   ```html
   <script>
   // 代码同上 使用的方法为：
   e.preventDefault()
   </script>
   ```

   

3. Django连接MySQL数据库有哪些注意事项

   ```python
   # 配置
   # 补丁
   ```

   

4. 说说你知道哪些request对象的方法

   ```python
   request.method
   # 获取发起请求的请求方式
   
   request.POST
   # 获取输入的请求数据
   
   get/getlist
   # 最后一个元素 / 列表
   ```

   

5. 什么orm框架，Django如何操作的

   ```python
   # ORM是一种将对象与关系型数据库之间的映射的技术
   
   # ● 与其他ORM框架相比，Django ORM拥有以下优点：
   # ● 简单易用：Django ORM的API非常简单，易于掌握和使用。
   # ● 丰富的API：Django ORM提供了丰富的API来完成常见的数据库操作，如增删改查等，同时也支持高级查询和聚合查询等操作。
   # ● 具有良好的扩展性：Django ORM可以与其他第三方库进行无缝集成，如Django REST framework、Django-Oscar等。
   # ● 自动映射：Django ORM支持自动映射，开发者只需要定义数据库表的结构，就可以自动生成相应的Python类，从而减少开发过程中的重复代码量。
   
   # 1.定义模型表
   from django.db import models
   
   # 定义一个类 必须继承 models.Model
   class Student(models.Model):
       # DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
       name = models.CharField(max_length=32)
       age = models.IntegerField()
   
   # 2.迁移模型表生成数据迁移记录
   # 现在是用Python代码写的数据库结构
   # 将Python代码转换为 SQL 语句结构
   
   python manage.py makemigrations
   # 将 Python 定义的数据库结构进行转换
   
   # 3.将生成的SQL记录迁移进MySQL数据库中
   python manage.py migrate
   
   # 4.打开数据库刷新
   # 看到很多表
   # 刚才创建的表是以 app名字_类名来定义的
   ```

   

   

## 03

1. Django的生命周期流程

   ```python
   # 当一个请求进入到Django到走出Django到浏览器渲染的全生命流程
   
   # 请 求 进 入  生命开始
   # 到浏览器渲染  生命结束
   
   # 【一】地址栏回车到浏览器渲染的流程
   # 【1】在地址栏回车（浏览器输入地址）
   # ---> 根服务器解析 IP 和 PORT
   # 【2】浏览器向 IP 和 PORT 发起请求
   # 【3】请求进入到服务端
   # 服务端处理请求 ---> 返回数据
   # 【4】服务器打包响应数据然后返回
   # 【5】浏览器接收到返回的数据 并渲染
   
   # 【二】Django的请求进入到走
   # 【1】浏览器获取到IP和PORT
   # 发起请求 ---> 走到Django 里面
   # 【2】wsgiref 模块 打包
   # 将你的浏览器请求进行打包 打包成符合 HTTP 协议的请求对象
   # 【3】去Django框架里面
   # （1）中间件 --> 整个Django框架的保安 对请求进行校验
   # （2）urls.py 路由系统 ---> 对请求中的路径进行分发 ---> 找到对应的视图函数
   # （3）view.py 视图系统 ---> 对请求中携带的数据或其他数据进行处理 ---> 返回一个 Django的响应对象 三板斧
   # （4）中间件 --> 整个Django框架的响应数据进行校验
   # 【4】wsgiref 模块 打包
   # 八Django的相应数据打包成符合 HTTP 协议的响应数据
   # 【5】回给客户端 （浏览器） 进行渲染
   ```

   

2. Django的数据库orm操作

   模型定义 创建表 增删改查这些

   ```python
   from django.db import models
   
   # Create your models here.
   
   # ORM 框架中的类对应数据库中的表
   class Student(models.Model):
       # ORM 框架中的属性对应数据库表中的字段
       # name varchar(32)
       name = models.CharField(max_length=32)
   
       # age int()
       # age = models.IntegerField()
       age = models.CharField(max_length=32)
   
       # gender varchar(32)
       # gender = models.CharField(max_length=32, default="")
   
       # 在当前类下面配置一个 类 Meta
       class Meta:
           # 这个属性就是定义当前数据库中的表明的
           # 如果不配置 就是 以当前 app名_类名 命名
           db_table = "student"
   
   # ORM 框架中的实例对应数据库表中的记录
   
   
   ###
   
   # 【一】前提
   # 在Django中操作Django的模型表 必须让 Django 处于运行的状态
   # 必须启动 Django 才能操作Django 的模型表
   # 操作Django的模型表必须在视图函数内
   
   # 【二】但是
   # 有别的方法
   # 为了测试数据的方便 Django提供给我们一种 解决办法
   # 第一步：创建一个 main 入口
   '''
   if __name__ == '__main__':
   '''
   # 第二步：打开 manage.py 文件
   # copy main 函数下面的 第一行代码
   # os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'djangoProject03.settings')
   # 第三步：导入Django模块
   # import django
   # 第四步：执行Django启动命令
   #  django.setup()
   # 第五步：导入Django模型表
   # from user.models import Student
   # 第六步：操作模型表
   
   # 这个运行环境是一次性的 执行完就自己关闭Django项目
   # 如果我以当前模式启动Django项目 http://127.0.0.1:8000/index
   # 问能看到 index 路径下的页面吗？
   # 看不到我们的视图函数以及定义的页面
   # 只能让我们临时操作Django ---> 临时进行数据库操作
   
   # 第一步：创建一个 main 入口
   if __name__ == '__main__':
       import os
   
       # 第二步：打开 manage.py 文件
       os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'djangoProject03.settings')
   
       # 第三步：导入Django模块
       import django
   
       # 第四步：执行Django启动命令
       django.setup()
   
       # 第五步：导入Django模型表
       from user.models import Student
   
       # 第六步：操作模型表
       # 【1】增加数据
       # 语法：model.objects.create(字段名=字段值)
       # 新增一个人 name=dream age=8
   
       # （1）方式一：直接模型表.objects.create 常用的 ---> 我们常用
       # result = Student.objects.create(name="dream",age=18)
       # print(result) # Student object (2)
       # Django中如果 数据库中是字符串类型 在 创建语句中 会对数字类型进行强转
   
       # （2）方式二：直接模型表(字段名=字段值).save() --- 少用
       # student_obj = Student(name="dream01", age=18)
       # print(student_obj)  # Student object (None) 这里执行结束没有数据库记录生成
       # result = student_obj.save() # 相当于是提交事务 将数据插入到数据库中
       # print(result)  # None
   
       # 【2】查看数据
       # （1）方式一： 模型表.objects.all()
       '''
       result = Student.objects.all()
       print(result)  # 返回值是一个QuerySet对象并且会将数据库中的所有结果拿到
       # <QuerySet [<Student: Student object (1)>, <Student: Student object (2)>, <Student: Student object (3)>]>
       # <Student: Student object (1)>
       # 列表里面放的就是每一个数据
       student_obj_one = result[0]
       print(student_obj_one, type(student_obj_one))
       # Student object (1) <class 'user.models.Student'>
       # 直接通过对象.属性名获取属性值
       print(student_obj_one.name)  # dream
       print(student_obj_one.age)  # 18 
       print(student_obj_one.id)  # 1
       '''
   
       # （2）方式二：模型表.objects.get(属性名=属性值) 有就拿到具体的对象 没有 则直接报错 只能筛选一个对象对于1个就报错
   
       # 按照指定条件过滤数据 数据存在
       # student_one = Student.objects.get(id=1)
       # print(student_one)  # Student object (1)
   
       # 按照指定条件过滤数据 数据不存在
       # student_one = Student.objects.get(id=8)
       # user.models.Student.DoesNotExist: Student matching query does not exist.
   
       # 按照指定条件过滤数据  有两条数据都叫 dream
       # student_one = Student.objects.get(name="dream")
       # user.models.Student.MultipleObjectsReturned: get() returned more than one Student -- it returned 2!
   
       # （3）方式三：模型表.objects.filter()
   
       # [1] 按照指定条件过滤数据 获取到存在的数据
       # result = Student.objects.filter(id=1)
       # print(result) # 获取到的结果是 QuerySet 对象 返回值是一个列表
       # # <QuerySet [<Student: Student object (1)>]>
       # print(result[0]) # Student object (1)
   
       # [2]按照指定条件过滤数据 获取到存在的数据 获取到第一条符合的数据 / 最后一条符合的数据
       '''
       result = Student.objects.filter(id=1).first()  # === Student.objects.get(id=1)
       print(result)  # 获取到的结果是 QuerySet 对象 返回值是一个列表
       # Student object (1)
       
       result = Student.objects.filter(id=1).last()  # === Student.objects.get(id=1)
       print(result)  # 获取到的结果是 QuerySet 对象 返回值是一个列表
       # Student object (1)
       '''
   
       # [3]按照指定条件过滤数据 获取到不存在的数据  -- 空数据不会报错
       # result = Student.objects.filter(id=8)
       # print(result)  # 获取到的结果是 QuerySet 对象 返回值是一个列表
       # # <QuerySet []>
   
       # [4] 获取到多个数据 -- 不会报错
       # result = Student.objects.filter(name="dream")
       # print(result)  # 获取到的结果是 QuerySet 对象 返回值是一个列表
       # # <QuerySet [<Student: Student object (1)>, <Student: Student object (2)>]>
   
       # （4）方式四：过滤掉指定的数据 模型表.objects.exclude(属性名=属性值)
   
       # 按照指定条件过滤数据 排出掉当前符合条件的数据
       # result = Student.objects.exclude(name="dream")
       # print(result)  # 获取到的结果是 QuerySet 对象 返回值是一个列表
       # <QuerySet [<Student: Student object (3)>]>
   
       # (总结)
       # 模型表.objects.all()
       #   获取所有数据
       #   返回值是 QuerySet 对象 是一个列表
       # 模型表.objects.get()
       #   返回指定条件的数据
       #   返回值是一个 具体的对象
       #   只能获取1个 超过1个就报错 没有符合条件的数据也会报错
       # 模型表.objects.filter()
       #   返回过滤后的数据 符合条件的数据
       #   返回值是一个 QuerySet 对象 是一个列表
       #   没有符合条件的数据就是空的 QuerySet 对象 是一个空列表
       #   符合条件的多个数据 QuerySet 对象 是一个列表 ---> .first() / .last() 获取第一条或者最后一条数据
       # 模型表.objects.exclude()
       #   返回过滤后的数据 排除符合条件的数据
       #   返回值是一个 QuerySet 对象 是一个列表
   
       # 【3】删除数据
       # （1）方式一： 模型表.objects.filter(属性名=属性值).delete()
   
       # 只有一条符合数据的时候
       # Student.objects.filter(id=1).delete()
   
       # 有多条数据符合 --- 会删除所有符合条件的数据
       # Student.objects.filter(name="dream").delete()
   
       # （2）方式二：先过滤出制定的对象 然后再删除指定对象
       # student_obj = Student.objects.get(id=3)
       # student_obj.delete()
   
       # 【4】修改数据
       # Student.objects.create(
       #     name="dream",
       #     age=18
       # )
   
       # （1）方式一：模型表.objects.filter(属性名=属性值).update(新的参数)
       # 按照指定的参数进行过滤然后修改 返回值是修改成功的条件
       # result = Student.objects.filter(id=4).update(
       #     name="dream_1"
       # )
       # print(result) # 1
   
       # 同时修改三条符合条件的数据
       # result = Student.objects.filter(name="dream").update(
       #     age=999
       # )
       # print(result)  #3
       #
       # （2）方式二：对象.属性名=属性值 一定不要忘了 .save() 触发保存
       student_obj = Student.objects.get(id=4)
       student_obj.name = "dream_2"
   
       student_obj.save()
   ```

   

3. Django的数据库和MySQL对应关系，如何将Django中定义的模型表变成数据库中的

   ```python
   # 1.定义模型表
   from django.db import models
   
   # 定义一个类 必须继承 models.Model
   class Student(models.Model):
       # DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
       name = models.CharField(max_length=32)
       age = models.IntegerField()
   
   # 2.迁移模型表生成数据迁移记录
   # 现在是用Python代码写的数据库结构
   # 将Python代码转换为 SQL 语句结构
   
   python manage.py makemigrations
   # 将 Python 定义的数据库结构进行转换
   
   # 3.将生成的SQL记录迁移进MySQL数据库中
   python manage.py migrate
   
   # 4.打开数据库刷新
   # 看到很多表
   # 刚才创建的表是以 app名字_类名来定义的
   ```

   

4. 写出你知道的request对象的方法有哪些

   ```py
   request.method
   # get
   # post
   
   request.GET
   request.POST
   
       # META -- 后面讲
       # COOKIES  -- 后面讲
       # FILES  -- 后面讲
       # body  -- 后面讲
       # headers  -- 后面讲
       # is_ajax -- 后面讲
   
       # path  -- 后面讲
       # path_info  -- 后面讲
   







