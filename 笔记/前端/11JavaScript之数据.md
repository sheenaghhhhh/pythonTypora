# 11 JavaScript之数据

## 1 常用的调试语句

```javascript
// alert 警告语句
alert("内容") --- 在页面上弹出一个警告框 点击确定后消失 刷新后重新触发
alert("这是警告语句")

// prompt 对话框 
prompt("提示内容",默认值)
result = prompt("出生年份",2001)

// console 打印语句
console.log("需要打印的内容")
console.log(result)
// 在我们写的 html / js 直接敲 log 自动补全  console.log()
```



## 2 javascript中的数据类型

### 2.1 js中有哪些数据类型

- 简单：Number、String、undefined、Boolean、null
- 复杂：object

### 2.2 如何查看变量的数据类型

```javascript
typeof 变量名

typeof "sheenagh"
'string'
typeof 23
'number'
typeof false
'boolean'
```

### 2.3 字面量

- 字面量是用于表达固定值的方法，又叫常量
- 所见即所得
- 比如：数字、字符串、布尔值等

### 2.4 特殊值

- Infinity 无穷：无穷大 Infinity , 无穷小 -Infinity，可以通过isFinite 来判断
- NaN：不是一个正常的数，是数字类型，可以通过 isNaN 来判断

```javascript
typeof Infinity
'number'
typeof NaN
'number'
40 - 23
17
NaN + 23
NaN
```



## 3 常量和变量

```javascript
// 常量就是定义之后不会改变的变量
// 变量是用来存储用户数据的标识

// 【一】声明变量的语法
// 关键字 var + 变量名 = 变量值
var name = "sheenagh"
undefined

// 输出了 undefined 就跟你在Python中调用了函数但是函数没有返回值 默认就是None = undefined

// 直接敲 name 输出 sheenagh 因为控制台内置了 log 语法 ， 不用写 print 关键字就能查看 变量的意思是一样的
name
'sheenagh'

// console.log 相当于调用了函数但是没有返回值 None = undefined
console.log(name)
sheenagh
undefined

// 【二】ES6新语法
// let + 变量名 = 变量值;
let name = "sheenagh";

//【三】var 和 let  的区别
// 变量的作用域不一样
// 用 var 声明的变量可以全局使用
// let 声明的变量只能局部使用

// 类似于在Python中内部 用 var 声明 在函数外也能使用
// 类似于在Python中内部 用 let 声明 在函数外不能使用

// 在Python中有严格的作用域 在函数内部的变量在函数外部无法调用

// 【三】jS中的变量名规范
// 变量名只能是 数字 + 字符 + 下划线 + $
// 变量名命名建议用驼峰体 小驼峰 userName passWord 
// 变量名不能关键字开头
// 变量名不能用数字开头

// 【四】常量
// 在Python中定义的常量可以改
// 在js代码中的常量 一旦定义无法修改
// const + 变量名 = 变量值;
const age = 18;
```

