# 1 三板斧对象

````python
# 【一】三板斧对象
# render : 渲染前端模版
# HttpResponse : 返回字符串格式的数据
# redirect : 重定向我们的路由

# 【二】render
# 【1】定义视图函数
"""
# 定义每一个视图函数都必须有一个位置参数 叫 request
def index(request):
    # render(request, template_name, context=None, content_type=None, status=None, using=None)
    # request 当前的 request 对象
    # template_name 当前需要渲染的模板文件的名字 放在 templates 文件夹里面的
    # context jinja2 语法 给前端传递数据 render({"key":"value"}) === context
    # content_type : 定义响应文档的类型 text/html / json
    # status : 响应状态码 默认的状态码就是 200 OK
    # using : 执行使用的是哪个模板引擎 使用默认的
    return render(request, "index.html")

# settings.py 中的 TEMPLATES 配置块
'''
TEMPLATES = [
    {
        # 使用的 Django 提供的模板渲染引擎 我们用过 jinja2 引擎
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # 检测哪些目录下的 templates 文件夹
        'DIRS': [],
        # True 的含义是会自动检测当前自己APP下面的templates文件夹
        'APP_DIRS': True,
        # 可选的配置参数
        'OPTIONS': {
            'context_processors': [
                # debug 开发者模式 如果报错了会提示给你详细的报错信息
                'django.template.context_processors.debug',
                # request 使用视图函数逇 request 对象
                'django.template.context_processors.request',
                # 用户模块
                'django.contrib.auth.context_processors.auth',
                # 信息
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
'''
"""
# 【2】定义模板文件
# user/templates/index.html
'''
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../../static/jquery.min.js"></script>
    <link href="../../static/bootstrap.min.css" rel="stylesheet">
    {# ../(templates)../(user) --> static --> /bootstrap.min.js #}
    <script src="../../static/bootstrap.min.js"></script>

</head>
<body>
<h1>这是 index 页面</h1>
</body>
</html>
'''

# 【3】去路由系统中注册当前视图
# djangoProject01/urls.py
'''
from user.views import index

urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', index),
]
'''

# 【三】redirect 对象
"""
def func(request):
    # redirect(to, *args, permanent=False, **kwargs)
    # to 代表想要重定向的目标地址
    #   如果写详细的地址 https:www.baidu.com 会重定向到指定的地址上
    #   如果简写 /index/ 会自动拼接 http://127.0.0.1:8000 + /index/ ---> http://127.0.0.1:8000/index/ 回到了主页上
    #   如果写 index/ 会自动拼接 当前路径 + index/ ---> http://127.0.0.1:8000/func + /index/ -- > http://127.0.0.1:8000/func/index/
    # 后面将 分组 要给某个 路径携带参数 args kwargs 传递关键字参数
    return redirect("index/")

'''
# 临时重定向和永久重定向

# ● 临时重定向（响应状态码：302）和永久重定向（响应状态码：301）对普通用户来说是没什么区别的，它主要面向的是搜索引擎的机器人。 
#   ○ A页面临时重定向到B页面，那搜索引擎收录的就是A页面。 # 可以通过 回退回去
#   ○ A页面永久重定向到B页面，那搜索引擎收录的就是B页面。 # 不可以通过 回退回去
'''
"""
````



# 2 静态文件static

````python
# 在将 jinja2 的时候 静态文件
# 在Django的静态系统中也有静态文件的语法

# 【一】静态文件的分类
# 【1】模板文件
# templates 文件夹 里面存的就是 页面的静态源码
# 【2】资源文件
# static 文件夹
#   网站写好的 css 源码样式
#   网站写好的 js 代码
#   引入的第三方框架 ---> bootstrap 框架
#   图片文件 img 文件
#   视频文件 video 文件夹
'''
static
├── css
├── img
├── js
├── plugins
│ ├── bootstrap
│ │ ├── bootstrap.min.css
│ │ └── bootstrap.min.js
│ └── jQuery
│     └── jquery.min.js
└── video
'''

# 【二】Django中的 static 静态文件语法
# 【1】如果不用 静态文件语法 导入文件
# <script src="../../static/plugins/jQuery/jquery.min.js"></script>

# 如果文件太多 我们就会发现我们导入的路径发生了错乱

# 【2】静态文件语法
# （1）开放接口
'''
# Static files (CSS, JavaScript, Images)
# 提供了 Static 官方文档链接
# https://docs.djangoproject.com/en/3.2/howto/static-files/

# 配置的 Static 静态文件的路由
# 访问静态文件路由 : http://127.0.0.1:8000/static/ + 文件名
# http://127.0.0.1:8000/static/img/1.jpg
# http://127.0.0.1:8000/static/plugins/bootstrap/bootstrap.min.css
STATIC_URL = '/static/'

# 在配置一个参数
# 静态文件目录
STATICFILES_DIRS = [
    # 根目录下的文件目录
    "static",
]
# 将app目录放在 static 目录下 也会通过静态语法找到制定的文件
# 容易造成信息的泄露
# http://127.0.0.1:8000/static/user/templates/index.html

# static 文件夹一般存一些静态的不重要的数据
'''
# （2）使用语法
'''
{# 第一句 ： 加载静态文件语法系统 #}
{% load static %}

<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../../static/plugins/jQuery/jquery.min.js"></script>
    <link href="../../static/plugins/bootstrap/bootstrap.min.css" rel="stylesheet">
    {# ../(templates)../(user) --> static --> /bootstrap.min.js #}
    <script src="{% static 'plugins/bootstrap/bootstrap.min.js' %}"></script>
    {#  页面上的展示是 ： <script src="/static/plugins/bootstrap/bootstrap.min.js"></script>  #}
    {#  /static/plugins/bootstrap/bootstrap.min.js ---> 从根目录下开始加载 ---> http://127.0.0.1:8000/static/plugins/bootstrap/bootstrap.min.js  #}
</head>
'''

# 开放静态资源的文件一定不是隐私数据
````



# 3 request对象初识

````python
# 【一】请求方式
# {#      action : 将数据提交到那个目标地址      #}
# {#      action : 不写就是默认提交到当前地址 写了就是指定的地址      #}
# {#      method : 当前请求的请求方式 默认是 post      #}
# {# http://127.0.0.1:8000/login/?username=dream&password=666 #}
# <form action="" method="get">

# 【1】GET 请求的特点是会将请求参数携带在请求路径上
# # {# http://127.0.0.1:8000/login/?username=dream&password=666 #}

# 【2】POST 请求会发现 在请求地址上没有参数
# http://127.0.0.1:8000/login/
# 报了个错：CSRF verification failed. Request aborted.
# 结局办法：settings.py ---> MIDDLEWARE -- >     注解 ： 'django.middleware.csrf.CsrfViewMiddleware',

# 【二】request对象初识
"""
def login(request):
    # 【0】如何查看当前对象有那些属性？
    # print(dir(request))
    '''
    ['COOKIES', 'FILES', 'GET', 'META', 'POST', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_current_scheme_host', '_encoding', '_get_full_path', '_get_post', '_get_raw_host', '_get_scheme', '_initialize_handlers', '_load_post_and_files', '_mark_post_parse_error', '_messages', '_read_started', '_set_content_type_params', '_set_post', '_stream', '_upload_handlers', 'accepted_types', 'accepts', 'body', 'build_absolute_uri', 'close', 'content_params', 'content_type', 'encoding', 'environ', 'get_full_path', 'get_full_path_info', 'get_host', 'get_port', 'get_raw_uri', 'get_signed_cookie', 'headers', 'is_ajax', 'is_secure', 'method', 'parse_file_upload', 'path', 'path_info', 'read', 'readline', 'readlines', 'resolver_match', 'scheme', 'session', 'upload_handlers', 'user']
    '''
    # META -- 后面讲
    # COOKIES  -- 后面讲
    # FILES  -- 后面讲
    # body  -- 后面讲
    # headers  -- 后面讲
    # is_ajax -- 后面讲

    # path  -- 后面讲
    # path_info  -- 后面讲

    # 【1】获取当前请求的方法
    # method
    print(request.method)  # 获取到当前提交的请求的请求方法
    # GET / POST
    if request.method == "GET":
        # 【2】获取GET请求携带的请求参数
        # 应该将路径中携带的参数提取出来和数据库或者其他数据进行校验
        # http://127.0.0.1:8000/login/?username=dream&password=666
        print(request.GET)  # 并且将请求携带的参数转换为字典
        # <QueryDict: {'username': ['dream'], 'password': ['dasdadsa'], 'hobby': ['music', 'run', 'dance']}>
        username = request.GET.get("username")
        password = request.GET.get("password")
        # 对于一个键多个值的情况要使用 getlist
        # hobby = request.GET.get("hobby") # dance
        hobby = request.GET.getlist("hobby")  # ['music', 'run', 'dance']
        print(username, password)  # dream 666
        print(hobby)  # ['music', 'run', 'dance']
    else:
        # POST
        # 【3】获取 POST 请求携带的请求体参数
        # 在POST请求中 携带的数据不会展示在 路径上
        # http://127.0.0.1:8000/login/
        print(request.POST)  # 并且将请求携带的参数转换为字典
        # <QueryDict: {'username': ['dream'], 'password': ['66'], 'hobby': ['music', 'run']}>
        username = request.POST.get("username")
        password = request.POST.get("password")
        # 对于一个键多个值的情况要使用 getlist
        hobby = request.POST.get("hobby")  # dance
        # hobby = request.POST.getlist("hobby") #['music', 'run']
        print(username, password)  # dream 66
        print(hobby)  # ['music', 'run']

    return render(request, "login.html")
"""
````



# 4 Django连接MySQL

````python
# 【一】引入
# 我们以前操作数据库是通过 SQL 语句
# 现在我们使用Django框架 框架内部就提供给我们一些操作的方法

# 【二】用Python代码操作数据库

# 【三】配置数据库
# 在setting.py
'''
# Django使用的默认的数据库 sqlite3
DATABASES = {
    'default': {
        # 使用的哪种数据库引擎 django.db.backends.sqlite3
        'ENGINE': 'django.db.backends.sqlite3',
        # BASE_DIR / 'db.sqlite3' ： 数据库文件
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Django链接MySQL数据库

'''

# 【2】Django链接MySQL报错
# Django想要链接数据库但是不允许 链接
# django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.
# Did you install mysqlclient?
# （1）猴子补丁
# 你有bug 但是我给你搭一个补丁 把bug绕过去
# 第一步安装 pymysql ： pip install pymysql
# 第二步在你的 项目的 __init__ 文件下加一句话
'''
import pymysql

pymysql.install_as_MySQLdb()
'''

# （2）安装第三方模块
# Did you install mysqlclient?
# 安装 mysqlclient : pip install mysqlclient
# MacOS系统没办法通过 pip 安装 mysqlclient
# 想要安装就必须 想办法
# windows系统正常下载

# （3）安装 wheel 文件
# 第一步：访问 模块的官网：https://pypi.org/project/mysqlclient/#files
# 第二步：下载whl文件： mysqlclient-2.2.4-pp310-pypy310_pp73-win_amd64.whl (203.2 kB view hashes)
# mysqlclient-2.2.4 当前 mysqlclient 的版本
# pp310 # 解释器版本 3.10
# pypy310_pp73 # 解释器版本 3.10
# win_amd64.whl # 根据你的操作系统来变

# mysqlclient-2.2.4-cp312-cp312-win_amd64.whl (203.3 kB view hashes)
# 第三步：点击链接下载文件
# 第四步：pip install + 将你的文件位置拖进来

# 【3】数据库不存在会直接报错
# django.db.utils.OperationalError: (1049, "Unknown database 'django_day02'")
# 创建数据库
# create database django_day02;
````



# 5 ORM初识

````python
# 【一】什么是ORM
# ● ORM是一种将对象与关系型数据库之间的映射的技术，主要实现了以下三个方面的功能：
#   ○ 数据库中的表映射为Python中的类
#   ○ 数据库中的字段映射为Python中的属性
#   ○ 数据库中的记录映射为Python中的实例
# ● ORM的主要优点是可以减少开发人员编写重复的SQL语句的时间和工作量，并且可以减少由于SQL语句的调整和更改所带来的错误。
# 【二】Django ORM的优点
# ● 与其他ORM框架相比，Django ORM拥有以下优点：
# ● 简单易用：Django ORM的API非常简单，易于掌握和使用。
# ● 丰富的API：Django ORM提供了丰富的API来完成常见的数据库操作，如增删改查等，同时也支持高级查询和聚合查询等操作。
# ● 具有良好的扩展性：Django ORM可以与其他第三方库进行无缝集成，如Django REST framework、Django-Oscar等。
# ● 自动映射：Django ORM支持自动映射，开发者只需要定义数据库表的结构，就可以自动生成相应的Python类，从而减少开发过程中的重复代码量。

# 【三】ORM操作
# 【1】定义模型表
# 在你的app下面有一个文件 --->  models.py
# 在文件内定义数据库模型表
'''
from django.db import models


# Create your models here.

#   ○ 数据库中的表映射为Python中的类
# 定义一个类 必须继承 models.Model
class Student(models.Model):
    # 在SQL语句中会创建一个主键 ID
    # 在Django里面会 给我们默认配一个字段 叫 ID === 我们自己定义的主键ID
    # DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

    #   ○ 数据库中的字段映射为Python中的属性
    # name : 字符串类型
    # name varchar(32) # 在SQL语句定义的时候要指定默认长度
    # 在Django定义模型表的时候也要指定默认长度
    name = models.CharField(max_length=32)

    # age : 数字类型
    # age int() # SQL 语句
    age = models.IntegerField()
'''

# 【2】迁移模型表生成数据迁移记录
# 现在是用Python代码写的数据库结构
# 将 Python代码转换为 SQL 语句结构
'''
python manage.py makemigrations
# make  +  migrations
'''
# migrations 文件夹里面会存放迁移记录
# 0001_initial.py
'''
将 Python 定义的数据库结构进行转换
'''

# 【3】将生成的SQL记录迁移进MySQL数据库中
'''
python manage.py migrate
# migrations --> migrate
'''

'''
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, user
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
  Applying user.0001_initial... OK
'''

# 【4】打开数据库刷新
# 看到很多表
# 后面都会挨个将
# 我们自己刚才创建的表是以 app名字_类名来定义的


# 【5】Pycharm提供了更加方便的方式
'''
Tools -> run manage.py task ---> makemigrations ---> migrate 
'''
````

