# 1 Ajax介绍

````python
# 【一】Django如何返回JSON格式的数据
# 1.为什么要返回JSON格式的数据
# JSON数据格式是前后端传输数据通用的数据格式

# 前端 数组array [] 对象object {} -> 前端认识 后端不认识
# 后端 列表 [] 字典 {} -> 后端认识 但前端不认识

# JSON.parse / JSON.stringify         json.loads /json.dumps
# 前端 数组array [] -> 中间层(JSON) -> 后端 列表 []

# 2.Django如何返回JSON格式数据
# (1)方式一
# HttpResponse({},content_type="application/json")
# (2)方式二
# JSONResponse()


# 【二】Ajax
# 通过 form 表单传递数据的时候传递数据的方式不可控
# 只能通过 input 标签上的 name 传递数据
# 在传输之前想对数据进行二次校验 拦截不到做不了
# 于是就有了这样一种技术
# 先获取输入的内容
# 对内容进行处理
# 再将处理后的数据发送给后端(Ajax 如何在前端主动向后端发起请求并处理)

# 1.Ajax介绍事务是指一系列相关操作的集合，这些操作被视为一个不可分割的工作单元。

# ACID 原子性、一致性、隔离性、持久性
# ● AJAX（Asynchronous Javascript And XML）翻译成中文就是“异步的Javascript和XML”。
#   ○ 即使用Javascript语言与服务器进行异步交互，传输的数据为XML（当然，传输的数据不只是XML）。
# ● AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
# ● AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。（这一特点给用户的感受是在不知不觉中完成请求和响应过程）
# ● AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。
#   ○ 同步交互：
#     ■ 客户端发出一个请求后，需要等待服务器响应结束后，才能发出第二个请求；
#   ○ 异步交互：
#     ■ 客户端发出一个请求后，无需等待服务器响应结束，就可以发出第二个请求。

# 2.复习一下常用的提交请求方式
# 对于浏览器来说: 直接在地址栏回车 - GET
# 对于a标签来说: 在 href 属性上定义请求地址 - GET
# 对于 form 标签来说: 在 method 属性上定义请求方式 - GET / POST
# 对于 Ajax 来说: - GET / POST
````



# 2 Ajax基础使用

## 2.1 基于Ajax的用户注册

````python
class RegisterView(View):
    def get(self, request, *args, **kwargs):
        return render(request, "register.html", locals())

    def post(self, request, *args, **kwargs):
        print(request.POST)
        # 无法返回重定向的页面或返回渲染的页面
        # return redirect(reverse("index"))
        # return HttpResponse("ok")
        # HttpResponse 返回json数据的时候一定要先将字典序列化
        # return HttpResponse(json.dumps({"success": "True"}), content_type="application/json")
        return JsonResponse({"success": True})
````

````html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="{% static 'js/jquery.min.js' %}"></script>
</head>
<body>
<h1>
    注册页面
</h1>
<form>
    <div>
        username <input type="text" id="username">
    </div>
    <div>
        password <input type="text" id="password">
    </div>
    <div>
        <input type="button" id="btn_submit" value="提交数据"></input>
    </div>
</form>
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
</body>
</html>
````

## 2.2 基于Ajax的异步交互

````python
class AddNumberView(View):
    def get(self, request, *args, **kwargs):
        return render(request, "add.html", locals())

    def post(self, request, *args, **kwargs):
        numberOne = request.POST.get("numberOne")
        numberTwo = request.POST.get("numberTwo")
        method = request.POST.get("method")
        if not numberOne.isdigit() or not numberTwo.isdigit() or not method:
            return JsonResponse({"success": False, "error": "非数字类型或非法运算"})
        if method == "+":
            return JsonResponse({"success": True, "result": int(numberOne) + int(numberTwo)})
````

```html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="{% static 'js/jquery.min.js' %}"></script>
    <script src="{% static 'plugins/bootstrap/js/bootstrap.min.js' %}"></script>
    <link rel="stylesheet" href="{% static 'plugins/bootstrap/css/bootstrap.min.css' %}">
</head>
<body>
<form>
    <p>
        number_one :>>>>
        <input type="text" id="number_one">
    </p>
    <p>
        number_two :>>>>
        <input type="text" id="number_two">
    </p>

    <div class="radio">
        <label>
            <input type="radio" name="optionsRadios" id="method_1" value="+">➕
        </label>
        <label>
            <input type="radio" name="optionsRadios" id="method_2" value="-"> ➖
        </label>
        <label>
            <input type="radio" name="optionsRadios" id="method_3" value="*">✖️
        </label>
        <label>
            <input type="radio" name="optionsRadios" id="method_4" value="/"> ➗
        </label>
    </div>

    </div>
    <p>
        result : >>>> <input type="text" id="result">
    </p>
    <p>
        <input type="button" id="btn_submit" value="计算">
    </p>
</form>

<script>
    $(document).ready(
        $("#btn_submit").click(function () {
            // 获取到输入的值
            let numberOne = $("#number_one").val()
            let numberTwo = $("#number_two").val()
            let method = $("input:radio:checked").val()
            // 向后端提交数据
            $.ajax({
                // 目标地址
                url: "",
                // 请求方式
                type: "post",
                // 数据
                data: {
                    numberOne: numberOne,
                    numberTwo: numberTwo,
                    method: method ? method : ""
                },
                // 响应的数据处理方法
                success: function (data) {
                    if (data.success) {
                        $("#result").val(data.result)
                    } else {
                        alert(data.error)
                    }
                }
            })
        })
    )
</script>


</body>
</html>
```



# 3 Ajax进阶使用

- 基于Ajax传输文件数据

````python
class RegisterView(View):
    def get(self, request, *args, **kwargs):
        return render(request, "register.html", locals())

    def post(self, request, *args, **kwargs):
        print(request.method)
        # 用了 Ajax 之后 又多了一个方法 可以用来判断当前提交的请求是否是 Ajax 请求
        print(request.is_ajax())  # Ajax --> True / form -- > False

        # 通过 Ajax  的 FormData 对象传入的键值对数据 存储在 POST 里面
        print(request.POST)
        # 文件数据依旧按照form表单提交的文件数据处理方式处理
        print(request.FILES)
        # return redirect(reverse("index"))
        # return HttpResponse("ok")
        # HttpResponse 返回json数据的时候一定要先将字典序列化
        # return HttpResponse(json.dumps({"success": "True"}), content_type="application/json")
        return JsonResponse({"success": True})
````

````html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="{% static 'js/jquery.min.js' %}"></script>
</head>
<body>
<h1>
    注册页面
</h1>
<form>
    <div>
        username <input type="text" id="username">
    </div>
    <div>
        password <input type="text" id="password">
    </div>
    <div>
        <input type="file" id="avatar">
    </div>
    <div>
        <input type="button" id="btn_submit" value="提交数据"></input>
    </div>
</form>
<script>
    // Ajax 是 js 代码封装的
    $(document).ready(
        //
        $("#btn_submit").click(function () {
            // 获取到用户输入的用户名和密码
            let username = $("#username").val()
            let password = $("#password").val()
            {#let avatar = $("#avatar").val() // C:\fakepath\梦梦卡商.png#}

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
</body>
</html>
````



# 4 Ajax模版总结

## 4.1 传输普通键值对

```javascript
<script>
    // 先给按钮绑定点击事件
    $('#btn').click(function () {
        // 向后端发送Ajax请求
        $.ajax({
            // 1.指定发送后端的请求接口
            url: '',// 不写就是朝当前地址发送请求
            
            // 2.请求方式
            type: "post", // 不指定默认就是 get 全小写
          
            // dataType 参数应该是一个字符串，表示期望从服务器返回的数据类型（如 "json", "xml", "script", "html" 等）
            // dataType: "json",
          
            // 3.提交数据
            data: {'i1': $('#d1').val(), 'i2': $('#d2').val()},
            
            // 4.异步提交有一个回调函数 （异步回调机制）
            // 当后端返回结果的时候会自动触发，args 会自动接受后端传过来的结果
            success: function (args) {
                {#alert(args)#}
                // 通过DOM操作动态数据渲染到第三个 input 框中
                console.log(args) // string
                $('#d3').val(args)
            },
        })
    })
</script>
```

## 4.2 传输文件数据

`````javascript
// 点击按钮向后端发送普通键值对数据和文件数据
$("#btn").on('click', function () {
    // 1.先生成一个内置对象
    let formDataObj = new FormData();

    // 2.支持添加普通的键值对
    formDataObj.append('username', $("#d1").val());
    formDataObj.append('password', $("#d2").val());

    // 3.支持添加文件对象 ---> 先拿到标签对象 ----> 再拿到文件对象
    formDataObj.append('myfile', $("#d3")[0].files[0]);

    // 4.基于Ajax，将文件对象发送给后端
    $.ajax({
    url: '',
    type: 'post',
    // 直接将对象放到data里面即可
    data: formDataObj,

    // Ajax发送文件必须添加的两个参数
    // 不需要使用任何编码 -  Django后端能自动识别 formdata 对象
    contentType: false,
    // 告诉浏览器不要对我的数据进行任何处理
    processData: false,

    success: function (args) {

    }
})
`````



# 5 二次弹框补充

````python
class AlertView(View):
    def get(self, request, *args, **kwargs):
        return render(request, "alert.html", locals())

    def post(self, request, *args, **kwargs):
        ...
````

````html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="{% static 'js/jquery.min.js' %}"></script>
    <script src="{% static 'plugins/bootstrap/js/bootstrap.min.js' %}"></script>
    <script src="{% static 'js/sweetalert.min.js' %}"></script>
    <link rel="stylesheet" href="{% static 'plugins/bootstrap/css/bootstrap.min.css' %}">
</head>
<body>
<table class="table">
    <caption>Optional table caption.</caption>
    <thead>
    <tr>
        <th>#</th>
        <th>书名</th>
        <th>价格</th>
        <th>操作</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th scope="row">1</th>
        <td>西游记</td>
        <td>999$</td>
        <td>
            <button class="btn btn-xs btn-success">编辑</button>
            <button class="btn btn-xs btn-danger" value="1" id="btn_del">删除</button>
        </td>
    </tr>
    </tbody>
</table>

<script>
    // 获取到当前 删除 按钮绑定点击事件
    $("#btn_del").on("click", function () {
        let bookId = $(this).attr("value");
        swal("你确定删除这个书吗？", {
            buttons: {
                cancel: "取消删除",
                catch: "确认删除!",
            },
        })
            .then((value) => {
                console.log(value)
            })
    })
    /*
     $("#btn_del").on("click", function () {
         let bookId = $(this).attr("value");
         swal("你确定删除这个书吗？", {
             buttons: {
                 cancel: "取消删除",
                 catch: {
                     text: "确认删除!",
                     value: "catch",
                 },
                 defeat: true,
             },
         })
             .then((value) => {
                 switch (value) {
                     case "defeat":
                         swal("请重新选择!");
                         break;

                     case "catch":
                         $.ajax({
                             url: "/user/del/" + bookId + '/',
                             type: "get",
                             data: {},
                             success: function (data) {
                                 console.log(data)
                                 if (data.code === 200) {
                                     // ok 的图标和文本
                                     swal("删除成功!", `已删除id为 ${bookId}!`, "success");
                                 } else {
                                     swal({
                                         text: `删除失败!原因是 :>>> ${data.error}!`,
                                         icon: "error",
                                     });
                                 }

                             }
                         })

                         break;

                     default:
                         swal("你是个慎重的人!");
                 }
             });
     })
    * */
    /*
     $("#btn_del").on("click", function () {
         let tag = window.confirm("你确定删除当前的书吗?")
         if (tag) {
             var bookId = $(this).attr("value");
             $.ajax({
                 url: "/user/del/" + bookId + '/',
                 type: "get",
                 data: {},
                 success: function (data) {
                     console.log(data)
                 }
             })
         } else {
             window.location.reload()
         }
     })
    * */
</script>
</body>


</html>
````

```python
# 【二】SweetAlert
# https://sweetalert.js.org/
# 【三】layer
# https://layui.dev/
```



# 6 批量插入

- create 插入慢

`````python
from user.models import Book


def insert_book_one(request):
    start_time = time.time()
    for i in range(0, 1000000):
        Book.objects.create(
            name=f"水浒传_{i}部",
            price=9.9 * i
        )
    book_all = Book.objects.all()
    print(f"总耗时 :>>> {time.time() - start_time} s")
    return render(request, "book_index.html", locals())
`````

- bulk_create插入快

````python
# Django 提供给我们一个更加快捷插入大量数据的方式
def insert_book_two(request):
    start_time = time.time()
    book_obj_list = []
    for i in range(0, 1000000):
        book_obj = Book(
            name=f"水浒传_{i}部",
            price=9.9 * i
        )
        book_obj_list.append(book_obj)
    book_all = Book.objects.bulk_create(book_obj_list)
    print(f"总耗时 :>>> {time.time() - start_time} s")
    return render(request, "book_index.html", locals())
````



# 7 Django自带的序列化

````python
def book_index(request):
    book_all = Book.objects.all()  # 拿到的事 queryset 对象 [{},{}]
    info_list = []
    for book_obj in book_all:
        info_list.append({
            "name": book_obj.name,
            "price": book_obj.price
        })
    # 因为传入给前端的是列表 所以为了 让前端可以渲染列表 就必须
    # In order to allow non-dict objects to be serialized set the safe parameter to False.
    return JsonResponse(info_list, safe=False)
````

````python
from django.core import serializers


def book_index_one(request):
    book_all = Book.objects.all()  # 拿到的事 queryset 对象 [{},{}]
    result = serializers.serialize("json", book_all)
    return HttpResponse(result)
````



# 8 分页器

## 8.1 推导

````python
# 【一】数据库中有 100w 本书
# 渲染到前端 一次性渲染？
# 博客园做分页处理

# 【二】分页思路
# 101条数据 1页 10条 分多少页？ 11 页
# 100条数据 1页 10条 分多少页？ 10 页

every_page = 10
start_page = 0
end_page = start_page + every_page
'''
 页数   起始索引 结束索引 每一页数据
  1    0       9      10
  2   10       19      10
  3   20       29      10
start_page= (page-1) * every_page


every_page 每一页数据
start_page 起始索引
end_page 结束索引

第几页    起始索引         结束索引     每一页数据
page    start_page       end_page     every_page
  
start_page= (page-1) * every_page
end_page = page * every_page

# 切片顾头不顾尾
queryset = [{},{},{}]
queryset[start_index:end_index]
queryset[(page-1) * every_page:page * every_page]
queryset[0:10] ---》 【0 - 9】
queryset[10:20] ---》 【10 - 19】
queryset[20:30] ---》 【20 - 29】
'''
````

## 8.2 自定义分页器

````python
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
````

````html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="{% static 'js/jquery.min.js' %}"></script>
    <script src="{% static 'plugins/bootstrap/js/bootstrap.min.js' %}"></script>
    <script src="{% static 'js/sweetalert.min.js' %}"></script>
    <link rel="stylesheet" href="{% static 'plugins/bootstrap/css/bootstrap.min.css' %}">
</head>
<body>
<div>
    <nav aria-label="...">
        <ul class="pagination">
            <li class="disabled"><a href="#" aria-label="Previous"><span aria-hidden="true">«</span></a></li>
            {% if current_page <= 3 %}
                {% if current_page == 1 %}
                    <li class="active"><a href="{% url 'book_split_page' %}?page=1">1</a>
                    </li>
                    <li><a class="active" href="{% url 'book_split_page' %}?page=2">2</a>
                    </li>
                {% elif  current_page == 2 %}
                    <li><a href="{% url 'book_split_page' %}?page=1">1</a>
                    </li>
                    <li class="active"><a class="active" href="{% url 'book_split_page' %}?page=2">2</a>
                    </li>
                    <li><a href="{% url 'book_split_page' %}?page=3">3</a>
                        {% elif  current_page == 3 %}
                    <li><a href="{% url 'book_split_page' %}?page=1">1</a>
                    </li>
                    <li><a class="active" href="{% url 'book_split_page' %}?page=2">2</a>
                    </li>
                    <li class="active"><a href="{% url 'book_split_page' %}?page=3">3</a>
                {% endif %}
            </li>
                <li><a href="{% url 'book_split_page' %}?page=4">4</a>
                </li>
                <li><a href="{% url 'book_split_page' %}?page=5">5</a>
                </li>
                </li>
            {% else %}
                <li><a href="{% url 'book_split_page' %}?page={{ current_page|add:-2 }}">{{ current_page|add:-2 }}</a>
                </li>
                <li><a href="{% url 'book_split_page' %}?page={{ current_page|add:-1 }}">{{ current_page|add:-1 }}</a>
                </li>
                <li class="active"><a href="#">{{ current_page }} <span class="sr-only">(current)</span></a></li>
                <li><a href="{% url 'book_split_page' %}?page={{ current_page|add:1 }}">{{ current_page|add:1 }}</a>
                </li>
                <li><a href="{% url 'book_split_page' %}?page={{ current_page|add:2 }}">{{ current_page|add:2 }}</a>
                </li>
            {% endif %}
            <li><a href="{% url 'book_split_page' %}?page={{ current_page|add:1 }}" aria-label="Next"><span
                    aria-hidden="true">»</span></a></li>
        </ul>
    </nav>
</div>
<div>
    {% for book_obj in query_set %}
        <p>书名 {{ book_obj.name }} | 价格 {{ book_obj.price }}</p>
    {% endfor %}

</div>

</body>
</html>
````

## 8.3 分页器模版

````python
class Pagination(object):
    def __init__(self, current_page, all_count, per_page_num=2, pager_count=11):
        """
        封装分页相关数据
        :param current_page: 当前页
        :param all_count:    数据库中的数据总条数
        :param per_page_num: 每页显示的数据条数
        :param pager_count:  最多显示的页码个数
        """
        try:
            current_page = int(current_page)
        except Exception as e:
            current_page = 1

        if current_page < 1:
            current_page = 1

        self.current_page = current_page

        self.all_count = all_count
        self.per_page_num = per_page_num

        # 总页码
        all_pager, tmp = divmod(all_count, per_page_num)
        if tmp:
            all_pager += 1
        self.all_pager = all_pager

        self.pager_count = pager_count
        self.pager_count_half = int((pager_count - 1) / 2)

    @property
    def start(self):
        return (self.current_page - 1) * self.per_page_num

    @property
    def end(self):
        return self.current_page * self.per_page_num

    def page_html(self):
        # 如果总页码 < 11个：
        if self.all_pager <= self.pager_count:
            pager_start = 1
            pager_end = self.all_pager + 1
        # 总页码  > 11
        else:
            # 当前页如果<=页面上最多显示11/2个页码
            if self.current_page <= self.pager_count_half:
                pager_start = 1
                pager_end = self.pager_count + 1

            # 当前页大于5
            else:
                # 页码翻到最后
                if (self.current_page + self.pager_count_half) > self.all_pager:
                    pager_end = self.all_pager + 1
                    pager_start = self.all_pager - self.pager_count + 1
                else:
                    pager_start = self.current_page - self.pager_count_half
                    pager_end = self.current_page + self.pager_count_half + 1

        page_html_list = []
        # 添加前面的nav和ul标签
        page_html_list.append('''
                    <nav aria-label='Page navigation>'
                    <ul class='pagination'>
                ''')
        first_page = '<li><a href="?page=%s">首页</a></li>' % (1)
        page_html_list.append(first_page)

        if self.current_page <= 1:
            prev_page = '<li class="disabled"><a href="#">上一页</a></li>'
        else:
            prev_page = '<li><a href="?page=%s">上一页</a></li>' % (self.current_page - 1,)

        page_html_list.append(prev_page)

        for i in range(pager_start, pager_end):
            if i == self.current_page:
                temp = '<li class="active"><a href="?page=%s">%s</a></li>' % (i, i,)
            else:
                temp = '<li><a href="?page=%s">%s</a></li>' % (i, i,)
            page_html_list.append(temp)

        if self.current_page >= self.all_pager:
            next_page = '<li class="disabled"><a href="#">下一页</a></li>'
        else:
            next_page = '<li><a href="?page=%s">下一页</a></li>' % (self.current_page + 1,)
        page_html_list.append(next_page)

        last_page = '<li><a href="?page=%s">尾页</a></li>' % (self.all_pager,)
        page_html_list.append(last_page)
        # 尾部添加标签
        page_html_list.append('''
                                           </nav>
                                           </ul>
                                       ''')
        return ''.join(page_html_list)
````

`````python
def book_split_page(request):
    # 当前页 current_page 从当前的 请求地址中获取
    # 起始页(起始索引) start_page
    # 结束页(结束索引) end_page
    # 每一页的条数 per_page_num

    # 【一】查询所有数据
    book_all = Book.objects.all()
    # 【二】获取当前页码
    current_page = request.GET.get("page")
    # 【三】交给分页器类分页
    page_obj = Pagination(current_page=current_page,
                          all_count=book_all.count(),
                          per_page_num=1000)
    # 【四】调用分页器对象分页数据
    query_set = book_all[page_obj.start:page_obj.end]

    return render(request, "book_page.html", locals())
`````

````html
<div class="container">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            {% for book in page_queryset %}
            <p>{{ book.title }}</p>
            {% endfor %}
            {{ page_obj.page_html|safe }}
        </div>
    </div>
</div>
````

