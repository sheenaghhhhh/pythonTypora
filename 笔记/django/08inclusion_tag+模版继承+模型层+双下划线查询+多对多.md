# 1 制作inclusion_tag

```python
# 【一】场景
# 在某些情况下 会在多个页面上使用同一块内容的框架 但是内容不一样
# 比如 投放广告位
# 以前学函数 传入不同的参数 针对不同的参数做一样的事情

# 【二】在前端渲染函数 传不了参数
# Django提供了一种语法系统
# inclusion_tag 语法
# 可以实现我们想要的 做一个代码片段 在不同页面上加载当前的代码片段

# 【三】制作 inclusion_tag
# 0.创建app user
# 1.在user里创建一个文件夹 templatestags
# 2.在这个文件夹创建 一个文件 任意名字 目前叫 CommoninclusionTag.py
# 3.书写当前的模版逻辑

"""
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
"""
```



# 2 模版继承

```python
# 【一】场景
# 在某些情况下，有一个基础页面其他页面都是基于基础页面修改的
# 常见情况 导航栏

# 【二】步骤
# 1.要有基础页面
# 在基础页面行加上
"""
{% block 当前块的名字 %}
可能会被修改的内容
{% endblock %}
"""

# 2.子页面
# 继承了上面模版页面的子页面
"""
{% extends "原页面文件名" %}
{% block 原页面修改块的名字 %}
修改的内容
{% endblock %}
"""
```



# 3 模型层基础

````python
# 【一】模型层
# 在每个app 下的 model.py 都可以定义各自的 模型表
# 操作数据库的层就叫 模型层

# 【二】测试环境搭建
# 1.创建main入口
# if __name__ == '__main__':
# 2.找到manage.py 文件中的 main入口下面的第一行代码(不是注释)
#     os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'djangoProject08.settings')
# 3.导入Django模块
# import django
# 4.临时启动Django
# django.setup()
# 5.操作模型表
# 这些都写在待会的四里 这里先过一遍 明白搭建的步骤

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

# 补充:一般配置时都是写一个新的 没有创建的库
# 在Pycharm里 Terminal这边点一个加号
# 出现Local 输入mysql 再输入create database

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
# 一定要先建好数据库 才能迁移

# 然后就可以在Pycharm里表了
# 打开右侧Database 选择mysql 输入用户密码和数据库名字进行连接
# 刷新后 可以看到有上面的db_table名字 如果没刷新出来 需要排错

# 【四】操作数据库
import os
import django

if __name__ == '__main__':
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'djangoProject08.settings')
    django.setup()
    # 启动之后再导入模型表
    from user.models import User

    # 【一】插入数据
    # 1.直接插入 model.objects.create()
    # User.objects.create(name="sheenagh", age=23)
    # 2.间接插入 model().save()
    # User(name="heaven",age=100).save()
    # 3.在插入数据的时间会自动创建 我们想要插入自己的时间
    # 这里有问题 设置了 auto_now_add和auto_now=True的话 插入自己的时间虽然不会报错 但是会无效
    # import datetime
    # create_time = datetime.datetime(2023, 10, 10, 10, 30, 0)
    # User.objects.create(name="monica2", age=32, update_time=create_time)

    # 【二】删除数据
    # 1.直接删除 model.objects.filter().delete()
    # User.objects.filter(name="monica2").delete()
    # 2.间接删除 model.objects.get().delete()
    # User.objects.get(name="monica").delete()
    # get查询单条记录 所以多条或者查不到都会抛出异常

    # 【三】查看数据
    # 生成五个用户
    # for i in range(5):
    #     User.objects.create(name=f"fiveuser_{i}", age=i * i)
    # 1.按照指定条件过滤 ---> 获取到QuerySet 里面 有每一个具体的对象
    # result = User.objects.filter(name="fiveuser_2")
    # print(result)
    # <QuerySet [<User: User object (8)>]>
    # (1)获取当前数据集的第一个数据 ---> 具体的对象
    # result = User.objects.filter(name="fiveuser_2").first()
    # print(result)
    # User object (8)
    # (2)获取当前数据集的最后一个数据 ---> 具体的对象
    # result = User.objects.filter(age__gt=5).last()
    # print(result)
    # User object (10)
    # .first 和 .last 从queryset里提取一条记录 所以会以单个实例的形式返回

    # 2.按照指定条件查找 ---> 获取到的是具体的对象
    # obj = User.objects.get(name="fiveuser_2")
    # print(obj)
    # User object (8)
    # get 只能获取一个 多余一个的数据就报错
    # get 拿不到会报错

    # 3.获取当前表下的所有数据 ---> 返回了一个QuerySet QuerySet包含当前表中的所有数据-即对象
    # res = User.objects.all()
    # print(res)
    # <QuerySet [<User: User object (1)>, <User: User object (2)>, ...>

    # 【四】修改数据
    # 1.直接修改 model.objects.filter().update()
    # User.objects.filter(name="fiveuser_0").update(name="fiveuser_5")
    # 只要是 filter 过滤出的所有符合的数据都会被更新
    # 2.间接修改 先获取到对象然后再修改
    # User.objects.filter(name="fiveuser_5").update(name="fiveuser_0")
    # 先 filter 过滤 再 update 不会更新时间
    # 刚才两个都没有更新时间 应该怎么修改才能改变呢？
    # 要用get出来的对象.属性="新值"
    # 先 get 拿到对象 更新属性 然后触发保存 会更新时间
    # user_obj = User.objects.get(name="fiveuser_0")
    # print(user_obj)
    # User object (6)
    # user_obj.name = "fiveuser_5"
    # 触发保存
    # user_obj.save()  # 只有save 才会触发更新时间

    # 【五】查看当前表下的实际数据
    # 1.获取字段的数据返回格式是字典 model.objects.all().values("filed_name")
    # 提取出 all/filter 过滤后的数据中每一个对象对应字段的值 .values.first() 提出来 ---> 变成了一个字典而不是 Django 的对象了
    # result_one = User.objects.all().values("name", "age").first()
    # print(result_one, type(result_one))
    # {'name': 'sheenagh', 'age': 23} <class 'dict'>
    # 2.获取字段的数据返回格式是列表
    # result_many = User.objects.all().values_list("name", "age")
    # print(result_many, type(result_many))
    # <QuerySet [('sheenagh', 23), ..., ('fiveuser_4', 16)]> <class 'django.db.models.query.QuerySet'>

    # 【六】在MySQL有去重 distinct
    # res = User.objects.all().values("name").distinct()
    # print(res)

    # 【七】排序 order_by
    # 1.默认是按照升序排列数据
    # res = User.objects.order_by("age").values("name","age")
    # print(res)
    # 2.降序排序 :  直接在排序的字段上加 -
    # res = User.objects.order_by("-age").values("name","age")
    # print(res)
    # 3.翻转顺序 reverse
    # res = User.objects.order_by("age").values("name","age").reverse()
    # print(res)

    # 【八】统计当前数据数量 count
    # res = User.objects.count()
    # print(res)
    # 7

    # 【九】剔除结果 exclude
    # res = User.objects.exclude(name="sheenagh").values("name")
    # print(res)

    # 【十】判断当前数据是够存在 exists
    # res = User.objects.filter(name="sheenagh").exists()
    # print(res)
    # True
    # res = User.objects.filter(name="sheenagh").count()
    # print(res)
    # 1

    # 【十一】查看当前转成SQL语句
    # result = User.objects.all()
    # print(result)
    # result = User.objects.all().query
    # print(result)
    # SELECT `user`.`id`, `user`.`name`, `user`.`age`, `user`.`register_time`, `user`.`update_time` FROM `user`

# 必知必会N条
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
````



# 4 神奇的双下划线查询

````python
import os
import django

if __name__ == '__main__':
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'djangoProject08.settings')
    django.setup()
    from user.models import User

    # 【一】大于 filed_name__gt=值
    result = User.objects.filter(age__gt=4).values("name", "age")
    print(result)
    # 【二】大于等于 filed_name__gte=值
    result = User.objects.filter(age__gte=4).values("name", "age")
    print(result)
    # 【三】小于 filed_name__lt=值
    result = User.objects.filter(age__lt=4).values("name", "age")
    print(result)
    # 【四】小于等于 filed_name__lte=值
    result = User.objects.filter(age__lte=4).values("name", "age")
    print(result)

    # 【五】或 filed_name__in=值
    # 查询 age=0 / age=16
    result = User.objects.filter(age__in=(0, 4, 16)).values("name", "age")
    print(result)

    # 【六】两个条件之间 filed_name__range=值
    # sql : between ... and ...
    result = User.objects.filter(age__range=(0, 9)).values("name", "age")
    print(result)

    # 【七】模糊查询 filed_name__contains=值 (区分大小写) filed_name__icontains=值 (不区分大小写)
    # 默认是区分大小写
    result = User.objects.filter(name__contains="e").values("name", "age")
    print(result)
    # 不区分大小写
    result = User.objects.filter(name__icontains="F").values("name", "age")
    print(result)

    # 【八】判断以 xxx 开头 / 判断以 xxx 结尾
    result = User.objects.filter(name__startswith="s").values("name", "age")
    print(result)
    result = User.objects.filter(name__endswith="h").values("name", "age")
    print(result)

    # 【九】按照指定日期进行查询
    result = User.objects.filter(register_time__day="11").values("name", "age")
    print(result)
    result = User.objects.filter(register_time__year="2001").values("name", "age")
    print(result)
    print("----")
    result = User.objects.filter(register_time__gt="2001-01-08").values("name", "age")
    print(result)
    result = User.objects.filter(register_time__lt="2001-01-08").values("name", "age")
    print(result)
````



# 5 多对多表关系创建的三种方式

````python
# 【一】表关系分析
# 1.一对一 用户表和用户详情表
# foreign key
# 2.一对多 员工表和部门表
# 先创建被关联的表 再创建关联表
# 部门 有了 再创 员工
# 3.多对多 作者表和图书表
# 对方都和对方相关联
# 借助第三张表存储外键关系

# 【二】Django中的 多对多 表关系
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

    # 创建一个一对多字段
    book = models.ManyToManyField(
        # 关联到那张表
        to="Book"
    )

    class Meta:
        db_table = "author"
"""
````

