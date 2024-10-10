# 1 三板斧对象补充

## 1.1 HttpResponse

````python
# 回顾之前学习的用法和返回json格式数据的用法

# url里面要写好
path('http_response/', views.http_response, name='http_response'),

# appforsbf/view.py
from django.http import HttpResponse
import json

def http_response(request, *args, **kwargs):
    # 1.最常见的是返回纯文本类型的数据
    # return HttpResponse("ok")
    # 这样就是直接 去/http_response/ 可以显示想要的内容

    # 2.那么如果返回JSON格式的数据 会是什么效果呢
    # 写一个字典
    data = {"name": "sheenagh", "age": 23}

    # 【1】直接将字典给 HttpResponse 返回的数据是字典的键
    # return HttpResponse(data)
    # nameage - 可是这不是预期的json格式

    # 【2】对 HttpResponse 的 content_type 进行约束 ，响应格式是 application/json
    # 前端的 content-type:application/json
    # 通过设置 content_type 指定响应类型是json 把想要返回的东西传出去
    # return HttpResponse(data, content_type="application/json")
    # nameage - 样式有变化 但内容仍然和想要的不一样
    # 这是因为 data是dict 而不是 json

    # 【3】对 HttpResponse 的 content_type 进行约束 然后对数据进行序列化
    # 把字典进行序列化 用dumps转成json字符串
    data_str = json.dumps(data)
    # 再通过设置 content_type 指定响应类型是json 把想要返回的东西传出去
    return HttpResponse(data_str, content_type="application/json")
    # {
    #     "name": "sheenagh",
    #     "age": 23
    # }
````

## 1.2 render

```python
# render 和 httpresponse 不太一样 它是把指定的模板渲染成html 把上下文数据传给模版
# 这样的话 前端可以显示动态内容 这是 httpresponse 据了解 做不到的部分
# 怎么用呢
# url里面写好
path("render_response/", views.render_response, name='render_response'),

# view里面写好
# templates里面写好


# appforsbf/view.py
def render_response(request):
    info_data = {"name": "sheenagh", "age": 23}
    # 这些是render方法 接收的参数
    # request: 视图函数中的 request 对象
    # template_name: 前端文件名
    # context: 上下文对象
    # content_type: 响应数据格式

    # 1.以指定的键值对传递数据
    # info_data ---> {"name": "sheenagh", "age": 23}
    # return render(request, "render_response.html", info_data)
    # 效果: name age 能出来 info_data 和 info_data.name出不来

    # 2.加载局部名称空间
    # locals()  ---> {"info_data":info_data}
    return render(request, "render_response.html", locals())
    # 效果: 相反 info_data 能出来 info_data.name 能出来

    # 进一步补充: 比如有dict1 dict2 都有name 通过locals传过去
    # 在html中 dict1.name dict2.name 就好了
```

```python
# 关于全局局部
# 查看全局名称空间 globals()
# 查看局部名称空间 locals()
# 局部修改全局名称空间 global
# 内嵌修改局部名称空间 nonlocal

print(globals())
print(locals())
number = 99
def outer():
    global number
    number = 999
    print(number)  # 999
    age = 18
    def inner():
        nonlocal age
        age = 28
        print(age) # 28
    print(age) # 28
print(number) # 999
```

## 1.3 redirect

```python
# 前面已经详细讲过了
# 重定向至指定的地址/解析地址
```



# 2 JSONResponse

## 2.1 通过  HttpResponse 渲染JSON格式的数据

````python
# 前面研究了怎么通过 httpResponse 渲染 json 格式的数据
# 1.对数据进行序列化
# data_str = json.dumps(data)
# 2.对 HttpResponse 的 content_type 进行约束
# return HttpResponse(data_str, content_type="application/json")
````

## 2.2 通过  JSONResponse 渲染JSON格式的数据

````python
# 实际上在django里有一个东西可以完成类似的功能 且更好
# 通过 JSONResponse 渲染JSON格式的数据

# appforsbf/view.py
from django.http import JsonResponse

def json_response(request):
    info_data = {"name": "sheenagh杨", "age": 23}

    # 【一】HttpResponse 返回JSON数据
    info_data_str = json.dumps(info_data, ensure_ascii=False)
    # {"name": "dream\u5927\u54e5", "age": 18} 中文会被转换为 ASCII 码
    # 补充 在我的电脑上 直接就可以英文中文 没有出现乱码情况
    # return HttpResponse(info_data_str, content_type="application/json")

    # 【二】JsonResponse 返回JSON格式数据
    # 1.直接渲染 字典数据 会按照 ASCII 码进行转码
    # 补充 在我的电脑上 直接就可以英文中文 没有出现乱码情况
    # 如果乱码 类似下面
    # {"name": "dream\u5927\u54e5", "age": 18} 中文会被转换为 ASCII 码
    # return JsonResponse(info_data)

    # 2.通过 json_dumps_params 添加额外的参数
    # 总之就记住这样的用法 记得把ensure_ascii设置成False
    return JsonResponse(info_data, json_dumps_params={"ensure_ascii": False})
    # {
    #     "name": "sheenagh杨",
    #     "age": 23
    # }
    # 这样可以正常显示了
````

```python
class JsonResponse(HttpResponse):
    """
    An HTTP response class that consumes data to be serialized to JSON.

    :param data: Data to be dumped into json. By default only ``dict`` objects
      are allowed to be passed due to a security flaw before EcmaScript 5. See
      the ``safe`` parameter for more information.
    :param encoder: Should be a json encoder class. Defaults to
      ``django.core.serializers.json.DjangoJSONEncoder``.
    :param safe: Controls if only ``dict`` objects may be serialized. Defaults
      to ``True``.
    :param json_dumps_params: A dictionary of kwargs passed to json.dumps().
    # json_dumps_params 我们想要让 json 模块进行额外操作的参数 
    """

    def __init__(self, data, encoder=DjangoJSONEncoder, safe=True,
                 json_dumps_params=None, **kwargs):
        if safe and not isinstance(data, dict):
            raise TypeError(
                'In order to allow non-dict objects to be serialized set the '
                'safe parameter to False.'
            )
        if json_dumps_params is None:
            json_dumps_params = {}
        kwargs.setdefault('content_type', 'application/json')
        data = json.dumps(data, cls=encoder, **json_dumps_params)
        super().__init__(content=data, **kwargs)

```



# 3 form表单提交文件数据

## 3.1 form表单提交普通数据

```html
{#创建容器#}
<div class="container-fluid">
    {#  栅格系统   #}
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
       <form action="" method="post" enctype="application/x-www-form-urlencoded">
                <div class="form-group">
                    <label for="InputUsername">Username</label>
                    <input type="text" name="username" class="form-control" id="InputUsername" placeholder="Username">
                </div>
                <div class="form-group">
                    <label for="InputPassword">Password</label>
                    <input type="password" name="password" class="form-control" id="InputPassword"
                           placeholder="Password">
                </div>
                <div class="form-group">
                    <label for="InputFile">Avatar</label>
                    <input type="file" id="InputFile" name="avatar">
                    <p class="help-block">点击上传文件 上传头像</p>
                </div>
                <div class="form-group">
                    <span><b>爱好</b></span>
                    <br>
                    <label class="checkbox-inline">
                        <input type="checkbox" id="inlineCheckbox1" name="hobby" value="music"> music
                    </label>
                    <label class="checkbox-inline">
                        <input type="checkbox" id="inlineCheckbox2" name="hobby" value="rap"> rap
                    </label>
                    <label class="checkbox-inline">
                        <input type="checkbox" id="inlineCheckbox3" name="hobby" value="basketball"> basketball
                    </label>

                </div>

                <div class="form-group">
                    <span><b>性别</b></span>
                    <br>
                    <label class="radio-inline">
                        <input type="radio" name="gender"  id="inlineRadio1" value="male"> 男
                    </label>
                    <label class="radio-inline">
                        <input type="radio" name="gender" id="inlineRadio2" value="female"> 女
                    </label>
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
        </div>
    </div>
</div>
```

````python
# 【一】上传数据
'''
<form action="" method="post" enctype="application/x-www-form-urlencoded">
'''
'''
print(request.POST)
<QueryDict: {'username': ['dream'], 'password': ['1111'], 'avatar': ['Python全栈学习路线图.png'], 'hobby': ['music', 'rap'], 'gender': ['male']}>
'''
username = request.POST.get("username")
password = request.POST.get("password")
avatar = request.POST.get("avatar")
hobby = request.POST.getlist("hobby")
gender = request.POST.get("gender")
print(f"""
username :>>>> {username}
password :>>>> {password}
avatar :>>>> {avatar, type(avatar)}
hobby :>>>> {hobby}
gender :>>>> {gender}
""")
# 我们发现  avatar :>>>> ('Python全栈学习路线图.png', <class 'str'>) 不是一个文件数据
# 默认 form 表单提交数据的格式是 : enctype="application/x-www-form-urlencoded"
# 会将所有的键值对数据传给后端 对文件数据不做额外的处理 只传递键值对
````

## 3.2 form表单提交文件数据

`````python
# 修改 form 表单提交数据的方式
'''
<form action="" method="post" enctype="multipart/form-data">
'''
# 将提交数据方式改为 multipart/form-data 发现原本的avatar 变成了 avatar :>>>> (None, <class 'NoneType'>)
# form表单在提交文件数据的时候对文件数据进行了单独的处理
# 对于后端来说也要进行单独的处理
file_obj = request.FILES.get("avatar")
print(file_obj, type(file_obj))
'''
<MultiValueDict: {'avatar': [<TemporaryUploadedFile: Python全栈学习路线图.png (image/png)>]}>
Python全栈学习路线图.png <class 'django.core.files.uploadedfile.TemporaryUploadedFile'>
'''
file_data = file_obj.read()
base_dir = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
file_path = os.path.join(base_dir, "static", 'avatar')
os.makedirs(file_path, exist_ok=True)
with open(os.path.join(file_path, file_obj.name), "wb") as fp:
    fp.write(file_data)

return redirect(reverse("register"))
`````



# 4 request方法补充

````python
def request_methods(request):
    # print(dir(request))
    '''
    ['COOKIES', 'FILES', 'GET', 'META', 'POST', 'accepted_types', 'accepts', 'body', 'build_absolute_uri', 'close', 'content_params', 'content_type', 'encoding', 'environ', 'get_full_path', 'get_full_path_info', 'get_host', 'get_port', 'get_raw_uri', 'get_signed_cookie', 'headers', 'is_ajax', 'is_secure', 'method', 'parse_file_upload', 'path', 'path_info', 'read', 'readline', 'readlines', 'resolver_match', 'scheme', 'session', 'upload_handlers', 'user']
    '''

    # 【一】 request.FILES 存放所有提交的文件数据
    # 【二】 request.COOKIES 存放所有本地的用户信息
    # 【三】 request.GET 存放 get 请求提交的数据
    # 【四】 request.POST 存放 post 请求提交的数据
    # 【五】 request.META 存放 本地的环境变量和 请求头中的部分参数
    # 【六】 request.body 存放 请求体的二进制数据
    # 【七】 request.get_full_path 存放 完整的路径带参数
    print(request.get_full_path())  # /user/request_methods/?username=
    # 【八】 request.get_full_path_info 存放 完整的路径带参数
    print(request.get_full_path_info())  # /user/request_methods/?username=
    # 【九】 request.path 存放 当前访问的路径不带参数
    print(request.path)  # /user/request_methods/
    # 【十】 request.path_info 存放 当前访问的路径不带参数
    print(request.path_info)  # /user/request_methods/

    return render(request, "request_methods.html", locals())
````



# 5 CBV和FBV

````python
# 【一】FBV
# function base view : 在视图函数中处理请求
# 我们现在一直在写的 视图就是 FBV 视图

# 【二】CBV
# class base view : 在视图类中处理请求
# 在一个类中定义视图函数并处理请求
# 【1】在视图文件中定义视图类
# 创建一个类 ---> 继承Django的视图类
from django.views import View


class LoginView(View):
    # 我们现在学的请求方式有两种 ---> post / get

    # 当前端在访问当前路由的时候是 get 请求就会走到这里
    def get(self, request):
        return render(request, "login.html", locals())

    # 当前端在访问当前路由的时候是 post 请求就会走到这里
    def post(self, request):
        username = request.POST.get("username")
        password = request.POST.get("password")
        hobby = request.POST.getlist("hobby")
        gender = request.POST.get("gender")
        avatar_obj = request.FILES.get("avatar")
        print(f"""
        username :>>>> {username}
        password :>>>> {password}
        avatar :>>>> {avatar_obj, type(avatar_obj)}
        hobby :>>>> {hobby}
        gender :>>>> {gender}
        """)
        return redirect(reverse("login"))
# 【2】在路由系统中注册当前视图类
path("login/", views.LoginView.as_view(), name="login"),
````



# 6 CBV源码剖析

````python
# 【一】入口
# 触发 LoginView.as_view()
path("login/", views.LoginView.as_view(), name="login")

# 【二】看 LoginView.as_view 里面写了什么逻辑
from django.views import View


class LoginView(View):
    # 我们现在学的请求方式有两种 ---> post / get

    # 当前端在访问当前路由的时候是 get 请求就会走到这里
    def get(self, request):
        ...

    # 当前端在访问当前路由的时候是 post 请求就会走到这里
    def post(self, request):
        ...


# 【1】进入到 LoginView 类里面会发现只有 get 方法 和 post 方法
# 【2】自己没有 ---> 去父类里面找 ---> View

# 【三】去 django.views 里面的 View 看 as_view
class View:
    """
    Intentionally simple parent class for all views. Only implements
    dispatch-by-method and simple sanity checking.
    """

    http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']

    def __init__(self, **kwargs):
        """
        Constructor. Called in the URLconf; can contain helpful extra
        keyword arguments, and other things.
        """
        # Go through keyword arguments, and either save their values to our
        # instance, or raise an error.
        for key, value in kwargs.items():
            setattr(self, key, value)

    # 【一】这是一个类的绑定方法 ---> 所以可以通过 类名.as_view 触发
    # LoginView.as_view()
    @classonlymethod
    def as_view(cls, **initkwargs):
        # 【1】initkwargs 参数字典 应该存着键值对数据 {"get":...}
        # 遍历上面参数字典的键
        for key in initkwargs:
            # 【2】判断字典中的键在不在 上面的 http_method_names
            if key in cls.http_method_names:
                # 如果 循环得到的键 在上述列表中 抛出异常
                raise TypeError(
                    'The method name %s is not accepted as a keyword argument '
                    'to %s().' % (key, cls.__name__)
                )
            # 【3】如果当前类属性中没有循环得到的键对应的属性值
            if not hasattr(cls, key):
                # 又抛出异常 如果已经在 initkwargs 中定义了 get键对应的值 就 不允许在当前视图类中再重写 get 方法
                raise TypeError("%s() received an invalid keyword %r. as_view "
                                "only accepts arguments that are already "
                                "attributes of the class." % (cls.__name__, key))

        # 【三】分析 view 被调用会发生什么事?
        def view(request, *args, **kwargs):
            # 【1】cls ---> LoginView
            # 将你的传入的额外的参数传递给 LoginView 实例化得到一个 LoginView 的对象
            self = cls(**initkwargs)  # self = LoginView(**initkwargs)
            # 【2】self.setup ---> view 里面的 setup 方法
            # 将head方法替换成 get 方法并且初始化 request属性和 args 和 kwargs 属性
            self.setup(request, *args, **kwargs)
            # 【3】判断自己是否有 request 属性
            if not hasattr(self, 'request'):
                # 如果自己的属性中没有 request 属性则直接抛出异常
                raise AttributeError(
                    "%s instance has no 'request' attribute. Did you override "
                    "setup() and forget to call super()?" % cls.__name__
                )
            # 【4】self.dispatch ： 获取 dispatch 方法的返回值 ---> view 里面的 dispatch 方法
            # self.dispatch ---> 调用了自己在 LoginView 定义的 get 或者 post 方法返回了 三板斧对象
            return self.dispatch(request, *args, **kwargs)

        # 【四】初始化属性
        view.view_class = cls
        view.view_initkwargs = initkwargs

        # 更新装饰器
        update_wrapper(view, cls, updated=())

        # 更新装饰器
        update_wrapper(view, cls.dispatch, assigned=())

        # 【二】 as_view() 返回值 是一个 view
        # 返回的是一个闭包函数
        return view

    # 【2.1】进入到 view 的 setup 方法 中
    def setup(self, request, *args, **kwargs):
        # （1）self = LoginView(**initkwargs)
        # hasattr(self, 'get') : 判断自己对象中是否有 get 方法
        # not hasattr(self, 'head') : 判断自己对象中没有 get 方法
        if hasattr(self, 'get') and not hasattr(self, 'head'):
            # （2）有 get 方法 并且 没有 head 方法
            # 于是将 head 属性替换成 get 属性
            self.head = self.get
        # （3）把Django的request对象初始化进自己的属性中
        self.request = request
        # （4）args 可变长位置参数初始化进去
        self.args = args
        # （5）kwargs 可变长关键字参数初始化进去
        self.kwargs = kwargs

    # 【4.1】进入到 view 的 dispatch 方法 中
    def dispatch(self, request, *args, **kwargs):
        # （1）request.method.lower()：将当前的请求方式转换成小写 GET ---> get
        # 判断小写的 请求方式在不在 上面定义好的 http_method_names
        if request.method.lower() in self.http_method_names:
            # （2）小写的请求方式在上面的列表中
            # self = LoginView(**initkwargs)
            # request.method.lower() ---> get / post
            # 在当前的对象中映射 get 方法 或者 post 方法
            # handler 其实就是当前类中定义的 get 的函数地址或者post的函数地址
            # self.http_method_not_allowed
            # 如果自己类里面写了方法 就返回方法 没有则报错 当前方法不被允许
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            # （3）如果提交的请求方式不在允许的请求方式范围内直接返回错误信息
            handler = self.http_method_not_allowed
        # （4）调用上面获取到的函数地址
        # 正常调用 get 函数 返回三板斧对象
        return handler(request, *args, **kwargs)

    # 【4.2】http_method_not_allowed 是什么
    def http_method_not_allowed(self, request, *args, **kwargs):
        # 记录了一条警告日志 请求方式不被允许
        logger.warning(
            'Method Not Allowed (%s): %s', request.method, request.path,
            extra={'status_code': 405, 'request': request}
        )
        # 返回了 HttpResponseNotAllowed
        # self._allowed_methods() ---> ["GET","POST"] --->如果没有写 http_method_names 里面的任何方法则返回一个空列表
        return HttpResponseNotAllowed(self._allowed_methods())

    def options(self, request, *args, **kwargs):
        """Handle responding to requests for the OPTIONS HTTP verb."""
        response = HttpResponse()
        response.headers['Allow'] = ', '.join(self._allowed_methods())
        response.headers['Content-Length'] = '0'
        return response

    # 【4.3】
    def _allowed_methods(self):
        # 返回了一个列表 列表中存的是当前对象中存在的 方法的大写名
        return [m.upper() for m in self.http_method_names if hasattr(self, m)]


# 【四】回到了urls路由映射 回到了入口
# 访问 request_methods ---> 触发 request_methods 返回了三板斧对象
path("request_methods/", views.request_methods, name='request_methods'),
# 视图类的路由定义方式
# LoginView.as_view() ---> 放的就是View的 view 的函数地址
# view 的函数地址 () --> get / post ---> 三板斧对象
path("login/", views.LoginView.as_view(), name="login"),

# 【五】小结
# LoginView.as_view() ---> 返回了 view 的函数地址 ---> view 返回了 self.dispatch ---> dispatch 找到对应的 get / post 方法调用返回 get / post 方法返回的 三板斧对象
````

