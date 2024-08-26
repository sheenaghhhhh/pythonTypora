# 0 内容回顾

```python
# 【一】忠犬八公小游戏
# 1.面条版
# 先定义参数字典
# 然后调用字典生成相应的语句

# 2.函数版本
# 将初始化属性的动作封装成函数
# 将动作也封装成函数

# 3.整合
# 将所有的数据属性(值) 和方法熟悉(函数) 封装给初始化属性的函数
# 只需要调用一个函数就可以获取到当前这个对象的所有属性和动作

# 4.上面这种思想就是面向对象的思想
# 将数据和功能封装到一起

# 【二】什么是面向对象和面向过程
# 1.对象和过程
# 过程在于 分步骤进行 流程化解决问题
# 对象 将程序 整合 由统一的一个对象 来调用自己的方法

# 2.面对过程的优缺点
# 分步骤解决问题 思路比较清晰
# 但需要很多步骤来进行拼接

# 3.面对对象的优点
# 可以完成大部分功能的整合
# 必须先构思好需要哪些属性和功能

# 【三】类 -- 在 Python中的面向对象
# 1.类语法
# class 类名():
#   代码体

# 类名后面的 () 可以省略 不写
# 或者可以写空的

# 2.定义了一个类
class Student:
    # (1)数据属性
    school = "toronto"

    # (2)功能数据(函数属性/方法属性)
    # 在定义功能函数的时候自动补全一个self
    # self就是这个对象
    def sing(self):
        print(f"singing")


# 3.调用类
# 实例化得到一个对象
student = Student()
print(student)  # <__main__.Student object at 0x00000259419B6E00>
print(student.sing())

# 4.给具体的对象初始化属性
# (1)对象.__dict__ 获取当前对象的名称空间
student = Student()
# 查看的是对象的名称空间就只能看到对象初始化后的属性
# 对象是可以调用类的属性的
print(student.__dict__)  # {}
# 查看的是类的名称空间就只能看到 类初始化后的属性
print(Student.__dict__)  # {'__module__': '__main__', 'school': 'toronto'}

# (2)向字典中添加键值对来实现属性的初始化
student.__dict__.update(name="sheenagh")
print(student.__dict__)  # {'name': 'sheenagh'}
print(student.name)  # sheenagh


# (3)多个对象的时候
# 都需要初始化属性
# 所以将初始化属性的代码封装成一个函数 调用函数完成初始化的动作
def init_obj(obj, **kwargs):
    obj.__dict__.update(**kwargs)


student_one = Student()
init_obj(obj=student_one, name="kaikai", age=18)
print(student_one.__dict__)


# {'name': 'kaikai', 'age': 18}


# (4)既然这个初始化的动作是属于当前对象的
# 将初始化动作添加到类里面 由实例化后的对象来调用初始化方法
class NewStudent:
    # 1.数据属性
    # 确定的值
    school = "toronto"

    # 2.功能属性(函数属性/方法属性)
    # 在定义功能函数的时候会自动补全self
    # self就是这个对象
    def sing(self):
        print(self)  # <__main__.NewStudent object at 0x000001C7E1863DC0>
        print(f"singing")

    def init_obj(self, **kwargs):
        self.__dict__.update(kwargs)


student_two = NewStudent()
student_two.init_obj(name="tia", age=18, gender="male")
student_two.sing()
print(student_two.__dict__)  # {'name': 'tia', 'age': 18, 'gender': 'male'}


# 5.初始化方法
# 只需要在类的内部定义一个方法属性 __init__ 方法
# 就可以在初始化类得到对象的时候将需要的参数传进去完成初始化的动作
class NewNewStudent:
    # (1)数据属性
    # 确定的值
    school = "toronto"

    def __init__(self, name, **kwargs):
        self.__dict__.update(kwargs)
        self.__dict__.update({
            "name": name
        })

    # (2)功能数据(函数属性/方法属性)
    # 在定义功能函数的时候会自动补全一个self
    # self 就是当前这个对象
    def run(self):
        print(self)  # <__main__.NewNewStudent object at 0x0000019644633CD0>
        print(f"running")


student_three = NewNewStudent(name="sheenagh", class_id=3)
student_three.run()
print(student_three.__dict__)


class NewNewStudent:
    # (1)数据属性
    # 确定的值
    school = "toronto"

    def __init__(self, name, **kwargs):
        # 可以写成
        self.name = name
        self.__dict__.update(kwargs)

    # (2)功能数据(函数属性/方法属性)
    # 在定义功能函数的时候会自动补全一个self
    # self 就是当前这个对象
    def run(self):
        print(self)  # <__main__.NewNewStudent object at 0x0000019644633CD0>
        print(f"running")


# 【四】补充了一些内置的属性
print(Student.__name__)  # 查看类名
print(Student.__dict__)  # 查看名称空间
print(Student.__doc__)  # 查看注释文档
print(Student.__base__)  # 查看继承的第一个类
print(Student.__bases__)  # 查看继承的所有父类
print(Student.__module__)  # 查看当前类所在的模块名
```



# 1 面向对象的三大特性

```python
# 封装、继承、多态
# 其中最重要的一个特性就是封装
```



# 2 什么是封装

```python
# 封装就是将某些数据保护和隐藏起来
# 不希望除了开发者以外的其他人可以访问当前的属性和方法
```



# 3 为什么要封装

```python
# 保护数据 防止外部代码随意修改对象内部的代码
"""
class Student:
    school = "toronto"

student = Student()
student.school = "kkkk"
print(student.school)
"""
```



# 4 如何进行封装

```python
class Student:
    # 在需要被封装的属性位置 改变变量名
    __school = "toronto"  # _Student__school

    def __init__(self, name, age, gender):
        self.name = name
        self.age = age
        self.gender = gender

    def show_info(self):
        print(f"My name is {self.name}, my age is {self.age}, my gender is {self.gender}, my school is {self.__school}.")

    def __read(self):
        print(f"{self.__school} 的 {self.name} 正在读书")


student = Student("sheenagh", 23, "female")
student.show_info()
print(student.__dict__)
student.__school = "aba"
student.show_info()
print(student.__school)
# 为什么? self.__school 调用的是 _Student__school 看3
# 如果要用这个__school 在外面直接打印就好了
print(student.__dict__)
print(Student.__dict__)
student._Student__school = "aba"
student.show_info()

# 总结:
# 1.在类内部 将变量名 用双下划线 声明
# 在类初始化的时候就会由原来的变量名 __变量名 变换成 _类名__变量名
# 2.在类内部封装起来的属性
# 内部调用 self 去调用的时候直接 self.__变量名 就可以调用
# 如果是对象调用 隐藏起来的属性 那就需要 对象._类型__变量名 去调用
# 3.在类内部封装属性后变形只会发生一次
# 当使用对象或者类.__变量名去修改属性的时候 修改的是新的属性而不是原来的属性
```



# 5 开放接口

```python
# 将不允许用户查看和访问的数据进行封装
# 暴露给用户可以使用或者操作属性的方法

class Student:
    def __init__(self, name, age, gender):
        self.__name = name
        self.__age = age
        self.__gender = gender

    def show_info(self):
        print(f"{self.__name} 的年龄是 {self.__age} 性别是 {self.__gender}")

    def set_info(self, name, age):
        if not name.startswith("fire_"):
            raise ValueError(f"当前名字必须以 fire_ 开头")
        if not age.isdigit():
            raise ValueError("当前年龄必须为数字")
        # 符合上述条件就允许修改属性
        self.__name = name
        self.__age = age


student = Student("sheenagh", 23, "female")
student.show_info()
# sheenagh 的年龄是 23 性别是 female
student.name = "kkk"
student.show_info()
# sheenagh 的年龄是 23 性别是 female
# 用户想要修改属性 名字和年龄就必须通过我规范的接口进行修改
# student.set_info(name="opp", age=18)  # ValueError: ValueError: 当前名字必须以 fire_ 开头
# student.set_info(name="fire_k", age=18)  # AttributeError: 'int' object has no attribute 'isdigit'
student.set_info(name="fire_k", age="28")
student.show_info()
# fire_k 的年龄是 28 性别是 female
```



# 6 数据属性

## 6.1 引入

```python
import random


class Tools:
    def salt(self):
        code = ""
        for i in range(0, 6):
            code += str(random.randint(0, 9))
        return code


tool = Tools()
print(tool.salt)  # <bound method Tools.salt of <__main__.Tools object at 0x116b7bf70>


# 想要的效果： tool.salt 就是生成salt值 print出来是123456
# 可是现在这样写 salt是一个方法 没办法这样做


# 【一】BMI
# 用来衡量自己身健康水平的一个指标
# 计算公式：体重 / (身高*身高)
# ● 成人的BMI数值：
#   ○ 过轻：低于18.5
#   ○ 正常：18.5-23.9
#   ○ 过重：24-27
#   ○ 肥胖：28-32
#   ○ 非常肥胖, 高于32

class BMI:
    def __init__(self, name, height, weight):
        self.__name = name
        self.__height = height
        self.__weight = weight

    # bmi 计算得到的结果是一个数字 应该和你的姓名 性别 年龄 一样是一个数据属性
    # 而不是一个函数属性
    @property  # 要将函数属性伪装成数据属性 要使用property
    def bmi(self):
        return f"{self.__name} 的BMI数值是：{self.__weight / (self.__height * self.__height)}"
        # return self.__weight / (self.__height * self.__height)  # 22.54595907041276
        # return True, self.__weight / (self.__height * self.__height)  # (True, 22.54595907041276)
        # 分别是字符串 数字 元组


bim_tool = BMI(name="lishen", height=1.86, weight=78)

print(bim_tool.bmi)
# lishen 的BMI数值是：22.54595907041276
# 如果将函数 用 @property 装饰 以后
# 直接 .函数名 就可以获取到函数的返回值 而不需要使用()调用
```

## 6.2 property使用

```python
# 【二】为什么要用 @property 将函数属性伪装成数据属性
# 因为在定义类的时候有三个作用域
#   ○ 【public】
#     ■ 这种其实就是不封装,是对外公开的
#   ○ 【protected】
#     ■ 这种封装方式对外不公开
#     ■ 但对朋友(friend)或者子类(形象的说法是“儿子”,但我不知道为什么大家 不说“女儿”,就像“parent”本来是“父母”的意思,但中文都是叫“父类”)公开
#     ■ 不知道这段话是谁写的 但这样的疑问很好 也因此 今后我会有意识地使用"母类"
#   ○ 【private】
#     ■ 这种封装对谁都不公开
```

## 6.3 property装饰器

```python
class Student:
    def __init__(self, name, age, gender):
        self.__name = name
        self.__age = age
        self.gender = gender

    # 将函数属性包装成数据属性
    @property
    def real_name(self):
        return f"fire_{self.__name}"

    @real_name.setter
    def real_name(self, value):
        # print(f"real_name.setter real_name :>>>> {value}")
        # real_name.setter real_name :>>>> 
        if not len(value) <= 3:
            raise ValueError("名字必须是三个字及以下")
        self.__name = value

    @real_name.deleter
    def real_name(self):
        del self.__name


class Teacher:
    def __init__(self, name):
        self.name = name


student = Student(name="hey", age=20, gender="female")
# student.real_name ---> 返回了字符串属性
# 字符串() 会报错 因为字符串不能被调用 # TypeError: 'str' object is not callable

# 获取真实名字 触发 @property
# print(student.real_name())
print(student.real_name)
# fire_hey

# 直接改会报错
# 想修改当前的真实名字 触发 @real_name.setter
# student.real_name = "lil"  # AttributeError: can't set attribute 'real_name'
student.real_name = "lil"
print(student.real_name)
# fire_lil

# 删除当前的真实名字 触发 @real_name.deleter
'''
teacher = Teacher(name="dream")
print(teacher.name) # dream
del teacher.name # del 对象.属性名 可以将当前属性从对象中删除
print(teacher.name) # AttributeError: 'Teacher' object has no attribute 'name'
'''
# del student.real_name  # AttributeError: can't delete attribute 'real_name'
del student.real_name  # AttributeError: can't delete attribute 'real_name'
# print(student.real_name) # AttributeError: 'Student' object has no attribute '_Student__name'. Did you mean: '_Student__age'?
```

## 6.4 property内置函数

```python
class Student:
    def __init__(self, name, age, gender):
        self.__name = name
        self.__age = age
        self.gender = gender

    def __get_real_name(self):
        return f"fire_{self.__name}"
    # 将函数属性包装成数据属性

    def __set_real_name(self, value):
        if not len(value) <= 3:
            raise ValueError("名字必须是三个字及以下")
        self.__name = value

    def __del_real_name(self):
        del self.__name

    # 不用装饰器 使用内置函数 统一暴漏接口
    real_name = property(__get_real_name, __set_real_name, __del_real_name)


student = Student(name="hey", age=20, gender="female")
# student.real_name ---> 返回了字符串属性
# 字符串() 报错了 因为字符串不能被调用 # TypeError: 'str' object is not callable

# 获取真实名字 触发 @property
# print(student.real_name())
print(student.real_name)


# student.real_name = "lil"  # AttributeError: can't set attribute 'real_name'
student.real_name = "lil"
print(student.real_name)


del student.real_name  # AttributeError: can't delete attribute 'real_name'
# print(student.real_name)
# AttributeError: 'Student' object has no attribute '_Student__name'. Did you mean: '_Student__age'?
```