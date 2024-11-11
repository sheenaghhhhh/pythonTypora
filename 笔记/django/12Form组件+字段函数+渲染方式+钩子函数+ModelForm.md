# 1 引入

````python
# 【一】我们在前端写 form 表单 都是直接写
# 于是Django是一个大而全的框架。，为了能够契合前端的 form 表单
# 于是自己内置了一个from组件的功能
# 【二】在前端写一个 form 表单
# 用户名
# 密码
# 提交按钮
````

```html
<form>
  <div class="form-group">
    <label for="inputUsername">Username</label>
    <input type="email" class="form-control" id="inputUsername" placeholder="Email">
  </div>
  <div class="form-group">
    <label for="inputPassword">Password</label>
    <input type="password" class="form-control" id="inputPassword" placeholder="Password">
  </div>
  <button type="submit" class="btn btn-default">Submit</button>
</form>
```



# 2 form组件初步使用

> 用Django的form组件来写一个前端form表单

1. 在任意位置创建一个组件文件 建议你那个app下的就创建在那个 app 下
2. 在组件文件中写组件类

```python
# 第一步 导入 form组件
from django import forms


# 第二步 创建一个组件类
class RegisterForm(forms.Form):
    # 定义我们想要创建的 form 表单的 每一个标签
    # 用户名的标签
    # 区分 models.CharField 里面的max_length
    #  models.CharField 里面的max_length 含义是当前 varchar 类型的最大长度
    #  forms.CharField 里面的max_length 含义是 在前端输入的标签中的最大长度
    # <input type="text" class="form-control" maxlength="5" minlength="3" id="inputUsername" placeholder="Email">
    username = forms.CharField(max_length=8, min_length=3)
    # 密码的标签
    password = forms.CharField(max_length=6, min_length=2)
    # 邮箱的标签
    email = forms.EmailField()
```

3. 使用组件类

```python
# 在视图上先创建一个组件类 的对象
def get(self, request, *args, **kwargs):
    # 第一步导入自己创建的组件类 RegisterForm
    # 第二步创建一个 form 组件的对象
    form_obj = RegisterForm()
    return render(request, "register.html", locals())
```

4. 一定要记得在前端的form对象中加 from 标签个提交按钮

````html
# 点击提交就会提交数据给后端
<form action="" method="post">
    {{ form_obj }}
    <p>
        <input type="submit">
    </p>
</form>
````



# 3 常见的form组件字段参数

````python
from django.core.validators import RegexValidator


# 创建组件并添加额外的参数
class RegisterForm(forms.Form):
    username = forms.CharField(
        # 当前输入的最大字符长度
        max_length=8,
        # 当前输入的最小长度
        min_length=3,
        # 当前参数是否是必须输入的参数 默认是必须传的参数
        required=True,
        # 为 当前的标签设置初始值
        initial="dream",
        # 给当前 label 标签中的提示文本修改
        # 如果改了就是自定义的值 没改就是当前的字段名
        label="用户名",
        # 给当前的 label 标签提示文字后面的标识 默认是 :
        label_suffix=":>>>>",
        # 给当前字段添加的额外的组件
        # 比如增加的 class 值 style  值
        # 向让当前输入框是一个 密码输入框 / 单选框  / 多选框
        widget="",
        # 给当前标签的注释文本
        help_text="这是用户名的输入框",
        # 如果当前字段 中的校验出校错误 可以定制错误的信息
        error_messages={
            "required": "用户名必须传!",
            "max_length": "当前用户名的长度最大是 8 位!"
        },
        # 是否使用当前本地化 在美国用 en-us 中国 zn-hans
        localize=True,
        # 在前端是否禁用当前标签的输入
        disabled=True
    )
    # 密码的标签 -- PasswordInput
    password = forms.CharField(max_length=6, min_length=2,
                               # 当前组件类型是 password的输入框
                               # 给当前标签增加额外的属性名和属性值
                               widget=forms.PasswordInput(
                                   attrs={
                                       "class": "form-control"
                                   }
                               ), )

    # radio 单选框
    gender = forms.ChoiceField(
        # 当前单选框的备选项
        choices=((0, "女"), (1, "男")),
        # 当前提示内容
        label="性别",
        # 当前的默认值
        initial=0,
        # # 当前组件类型是 RadioSelect 单选框
        widget=forms.RadioSelect()
    )

    # select 下拉单选框
    # SelectMultiple 下拉多选框
    # Select 下拉单选框
    hobby_ball = forms.MultipleChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"),),
        label="爱好",
        initial=[1, 3],
        # widget=forms.widgets.SelectMultiple()
        widget=forms.widgets.Select()
    )

    # 多选矿
    keep = forms.ChoiceField(
        label="是否记住密码",
        # 默认勾选记住密码
        initial="checked",
        widget=forms.widgets.CheckboxInput()
    )

    hobby = forms.MultipleChoiceField(
        choices=((1, "唱"), (2, "跳"), (3, "rap"),),
        label="爱好",
        initial=[1, 3],
        widget=forms.widgets.CheckboxSelectMultiple()
    )

    # 某一天你觉的上面的校验规则不好
    # 校验当前的电话是 符合 11 位 并且是符合国内的
    # 自定义校验器
    # 前端没有变化 ====> 校验器主要体现在 is_valid 触发校验 按照正则 匹配 如果符合 就通过 不符合就给提示信息
    phone = forms.CharField(
        validators=[RegexValidator(
            r'^1\d{10}$|^(0\d{2,3}-?|\(0\d{2,3}\))?[1-9]\d{4,7}(-\d{1,8})?$', '请输入电话'
        )])

    # 邮箱的标签
    email = forms.EmailField()
````

````python
Field
    required=True,               是否允许为空
    widget=None,                 HTML插件
    label=None,                  用于生成Label标签或显示内容
    initial=None,                初始值
    help_text='',                帮助信息(在标签旁边显示)
    error_messages=None,         错误信息 {'required': '不能为空', 'invalid': '格式错误'}
    validators=[],               自定义验证规则
    localize=False,              是否支持本地化
    disabled=False,              是否可以编辑
    label_suffix=None            Label内容后缀
 
 
CharField(Field)
    max_length=None,             最大长度
    min_length=None,             最小长度
    strip=True                   是否移除用户输入空白
 
IntegerField(Field)
    max_value=None,              最大值
    min_value=None,              最小值
 
FloatField(IntegerField)
    ...
 
DecimalField(IntegerField)
    max_value=None,              最大值
    min_value=None,              最小值
    max_digits=None,             总长度
    decimal_places=None,         小数位长度
 
BaseTemporalField(Field)
    input_formats=None          时间格式化   
 
DateField(BaseTemporalField)    格式：2015-09-01
TimeField(BaseTemporalField)    格式：11:12
DateTimeField(BaseTemporalField)格式：2015-09-01 11:12
 
DurationField(Field)            时间间隔：%d %H:%M:%S.%f
    ...
 
RegexField(CharField)
    regex,                      自定制正则表达式
    max_length=None,            最大长度
    min_length=None,            最小长度
    error_message=None,         忽略，错误信息使用 error_messages={'invalid': '...'}
 
EmailField(CharField)      
    ...
 
FileField(Field)
    allow_empty_file=False     是否允许空文件
 
ImageField(FileField)      
    ...
    注：需要PIL模块，pip3 install Pillow
    以上两个字典使用时，需要注意两点：
        - form表单中 enctype="multipart/form-data"
        - view函数中 obj = MyForm(request.POST, request.FILES)
 
URLField(Field)
    ...
 
 
BooleanField(Field)  
    ...
 
NullBooleanField(BooleanField)
    ...
 
ChoiceField(Field)
    ...
    choices=(),                选项，如：choices = ((0,'上海'),(1,'北京'),)
    required=True,             是否必填
    widget=None,               插件，默认select插件
    label=None,                Label内容
    initial=None,              初始值
    help_text='',              帮助提示
 
 
ModelChoiceField(ChoiceField)
    ...                        django.forms.models.ModelChoiceField
    queryset,                  # 查询数据库中的数据
    empty_label="---------",   # 默认空显示内容
    to_field_name=None,        # HTML中value的值对应的字段
    limit_choices_to=None      # ModelForm中对queryset二次筛选
     
ModelMultipleChoiceField(ModelChoiceField)
    ...                        django.forms.models.ModelMultipleChoiceField
 
 
     
TypedChoiceField(ChoiceField)
    coerce = lambda val: val   对选中的值进行一次转换
    empty_value= ''            空值的默认值
 
MultipleChoiceField(ChoiceField)
    ...
 
TypedMultipleChoiceField(MultipleChoiceField)
    coerce = lambda val: val   对选中的每一个值进行一次转换
    empty_value= ''            空值的默认值
 
ComboField(Field)
    fields=()                  使用多个验证，如下：即验证最大长度20，又验证邮箱格式
                               fields.ComboField(fields=[fields.CharField(max_length=20), fields.EmailField(),])
 
MultiValueField(Field)
    PS: 抽象类，子类中可以实现聚合多个字典去匹配一个值，要配合MultiWidget使用
 
SplitDateTimeField(MultiValueField)
    input_date_formats=None,   格式列表：['%Y--%m--%d', '%m%d/%Y', '%m/%d/%y']
    input_time_formats=None    格式列表：['%H:%M:%S', '%H:%M:%S.%f', '%H:%M']
 
FilePathField(ChoiceField)     文件选项，目录下文件显示在页面中
    path,                      文件夹路径
    match=None,                正则匹配
    recursive=False,           递归下面的文件夹
    allow_files=True,          允许文件
    allow_folders=False,       允许文件夹
    required=True,
    widget=None,
    label=None,
    initial=None,
    help_text=''
 
GenericIPAddressField
    protocol='both',           both,ipv4,ipv6支持的IP格式
    unpack_ipv4=False          解析ipv4地址，如果是::ffff:192.0.2.1时候，可解析为192.0.2.1， PS：protocol必须为both才能启用
 
SlugField(CharField)           数字，字母，下划线，减号（连字符）
    ...
 
UUIDField(CharField)           uuid类型
````



# 4 form组件的渲染方式

1. 原汁原味

````html
<form action="" method="post">
    {{ form_obj }}
    <p>
        <input type="submit">
    </p>
</form>
````

2. 渲染成 无序列表

```html
<form action="" method="post">
    {{ form_obj.as_ul }}
    <p>
        <input type="submit">
    </p>
</form>
```

3. 渲染成 p 标签

```html
<form action="" method="post">
    {{ form_obj.as_p }}
    <p>
        <input type="submit">
    </p>
</form>
```
4. 渲染成表格标签

````html
<form action="" method="post">
    <table> // table 标签包进去才会生效
        {{ form_obj.as_table }}
    </table>
    <p>
        <input type="submit">
    </p>
</form>
````

5. for循环遍历渲染标签

```html
{% for form in form_obj %}
{# 遍历出来的对象只是 input 标签  #}
<p>
    {# 给当前 input 标签创建的 label 标签 #}
    {{ form.label_tag }}
    <br>
    {# name 就是当前的 form 组件中的字段名 #}
    {{ form.name }}
    <br>
    {# 在 form 组件字段上定义的  label 内容 #}
    {{ form.label }}
    <br>
    {# 获取到当前的 form 对象校验后的错误信息 #}
    {{ form.errors }}
    <br>
    {# 当前 input 标签上的id值 #}
    {{ form.id_for_label }}
    <br>
    {# 当前 input 标签上的id值 #}
    {{ form.auto_id }}
    <br>
    {# 在 form 组件字段上 定义的 help_text 的值 #}
    {{ form.help_text }}
    <br>
    {{ form }}</p>
{% endfor %}
```

- 取消form’表单的强制校验

````html
<form action="" method="post" novalidate>
````



# 5 钩子函数

````python
# 【一】钩子函数介绍
# 在form组件中我们通过 is_valid() 触发及哦啊眼
# 在某些情况下我们想对某个字段进行单独的校验

# 【二】取用户名出来 校验是否以 nb_ 开头
# 用钩子函数 把用户名 勾出来 进行校验

# 【三】钩子函数氛围两种
# 全局钩子 把所有数据全部拿出来
# 局部钩子 只针对某个具体的数据

# 【四】常见的钩子函数
# 1.onInputChange
# ● 当输入框的值发生变化时触发。
# ● 你可以通过这个钩子函数获取最新的输入值，并进行相应的处理。
# 2.onSubmit
# ● 当表单提交时触发。你可以在这个钩子函数中获取表单中的所有字段值，并进行数据验证、提交或其他操作。
# 3.onBlur
# ● 当输入框失去焦点时触发。
# ● 你可以在这个钩子函数中执行验证操作
# ● 例如检查输入是否符合预期的格式或是否满足某些条件。
# 4.onFocus
# ● 当输入框获得焦点时触发。
# ● 你可以在这个钩子函数中执行一些针对输入框焦点状态的逻辑操作
# ● 例如显示一个下拉列表或提示信息。
# 5.onReset
# ● 当表单重置时触发。
# ● 你可以在这个钩子函数中对表单进行一些初始化操作
# ● 将表单恢复到初始状态。

# 6.全局钩子
# clean

# 7.局部钩子
# clean_字段名
````

- 自定义钩子函数

````python
class LoginFrom(forms.Form):
    username = forms.CharField(
        required=False,
        max_length=8,
        min_length=3,
        label="用户名",
        label_suffix=":>>>>",
        widget=forms.TextInput(attrs={
            "class": "from-control"
        })
    )

    password = forms.CharField(
        required=False,
        max_length=8,
        min_length=3,
        label="密码",
        label_suffix=":>>>>",
        widget=forms.PasswordInput(
            attrs={
                "class": "from-control"
            }
        )
    )

    confirm_password = forms.CharField(
        required=False,
        max_length=8,
        min_length=3,
        label="确认密码",
        label_suffix=":>>>>",
        widget=forms.PasswordInput(
            attrs={
                "class": "from-control"
            }
        )
    )

    # 局部钩子 校验当前的用户名必须带 nb_ 开头 --- 只能对单个只进行沟渠
    def clean_username(self):
        # cleaned_data 虽然是 全局校验成功的数据 但是在局部钩子中只能拿到 当前 钩子勾出的数据 username
        # print(self.cleaned_data)  # {'username': 'dream'}
        username = self.cleaned_data.get("username")
        if not username.startswith("nb_"):
            # 给当前字段增加错误信息
            self.add_error("username", f"不是以 nb_ 开头 ")

        # 在用局部钩子将参数勾出来校验后b要将原本的数据放回去
        return self.cleaned_data.get("username")

    # 局部钩子 校验 两次密码是否一致
    # 全局钩子 校验 两次密码是否一致 --- 多多个值进行沟渠
    def clean(self):
        # 在全局钩子中可以将所有校验后的数据获取到
        # print(self.cleaned_data)
        # {'username': 'nb_dream', 'password': '666', 'confirm_password': '666'}

        password = self.cleaned_data.get("password")
        confirm_password = self.cleaned_data.get("confirm_password")

        # 校验是够一直
        if password != confirm_password:
            self.add_error("confirm_password", "两次密码不一致!")

        # 将数据返回出去
        return self.cleaned_data
````

- 使用组件触发校验

````python
class LoginView(View):
    def get(self, request, *args, **kwargs):
        # 第一步导入自己创建的组件类 RegisterForm
        # 第二步创建一个 form 组件的对象
        form_obj = LoginFrom()
        return render(request, "login.html", locals())

    def post(self, request, *args, **kwargs):
        form_obj = LoginFrom(request.POST)
        if not form_obj.is_valid():
            print(form_obj.errors)
        else:
            print(form_obj.cleaned_data)
            # 进行模型表数据的新增
````



# 6 ModelForm

- form 组件的升级版本
- ModelForm 需要跟你的模型表搭配使用

```python
class User(models.Model):
    username = models.CharField(max_length=32)
    password = models.CharField(max_length=32)
    confirm_password = models.CharField(max_length=32)
```

- 自定义 ModelForm 组件

````python
from user.models import User


class RegisterModelFrom(forms.ModelForm):
    # 支持自定义字段规则 如果自己定义了 就用自己的 如果自己没有定义就用默认生成的
    username = forms.CharField(
        required=False,
        max_length=8,
        min_length=3,
        label="用户名",
        label_suffix=":>>>>",
        widget=forms.TextInput(attrs={
            "class": "from-control"
        })
    )

    # 需要创建一个 meta 属性
    class Meta:
        # 需要关联的模型表名
        model = User
        # 需要的是哪些字段 "__all__" 所有字段
        # fields = "__all__"
        # 用列表来区分哪些字段能够渲染 哪些字段抛弃
        fields = ["username", "password"]

        # 排除哪些字段不显示
        exclude = ["password"]

        # 要对哪些字段支持本地序列化 在创建时间的时候 不写就是国际时间 写了 字段就是对某个日期字段使用本地化
        localized_fields = []

    # 局部钩子 校验当前的用户名必须带 nb_ 开头 --- 只能对单个只进行沟渠
    def clean_username(self):
        # cleaned_data 虽然是 全局校验成功的数据 但是在局部钩子中只能拿到 当前 钩子勾出的数据 username
        # print(self.cleaned_data)  # {'username': 'dream'}
        username = self.cleaned_data.get("username")
        if not username.startswith("nb_"):
            # 给当前字段增加错误信息
            self.add_error("username", f"不是以 nb_ 开头 ")

        # 在用局部钩子将参数勾出来校验后b要将原本的数据放回去
        return self.cleaned_data.get("username")

    # 局部钩子 校验 两次密码是否一致
    # 全局钩子 校验 两次密码是否一致 --- 多多个值进行沟渠
    def clean(self):
        # 在全局钩子中可以将所有校验后的数据获取到
        # print(self.cleaned_data)
        # {'username': 'nb_dream', 'password': '666', 'confirm_password': '666'}

        password = self.cleaned_data.get("password")
        confirm_password = self.cleaned_data.get("confirm_password")

        # 校验是够一直
        if password != confirm_password:
            self.add_error("confirm_password", "两次密码不一致!")

        # 将数据返回出去
        return self.cleaned_data

# 在后边 DRF 中的序列化组件 serializers 在其中 所有的规则都是沿用的 form 组件
# forms.From -- serializers.Serializer
# forms.ModelForm --- serializers.ModelSerializer
````

- 使用

````python
class RegisterModelView(View):
    def get(self, request, *args, **kwargs):
        # 第一步导入自己创建的组件类 RegisterForm
        # 第二步创建一个 form 组件的对象
        form_obj = RegisterModelFrom()
        return render(request, "login.html", locals())

    def post(self, request, *args, **kwargs):
        form_obj = RegisterModelFrom(request.POST)
        if not form_obj.is_valid():
            print(form_obj.errors)
        else:
            print(form_obj.cleaned_data)
            # 进行模型表数据的新增
````





