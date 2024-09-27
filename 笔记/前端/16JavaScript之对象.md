# 16 JavaScript之对象

## 1 对象

- js中的对象是无序的属性集合
- 我们可以把js中的对象想象成键值对，其中值可以是数据或者函数
- 特征-在对象中属性表示
- 行为-在对象中用方法表示

可以看成Python中的字典，但是在JS中的自定义对象要比Python里面的字典操作起来更方便



## 2 对象创建

```js
// 两种方式
// 方式1: 直接定义
var obj = {}
var obj = {name: "sheenagh", age:23}
console.log(tyoeof obj)

// 方式2: 构造函数来生成对象
var obj = new Object()
console.log(tyoeof obj)

// 方式3: 自定义对象
function Person(name, age) {
    this.name = name
    this.age = age
}
var obj = Person("sheenagh", 23)
console.log(tyoeof obj)

// 方式4: ES6中的新语法
class Student {
    constructor(name, gender) {
        this.name = name
        this.gender = gender
    }
}

var obj = new Student("sheenagh", "female")
console.log(typeof obj)
```



## 3 对象调用

```js
// 定义函数
function Person(name, age) {
    this.name = name
    this.age = age
    this.logAge = function () {
        console.log(`my age is ${this.age}`)
    }
    this.logName = function () {
        this.logAge()
        console.log(`my name is ${this.name}!`)
    }
}

// 用自定义函数生成对象
var person = new Person("sheenagh", 23)

// 1.对象的类型
console.log(typeof person)
// object

// 2.对象属性的访问
// 方式一：直接 对象.属性名
console.log(person.name)
console.log(person.age)

// 方式二：在类内部通过 this 调用自己的属性或方法

// 方式三：直接在类内部定义的函数内部用this调用自己的方法
person.logName()
// my name is sheenagh!

// 方法四：通过对象[key] 获取属性
console.log(person["name"])

// 3.索引访问
// 向对象中增加属性名和属性值 
person[0] = "hope"
console.log(person[0])

// 4.对象还可以被遍历
// 遍历对象获取到的是所有的属性名
for (let key in person){
    console.log(key)
    console.log(person[key])
}
```



## 4 日期对象Date

```js
// 1.创建一个日期对象
var dateObj = new Date();

// 2.获取到时间属性
// 获取年份（四位数）
dateObj.getFullYear() // 2024
// 获取月份（0-11）（0表示一月，11表示十二月）
dateObj.getMonth() // 8
// 获取当前的日期
dateObj.getDate() // 25
// 获取当前的星期
dateObj.getDay() // 3
// 获取当前小时
dateObj.getHours() // 16
// 获取当前分钟
dateObj.getMinutes() // 35
// 获取秒数
dateObj.getSeconds(); // 33
// 获取毫秒数
dateObj.getMilliseconds(); // 154
// 获取到当前的时间戳 等同于python里的 time.time()
dateObj.getTime() // 1727253333154
// 获取当前英国日期
dateObj.getUTCDate() // 25

// 3.格式化时间
dateObj
// Wed Sep 25 2024 16:35:33 GMT+0800 (中国标准时间)
dateObj.toString()
// 'Wed Sep 25 2024 16:35:33 GMT+0800 (中国标准时间)'
dateObj.toLocaleString()
// '2024/9/25 16:35:33'
dateObj.toDateString()
// 'Wed Sep 25 2024'
dateObj.toLocaleTimeString()
// '16:35:33'

// 4.支持自定义时间
var oldDate = new Date(2001, 5, 26, 13, 8, 60)
console.log(oldDate.toLocaleString())
// 2001/6/26 13:09:00

var oldDateOne = new Date("2028/11/11 11:11:11")
console.log(oldDateOne.toLocaleString())
// 2028/11/11 11:11:11
// 在控制台和Pycharm里 输出的格式会有一定的差异

// 5.支持设置时分秒
var dataObj = new Date();
dateObj.setFullYear(2024); // 设置年份
dateObj.setMonth(9); // 设置月份（表示一月，11表示十二月）
dateObj.setDate(25); // 设置日期（月中的某一天）
dateObj.setHours(16); // 设置小时
dateObj.setMinutes(45); // 设置分钟
dateObj.setSeconds(20); // 设置秒数
console.log(dateObj.toLocaleString())
// 2024/10/25 16:45:20
```



## 5 JSON对象

```js
// 在 Python中我们经常会使用 json 文件存储数据
// 在js 中同样要有json序列化和反序列化的操作

// 【一】Python中的序列化和反序列化
// 序列化：将Python对象转换为 json 字符串 --- json.dump (写文件) / json.dumps (转字典)
// 反序列化：将 json 字符串转换为 Python对象  --- json.load / json.loads

// 【二】在js中的序列化和反序列化
// 序列化：将 JS对象 转换为 json 字符串
// 反序列化：将 json字符串 转换为 JS对象

var userInfo = {
    'name': "sheenagh",
    'age': 23
}
var json_str = JSON.stringify(userInfo)
console.log(json_str)
// {"name":"sheenagh","age":23}
console.log(typeof json_str)
// string

var json_js = JSON.parse(json_str)
console.log(json_js)
// {name: 'sheenagh', age: 23}
console.log(typeof json_js)
// object

// 在 js 对象的定义中属性名是允许不加 双引号的 并且会被看做是一个参数名 而不是变量名
// 在Python中不行 Python中的字典的键如果是参数就必须加 引号
```



## 6 正则对象 RegExp

```js
// 正则对象 ： RegExp

// ● 正则表达式（Regular Expression）是一种用来匹配、查找和操作字符串的工具。
// ● 在Python中如果需要使用正则 需要借助于re模块
// ● 在JavaScript中，我们可以使用RegExp对象来创建和操作正则表达式。

// 【一】生成正则对象
// 方式1: 使用 类 构建对象
var regObj = new RegExp("[a-z]");

// 方式2: 直接写正则表达式 /正则表达式/
// [a-z] 字符组 匹配字符组中的任意一个字符
var regObj = /[a-z]/;

// 【二】正则方法
var data = "Hello World!"

// 1.正则对象.test(原始数据) 方法
var pattern = /Hello/
// 校验当前字符串中是否有符合上述表达式的数据
console.log(pattern.test(data))
// true

// 2.正则对象.exec(原始数据) 方法
var pattern = /l+/g;
// 查找到当前 表达式对应的 元素的索引位置
// 找不到的话 返回 null
console.log(pattern.exec(data))
// [ 'll', index: 2, input: 'Hello World!', groups: undefined ]

// 3.原始数据.match(正则对象) 方法
var pattern = /l+/g;
// 在原始数据中 匹配符合上述正则表达式的数据
console.log(data.match(pattern))
// [ 'll', 'l' ]

// 4.原始数据.search(正则对象)
var pattern = /lo/
// 在原始数据中匹配符合上述表达式的数据 并返回当前数据对应的索引位置
console.log(data.search(pattern))
// 3

// 5.原始数据.replace(正则对象,替换进去的字符)
var pattern = /o/g;
// 在原始数据中匹配符合正则表达式的数据并进行替换
console.log(data.replace(pattern, "A"))
// HellA WArld!

// 6.正则对象.flags
// 返回当前正则对象的模式修正符 默认是空修正符
var pattern = /ll/g;
console.log(pattern.flags) // g
var pattern = /ll/;
console.log(pattern.flags) //


// 【二】正则对象中的 bug
// 以 a-zA-Z 任意字符开头 后面是任意字符
var pattern = /^[a-zA-Z][A-Za-z0-9]/g;
var data = "sheenagh"

// 用 test 检测当前是否符合正则表达式
// 第一次检测发现 数据是符合的
console.log(pattern.test(data))
// true
console.log(pattern.lastIndex)
// 2
// 第二次检测发现数据又不符合了
console.log(pattern.test(data))
// false
console.log(pattern.lastIndex)
// 0
// 第三次发现又符合
console.log(pattern.test(data))
// true
console.log(pattern.lastIndex)
// 2
// 第四次检测发现数据又不符合了
console.log(pattern.test(data))
// false
console.log(pattern.lastIndex)
// 0

// 原因是 因为 js 中的正则 test 之后会移动光标
// 第一次检测过后的索引不是 0 会发现下一次检测数据 没有
// 但是检测完数据之后索引切回 0 就会有数据

// 【三】匹配数据为空
var pattern = /^[a-zA-Z][A-Za-z0-9]/g;

// 如果在 Python中 re中不传入数据就会报错
// 如果在 js 中 正则对象不传入数据返回的就是 true
// 默认传入参数 undefined
console.log(pattern.test())
// true
```



## 7 math对象

```js
// 数学对象 Math

// 先生成一个对象
// abs(x)      返回数的绝对值。
// exp(x)      返回 e 的指数。
// floor(x)    对数进行下舍入。
// log(x)      返回数的自然对数（底为e）。
// max(x,y)    返回 x 和 y 中的最高值。
// min(x,y)    返回 x 和 y 中的最低值。
// pow(x,y)    返回 x 的 y 次幂。
// random()    返回 0 ~ 1 之间的随机数。
// round(x)    把数四舍五入为最接近的整数。
// sin(x)      返回数的正弦。
// sqrt(x)     返回数的平方根。
// tan(x)      返回角的正切。

// 返回 0 ~ 1 之间的随机数。
console.log(Math.random())


Math.abs(-10)
// 10
Math.exp(3)
// 20.085536923187668
Math.floor(3.51)
// 3
Math.log(1)
// 0
Math.max(5,2)
// 5
Math.min(2,5)
// 2
Math.pow(2,5)
// 32
Math.random()
// 0.5401606640069943
Math.round(3.51)
// 4
Math.sin(Math.PI/2)
// 1
Math.sqrt(25)
// 5
Math.tan(Math.PI/4)
// 0.9999999999999999
```



