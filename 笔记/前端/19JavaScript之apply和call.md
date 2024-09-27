# 19 JavaScript之apply和call

## 1 引言

- call() 和 apply()都是为了改变被调用的函数内部的上下文（context）而存在的，即改变被调用的函数内部的this指向
- call() 和 apply()二者的作用完全一样，只是接受参数的方式不同，call接收参数列表，apply接收一个数组或伪数组



## 2 call方法

```js
// 1.语法
function.call(thisArg, arg1, arg2, arg...);

// 2.语法参数说明
// call()方法调用一个函数function
// 其具有一个指定的this值和分别提供的参数

// 3.作用
// 可以让call()中的对象调用当前对象所拥有的函数
// call()中的对象指的是：thisArg,即传递到call()方法中的this
// 当前对象指的是：function对象

// 4.区别
// 和apply()的区别在于传递的参数不同
// apply传递的参数是一个数组或伪数组

// 5.补充
// 非严格模式中： this为null和undefined时会自动指向全局对象(window对象)
```

```js
// 6.案例
// 6.1 继承父构造函数中的成员

// 使用Food构造函数创建的对象实例拥有在Product构造函数中添加的name属性和price属性
// 但category属性是在Food的构造函数中定义的。
// 使用call()实现继承父构造函数中的成员

// 【一】定义一个函数 Product
// 具有 name 和 price 参数
// 具有 name 和 price 和 show 属性
function Product(name, price) {
    this.name = name;
    this.price = price;
    this.show = function () {
        console.log("今天" + this.name + "的价格：￥" + this.price + "元");
    }
    // 如果传入的价格小于 0 抛出错误
    if (price < 0) {

        throw RangeError(this.name + "的价格不能为负数");
    }
}

// 【二】构造一个函数 Food
// 具有 name 和 price 参数
function Food(name, price) {
    //使用call()方法调用父构造函数可以实现继承
    Product.call(this, name, price);
    this.category = "food";
}

// 【三】构造对象
// 1.构造 Product 函数的对象
product = new Product(name = "西红柿", price = 16);
// 查看对象的属性
console.log(product.name) // 西红柿
console.log(product.price) // 16
console.log(product.show) // [Function (anonymous)]

// 2.构造 Food 函数的对象
food = new Food(name = "大米", price = 2.50)
// 查看对象的属性 
console.log(food.name) // 大米
console.log(food.price) // 2.5
console.log(food.show) // [Function (anonymous)]
console.log(food.category) // food
```

```js
// 以上代码等价于下述代码
// 【一】定义一个函数 Product
// 具有 name 和 price 参数
// 具有 name 和 price 和 show 属性
function Product(name, price) {
    this.name = name;
    this.price = price;
    this.show = function () {
        console.log("今天" + this.name + "的价格：￥" + this.price + "元");
    }
    // 如果传入的价格小于 0 抛出错误
    if (price < 0) {
        throw RangeError(this.name + "的价格不能为负数");
    }
}

// 【二】构造一个函数 Food
// 具有 name 和 price 参数
function Food(name, price) {
    this.name = name;
    this.price = price;
    this.show = function () {
        console.log("今天" + this.name + "的价格：￥" + this.price + "元");
    }
    if (price < 0) {
        throw RangeError(this.name + "的价格不能为负数");
    }

    this.category = "food";
}

// 【三】构造对象
// 1.构造 Product 函数的对象
product = new Product(name = "西红柿", price = 16);
// 查看对象的属性
console.log(product.name) // 西红柿
console.log(product.price) // 16
console.log(product.show) // [Function (anonymous)]

// 2.构造 Food 函数的对象
food = new Food(name = "大米", price = 2.50)
// 查看对象的属性
console.log(food.name) // 大米
console.log(food.price) // 2.5
console.log(food.show) // [Function (anonymous)]
console.log(food.category) // food
```

```js
// 6.2 继承父类的属性
function Animal(name) {
    this.name = name;
    this.showName = function () {
        console.log(this.name);
    }
}

function Cat(name) {
    // 使用call()方法调用父构造函数可以实现继承父类的属性
    Animal.call(this, name);
}

// 创建一个Cat对象
var cat = new Cat("Black Cat");
// 调用Cat对象的showName方法
cat.showName(); //Black Cat

// 6.3 调用匿名函数
// 在下例中的for循环体内
// 我们创建了一个匿名函数
// 然后通过调用该函数的call方法，将每个数组元素作为指定的this值执行了那个匿名函数。
// 这个匿名函数的主要目的是给每个数组元素对象添加一个print方法，这个print方法可以打印出各元素在数组中的正确索引号。
// 当然，这里不是必须得让数组元素作为this值传入那个匿名函数（普通参数就可以），目的是为了演示call的用法。
var animals = [
    {species: 'Lion', name: 'King'},
    {species: 'Whale', name: 'Fail'}
];

for (var i = 0; i < animals.length; i++) {
    (function (i) {
        this.print = function () {
            console.log('#' + i + ' ' + this.species + ': ' + this.name);
        }
        this.print();       //#0 Lion: King     //#1 Whale: Fail
    }).call(animals[i], i);
}
```



### 3 apply方法

```js
// 1.语法
function.apply(thisArg, [argsArray]);

// Function.apply(obj,args)方法能接收两个参数：
// obj：这个对象将代替Function类里this对象
// args：这个是数组或类数组，apply方法把这个集合中的元素作为参数传递给被调用的函数。

// 2.语法参数说明
// apply()方法调用一个函数function
// 其具有一个指定的this值和数组或伪数组

// 3.作用
// 可以让apply()中的对象调用当前对象所拥有的函数
// apply()中的对象指的是：thisArg,即传递到apply()方法中的this
// 当前对象指的是：function对象

// 4.区别
// 和call()的区别在于传递的参数不同
// call()传递的参数是一个个的参数

// 5.补充
// 非严格模式中： this为null和undefined时会自动指向全局对象(window对象)
// Chrome 14 以及 Internet Explorer 9 仍然不接受伪数组对象。如果传入伪数组对象，它们会抛出异常
```



## 4 apply()和call()参数对比

```js
// 【一】参数是数据列表
(function () {
    Array.prototype.push.call(arguments, 6, 7);
    console.log(arguments);
    //[1, 2, 3, 6, 7]
})(1, 2, 3);

// 【二】参数是数组
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];
Array.prototype.push.apply(arr1, arr2);
console.log(arr1);
//[1, 2, 3, 4, 5, 6]
console.log(arr2);
//[4, 5, 6]
```



## 5 判断数据类型

```js
console.log(Object.prototype.toString.call(123));
//[object Number]

console.log(Object.prototype.toString.call('123'));
//[object String]

console.log(Object.prototype.toString.call(undefined));
//[object Undefined]

console.log(Object.prototype.toString.call(true));
//[object Boolean]

console.log(Object.prototype.toString.call({}));
//[object Object]

console.log(Object.prototype.toString.call([]));
//[object Array]

console.log(Object.prototype.toString.call(function () {
}));
//[object Function]
```



## 6 严格模式与非严格模式的this

### 6.1 非严格模式

```js
// 在非严格模式下当我们第一个参数传递为null或undefined时
// 函数体内的this会指向默认的宿主对象
// 在浏览器中则是window

window = global;
function test() {
  console.log(this === window);
}

test.call(null);
// true
test.apply(undefined);
// true
```



### 6.2 严格模式

```js
// 在ECMAScript 5的strict模式下
// 这种情况下的this已经被规定为不会指向全局对象window
// 而是undefined

function test() {
    "use strict";
    console.log(this);
}

test(); // undefined
```

