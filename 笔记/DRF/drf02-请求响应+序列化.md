# 昨日回顾

```python
# 1 前后端开发模式
	-混合开发:
    	模板:里面有html,css,js + 模板语法（同语言的模板语法）-dtl
		模板在后端渲染->类python语法->后端执行->本质是字符串替换
        后端返回给前端时->http响应->响应体->就是原来的模板被渲染完后的字符串
    -前后端分离开发:
    	前端和后端完全分开
        前端使用前端框架:Vue,react -> 只写前端
        前端通过ajax->后端提供接口->获取到数据->通过js+dom操作 渲染页面
    	后端只需要提供接口
        大前端:Web前端,小程序,app,桌面应用(qt)
    
# 2 API接口
	-一个路径,放回就会返回xml或json格式的数据
    -需要有:
        -地址
        -请求方式
        -请求参数
        	-路径中 地址栏中参数
            -请求体中 请求体中参数:urlencoded，form-data，json(这个用的多)
        -请求头
        -响应:json

# 3 接口测试工具-postman
	-pycharm,postman,后面需要用的各种软件->千万不要汉化 慢慢习惯上手 不会的多看 涉及单词量并不多只是没见而已
    -基本使用
    	-发送请求:get,post,delete,put(*面试题问区别)
        -携带请求参数:地址栏
        -携带请求体:多种编码格式
        -携带请求头:key-value [cookie,user-agent,referer]
        -响应_响应状态码
        -响应_响应头:key-value[cookie]
        -响应_响应体:html,json,xml
    -注册登录后
    	-保存之前访问过的路径
        -导出导入
        
# 4 restful规范(*多看 背一背)
    -1 数据安全保证，使用https部署
    -2 接口中带api标识
    	http://api.lqz.com/
        http://www.lqz.com/api/
    -3 接口中带版本标识
    	http://www.lqz.com/api/v1
    -4 数据即资源，以名词表示，可以使用复数
    -5 请求方式决定对资源的操作方式
    	-get:获取数据  获取所有/获取单体
        -post:新增数据
        -put:修改数据
        -delete:删除数据
    -6 响应带状态码
    	-http: 2xx (背)
        -自己定制的状态码
    -7 请求地址中带过滤条件  ?name=xx
    -8 响应中带错误信息
    	{code:100,msg:'xx'}
    -9 响应中带链接地址
    -10 响应遵循以下规则
    	-查询所有 [{},{}]
        -查询单条 {}
    	-新增一条 {}
        -修改一条 {}
        -删除一条 ''
        
# 5 序列化和反序列化
	-序列化:把我们能识别的数据格式(python:列表,字典,对象) 转换成其他格式(json格式字符串) 给其他人
    -反序列化:把别人的数据格式(json格式),转成我们能识别的数据格式(python:列表,字典,对象)
        
# 6 drf
	-必须用在django框架上 本质是django的一个第三方app
	-方便我们快速实现符合restful规范的接口
    -版本: django:4.2.2 drf:最新 mysql:必须8.0以上
    -5个接口-> 以后所有接口都是这5个接口及变形
    	-增一条
        -查一条
        -删一条
        -改一条
        -查所有
        
# 6 drf要学习的
	-1 请求和响应
    	-request:请求对象:请求方式，请求携带的数据，request.GET,request.POST,requset.body,request.META:请求头中得
        -四件套
        	-HTTPResponse('sdfs')
        	-JsonResponse(字典列表)
        	-render(request,模板文件)
        -drf有自己的请求和响应
        	-request
            -Response 的对象
            
    -2 序列化类:Serializer
    	-做序列化
        -做反序列化的
        -反序列化的校验
        
    -3 视图类:drf提供了非常多 视图类可以继承
    -4 路由
    -5 认证:登录认证
    -6 权限
    -7 频率限制
    -9 统一返回错误
    -10 jwt:登录认证用
    -11 基于角色的访问控制
    
```



# 补充

```python
#0 自己先补 不然容易乱
	-关于http请求这个东西:
	-请求结构包括请求行,请求头,空行,请求体四个部分
    -请求方法最主要的包括 get,post,put,delete 这也和这个单元息息相关
    
    -我作为客户端 向服务器发送http请求
    -这个请求里面 请求行就有包括上面的请求方法 请求体就有下面的3种格式
    -不同的请求方法+不同的请求体格式 会有不同的效果出来

#1 http请求 请求体有不同编码格式，请求体什么样子
	-1 form-data     上传文件+数据
    -2 urlencoded    form表单默认的编码格式
    -3 json          json格式字符串形式

# 1.1 jquery的ajax处理请求体的不同编码格式要怎么做
	-编码格式是 urlencoded
    $.ajax({
        url:地址,
        method:post,
        content-type:urlencoded
        data:{name:sheenagh,password:526}, # name=sheenagh&password=526 请求体中
        success:function(data){
            console.log(data)
        }
    })
    
    -编码格式是 form-data
	-形式:
        ----------------------------153808361197024804700642\r\n
        数据部分
        ----------------------------153808361197024804700642\r\n
        文件二进制部分
        ----------------------------153808361197024804700642--\r\n'
    $.ajax({
        url:地址,
        method:post,
        processData:false, # 不预处理数据
        content-type:false, # 不指定编码
        data:formdata对象
        success:function(data){
            console.log(data)
        }
    })
    
	-编码格式是 json
    $.ajax({
        url:地址,
        method:post,
        content-type:application/json, # 指定编码
        data:{name:sheenagh,password:526} # json格式字符串 {name:sheenagh,password:526}
        success:function(data){
            console.log(data)
        }
    })

# 1.2 postman 
	-urlencoded
    -form-data
    -json
    
# 1.3 python request模块
	-urlencoded
    -form-data
    -json
    
#2 get请求能不能在请求体中携带数据？
	-能
    -一般都是post或put请求在请求体中带数据
    -地址栏中输入?name=sheenagh的时候 用request.body就可以看到这个数据了
    
# 3 get请求和post请求区别？
# 总结:
	-django视图类或视图函数的request
    	-request.POST:只针对于 post请求的urlencoded编码格式和form-data的数据 才有数据
        -request.GET:请求地址栏中得参数:get，post，delete，put
        	-<QueryDict: {'key': ['88', '99']}>
            -类字典:取出key的值
            	-只取一个:request.GET.get('key')
                -两个都取:request.GET.getlist('key')
        -request.body:无论什么请求,无论什么编码格式,都在里面
        
   	-http不同编码格式
    	-urlencoded:在请求体中是  key=value&key=value
        -json:在请求体中是  {"name":"lqz","age":19}
        -form-data: asdfasdf-------数据部分  asdfasdfasdf-----文件部分
        
        
# put请求，请求体中数据如何获取?
	-统一使用json格式->从request.body中取出后，自行反序列化
    
# 所以:django默认只反序列化了 post请求的urlencoded格式
	-其他格式，其他请求方式，dou 需要我们自行处理
```



# 今日内容

```python
# drf 是django的一个app -> 作用是增强Django -> 可以快速实现符合restful规范的接口

# 预览
# drf封装的一些东西
	1 请求对象
    2 响应对象
    3 序列化类
    	-数据库取出来 -> 序列化 -> 给前端:序列化
        -前端传入 -> 校验过后 -> 保存到数据库:反序列化
    4 视图类
    	-非常非常多
    5 路由
    6 排序 
    7 过滤
    8 分页
    9 全局异常处理
    10 认证
    11 权限
    12 频率
    13 JWT
    14 接口文档
    15 acl，rbac权限控制
```



```python
# 记录搭建一个API类的过程
# 1.先创建好project(CountDown95)和app(app01)
# 2.app01/views.py

from django.http import HttpResponse
# ### 1 以后都写视图类 CBV ### #
from django.views import View
# ### 2 以前继承View ###
# ### 3 以后继承APIView ###
from rest_framework.views import APIView

class DemoView(APIView):
    def get(self, request):
        return HttpResponse("ok-get-demo")

# 3.app01/urls.py

from django.urls import path
from .views import DemoView

urlpatterns = [
    path('demo/', DemoView.as_view()),
]

# 4.CountDown95/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/app01/', include('app01.urls')),
]

# 5.访问http://localhost:8000/api/v1/app01/demo/
# 可以得到想要的response

# 6.多做一步 不注册的话有些东西调用不了
INSTALLED_APPS = [
    ...
    'rest_framework',
    ...
]
```



# 1 新的Request对象和Response

## 1.1 Request

```python
# 1 以后只要继承了APIView的视图类，request对象，就变成新的了
	-新的:rest_framework.request.Request
    -老的:django.core.handlers.wsgi.WSGIRequest
    
# 2 WSGIRequest的属性或方法 -> request对象 本质是 http请求，当次请求对象
	-request.method
    -request.POST
    -request.GET
    -request.FILES
    -request.body
    -request.get_full_path()
    -request.META # 字典 -> 请求头的数据，从这里面取出
    	-客户端类型:HTTP_USER_AGENT
        -客户端地址:REMOTE_ADDR
        -referer
        -客户端可以自行携带请求头，但是后端取的时候，需要 HTTP_请求头名
        ...
        
# 3 新的Request
	-用起来跟老的一样 -> 兼容老的
    
    -多的:
    	request.data  # 无论哪种编码格式(urlencoded,form-data,json),无论那种请求方式，都从它中取出来 所以很好
        request.query_params # 本质就是request.GET,为了迎合restful规范中的请求地址中带查询条件
        
        request._requset  # 原来老的request变为这个
```

```python
# 再总结下
# 1 以后全是CBV写法，继承一个基类 -> drf的APIView【也继承了Django的View】及子类
# 2 继承APIView后，以后响应统一用 drf 的响应
'''
        1 request 请求对象:http请求的所有内容，都会在这个对象中
        2 继承APIView后的request对象，不是之前的request对象了
            - 原来的:django.core.handlers.wsgi.WSGIRequest
            - 新的:rest_framework.request.Request
        3 用起来跟之前一样
            print(request.method)
            print(request.POST)
            print(request.GET)
            print(request.get_full_path())
        4 request.data 新用法
            -无论哪种编码格式，请求体的数据，都在这里面
                -urlencoded,form-data:是QueryDict的对象
                -json:是普通字典对象
                -但是，都当字典用即可
        5 request.FILES
            -前端传的文件从这里取
        6 request.query_params # 查询参数
            -request.GET
'''
```

## 1.2 Response

```python
# 0 以后写drf的响应，统一都用 Response -> 不要再用四件套了
	-Response等同于:HttpResponse+JsonResponse
    -就是可以放字符串/字典/列表
    
# 1 from rest_framework.response import Response

# 2 如果使用浏览器访问会报错，使用postman访问不报错
	## 报错
    TemplateDoesNotExist at /api/v1/app01/demo/
    rest_framework/api.html
    ## 解决，在配置文件中，注册rest_framework 这个app即可
    ## 因为如果是浏览器访问 -> drf会用一个好看的模板渲染，不注册app，找不到模板
    
# 3 如何用？
	-1 直接放字典，列表或字符串 Response('ok')
    	-放在响应体中了
        
# 4 如何放状态码？Response('ok',status=201)

# 5 如何放响应头？Response('ok',status=201,headers={'xx':'yy'})

# Response(data='ok--post--demo',status=201,headers={'aaa':'aaaa'})

# 6 总结 Response
data=None # 响应体:可以传 字典，列表或字符串
status=None # http响应 状态码 -> drf封装了所有常量   HTTP_200_OK = 200        
headers=None # 响应头   

exception=False # 异常
content_type=None # 响应编码格式，一般不动，都用json
template_name=None # 忽略掉 -> 指定使用浏览器访问返回的模板，没写用默认模板
```



# 2 序列化类介绍

```python
# 序列化类涉及:
	-序列化
    -反序列化
    -数据校验
    
# 使用步骤
	-1 写个类，继承某个父类
    -2 类实例化得到对象后，进行序列化 -> 序列化
    -3 通过实例化得到的对象，可以进行数据校验 -> 校验
    -4 通过实例化得到的对象，可以保存或修改mysql中的数据 -> 反序列化
```



# 3 序列化类快速使用

## 3.0 快速使用

```python
#### models.py
from django.db import models

class Student(models.Model):
    age = models.IntegerField()
    name = models.CharField(max_length=32)
    school = models.CharField(max_length=32)
    
    
#### serializer.py
from rest_framework import serializers

class StudentSerializer(serializers.Serializer):
    # 写需要序列化的字段 一一对应
    # 不需要在页面里展示的 直接不写就可以了
    id = serializers.IntegerField()
    age = serializers.IntegerField()
    name = serializers.CharField()
    school = serializers.CharField()
    
    
#### views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from .models import Student
from .serializer import StudentSerializer
class StudentView(APIView):
    # 查询所有
    def get(self, request):
        # 1.取出所有的数据
        students = Student.objects.all()
        # 2.进行序列化处理 -> drf 提供的序列化类来完成这件事
        # 如果是多条 一定要传many=True
        # 多条不是指 数据库里到底是几条 就算只有一条数据 all也形式不符合要求
        # 只有.first() 才能拿出来一条数据
        serializer = StudentSerializer(students, many=True)
        # 3.返回给前端
        return Response(serializer.data)

class StudentDetailView(APIView):
    def get(self, request, pk):
        # 1.根据pk取出数据
        student = Student.objects.filter(pk=pk).first()
        # 一定要first才是单条
        # 2.进行序列化处理
        # many默认=False
        serializer = StudentSerializer(student)
        # 3.返回给前端
        return Response(serializer.data)
    
    
#### urls.py
from django.urls import path
from .views import DemoView, StudentView, StudentDetailView

urlpatterns = [
    path('demo/', DemoView.as_view()),
    path('students/', StudentView.as_view()),
    path('students/<int:pk>/', StudentDetailView.as_view()),
]

# 通过以上的案例 完成了查询1个和查询所有两个接口。
```



## 3.1 常用字段类

```python
# 1 记住 -> 跟models中的 model.字段类， 一一对应

# 2 具体有哪些--了解
CharField *
IntegerField *
DateTimeField *
DateField *
TimeField *

BooleanField
NullBooleanField	
EmailField	
RegexField	
SlugField	
URLField	
UUIDField	
IPAddressField

FloatField
DecimalField	

ChoiceField	
MultipleChoiceField	
FileField	
ImageField	

# --------两个特殊的，后面讲，很重要--------
ListField
DictField	


# 3 大绝招:如果不知道models和Serializer如何对应
	序列化类统一用 CharField 
```



## 3.2 常用字段参数

```python
# 1 序列化的 字段类在实例化时，传参数
	-作用是:为了反序列化校验用
    
# 2 有哪些字段参数 -> 了解
	# 只针对于:CharField
	max_length	最大长度
    min_lenght	最小长度
    allow_blank	是否允许为空
    trim_whitespace	是否截断空白字符
    
    # 只针对于:IntegerField
    max_value	最小值
    min_value	最大值
    
    # 只针对于:DateTimeField
    # format 比较特殊 这是序列化而不是反序列化
    format:序列化时，显示的格式
    DateTimeField(format='Y年m月d日')
    
    # 所有字段类都用
    required: 是否必填	
    default: 反序列化时使用的默认值
    allow_null: 表明该字段是否允许传入None，默认False
    -注意allow_null和allow_blank的区别 一个是None 一个是""
    
# 3 两个重点-先不讲
    read_only	表明该字段仅用于序列化输出，默认False
    write_only	表明该字段仅用于反序列化输入，默认False
```



# 4 反序列化保存

## 4.1 新增

```python
#### views.py
from .models import Student
from .serializer import StudentSerializer


class StudentView(APIView):
    # 新增一条 http://localhost:8000/api/v1/app01/students/post/
    def post(self, request):
        # 1.取出前端传入的请求体中的数据 (可能是json,urlencoded,form-data)
        # request.data
        # 2.序列化类进行实例化 得到对象->反序列化保存,传入data->就是前端传入的数据
        serializer = StudentSerializer(data=request.data)
        # 3.数据校验
        if serializer.is_valid():
            # 4.校验通过,保存,返回信息
            serializer.save()
            # 直接这样 它也不知道要保存到哪里呀 -> 要在序列化类中 重写create方法
            # save 如果是新增 会触发 serializer.create 方法的执行
            return Response(serializer.data)  # 返回给前端新增的对象
        else:
            # 5.校验不通过,返回错误信息
            return Response(serializer.errors)  # 返回给前端错误信息
        
        
#### serializer.py
from rest_framework import serializers
from .models import Student
# 既能序列化，又能反序列化
class StudentSerializer(serializers.Serializer):
    # 写需要序列化的字段 一一对应
    # 不需要在页面里展示的 直接不写就可以了
    # id = serializers.IntegerField()
    age = serializers.IntegerField()
    name = serializers.CharField()
    school = serializers.CharField()

    def create(self, validated_data):
        # validated_data: 前端传入的校验过后的数据
        # 保存到指定的表中
        student = Student.objects.create(**validated_data)
        # 返回新增对象
        return student

#### 关于新增 没有写页面上的什么form拿数据 要怎么进行新增呢?
# 我们在views的StudentView里写明了用post去新增
# 所以去postman工具里 向http://localhost:8000/api/v1/app01/students/ 发送一个post请求就可以
# 在body的raw里写
# {"name":"teststupost", "age":30, "school":"ohh"}
# 其他的内容删掉
# 后端就可以拿到并保存了!
```



## 4.2 修改

```python
#### serializer.py

from rest_framework import serializers
from .models import Student
# 既能序列化，又能反序列化
class StudentSerializer(serializers.Serializer):
    # 写需要序列化的字段 一一对应
    # 不需要在页面里展示的 直接不写就可以了
    # id = serializers.IntegerField()
    age = serializers.IntegerField()
    name = serializers.CharField()
    school = serializers.CharField()

    def create(self, validated_data):
        # validated_data: 前端传入的校验过后的数据
        # 保存到指定的表中
        student = Student.objects.create(**validated_data)
        # 返回新增对象
        return student

    def update(self, instance, validated_data):
        # instance是待修改对象 validated_data是校验过后的数据
        # 笨办法
        # instance.age = validated_data.get('age')
        # instance.name = validated_data.get('name')
        # instance.school = validated_data.get('school')
        # 有很多个就很笨啊

        # 聪明的办法
        for key in validated_data:
            # key
            setattr(instance, key, validated_data.get(key))
            # setattr(instance, "name", "sheenagh")
            # 一劳永逸 无需改动
        # 保存到数据库
        instance.save()
        return instance
    

#### views.py
class StudentDetailView(APIView):
    # 修改一条 http://localhost:8000/api/v1/app01/students/1/
    def put(self, request, pk):
        # 1.根据pk取出数据
        student = Student.objects.filter(pk=pk).first()
        # 一定要first才是单条
        # 2.实例化得到序列化对象
        serializer = StudentSerializer(instance=student,data=request.data)
        # 会根据data的数据 更新instance
        # 3.数据校验
        if serializer.is_valid():
            # 4.校验通过,保存,返回信息
            serializer.save()
            # 和上面新增一条不一样 这里应该通过 save 触发 update 方法执行
            # 源码中 save:instance不是None 就update 是None 就create 写好了
            return Response(serializer.data)  # 返回给前端更改的对象
        else:
            # 5.校验不通过,返回错误信息
            return Response(serializer.errors)  # 返回给前端错误信息

#### 类似新增单条数据 写一下怎么用它
# 我们在views的StudentDetailView里写明了用put去修改
# 所以去postman工具里 向 http://localhost:8000/api/v1/app01/students/3/ 发送一个put请求就可以
# 在body的raw里写
# {"name":"teststuput", "age":90, "school":"ohh"}
# 后端就可以拿到并修改了!
# 检查return回来的数据和库里面 发现成功啦
```



# 作业

```python
# 1-2-0-3-4
# 0 补充:√
	-用postman+django -> 使用不同编码格式和不同的请求方式 -> 分别验证:requset.GET,request.POST,request.body
    -学的好的:用requests模块
    
# 1 讲的request，response整理一下√
# 2 把学生的5个接口写完，使用序列化类√
	-删除
    -思考
        -首先删除也是删除一个
        -所以选择写在StudentDetailView里
        -和put get的区别是 好像没有request新东西需要输入进来 只要发送一个delete请求就好了
        -感觉就是怎样去触发它的delete
        -但是 触发 是因为 前面那些接口 都要通过序列化
        -可是delete 它不涉及序列化 也不涉及反序列化 所以它不需要去serializer
        
    # 删除一条 http://localhost:8000/api/v1/app01/students/1/
    def delete(self, request, pk):
        # 1.根据pk取出数据
        student = Student.objects.filter(pk=pk).first()
        # 一定要first才是单条
        # 2.如果找不到 返回错误信息
        if not student:
            return Response({"message": "Student deleted failed"})
        # 3.如果找得到 返回成功信息
        student.delete()
        return Response({"message": "Student deleted successfully"})
    
    	-测试 删除/4/ 不存在的 和删除 /2/ 都跑得通 可以√
        
    
# 3 编写一个装饰器，装在fbv上，让原生django的request对象，具备跟drf的request一样的data功能
# √用chatgpt调教完成 不知道是否符合需要的效果

--------
# 4 研究一下:QueryDict
	-有哪些方法，有什么作用？
```





