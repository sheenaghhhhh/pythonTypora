# 1 CSRF跨站请求伪造

## 1.1 介绍

```python
# CSRF跨站请求伪造是一种常见的网络攻击手段
# 你在页面上能看到的页面不一定是真实的页面
# 大学四级缴费的时候 让你扫码付款 但是有的人付完款了快考试的时候发现没有自己的预约信息
# 你看到页面是钓鱼网站，黑客做了一个自己的收款码把真实的收款码覆盖了

# 用一个钓鱼网站来掩饰真正的网页
```

## 1.2 解决方案

````python
# 1.CSRF令牌
# 在每一个页面上给一个特定的令牌 ， 只有携带令牌才能通过请求
# 我们在提交 post请求的时候 会一直提示 forbbiden ---> 我们没有携带 csrftoken
# 我们在看 csrf 中间件的时候 csrfmiddwaretoken
from django.middleware.csrf import CsrfViewMiddleware
# request_csrf_token = request.POST.get('csrfmiddlewaretoken', '')
# 2.启用SameSite属性
# 设置 Cookie 的 启用SameSite属性 限制跨域请求伪造
# 3.严格验证请求来源
# 在你的请求头上面会有一个参数 Refer
# Refer 当前请求来源的上一个请求地址 校验上一个请求地址是否有
# 4.使用验证码
# 频繁操作 中间件里面重定向给她 一个 让他输入验证码的页面
````

## 1.3 自己伪造钓鱼网站

````python
# <form action="" method="post">
# 不做任何操作会被 Forbidden
# 解决方案
# 方案一：直将中间件注释掉
# 方案二：加装饰器
# 方案三：在form表单内增加一个 csrf_token 的标签
````

### （1）真实的后台

```html
<form action="" method="post">
    <p>
        请输入目标用户账号 :>>>> <input type="text" name="to_account">
    </p>
    <p>
        请输入付款金额 :>>>> <input type="text" name="pay_money">
    </p>
    <p>
        <input type="submit">
    </p>
</form>
```

### （2）伪造的后台

````html
<form action="http://localhost:8000/pay/" method="post">
    <p>
        请输入目标用户账号 :>>>> <input type="text" >
        <input type="text" name="to_account" value="9696969696969696" hidden>
    </p>
    <p>
        请输入付款金额 :>>>> <input type="text" name="pay_money">
    </p>
    <p>
        <input type="submit">
    </p>
</form>
````

### （3）添加csrf_token 

```html
<form action="" method="post">
    {% csrf_token %}
    <p>
        请输入目标用户账号 :>>>> <input type="text" name="to_account">
    </p>
    <p>
        请输入付款金额 :>>>> <input type="text" name="pay_money">
    </p>
    <p>
        <input type="submit">
    </p>
</form>
```

```python
# 生成的 csrfmiddleware 是随机的
# 33PF9aeYgPTxvAyidkE4AGAOjnOyqIbjQzjgUWEIGU8CAClN0zaF5IMs0sanXXdF
# gpAJjzX9U9TCnGanxlzkEeXgcxt7QBaN3V4k4lnTke8HsIXSkA5V9g9UTCPWnQc9
```

## 1.4 Ajax 携带 csrf

```python
# form 自动将 标签内部的所有标签传送过去
# 用 Ajax form标签 内部的不管用了
# Forbidden (CSRF token missing or incorrect.): /pay/
```

- 1.方案一：直接根据标签的 name 属性获取标签所对应的值

```javascript
<script>
    $("#btn_submit").click(function () {
        let account = $("#to_account").val()
        {#let account = $("input[name='to_account']").val()#}
        let money = $("#pay_money").val()
        {#let money =$("input[name='pay_money']").val()#}
        $.ajax({
            url: "",
            type: "post",
            data: {
                "to_account": account,
                "pay_money": money,
            },
            success: function (data) {
                console.log(data)
            }
        })

    })
</script>

// let csrfmiddlewaretoken = $("input[name='csrfmiddlewaretoken']").val()
```

- 2.方案二：利用Django提供给我们的模版语法

```javascript
"csrfmiddlewaretoken": "{{csrf_token}}",
```

- 3.方案三：Ajax官网提供给我们的方案 创建一个 js 脚本并引入 会自动将 csrftoken 增加到请求头参数中

```javascript
function getCookie(name) {
    var cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        var cookies = document.cookie.split(';');
        for (var i = 0; i < cookies.length; i++) {
            var cookie = jQuery.trim(cookies[i]);
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}
var csrftoken = getCookie('csrftoken');

function csrfSafeMethod(method) {
  // these HTTP methods do not require CSRF protection
  return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}

$.ajaxSetup({
  beforeSend: function (xhr, settings) {
    if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
      xhr.setRequestHeader("X-CSRFToken", csrftoken);
    }
  }
});
```

# 2 Auth模块介绍

````python
# 【一】Auth模块介绍
# 在我们创建Django项目之后
# 我们在迁移表的时候会自动生成两张表
# django_session : 操作session必须有的表
# auth_user : Django 同给我们的默认的用户表 存储我们的用户信息 以前我们都是自己创建
# auth_permission : 用户权限表
# auth_user_groups : 用户分组表
# auth_group_permission : 用户分组权限表 根据你的分组分配权限
# auth_user_user_permissions : 用户权限权限表

# 部门里面有很多人
# id name role dep
#  1   a   员工  开发  ---- 提交和查看代码
#  2   b   主管  开发  ---- 提交和查看和修改
#  3   c   老板  董事会 ---- 提交和查看和修改和删除
#  4   d   员工  开发 ---- 提交和查看代码


# auth_user --- 员工信息表
# id name
#  1   a
#  2   b
#  3   c
#  4   d

# auth_permission --- 权限表
# id name
#  1  增加
#  2  删除
#  3  查看
#  4  修改

# auth_user_groups -- 用户分组表
# id name
#  1  员工
#  2  主管
#  3  老板

# auth_group_permission --- 用户分组权限表
# id group_id  permission_id
#  1    1        1,3
#  2   2        1,3,4
#  3    3        1,2,3,4

# auth_user_user_permissions 用户分组权限关联表permission_id关联的是 auth_group_permission
# id user_id permission_id
#  1    1        1
#  1    2        2
#  1    3        3
#  1    4        1

# RBAC 权限系统 ---> 基于角色的访问控制权限管理系统
````

# 3 常用操作

## 3.1 创建超级管理员

````python
# 【一】admin后台
# 在创建Django项目后会自动有一个路由
# admin/
# 访问后会发现让我们输入用户名和密码
# 【二】创建超级管理员
# python manage.py createsuperuser # 创建超级管理员
# python manage.py createuser # 创建普通用户

# 1.用户信息存到了哪里？
# 存到了我们迁移后产生的  auth_user 表中

# 2.创建超级管理员命令
'''
manage.py@djangoProject14 > createsuperuser
bash -cl "/usr/local/bin/python3.10 /Applications/PyCharm.app/Contents/plugins/python/helpers/pycharm/django_manage.py createsuperuser /Users/dream/Documents/PythonProjects/Python30/DjangoProjects/djangoProject14"
Tracking file by folder pattern:  migrations

# 让你输入用户名  如果你不输入 就默认使用当前电脑的用户名
Username (leave blank to use 'dream'):

# 让你输入邮箱地址 不填也行
Email address:

# 让你输入密码 Django限制你的密码必须很复杂才行
# 但是你输入简单的密码后 强制让他使用也可以
Warning: Password input may be echoed.
Password:
Warning: Password input may be echoed.
Password (again):

# 这个密码 太短了 必须包含 至少 8 个字符
This password is too short. It must contain at least 8 characters.
# 这个密码太普通了
This password is too common.
# 这个密码必须是唯一的
This password is entirely numeric.
# 你确认无论如何都要使用这个密码吗 写确认 y
Bypass password validation and create user anyway? [y/N]:
Superuser created successfully.

Process finished with exit code 0
'''

# 3.看管理员信息
# 基于Django自带的加密系统进行的加密 sha256 ---> 基于 sha256 算法进行摘要
# password : pbkdf2_sha256$260000$apDmzlKf7N8vNLZ0nkPfog$IC3XTZQMiX/efdEQfuGCDfwOJ690zZ7FCFuNe1CDd64=
# last_login:我们发现是国际 UTC 时区 时间 ---> 变成本地时间 修改 settings.py 中的
# is_active : 控制账号的激活状态 如果是false意思是账号被封禁
````

## 3.2 创建用户

````python
class RegisterView(View):
    def get(self, request, *args, **kwargs):
        return render(request, "register.html", locals())

    def post(self, request, *args, **kwargs):
        username = request.POST.get("username")
        password = request.POST.get("password")

        # 【二】根据获取到用户名和密码进行注册
        # 在你的模型表中有用户模型表吗？
        # 1.导入用户模型表
        # from django.contrib.auth.models import User
        # 2.username字段值唯一
        # django.db.utils.IntegrityError: UNIQUE constraint failed: auth_user.username
        # 3.直接使用模型表创建用户是明文密码没办法用
        # 在你创建的用户中 用户名必须唯一
        # User.objects.create(username=username, password=password)
        # 这样创建的用户的密码是明文的但是Django的登陆系统只认识密文的密码 所以识别不出来就登录不进去了
        # 4.创建用户用自带的方法
        # （1）create_user 创建普通用户
        # User.objects.create_user(username=username,password=password)
        # （2）create_superuser 创建超级管理员用户
        # User.objects.create_superuser(username=username,password=password)
````

## 3.3 登陆验证和保存登陆状态

```python
from django.contrib.auth.models import AbstractUser, User
from django.contrib import auth

class LoginView(View):
    def get(self, request, *args, **kwargs):
        return render(request, "register.html", locals())

    def post(self, request, *args, **kwargs):
        # 【一】获取到用户名和密码
        username = request.POST.get("username")
        password = request.POST.get("password")

        # 【二】比对用户名和密码
        # 1.直接模型表 filter 发现查不出来 --- 原因是密码是密文而我们传进来的事明文
        # obj = User.objects.filter(username=username,password=password)
        # print(obj) # <QuerySet []>
        # 2.借助 auth 模块完成登陆认证
        # authenticate(request,username=传入的,password=传入的)
        # （1）如果用户名和密码正确就返回正确的用户对应的对象
        user_obj = auth.authenticate(request, username=username, password=password)
        # print(result, type(result))
        # dream_1 <class 'django.contrib.auth.models.User'>
        # （2）如果用户名和密码错误就会返回None
        # print(user_obj, type(user_obj))
        # None <class 'NoneType'>
        # 3.登陆功能实现
        if not user_obj:
            return HttpResponse(f"当前用户 {username} 用户名或密码错误!")
        # 4.用户名和密码正确的情况下 保存你的登陆状态 --- auth 模块可以实现 保存用户的登陆状态
        # print(request.user.is_authenticated)  # False
        auth.login(request, user_obj)
        # 5.如何判断当前是否是出于登陆状态呢？
        # print(request.user.is_authenticated)  # True
        # print(request.get_full_path())  # /login/?next=/index/
        next_url = request.GET.get("next")
        # print(next_url) # /index/
        if next_url:
            return redirect(next_url)
        return HttpResponse(f"当前用户 {user_obj.username} 登陆成功!")
```

## 3.4 获取登陆状态

````python
def home(request):
    # 先验证当前用户是登陆后才能看到页面
    # 1.方式一 : 通过 request.user 查看当前登录的用户对象
    # print(request.user)  # AnonymousUser
    # 如果没有登录过 没有保存用户状态 就是 AnonymousUser 匿名用户
    # 如果登陆成功后 打印的就是当前已经登陆的 用户对象 dream_1
    # print(type(request.user))  # <class 'django.utils.functional.SimpleLazyObject'>
    # if request.user == "AnonymousUser":
    #     return HttpResponse(f"未登录请先登陆!")
    # 2.方式二 ： request.user.is_authenticated 获取当前用户的登陆状态

    # 3.提醒：不要用request.user来判断是否登录过。 request.user 无论是否登陆过都有值
    if request.user.is_authenticated:  # True
        return HttpResponse(f"登录后才能看到的页面")
    else:
        return HttpResponse(f"登录后才能看到")
````

## 3.5 自定义登陆装饰器

````python
def login_auth(func):
    def inner(request, *args, **kwargs):
        if request.user.is_authenticated:
            result = func(request, *args, **kwargs)
            return result
        else:
            # 重定向到登陆页面
            # http://localhost:8000/admin/login/?next=/admin/
            now_path = request.path_info
            print(now_path)  # /index/
            print(reverse("login"))  # /login/
            new_url = reverse("login") + '?' + f"next={now_path}"
            print(new_url)  # /login/?next=/index/
            return redirect(new_url)

    return inner


@login_auth
def index(request):
    # 做一个登陆认证重定向
    # 只有登录后什么都不干 没有登陆的时候必须重定向到登陆页面进行登录
    return HttpResponse("登录后才能看到的页面")
````

## 3.6 auth登陆装饰器

````python
from django.contrib.auth.decorators import login_required


# auth 模块提供给我们一个登录跳转的配置 --- 局部配置
# 局部配置跳转的登陆地址
# @login_required # 不指定跳转地址 显示的是
# http://localhost:8000/accounts/login/?next=/func/
# 【方案一】：自己指定跳转的登陆地址
# @login_required(login_url="/login/")
# 【方案二】但是要在全局的配置文件中指定跳转的地址 LOGIN_URL = '/login/'
@login_required
#  http://localhost:8000/login/?next=/index/
def func(request):
    # 做一个登陆认证重定向
    # 只有登录后什么都不干 没有登陆的时候必须重定向到登陆页面进行登录
    return HttpResponse("登录后才能看到的页面")
````

## 3.7 注销登陆

````python
# 注销功能
def logout(request):
    # auth 模块注销
    auth.logout(request)
    return redirect(reverse("login"))
````

## 3.8 修改密码

```python
# CBV 视图添加装饰器 借助 装饰器
from django.utils.decorators import method_decorator


class ChangePwdView(View):
    @method_decorator(decorator=login_required)
    def get(self, request, *args, **kwargs):
        return render(request, "change_password.html", locals())

    def post(self, request, *args, **kwargs):
        username = request.POST.get("username")
        old_password = request.POST.get("old_password")
        new_password = request.POST.get("new_password")

        # 【二】修改密码
        # 1.先校验原密码是否正确
        if not request.user.check_password(old_password):
            return HttpResponse("当前原密码错误")
        # 2.密码正确的情况下修改密码
        # 将新密码交给 set_password 去修改
        request.user.set_password(new_password)
        # 但是只是缓存了但是没有提交
        # 主动提交
        request.user.save()
        # 退出当前用户的登陆状态 跳转到登陆页面去登录
        logout(request)
        return HttpResponse(F"修改密码成功!")
```



# 4 Auth模块方法总结

````python
# 【一】Auth模块借助 Django 的默认模型表
# 需要先迁移数据库才能生效

# 【二】创建用户功能 --- User.objects.create_user / User.objects.create_superuser
'''
# 2.username字段值唯一
# django.db.utils.IntegrityError: UNIQUE constraint failed: auth_user.username
# 3.直接使用模型表创建用户是明文密码没办法用
# 在你创建的用户中 用户名必须唯一
'''
# 1.导入用户模型表
# from django.contrib.auth.models import User
# User.objects.create(username=username, password=password)
# 这样创建的用户的密码是明文的但是Django的登陆系统只认识密文的密码 所以识别不出来就登录不进去了
# 2.auth 自带的方法
# （1）create_user 创建普通用户
# User.objects.create_user(username=username,password=password)
# （2）create_superuser 创建超级管理员用户
# User.objects.create_superuser(username=username,password=password)

# 【三】登陆校验密码是否正确 ---  auth.authenticate
# 1.直接模型表 filter 发现查不出来 --- 原因是密码是密文而我们传进来的事明文
# obj = User.objects.filter(username=username,password=password)
# print(obj) # <QuerySet []>
# 2.借助 auth 模块完成登陆认证
# authenticate(request,username=传入的,password=传入的)
# （1）如果用户名和密码正确就 返回 正确的用户对应的对象
# user_obj = auth.authenticate(request, username=username, password=password)
# print(result, type(result))
# dream_1 <class 'django.contrib.auth.models.User'>
# （2）如果用户名和密码错误就会返回 None
# print(user_obj, type(user_obj))
# None <class 'NoneType'>

# 【四】保存当前用户的登陆状态
# auth 模块可以实现 保存用户的登陆状态
# auth.login(request, user_obj)

# 【五】注销登陆状态
# auth.logout(request)

# 【六】获取当前登录的用户信息
# 通过 request.user 查看当前登录的用户对象
# print(request.user)  # AnonymousUser
# （1）如果没有登录过 没有保存用户状态 就是 AnonymousUser 匿名用户
# （2）如果登陆成功后 打印的就是当前已经登陆的 用户对象 dream_1
# print(type(request.user))  # <class 'django.utils.functional.SimpleLazyObject'>

# 【七】判断当前的用户登陆状态
# 1.方式一：基于 request.user 获取的用户信息校验
# if request.user == "AnonymousUser":
#     return HttpResponse(f"未登录请先登陆!")
# 2.方式二 ： request.user.is_authenticated 获取当前用户的登陆状态

# 【八】登陆认证装饰器
# 1.方案一：受挫
'''
def login_auth(func):
    def inner(request, *args, **kwargs):
        if request.user.is_authenticated:
            result = func(request, *args, **kwargs)
            return result
        else:
            # 重定向到登陆页面
            # http://localhost:8000/admin/login/?next=/admin/
            now_path = request.path_info
            print(now_path)  # /index/
            print(reverse("login"))  # /login/
            new_url = reverse("login") + '?' + f"next={now_path}"
            print(new_url)  # /login/?next=/index/
            return redirect(new_url)

    return inner
'''
# 2.方案二：auth自带的
# from django.contrib.auth.decorators import login_required
# （1）配置方案一：局部配置
# 【方案一】：自己指定跳转的登陆地址
# @login_required(login_url="/login/")
# （2）方案二：在全局配置文件中指定跳转地址
# 但是要在全局的配置文件中指定跳转的地址 LOGIN_URL = '/login/'
# @login_required

# 【九】修改密码
# 1.校验旧密码是否正确
# request.user.check_password(old_password)
# 2.修改原本的密码 --- 用 auth 模块自带的 因为数据库中的密码是密文的
# request.user.set_password(new_password)

# 提示：如果用了 auth 模块就用全套的 方法
# 否则就别用 基于模型表自己去加密密码 ..
````



# 5 扩展User表

````python
# 【一】导入原本的用户模型表的抽象类 AbstractUser
from django.contrib.auth.models import AbstractUser
from django.db import models


# 【二】我们也继承 AbstractUser 在 AbstractUser 字段的基础上进行字段的扩充
class User(AbstractUser):
    phone = models.CharField(max_length=11, verbose_name="手机号")
    gender = models.IntegerField(verbose_name="性别")

# 【三】启动就报错
# django.core.management.base.SystemCheckError: SystemCheckError: System check identified some issues:
# 解决办法
# 在 settings.py 中 增加一个配置
'''
# 扩展 User表所在的 app.扩展的表名
AUTH_USER_MODEL = "user.User"
'''
````



# 6 Django配置文件

````python
# 【一】Django的配置文件介绍
# 在 Django的配置文件系统中有两个系统
# 【二】方式一：直接通过路径.settings.py 导入配置
from djangoProject14 import settings
# 导入 settings 中定义的属性和参数
# 这样导入有一个弊端 在Django内部其实还有很多配置项

# 【三】方式二：导入Django内置的 settings 文件
from django.conf import settings
'''
class LazySettings(LazyObject):
    def _setup(self, name=None):
        # 【一】获取到 Django manage.py 中设置的配置文件的路径
        # ENVIRONMENT_VARIABLE = "DJANGO_SETTINGS_MODULE"
        # 在 manage.py 中 os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'djangoProject14.settings')
        settings_module = os.environ.get(ENVIRONMENT_VARIABLE)
        # 【二】如果环境中没有这个路径 九抛出异常 
        if not settings_module:
            desc = ("setting %s" % name) if name else "settings"
            raise ImproperlyConfigured(
                "Requested %s, but settings are not configured. "
                "You must either define the environment variable %s "
                "or call settings.configure() before accessing settings."
                % (desc, ENVIRONMENT_VARIABLE))

        # 【三】如果配置了静态文件就继续走 把 自定义配置文件路径传进去
        self._wrapped = Settings(settings_module)

# 【四】创建 Settings 对象
class Settings:
    # 1.获取到刚才配置的自定义的配置路径
    def __init__(self, settings_module):

        # 2.global_settings 是 Django自带的配置文件
        # dir(global_settings) ： 获取当前 global_settings 里面的所有变量名
        # DATABASES = {}
        for setting in dir(global_settings):
            # setting ： DATABASES
            # 3.判断当前的变量名是否为大写
            if setting.isupper():
                # 4.是大写的情况下就 setting 中设置 从 global_settings 里面拿出来的值
                # setattr(self, DATABASES, {})
                setattr(self, setting, getattr(global_settings, setting))

        # 5.将传进来的自定义的 配置文件路径存起来 SETTINGS_MODULE
        self.SETTINGS_MODULE = settings_module

        # 6.将自己配置的 配置文件导入到当前位置变成一个对象
        # 等价于 from djangoProject14 import settings
        # mod = settings 对象
        mod = importlib.import_module(self.SETTINGS_MODULE)
        
        # 7.做了一个 元组  INSTALLED_APPS / TEMPLATE_DIRS / LOCALE_PATHS
        tuple_settings = (
            "INSTALLED_APPS",
            "TEMPLATE_DIRS",
            "LOCALE_PATHS",
        )
        
        # 【8】做了一个变量 set() 生成 ---> 集合
        self._explicit_settings = set()
        
        # 【9】遍历当前自己的配置文件中的所有键
        for setting in dir(mod):
            # 【10】判断配置变量名全大写
            if setting.isupper():
                # 【11】从 自定义配置文件中获取出 对应的值 
                setting_value = getattr(mod, setting)
                
                # 【12】告诉你 INSTALLED_APPS 这个参数必须是 列表或者元祖
                if (setting in tuple_settings and
                        not isinstance(setting_value, (list, tuple))):
                    raise ImproperlyConfigured("The %s setting must be a list or a tuple. " % setting)
                
                # 【13】向当前的对象中设置 参数名和参数值
                setattr(self, setting, setting_value)
                
                # 【14】将当前的配置名增加到 集合 中
                self._explicit_settings.add(setting)
'''
from django.conf import global_settings
'''
# 是否开启开发者模式
DEBUG = False

# 并且要配置这个参数 
# HOST 是域名 关闭开发者模式就要限制当前能够访问项目的域名 * 代表任意域名都能访问
ALLOWED_HOSTS = ["*"]

# 配置当前的时区 
# UTC ： 国际时区
# Asia/Shanghai : 上海时区 
TIME_ZONE = 'UTC'

# 如果为 True 就是用上面的 TIME_ZONE 时区的时间
USE_TZ = True

# Django默认的提示语言
# zh-hans 中文 没有对所有暴多都改
LANGUAGE_CODE = 'en-us'

# 有兴趣就多看看
'''
````

