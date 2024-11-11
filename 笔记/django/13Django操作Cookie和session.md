# 1 Cookie和session介绍

````python
# 【一】Cookie和session的发展史
# 1.Cookie和session的用途
# 主要用于保存当前的登陆状态信息
# 2.在一开始的早期网站
# 早期网站：只有一个页面在页面上展示信息
# 没有办法保存你的个人信息
# 3.早期网站不能保存信息 --- Cookie
# 改革：登录成功后用本地的文件来存储用户的信息
# 在页面上输入用户名和密码 ---> 服务端 ---> 你保存到你的当前的浏览器的数据中一份
# 基于本地的保存
# 弊端：
#   一：十个人 登陆你的浏览器 保存十个人的信息 随着人数越多保存的信息越多
#   二：一旦你的电脑丢了  个人的信息 都是保存到本地 不法分子直接通过技术手段 把所有人的信息
# 4.基于本地的数据存储---- session
# 改革：用户登陆成功之后 在服务端通过对你的用户名和密码进行加密 ---> 得到一个加密串
# 再把加密串保存到数据库中一份 ---> 前面有一个 id # 0001(这段加密串ID) abcd(存的是密码加密的数据)
# 把这串加密串的ID发送给浏览器 浏览器再保存
# 下一次用 带着加密串的ID + 自己输入的用户名密码 ---> 对用户名密码加密 ---> 加密串的ID去数据库查上一次的加密串 ---> 就加密串和新加密串对比
# 弊端：
#   一旦一个网站超过百万级的用户数据 对于数据库来说压力就很大了
#   网站会随着时间的推移越来大
# 5.基于数据库的存储  --- token
# 改革：把你的用户名和密码发送给后端 ---> 后端经过一系列的加密逻辑 ---> 加密后将加密串直接返回给前端
# 下一次用 直接输入用户名 和 密码 + 加密串
# 先对  加密串 进行解密 ---> 解密出用户名和密码 ---> 比对你的输入的用户名和密码
````



# 2 Django操作Cookie

````python
# 【一】Cookie介绍
# ● 服务器保存在客户端浏览器上的信息都可以称之为cookie
# ● 指代服务端让客户端保存的数据(存储在客户端上与用户信息相关的数据)
# ● 它的表现形式一般都是k:v键值对(可以有多个)
# 【二】Django操作Cookie借助三板斧对象
# 1.不借助Cookie
'''
def post(self, request, *args, **kwargs):
    # 在前端接收到当前传入的数据
    username = request.POST.get("username")
    password = request.POST.get("password")
    # 并且对数据进行持久化存储 保存数据 保存登陆状态
    if username == "dream" and password == "666":
        with open("data.json", "a") as fp:
            json.dump(obj={
                username: True
            }, fp=fp)
def index(request):
    data = read_data()
    if data.get("dream"):
        return HttpResponse("index")
    else:
        return HttpResponse("未登录请先登陆")
'''

# 2.Django操作Cookie
# (1)设置Cookie
'''
 # 保存登陆状态
# 用变量将 三班度对象存起来
obj = HttpResponse("登陆成功!")
# 在三板斧对象中调用Cookie
obj.set_cookie("sign", username)
return obj
'''
# (2)获取Cookie
'''
# 从当前的请求对象中获取出当前携带的Cookie
print(request.COOKIES)  # {'sign': 'dream'}
sign = request.COOKIES.get("sign")
'''
# (3)清除Cookie
'''
# 退出登陆 将浏览器保存的 Cookie信息清除掉
# 清除登陆状态
# 用变量将 三班度对象存起来
obj = HttpResponse("登陆成功!")
# 在三板斧对象中调用Cookie
obj.delete_cookie("sign")
return obj
'''

# (4)在某些网站那
# 隔一周不登录当前网站再次登陆的时候需要重新登陆
# 当前网站给 Cookie 设置了一个有效时间
# 达到指定时间后会自动清除Cookie
"""
# 在三板斧对象中调用Cookie
obj.set_cookie("sign", username, expires=2)

# max_age 可以放数字 到指定时间后自动清除
# expires = max_age
# expires 参数比较古老 针对 IE 浏览器当时能用
'''
# expires 有效期
``expires`` can be: 
    # 可以是一个正确的字符串格式
    - a string in the correct format,
    # 可以是时间日期类型  UTC 时间
    - a naive ``datetime.datetime`` object in UTC,
    # 可以是 datetime 时间是任意地区的 时间 
    - an aware ``datetime.datetime`` object in any time zone.
    # max_age 可以是 datetime 类型
    If it is a ``datetime.datetime`` object then calculate ``max_age``.
'''
"""
````



# 3 Django操作session

````python
# 【一】session介绍
# ● 保存在服务器上的信息都可以称之为session
# ● 指代服务端保存的跟用户信息相关的数据
# ● 它的表现形式一般都是k:v键值对(可以有多个)

# 我们将数据发送给服务端 服务段加密 存到数据库中返回一个id
# 八ID给前端的浏览器存起来
# 【二】Django操作 session 就需要借助 数据库
# 要进行数据库的迁移
# Django的数据库会自动创还能一个表 django_session
# 1.设置 session
'''
# Django操作session表保存数据
# 借助 request 请求对象
# print(request.session)
# <django.contrib.sessions.backends.db.SessionStore object at 0x1295e7c70>
# Django内部帮你进行了加密
# 首先已经获取到需要加密的值 username + password
# Django操作自己的 session 对 username + password 进行加密
# 基于 settings 里面的加密键 进行加密
# 会将当前加密后的加密串保存到 django_session 表中
# 返回一个 sessionid 给前端浏览器
request.session["sign"] = username + password
return HttpResponse("登陆成功")
'''
# 2.获取session
'''
# 从请求中获取当前的 session
# 从前端获取到当前传入 的 sessionid ---> 去数据库中取 加密串的数据 ---> 反解 ---> 解除加密的时候放进去的值
sign = request.session.get("sign")
print(sign)  # dream666
'''
# 3.清除 session
'''
request.session.delete()
'''
# 4.设置过期时间
'''
# 设置过期时间
request.session.set_expiry(0)
"""
# 可以选的类型
Set a custom expiration for the session. ``value`` can be an integer,
a Python ``datetime`` or ``timedelta`` object or ``None``.

# 如果是一个数字 会经过指定 s 过期
# 如果设置为 0 关闭浏览器 会清空session 
If ``value`` is an integer, the session will expire after that many
seconds of inactivity. If set to ``0`` then the session will expire on
browser close.

# datetime 或  timedelta 对象
If ``value`` is a ``datetime`` or ``timedelta`` object, the session
will expire at that specific future time.

# 如果没有设置  使用全局的 默认时间 14 天
If ``value`` is ``None``, the session uses the global session expiry
policy.
"""
'''
# 5.刷新session
'''
# 客户端和服务端同时清空 session 
# flush 操作文件 MySQL --- > 
request.session.flush()
'''

# 【小作业/思考题】
# 如果我同时设置多个键相同的Cookie/session 取的时候会出现什么效果
# 如果我同时设置多个键不相同的Cookie/session 取的时候会出现什么效果 键都能被取出来还是只能取一个
# 【给你的图书管理系统】 + 注册和登陆功能
# 基于 Cookie  session保存登陆状态
````



# 4 CBV添加装饰器

````python
# 介绍一个装饰器
# 在提交 POST 请求的时候如果 你注释掉中间件
# 会报错

# Django给我们提供了两个装饰器
# 跟你的视图提交 添加事务 取消事务
# 添加 csrf 验证 取消 csrf 验证 两个装饰器

from django.views.decorators.csrf import csrf_protect, csrf_exempt

# csrf_protect 给视图函数添加 csrf 验证
# csrf_exempt 给视图函数取消csrf验证


# 【一】csrf_exempt
'''
from django.views.decorators.csrf import csrf_protect, csrf_exempt
from django.utils.decorators import method_decorator


# 全局取消 局部添加保护 加了一个  csrf 验证
# @csrf_protect # 添加保护
@csrf_exempt  # 取消保护
def login_one(request):
    print(request.POST)
    return render(request, "login.html", locals())


# name 参数就是指定给哪个视图函数加 装饰器
# 方式二：放在类上面给指定视图添加装饰器
# @method_decorator(decorator=csrf_exempt, name="post")
class LoginView(View):
    # 方式三：直接在dispatch上面加  csrf_exempt 可以生效取消当前的 csrf 保护
    @method_decorator(decorator=csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        # 先将当前的请求方式变为全小写 GET --> get
        # 判断当前的请求方式是否在自己允许的请求方式范围内
        if request.method.lower() in self.http_method_names:
            # 从 自己的对象中获取出 当前请求方式小写对应的属性值
            # 获取出当前自己写的 get 函数内存地址 没有就报错
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        # 调用 自己写的 get 函数 ---> get 函数返回了三板斧对象
        return handler(request, *args, **kwargs)

    def get(self, request, *args, **kwargs):
        return render(request, "login.html", locals())

    # @csrf_exempt  # 直接添加添加装饰器不生效
    # 方式一：直接写在具体的函数上面 没有效果
    # @method_decorator(decorator=csrf_exempt)  # 直接放到指定的视图函数头上 可以不写 name参数
    def post(self, request, *args, **kwargs):
        print(request.POST)
        return render(request, "login.html", locals())

'''

# 【二】csrf_protect
'''
from django.views.decorators.csrf import csrf_protect, csrf_exempt
from django.utils.decorators import method_decorator


# 全局取消 局部添加保护 加了一个  csrf 验证
@csrf_protect # 添加保护
# @csrf_exempt  # 取消保护
def login_one(request):
    print(request.POST)
    return render(request, "login.html", locals())


# name 参数就是指定给哪个视图函数加 装饰器
# 方式二：放在类上面给指定视图添加装饰器 没有效果
# @method_decorator(decorator=csrf_exempt, name="post")
# 方式二：放在类上面给指定视图添加装饰器 有效果
# @method_decorator(decorator=csrf_protect, name="post")
class LoginView(View):
    # 方式三：直接在dispatch上面加  csrf_exempt 可以生效取消当前的 csrf 保护
    # @method_decorator(decorator=csrf_exempt)
    @method_decorator(decorator=csrf_protect)
    def dispatch(self, request, *args, **kwargs):
        # 先将当前的请求方式变为全小写 GET --> get
        # 判断当前的请求方式是否在自己允许的请求方式范围内
        if request.method.lower() in self.http_method_names:
            # 从 自己的对象中获取出 当前请求方式小写对应的属性值
            # 获取出当前自己写的 get 函数内存地址 没有就报错
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        # 调用 自己写的 get 函数 ---> get 函数返回了三板斧对象
        return handler(request, *args, **kwargs)

    def get(self, request, *args, **kwargs):
        return render(request, "login.html", locals())

    # @csrf_exempt  # 直接添加添加装饰器不生效
    # 方式一：直接写在具体的函数上面 没有效果
    # @method_decorator(decorator=csrf_exempt)  # 直接放到指定的视图函数头上 可以不写 name参数
    # 方式一：直接写在具体的函数上面 有效果
    # @method_decorator(decorator=csrf_protect)  # 直接放到指定的视图函数头上 可以不写 name参数
    def post(self, request, *args, **kwargs):
        print(request.POST)
        return render(request, "login.html", locals())
'''

# 【总结】
# csrf_exempt : 取消当前的 csrf 保护
# method_decorator 给CBV视图添加
#   只对 dispatch 生效
# csrf_protect : 添加当前的 csrf 保护
# method_decorator 给CBV视图添加
#   对 dispatch 生效
#   在指定函数头上生效
#   放在类上面生效

# 如果想要给 类视图函数 加装饰器效果就必须借助 method_decorator 这个装饰器
````



# 5 中间件

````python
# 【一】Django的声明周期中
# 第一个进入的地方就是Django的中间件
# Django的中间也被称之为 Django的 保安

# 【二】Django中间件的作用
# 1.校验当前请求是否合法
# csrf 验证 如果不携带对应的标识就会抛出异常
'''
# 安全验证
'django.middleware.security.SecurityMiddleware',
# session帮你操作session
'django.contrib.sessions.middleware.SessionMiddleware',
# 公共的中间件
'django.middleware.common.CommonMiddleware',
# csrf 验证
'django.middleware.csrf.CsrfViewMiddleware',
# 认证和授权 --- Django的admin后台
'django.contrib.auth.middleware.AuthenticationMiddleware',
# Djang的信息机制
'django.contrib.messages.middleware.MessageMiddleware',
# Django2.x之后新增的一个中间 在一个 html 页面中又嵌入了一个 html 页面 ---> 可能被黑客攻击
'django.middleware.clickjacking.XFrameOptionsMiddleware',
'''

'''
<html>
<html>
</html>
</html>
'''
# 2.总结
# ● 认证和授权：
#   ○ 中间件可以在请求到达视图之前进行用户认证和权限验证，确保只有经过授权的用户才能访问敏感资源。
# ● 请求和响应处理：
#   ○ 中间件可以在请求到达视图之前对请求进行预处理
#     ■ 例如添加请求头信息、检查请求参数的合法性等操作。
#   ○ 同时，在视图函数返回响应给客户端之前，中间件还可以对响应进行后处理
#     ■ 例如添加额外的响应头、包装响应数据等操作。
# ● 异常处理：
#   ○ 中间件还可以捕获视图函数中可能抛出的异常，并做相应的处理
#     ■ 例如记录异常日志、返回自定义错误信息等。
# ● 性能优化：
#   ○ 通过中间件，可以对请求进行性能监测、缓存处理、压缩响应等操作，提升网站的整体性能。

# 【三】源码剖析
# 安全验证
from django.middleware.security import SecurityMiddleware
'''
# 处理请求进入的时候的安全校验的
def process_request(self, request):
    host = self.redirect_host or request.get_host() # 127.0.0.1:8000
    get_full_path 携带了请求参数 并且是 8000 后面的所有路径
    return HttpResponsePermanentRedirect(
                "https://%s%s" % (host, request.get_full_path())
            )
# 处理响应出去的时候的安全校验的
def process_response(self, request, response):
'''
# session帮你操作session
from django.contrib.sessions.middleware import SessionMiddleware
from django.contrib.sessions.backends.db import SessionStore
'''
# 校验请求中携带的 sessionid 是否合法
def process_request(self, request):
    session_key = request.COOKIES.get(settings.SESSION_COOKIE_NAME)
    request.session = self.SessionStore(session_key)
# 处理响应的
def process_response(self, request, response):
'''
# 公共的中间件
from django.middleware.common import CommonMiddleware
'''
def process_request(self, request):
def should_redirect_with_slash(self, request):
def get_full_path_with_slash(self, request):
def process_response(self, request, response):
'''
# csrf 验证
from django.middleware.csrf import CsrfViewMiddleware
'''
def process_request(self, request):
# 在真正的进入视图函数之前进行处理的逻辑
def process_view(self, request, callback, callback_args, callback_kwargs):
def process_response(self, request, response):
'''
# 认证和授权 --- Django的admin后台
from django.contrib.auth.middleware import AuthenticationMiddleware
'''
def process_request(self, request):
'''

# Djang的信息机制
from django.contrib.messages.middleware import MessageMiddleware
# Django2.x之后新增的一个中间 在一个 html 页面中又嵌入了一个 html 页面 ---> 可能被黑客攻击
from django.middleware.clickjacking import XFrameOptionsMiddleware


# 【四】自定义中间件
# 1.创还能一个存储中间件的文件
# middwares.py
# 2.去看别人怎么写
'''
from django.utils.deprecation import MiddlewareMixin


class MiddlewareOne(MiddlewareMixin):
    def process_request(self, request):
        print("process_request :>>>> MiddlewareOne")

    # 在真正的进入视图函数之前进行处理的逻辑
    def process_view(self, request, callback, callback_args, callback_kwargs):
        print("process_view :>>>> MiddlewareOne")

    def process_response(self, request, response):
        print("process_response :>>>> MiddlewareOne")
        return response
'''
# 3.注册中间件
'''
'''
````

