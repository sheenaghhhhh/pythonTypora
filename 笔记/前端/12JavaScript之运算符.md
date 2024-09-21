# 12 JavaScript之运算符

## 1 算数运算符

```js
// + - * / %
5 + 1
6
5 - 1
4
5 * 1
5
5 / 2
2.5
5 % 2
1

// 隐式转换
// NaN 参与 得到NaN
// null -> 0; undefined -> NaN
null + 1
1
undefined + 1
NaN
```



## 2 比较运算符

```js
// >  >= <= <
// == != 是弱比较 不强调类型一致
// === !== 是强比较 需要类型和值都一直
2 > 1
true
2 >= 2
true
2 < 1
false
2 <= 2
true
1 === "1"
false
1 == "1"
true

// js不可以写两个连
// 目前碰到的编程语言都不可以这样写
80 < 2 < 100
true
80 < 90 < 100
true
// 执行顺序是 
// 80 < 2 得到false -> 0 
// 80 < 90 得到true -> 1
// 不管1还是0都小于100
```



## 3 逻辑运算符

```js
// 与 或 非
// 与 && and
// 规则:两个都为真 才为真 
true && true
true
true && false
false
5 && 0
0
null && 1
null

// 或 || 0
// 规则:任一为真 就为真
true || false
true
false || false
false
null || "haha"
'haha'
0 || 42
42

// 非 ! not
// 规则:将数的布尔值取反
!0
true
!false
true
```





## 4 赋值运算符

```js
//+= -= *= /= %=
a = 5
5
a+=1 
6
a-=1
5
a*=2
10
a/=2
5
a%=2
1

//++ --
a++
1
a--
2
```



## 5 一元运算符

```JS
//关于a++和++a
a = 5
b = a++
5
a:6
b:5
//先返回a当前值 再自增

a = 5
b= ++a

b:6
a:6
// 先自增 再返回新值
```



## 6 运算优先级

括号>一元>算数>比较>逻辑>赋值


