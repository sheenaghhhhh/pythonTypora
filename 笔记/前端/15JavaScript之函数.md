# 15 JavaScript之函数

## 1 函数

- 函数function，也叫做功能，方法 ，函数可以将一段代码封装起来，函数就具备了特定的功能
- 函数的作用就是封装一段代码，将来可以重复使用
- 在Python中定义函数需要用 def
- 在JavaScript中定义函数需要用 function



## 2 函数声明

- 必须先声明 才能使用

```js
function 函数名(参数名){
    执行的函数体代码
};
```



## 3 函数调用

```js
// 无参函数
func()

// 有参函数
func(1, 2)
// js传多了也只拿需要的数据 传少了不报错
```



## 4 函数的参数

```js
// 无参无返回值
function foo(){}

// 有参无返回值
function foo(param){}

// 有参有返回值
function foo(param){return back_value}

// js也分形式参数和实际参数
```



## 5 函数的返回值

```js
// 1.return 可以把指定值返回给调用者

// 2.return也是用来结束函数的执行流程

// 3.在 js 中的默认返回值是 undefined 

// 4.一个函数的返回值可以作为其他函数的参数
```



## 6 函数表达式

- 函数表达式是函数定义的另一种形式，就是将函数的定义，匿名函数赋值给一个变量
- 函数定义赋值给一个变量，相当于将函数的整体矮化成了一个表达式
- 调用函数表达式方法是给变量名加（）执行，不能使用函数名（）执行
- 通常函数表达式都会使用匿名函数

```js
var foo = function (param){
    return back_value
}
```



## 7 闭包函数

```js
// python的写法
def outer(func):
	def inner(*args,**kwargs):
  	res = func(*args,**kwargs)
		return res
	return inner
// 在Python中使用装饰器可以用语法糖 @outer


function add(param){
    console.log(param)
}
undefined

function outer(func){
    function inner(args){
        console.log(func ,"开始执行")
        res = func(args)
        console.log(func ,"结束执行")
        return res
    }
    return inner
}
undefined

add = outer(add);
ƒ inner(args){
        console.log(func ,"开始执行")
        res = func(args)
        console.log(func ,"结束执行")
        return res
    }

add(1)
VM1246:3 ƒ add(param){
    console.log(param)
} '开始执行'
VM1238:2 1
VM1246:5 ƒ add(param){
    console.log(param)
} '结束执行'
undefined
```



## 8 arguments对象

```js
// js中arguments是一个特殊的对象 会接收所有的实参 可以通过length获取参数的个数

function foo(a,b,c){
	console.log(a,b,c)
}

// 调用函数 传入三个参数 发现只有三个
// 调用函数传入四个参数 不报错 发现传入的第四个参数不见了
function foo(a,b,c){
  	// 在函数内部有一个对象 arguments 对象
  	// 可以接收到 多余的位置参数
	console.log(arguments)
	console.log(arguments[3])
	console.log(a,b,c)
}

// 用处可以用来校验传入的参数是否符合指定的个数
function foo(a,b,c){
	console.log(arguments)
	// 在函数内部有一个对象 arguments 对象
	if (arguments.length > 3){
		console.log("参数传多了")
	}else {
		console.log("参数传少了")
    }
}
```



## 9 函数的递归

- 函数的内部可以调用函数自己本身，这种做法称为递归
- 函数递归需要考虑好结束递归的条件，不然会出现死递归导致程序崩溃，内存溢出
- 递归一般是解决数学中的问题



## 10 作用域

- 变量起作用的范围
- 函数内部是一个作用域
- 作用域也可以用一对大括号包裹起来，在es6以前是没有块级作用域这个概念，在后续es6中补充
- 变量退出作用域会自动销毁，全局变量只有关闭浏览器才会销毁



## 11 参数和函数的作用域

```js
// 在函数内部定义的参数 不允许外部调用 
// var是函数作用域 函数内部可以访问 函数外部不可见
// 但是 使用var name = name东西测试后 这个name变量会提升到函数的顶部 导致在函数体内部是一个局部变量name
// 访问全局 name并没有影响 所以是空字符串
function foo(a){
	var name = a;
}
undefined

foo("sheenagh")
undefined
name
''

// let 块级作用域 在 {} 中有效
// userName 局部变量 在函数外不可见 且局部变量无法访问
function bar(a){
	let userName = a;
}
undefined

bar("sheenagh")
undefined
userName
VM1390:1 Uncaught ReferenceError: userName is not defined
    at <anonymous>:1:1
```



## 13 函数的全局变量与局部变量

```js
// 和Python的类似

var city = "BeiJing";
function f() {
  var city = "ShangHai";
  function inner(){
    var city = "ShenZhen";
    console.log(city);
  }
  inner();
}

f();  //输出结果是？
// ShenZhen


var city = "BeiJing";
function Bar() {
  console.log(city);
}
function f() {
  var city = "ShangHai";
  return Bar;
}

var ret = f();
ret();  // 打印结果是？
// BeiJing


var city = "BeiJing";
function f(){
    var city = "ShangHai";
    function inner(){
        console.log(city);
    }
    return inner;
}
var ret = f();
ret();
// ShangHai
```



## 14 匿名函数

```js
// 学类的时候有一个默认的额参数 叫 self 可以调用自己内的参数
function foo(name){
	// 函数内部的 this 就是我们 类中的self 
	this.name = name
	console.log(`my name is name ${name}`)
	console.log(`my name is this.name ${this.name}`)
}

foo("sheenagh");
// 在 js 中没有 类 
// 所以 在js 中的函数其实就是 Python 中的类
// 在 Python 中 类用 self 调用对象本身的属性
// 在 js 中 用 this 来代表自己本身的对象
```



## 15 ES6新语法 箭头函数

```js
// 【ES6新语法】箭头函数
function foo(param){
  console.log(param)
  return 111 
}

// 转成箭头函数
(参数)=>{代码} 

// 原始函数转为箭头函数 
var foo = (param)=>{console.log(param)}

// 这样也可以 
var foo = (param)=>console.log(param)


function foo(param){
  return 111 
}
var foo = (param)=> param * param
```

