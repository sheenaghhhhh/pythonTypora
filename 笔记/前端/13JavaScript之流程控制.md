# 13 JavaScript之流程控制

## 1 if语法

```js
// js中的条件语句 
// 单分支
if (条件){
    条件成立执行的代码
}

// 双分支
if (条件){
    条件成立执行的代码
}
else{
    条件不成立执行的代码
}

// 多分支
if (条件){
    条件成立执行的代码
}
else if (条件){
    条件不成立执行的代码
}
else{
    条件不成立执行的代码
}
```

```js
score = 99;
if (score>=90 && score<=100) {
    console.log("优秀")
}
else if (score>=80) {
    console.log("良好")
}
else {
    console.log("不及格")
}
```



## 2 switch~case语法

```js
let day = 3;

switch (day) {
  case 1:
    console.log("Monday");
    break;
  case 2:
    console.log("Tuesday");
    break;
  case 3:
    console.log("Wednesday");
    break;
  case 4:
    console.log("Thursday");
    break;
  case 5:
    console.log("Friday");
    break;
  default:
    console.log("Invalid day");
}
// 输出: Wednesday


// 如果你在 case 之后不使用 break，它会继续执行后面的 case 语句，即使 case 的条件没有匹配。这种行为称为 fall-through（贯穿）。
// default: default 是可选的，它会在没有任何 case 匹配时执行。

// if 条件语句的 判断只能是布尔值 true 或false
// 但是 case 语句不仅仅能放布尔还能放指定的值
```



## 3 for循环

```js
// for (声明起始变量;判断条件;执行的条件){条件成立执行的代码}

for (let i = 0; i < 10; i++) {
    console.log(i)
}
```



## 4 while循环

```js
// while (结束条件){执行的代码}
/*
count = 0
while count < 10:
    print(count)
    count += 1
* */

var count = 0
while (count < 10) {
    console.log(count)
    count++
}
```



## 5 do~while循环

```js
// do ~ while 循环
// ● 后侧循环语句
// ● 最少执行一次

do {
  // 执行的代码
} while (条件);

let i = 1;
do {
  console.log(i);
  i++;
} while (i <= 5);
```





## 6 三元运算符

```js
// 三元运算符
// 为真的结果 if 判断条件 else 条件为假的结果
// 条件 ? 条件成立的结果 : 条件不成立的结果

a = 1
b = 2

// 条件是 a > b 
// 条件为真则取 a 条件为假 则 取 b
a > b ? a : b
```

