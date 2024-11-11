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
   







## 04

1. Django的生命周期流程

2. 什么是反向解析

3. 反向解析如何传递参数

4. 什么是路径转换器，有哪些。如何自定义
5. 什么是http协议，特点有哪些







## 05

1. CBV源码剖析

2. Django的模版语法渲染可迭代类型有哪些特点

   str list dict set tuple

   渲染的时候 可以x 也可以 x.0 字典还可以x.key/value/age 但是都不可以x[0]等取值方式

3. 反向解析如何传递参数给指定路由

   ```python
   
   # 无名分组 直接用 args
   url = reverse('article_detail', args=[42])
   # 有名分组 直接用 args 或者 还可以用kwargs 但键 即下面的id 必须和路由重定义的关键字一致
   url = reverse('article_detail', kwargs={'id': 42})
   ```

   



## 06

1. 列出你所知道的orm语句有哪些，各自的功能是什么

   ```py
   # model.objects. ...
   # 1. .create(field_name=filed_value) 新增数据
   # 2. model(field_name=filed_value).save() 新增数据
   # 3. .all() 查询当前表中的所有数据
   # 4. .filter(field_name=filed_value) 按照指定字段过滤数据 返回的是queryset对象
   # 5. .filter(field_name=filed_value).first() 数据集中的第一条数据
   # 6. .filter(field_name=filed_value).last() 数据集中的最后一条数据
   # 7. .get(field_name=filed_value) 按照指定字段查找指定对象 多了报错 没有报错
   # 8. .filter(field_name=filed_value).update(field_name=filed_value) 按照指定字段过滤数据后更新数据
   # 9. .filter(field_name=filed_value).delete() 按照指定字段过滤后删除数据
   # 10. .values(field_name) 提取当前数据集中的数据 返回的字典
   # 11. .value_list(field_name) 提取当前数据集中的数据 返回的是元组
   # 12. .values(field_name).distinct() 对指定字段进行去重
   # 13. .order_by(field_name) 按照升序对数据进行排序
   # 14. .order_by(-field_name) 按照降序对数据进行排序
   # 15. .order_by(field_name).reverse() 对排序后的数据进行翻转
   # 16. .count() 对查询到的数据进行计数
   # 17. .exclude(field_name) 对查询到的结果排除掉指定字段的内容
   # 18. .filter(field_name=filed_value).exists() 对过滤后的数据判断存在与否
   # 补充 .query 查看当前 orm 语句对应的sql语句 ---> 必须是 queryset 对象
   ```

   

2. 多对多创建表关系的几种方式和各自的特点

   ```python
   # 1.方案一：直接自己写第三张表
   """
   # 创建一张图书表
   class Book(models.Model):
       # 图书名
       title = models.CharField(max_length=255, verbose_name="书籍名")
       price = models.CharField(max_length=255, verbose_name="书籍价格")
       # 注册时间 auto_now 在第一次创建的时候自动加上当前时间 更新的时候不变
       create_time = models.DateTimeField(verbose_name="注册时间", auto_now_add=True)
       # 更新时间 auto_now_add  第一注册会加 然后每一次更新数据 会随着更新
       update_time = models.DateTimeField(verbose_name="更新", auto_now=True)
   
       class Meta:
           db_table = "book"
   
   
   # 创建一张作者表
   class Author(models.Model):
       name = models.CharField(max_length=32, verbose_name="姓名")
       age = models.IntegerField(verbose_name="年龄")
       # 注册时间 auto_now 在第一次创建的时候自动加上当前时间 更新的时候不变
       create_time = models.DateTimeField(verbose_name="注册时间", auto_now_add=True)
       # 更新时间 auto_now_add  第一注册会加 然后每一次更新数据 会随着更新
       update_time = models.DateTimeField(verbose_name="更新", auto_now=True)
   
       class Meta:
           db_table = "author"
   
   
   class BookToAuthor(models.Model):
       # 作者 CASCADE 级联更新 + 级联删除
       author = models.ForeignKey(to="Author", on_delete=models.CASCADE)
       # 图书
       book = models.ForeignKey(to="Book", on_delete=models.CASCADE)
       # 注册时间 auto_now 在第一次创建的时候自动加上当前时间 更新的时候不变
       create_time = models.DateTimeField(verbose_name="注册时间", auto_now_add=True)
       # 更新时间 auto_now_add  第一注册会加 然后每一次更新数据 会随着更新
       update_time = models.DateTimeField(verbose_name="更新", auto_now=True)
   
       class Meta:
           db_table = "author_to_book"
   """
   
   # 2.方案二：自己创建第三张表 然后再某一张表创建多对多字段
   """
   # 创建一张图书表
   class Book(models.Model):
       # 图书名
       title = models.CharField(max_length=255, verbose_name="书籍名")
       price = models.CharField(max_length=255, verbose_name="书籍价格")
       # 注册时间 auto_now 在第一次创建的时候自动加上当前时间 更新的时候不变
       create_time = models.DateTimeField(verbose_name="注册时间", auto_now_add=True)
       # 更新时间 auto_now_add  第一注册会加 然后每一次更新数据 会随着更新
       update_time = models.DateTimeField(verbose_name="更新", auto_now=True)
   
       class Meta:
           db_table = "book"
   
   
   # 创建一张作者表
   class Author(models.Model):
       name = models.CharField(max_length=32, verbose_name="姓名")
       age = models.IntegerField(verbose_name="年龄")
       # 注册时间 auto_now 在第一次创建的时候自动加上当前时间 更新的时候不变
       create_time = models.DateTimeField(verbose_name="注册时间", auto_now_add=True)
       # 更新时间 auto_now_add  第一注册会加 然后每一次更新数据 会随着更新
       update_time = models.DateTimeField(verbose_name="更新", auto_now=True)
   
       # 创建一个一对多字段
       book = models.ManyToManyField(
           # 关联到那张表
           to="Book",
           # 自己创建的第三张表的表明
           through="BookToAuthor",
           # 需要关联的字段
           through_fields=("author", "book"),
       )
   
       class Meta:
           db_table = "author"
   
   
   class BookToAuthor(models.Model):
       # 作者 CASCADE 级联更新 + 级联删除
       author = models.ForeignKey(to="Author", on_delete=models.CASCADE)
       # 图书
       book = models.ForeignKey(to="Book", on_delete=models.CASCADE)
       # 注册时间 auto_now 在第一次创建的时候自动加上当前时间 更新的时候不变
       create_time = models.DateTimeField(verbose_name="注册时间", auto_now_add=True)
       # 更新时间 auto_now_add  第一注册会加 然后每一次更新数据 会随着更新
       update_time = models.DateTimeField(verbose_name="更新", auto_now=True)
   
       class Meta:
           db_table = "author_to_book"
   """
   
   # 3.方案三：在关联表中 用多对多字段关联第三张表 Django会帮助我们创建第三张表
   # 之后会学 多对多表数据的增删查改
   # 从图书表查询到作者表 对多对字段就有相应的渐变方法
   # 跨表操作的时候 也有方便的方法
   """
   # 创建一张图书表
   class Book(models.Model):
       # 图书名
       title = models.CharField(max_length=255, verbose_name="书籍名")
       price = models.CharField(max_length=255, verbose_name="书籍价格")
       # 注册时间 auto_now 在第一次创建的时候自动加上当前时间 更新的时候不变
       create_time = models.DateTimeField(verbose_name="注册时间", auto_now_add=True)
       # 更新时间 auto_now_add  第一注册会加 然后每一次更新数据 会随着更新
       update_time = models.DateTimeField(verbose_name="更新", auto_now=True)
   
       class Meta:
           db_table = "book"
   
   
   # 创建一张作者表
   class Author(models.Model):
       name = models.CharField(max_length=32, verbose_name="姓名")
       age = models.IntegerField(verbose_name="年龄")
       # 注册时间 auto_now 在第一次创建的时候自动加上当前时间 更新的时候不变
       create_time = models.DateTimeField(verbose_name="注册时间", auto_now_add=True)
       # 更新时间 auto_now_add  第一注册会加 然后每一次更新数据 会随着更新
       update_time = models.DateTimeField(verbose_name="更新", auto_now=True)
   
       # 创建一个多对多字段
       book = models.ManyToManyField(
           # 关联到那张表
           to="Book"
       )
   
       class Meta:
           db_table = "author"
   """
   ```

   

3. Django连接MySQL数据库的注意事项

   ```python
   # 【三】连接MySQL 创建模型表
   # 1.配置数据库参数
   # setting.py
   """
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': "django08data",
           "USER": 'root',
           "PASSWORD": '123456',
           "HOST": '127.0.0.1',
           "PORT": '3306',
           "CHARSET": 'utf8mb4',
       }
   }
   """
   
   # 2.注入补丁或安装参数
   # __init__.py
   """
   import pymysql
   
   pymysql.install_as_MySQLdb()
   """
   
   # 3.创建模型表
   # user/model.py
   """
   class User(models.Model):
       name = models.CharField(max_length=32, verbose_name="姓名")
       age = models.IntegerField(verbose_name="年龄")
       # 注册时间 auto_now 第一次创建时自动加上当前时间 后面也不会变
       register_time = models.DateTimeField(verbose_name="注册时间", auto_now=True)
       # 更新时间 auto_now_add 第一次注册会加 然后每一次更新数据 都会随着更新改变
       update_time = models.DateTimeField(verbose_name="更新时间", auto_now_add=True)
       
       # 配置表 元信息
       class Meta:
           # 表名
           db_table = "user"
   """
   
   # 4.迁移数据库
   # 第一步生成迁移记录: python manage.py makemigrations
   # 第二步迁移记录生效: python manage.py migrate
   # 去run task - manage.py@项目名 那里跑
   # 一定要先建好数据库 再去迁移
   
   # 然后就可以在Pycharm里表了
   # 打开右侧Database 选择mysql 输入用户密码和数据库名字进行连接
   # 刷新后 可以看到有上面的db_table名字 如果没刷新出来 需要排错
   ```

   

4. 什么是事务，事务的特点

   事务是指一系列相关操作的集合，这些操作被视为一个不可分割的工作单元。

   ACID 原子性、一致性、隔离性、持久性







## 07

1. 你都知道哪些转换器，如何自定义转换器

   ```python
   # str 匹配除了 '/' 之外的非空字符串。
   # int 匹配 0 或任何正整数
   # slug 匹配任意由 ASCII 字母或数字以及连字符和下划线组成的短标签。
   # uuid 匹配一个格式化的 UUID 。
   # path 匹配非空字段，包括路径分隔符 '/' 。
   
   # 【3】自定义路径转换器
   # （1）先创建一个文件 任意命名 比如 path_converters.py
   # （2）在当前文件中定义自定义转换器的逻辑
   """
   # 【1】创建一个类 --->必须按照年份传入数据 2020 ---> 转成数字类型
   class FourDigitYearConverter:
       # 【2】定义正则匹配规则
       # [0-9] ---> 字符组
       # {4} {n,m} --> 贪婪匹配 最少是n 最多是 m 默认按最多取
       regex = r"[0-9]{4}"
       
   
       # 【3】定义一个函数
       def to_python(self, value):
           '''
           接收到当前路径中的参数并对参数进行转换转换成Python中的数据类型
           :return: 
           '''
           return int(value)
   
       # 【4】定义一个函数
       def to_url(self, value):
           '''
           根据上面定义的正则匹配表达式将 url 中的参数提取出来
           # 2024 ---> 2024
           :return: 
           '''
           return "%0d4" % value
   """
   # （3）使用自定义转换器
   '''
   # 【一】导入自定义的路径转换器类
   from order.path_converters import FourDigitYearConverter
   # 【二】借助Django的转换器语法转换成 Django的转换器
   from django.urls import register_converter
   
   # 【三】注册你的转换器
   register_converter(FourDigitYearConverter, "aaa")
   
   urlpatterns = [
       # 【0】自定义路径转换器
       path("self_pattern/<aaa:name>/", self_pattern, name="self_pattern"),
   ]
   '''
   ```

2. 什么是inclusion_tag 如何做

   inclusion_tag 可以用来实现 一个代码片段 但是可以在不同页面上加载

   ```python
   # 制作过程
   # 0.创建app user
   # 1.在user里创建一个文件夹 templatestags
   # 2.在这个文件夹创建 一个文件 任意名字 目前叫 CommoninclusionTag.py
   # 3.书写当前的模版逻辑
   '''
   # 【一】导入模块
   # 加一句话必须有
   from django import template
   
   # 【二】创建 register 对象
   # 这句话也固定 不要改名 易搞错
   # 需要往这个图书馆里 增加我们自定义的内容
   register = template.Library()
   
   
   # 【三】定义当前模板
   # 注册模板 - 参数 写自己的模版文件名(前端文件名)
   # 前端文件写在 templates 里面 检索到 templates 里面的文件
   # user - templates - 前端文件
   # user - templatetags - py
   @register.inclusion_tag("adv_temp_html.html")
   def adv_temp(info_data):
       # 将当前的局部名称空间返回
       return locals()
   
   
   # 【四】使用当前制作好的标签模版
   # 需要在哪个页面用就在那个页面加载
   # 加载当前 templatetags 文件夹下的 模版标签所在的文件名
   {% load CommoninclusionTag %}
   # 加载需要渲染的标签模版所在的函数名 有什么参数直接向后写 多个参数 , 分隔开
   {% adv_temp info_data %}
   # 自己写网页 待补充
   '''
   ```

3. 什么是正向什么是反向

   正向：从有外键字段的表出发去没有外键字段的表中查数据

   反向：从没有外键字段的表出发去有外键字段的表中查数据

4. 查询书籍id为1的作者的电话

   ```python
   # Book 外键 author 无外键
   book_info = Book.Object.filter(id=1).values(
       "name",
       "author__name",
   	"author__detail__phone"
   )
   
   book_info = AuthorDetail.filter(author__book__id=1).values(
       "author__book__name",
       "author__name",
   	"phone"
   )
   ```

5. 查询出版社 id为1的出版的书的作者电话

   ```python
   # publish book  author 无
   book_info = Publish.filter(author__book__id=1).values(
       "author__book__name",
       "author__name",
   	"phone"
   )
   ```






## 08

1. 什么是事务，事务有哪些特性

   事务是指一系列相关操作的集合，这些操作被视为一个不可分割的工作单元。

   ACID 原子性、一致性、隔离性、持久性

2. SQL中常用的约束条件有哪些， orm中常用的字段参数有哪些

   ```python
   """
   - 非空约束（not null）
   - 唯一性约束（unique）
   - 组合使用 not null 和 unique
   - 主键约束PK（primary key）
   - 外键约束FK（foreign key）
   """
   
   """
   null
   default
   primary_key
   unique
   choices
   """
   ```

3. 什么是闭包函数，闭包函数的应用场景

   内嵌函数对外部作用域有引用的函数就叫闭包函数

   装饰器 计时器timer 验证

   ```python
   def outer(func):
       def inner():
           ...
           return result
       return inner
   ```

4. 如何在前端页面书写定时器如何清除定时器(定时器和延时器都写

   ```js
   setInterval
   clearInterval
   setTimeout
   clearTimeout
   
   // 设置定时器/延时器
   // (1)定时器: 到指定时间就自动执行
   // 第一个参数放 延时执行的函数
   // 第二个参数放 延时的时间 以毫秒为单位
   // 效果就是隔执行时间自动执行
   setInterval(
       function () {
           console.log("定时器执行了!")
       },
       2000
   )
   
   // (2)延时器: 延迟指定时间后再执行
   // 第一个参数 延时执行的函数
   // 第二个参数 延时的时间 以毫秒为单位
   // 效果 延迟 指定时间自动执行
   setTimeout(
       function() {
           window.alert("延时器执行了!")
       },
       2000
   )
   
   // 删除定时器/延时器
   // (1)删除定时器
   // 参数是当前设置定时器时候的 定时器 ID
   clearInterval(
       10
   )
   
   // (2)删除延时器
   // 参数是当前设置延时器时候的 延时器 ID
   clearTimeout(
       10
   )
   ```

5. 如何在前端获取input输入框中的内容

   ```js
   // input标签 设置id=i1
   
   // js里
   var inputElement = document.getElementById("i1");
   var value = inputElement.value
   ```

6. 如何给标签绑定事件

   ```js
   // 1.方式一：
   // 获取到当前的标签对象直接 .click 绑定事件
   // 点击事件
   $("#btn").click(
       ()=>{
           alert(233)
       }
   )
   
   // 2.方式二：绑定事件类型去绑定事件
   // 获取到当前标签对象然后 .on绑定事件类型
   $("#btn").on("click", () => {
       alert(233)
   })
   ```

   



## 09

1. 什么是ajax

   AJAX，翻译成中文就是“异步的Javascript和XML”。

   就是使用Javascript语言与服务器进行异步交互，传输的数据为XML。

   可以在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。

2. ajax模版，两个，一个是发送普通键值对，一个是发送文件数据

   ```html
   <script>
       // Ajax 是 js 代码封装的
       $(document).ready(
           //
           $("#btn_submit").click(function () {
               // 获取到用户输入的用户名和密码
               let username = $("#username").val()
               let password = $("#password").val()
   
               // 向后端发起请求 发送当前的数据
               // 标准格式
               $.ajax({
                   // 想要提交数据的目标地址 form 表单上的 action
                   // 不写 默认就是当前访问到的路由地址 写了就是指定的路由地址
                   url: "",
   
                   // type 定义当前请求的请求方式
                   // get / post
                   type: "post",
   
                   // data : 向后端传递的数据 携带的数据 字典
                   // Ajax交互传递的事 json 格式的数据
                   data: {
                       "username": username,
                       "password": password,
                   },
   
                   // 最后一个必要的参数
                   // success : 想后端提交请求之后 Ajax是 异步操作 需要对响应会的数据进行一步的额外处理
                   success: function (data) {
                       console.log(data.success)
                   }
               })
           })
       )
   </script>
   
   
   <script>
       // Ajax 是 js 代码封装的
       $(document).ready(
           //
           $("#btn_submit").click(function () {
               // 获取到用户输入的用户名和密码
               let username = $("#username").val()
               let password = $("#password").val()
               {#let avatar = $("#avatar").val()
   
               // 让 Ajax 提交文件数据 就必须就借助另一个对象 FromData
               // 1.创建一个 FromData 对象
               let formObj = new FormData();
               // 2.将字符串数据放到 formObj 对象中
               formObj.append("username", username)
               formObj.append("password", password)
               // 3.提取出当前上传的文件对象
               let avatar = $("#avatar")[0].files[0]
               formObj.append("avatar", avatar)
   
               // 向后端发起请求 发送当前的数据
               // 标准格式
               $.ajax({
                   // 想要提交数据的目标地址 form 表单上的 action
                   // 不写 默认就是当前访问到的路由地址 写了就是指定的路由地址
                   url: "",
   
                   // type 定义当前请求的请求方式
                   // get / post
                   type: "post",
   
                   // data : 向后端传递的数据 携带的数据 字典
                   // Ajax交互传递的事 json 格式的数据
                   // 如果提交的数据是 formObj 里面带着 文件数据 用 字典就不行会报错
                   /*
                   *
                   *  {
                   "username": username,
                   "password": password,
                   "formObj": formObj
               }
                   * */
                   data: formObj,
   
                   // 4.在传输文件数据的时候增加额外的参数约束
                   // (1)告诉浏览器不需要对数据进行任何的编码 Django 可以自动识别到 FormData 对象并且提取出数据
                   contentType: false,
                   // (2)告诉浏览器不要对我已经处理过的数据进行二次处理
                   processData: false,
   
                   // 最后一个必要的参数
                   // success : 想后端提交请求之后 Ajax是 异步操作 需要对响应会的数据进行一步的额外处理
                   success: function (data) {
                       console.log(data.success)
                   }
               })
           })
       )
   </script>
   ```

3. 如何批量插入数据

   ```python
   # 使用bull_create 可以插入大量数据时节省大量时间
   
   book_all = Book.objects.bulk_create(book_obj_list)
   ```

4. 简述分页器原理和伪代码

   ```python
   # 原理
   # 想好 第 x 页的第一篇 和最后一篇位置的 数学规律
   # 再结合 切片器 顾头不顾尾
   # 每次要切的就是 [every_page*(page-1):every_page*page] (从0开始的话)
   # 伪代码
   
   def book_split_page(request):
       # 当前页 current_page 从当前的 请求地址中获取
       # 起始页(起始索引) start_page
       # 结束页(结束索引) end_page
       # 每一页的条数 per_page_num
   
       # 【一】获取到当前所有的图书对象
       book_all = Book.objects.all()
       # 【二】获取到当前的页码 current_page 从当前的 请求地址中获
       # http://localhost:8000/user/book_split_page/?page=1
       current_page = request.GET.get("page", 1)
       # 对当前页码进行判断是否是数字类型
       try:
           current_page = int(current_page)
       except:
           current_page = 1
       # 【三】每一页的条数 per_page_num
       per_page_num = 1000
       # 【四】开始创建分页参数
       # 1.知道一共有多少页 --- 用 divmod
       page_all, page_last = divmod(book_all.count(), per_page_num)
       if page_last > 0:
           page_all += 1
       # 2.起始页(起始索引) start_page
       start_page = (current_page - 1) * per_page_num
       # 3.结束页(结束索引) end_page
       end_page = current_page * per_page_num
   
       # 【五】对查询出的所有数据进行切片
       query_set = book_all[start_page:end_page]
       print(query_set, len(query_set))
   
       return render(request, "book_split_page.html", locals())
   ```

   



## 10

用form组件实现注册功能

自己创建模型表，创建组件类，校验规则

字段：用户名，密码，确认密码，爱好，性别，头像文件。除了确认密码，其他都保存到数据库中

```python
class RegisterView(View):
    def get(self, request, *args, **kwargs):
        form_obj = RegisterForm()
        gender_choices = User.gender_choices
        hobby_choices = User.hobby_choices
        return render(request, "register.html", locals())

    def post(self, request, *args, **kwargs):
        print(request.POST)
        
        
# models.py
class User(models.Model):
    gender_choices = (
        (0, 'male'),
        (1, 'female')
    )
    hobby_choices = {
        1: "唱",
        2: "跳",
        3: "rap",
        4: "篮球"
    }
    username = models.CharField(max_length=32)
    password = models.CharField(max_length=32)
    hobby = models.CharField(max_length=32)
    gender = models.IntegerField(choices=gender_choices)
    avatar = models.CharField(max_length=32)
    # 用户名，密码，确认密码，爱好，性别，头像文件
```

```html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="{% static 'js/jquery.min.js' %}"></script>
    <link href="{% static 'plugins/bootstrap/css/bootstrap.min.css' %}" rel="stylesheet">
    <script src="{% static 'plugins/bootstrap/js/bootstrap.min.js' %}"></script>

</head>
<body>
<div class="container-fluid">
    <div class="row">
        <div class="col-md-6 col-md-offset-3">
            <h1 class="text-center">这是注册页面</h1>
            <form>
                {% for form in form_obj %}
                    <div class="form-group">
                        {{ form.label_tag }}
                        {{ form }}
                    </div>
                {% endfor %}
                {# 性别的单选框 #}
                <div class="form-group">
                    <b>性别 :>>>></b>
                    <label class="radio-inline">
                        <input type="radio" name="inlineRadioOptions" id="gender_male" value="0"> 女
                    </label>
                    <label class="radio-inline">
                        <input type="radio" name="inlineRadioOptions" id="gender_female" value="1"> 男
                    </label>
                </div>
                {# 爱好多选框 #}
                <div class="form-group">
                    <b>爱好 :>>>></b>
                    <label class="checkbox-inline">
                        <input type="checkbox" id="hobby_1" value="1"> 唱
                    </label>
                    <label class="checkbox-inline">
                        <input type="checkbox" id="hobby_2" value="2"> 跳
                    </label>
                    <label class="checkbox-inline">
                        <input type="checkbox" id="hobby_3" value="3">rap
                    </label>
                    <label class="checkbox-inline">
                        <input type="checkbox" id="hobby_4" value="4"> 篮球
                    </label>
                </div>
                {# 头像上传框 #}
                <div class="form-group">
                    <label for="avatar">
                        <b>头像 :>>>></b>
                        <br>
                        <input type="file" id="avatar">
                    </label>
                </div>
                <div class="text-center form-group">
                    <input type="button" class="btn btn-success form-control" id="btn_submit" value="注册">
                </div>
            </form>
        </div>
    </div>
</div>

<script>

    $("#btn_submit").click(function () {
        // 上传文件需要借助 formData 对象
        // 【一】获取到用户名的值
        function getInputValue() {
            let formObj = new FormData()
            let userName = $("#id_username").val()
            let passWord = $("#id_password").val()
            let confirmPassWord = $("#id_confirm_password").val()
            // forEach 可以有四个参数 填两个 前面是 元素对应的索引 后边是当前元素
            let hobby = []
            for (item of $("input:checkbox:checked")) {
                hobby.push($(item).val())
            }
            let gender = $($("input:radio:checked")[0]).val()
            let avatar = $("#avatar")[0].files[0]
            formObj.append("username", userName)
            formObj.append("password", passWord)
            formObj.append("confirm_password", confirmPassWord)
            formObj.append("hobby", hobby.join(","))
            formObj.append("gender", gender)
            formObj.append("avatar", avatar)
            return formObj
        }

        $.ajax({
            url: "",
            type: "post",
            data: getInputValue(),
            contentType: false,
            processData: false,
            success: function (data) {
                console.log(data)
            }
        })
    })

</script>
</body>
</html>
```







## 11

1. 什么是静态方法，什么是动态方法，有什么区别

   分类 定义 调用方式

   静态方法 - 非绑定方法 @statisticmethod 

   cls.func() obj.func()

   动态方法 - 绑定给类 @classmethod

   obj.func() / cls.func() 都不需要传入额外的参数

   动态方法 - 绑定给对象 

   会自动把这个obj作为self传入 / cls.func(obj) 需要主动传入对象  

2. 什么是http协议。什么特点

   超文本传输协议 用于传输与超文本相关的资源文件 是一种无状态网络应用协议

   基于请求和响应的模型

   基于TCP和IP之上的应用层协议

   无状态

   短连接

3. 什么是事务，什么特点

   事务是指一系列相关操作的集合，这些操作被视为一个不可分割的工作单元。

   ACID 原子性、一致性、隔离性、持久性

4. Django连接MySQL有哪些注意事项

   ```python
   # 【三】连接MySQL 创建模型表
   # 1.配置数据库参数
   # setting.py
   """
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': "django08data",
           "USER": 'root',
           "PASSWORD": '123456',
           "HOST": '127.0.0.1',
           "PORT": '3306',
           "CHARSET": 'utf8mb4',
       }
   }
   """
   
   # 2.注入补丁或安装参数
   # __init__.py
   """
   import pymysql
   
   pymysql.install_as_MySQLdb()
   """
   
   # 3.创建模型表
   # user/model.py
   """
   class User(models.Model):
       name = models.CharField(max_length=32, verbose_name="姓名")
       age = models.IntegerField(verbose_name="年龄")
       # 注册时间 auto_now 第一次创建时自动加上当前时间 后面也不会变
       register_time = models.DateTimeField(verbose_name="注册时间", auto_now=True)
       # 更新时间 auto_now_add 第一次注册会加 然后每一次更新数据 都会随着更新改变
       update_time = models.DateTimeField(verbose_name="更新时间", auto_now_add=True)
       
       # 配置表 元信息
       class Meta:
           # 表名
           db_table = "user"
   """
   
   # 4.迁移数据库
   # 第一步生成迁移记录: python manage.py makemigrations
   # 第二步迁移记录生效: python manage.py migrate
   # 去run task - manage.py@项目名 那里跑
   # 一定要先建好数据库 再去迁移
   
   # 然后就可以在Pycharm里表了
   # 打开右侧Database 选择mysql 输入用户密码和数据库名字进行连接
   # 刷新后 可以看到有上面的db_table名字 如果没刷新出来 需要排错
   ```

5. 什么是数据库范式。什么是三大范式

   范式就是在设置数据库的表时，一些共同需要遵守的规范

   第一范式：确保原子性，表中每一个列数据都必须是不可再分的字段。

   第二范式：确保唯一性，每张表都只描述一种业务属性，一张表只描述一件事

   第三范式：确保独立性，表中除主键外，每个字段之间不存在任何依赖，都是独立的。

   



## 12

1. 什么是范式，你知道那些数据库范式

   范式就是一些共同需要遵守的规范

   第一范式：确保原子性，表中每一个列数据都必须是不可再分的字段。

   第二范式：确保唯一性，每张表都只描述一种业务属性，一张表只描述一件事

   第三范式：确保独立性，表中除主键外，每个字段之间不存在任何依赖，都是独立的。

2. 什么是事务，事务的四大特点。

   事务是指一系列相关操作的集合，这些操作被视为一个不可分割的工作单元。

   ACID 原子性、一致性、隔离性、持久性

3. 给你一段html代码前端，如果我不想要其中的script标签及中间的内容，你能想到哪些方案

   ```html
   // 前端
   // 后端
   ```

   

4. 你知道哪些魔法方法，各自的作用是什么

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

   

5. ajax传输文件数据有哪些注意事项

   ```html
   // 其他数据格式是一起去传递
   // 文件数据格式需要专门进行传递
   <script>
       // Ajax 是 js 代码封装的
       $(document).ready(
           //
           $("#btn_submit").click(function () {
               // 获取到用户输入的用户名和密码
               let username = $("#username").val()
               let password = $("#password").val()
               {#let avatar = $("#avatar").val() #}
   
               // 让 Ajax 提交文件数据 就必须就借助另一个对象 FromData
               // 【1】创建一个 FromData 对象
               let formObj = new FormData();
               // 【2】将字符串数据放到 formObj 对象中
               formObj.append("username", username)
               formObj.append("password", password)
               // 【3】提取出当前上传的文件对象
               let avatar = $("#avatar")[0].files[0]
               formObj.append("avatar", avatar)
   
               // 向后端发起请求 发送当前的数据
               // 标准格式
               $.ajax({
                   // 想要提交数据的目标地址 form 表单上的 action
                   // 不写 默认就是当前访问到的路由地址 写了就是指定的路由地址
                   url: "",
   
                   // type 定义当前请求的请求方式
                   // get / post
                   type: "post",
   
                   // data : 向后端传递的数据 携带的数据 字典
                   // Ajax交互传递的事 json 格式的数据
                   // 如果提交的数据是 formObj 里面带着 文件数据 用 字典就不行会报错
                   /*
                   *
                   *  {
                   "username": username,
                   "password": password,
                   "formObj": formObj
               }
                   * */
                   data: formObj,
   
                   // 【4】在传输文件数据的时候增加额外的参数约束
                   // （1）告诉浏览器不需要对数据进行任何的编码 Django 可以自动识别到 FormData 对象并且提取出数据
                   contentType: false,
                   // （2）告诉浏览器不要对我已经处理过的数据进行二次处理
                   processData: false,
   
                   // 最后一个必要的参数
                   // success : 想后端提交请求之后 Ajax是 异步操作 需要对响应会的数据进行一步的额外处理
                   success: function (data) {
                       console.log(data.success)
                   }
               })
           })
       )
   </script>
   ```

   

6. 什么是装饰器，什么是迭代器，什么是生成器

   在不修改被装饰对象源代码和调用方式的前提下为被装饰对象添加额外的功能

   迭代取值的工具 既有`__iter__`也有`__next__`方法的对象

   g = (x * 2 for x in range(5))

   def my_generator():
       yield 1
       yield 2
       yield 3

   节省内存 惰性计算 无限序列





## 13

1. 什么是装饰器，什么是迭代器，什么是生成器

   装饰器：在不修改被装饰对象源代码和调用方式的前提下为被装饰对象添加额外的功能

   迭代器：迭代取值的工具，迭代器对象是既有`__iter__`也有`__next__`方法的对象

   生成器：在需要时生成数据，而不必提前从内存中生成并存储整个数据集。

2. 你知道哪些魔法方法，各自的作用是什么

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

3. 什么是闭包函数，闭包函数的用途

   内嵌函数对外部作用域有引用的函数就叫闭包函数

   可以用来保存状态和实现装饰器
