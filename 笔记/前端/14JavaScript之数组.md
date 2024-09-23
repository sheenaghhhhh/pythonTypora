# 14 JavaScript之数组

## 1 什么是数组

- 函数function，也叫做功能，方法 ，函数可以将一段代码封装起来，函数就具备了特定的功能
- 函数的作用就是封装一段代码，将来可以重复使用
- 在Python中定义函数需要用 def
- 在JavaScript中定义函数需要用 function



## 2 创建数组

```js
// 1.方式一 ： 直接声明数组
var arr1 = [];
undefined
typeof(arr1)
'object'

// 2.方式二：利用类生成对象
var arr2 = new Array();
undefined
typeof(arr2)
'object'
```



## 3 获取元素

```js
// 声明数组
var arr = [1,2,3];
undefined
arr
(3) [1, 2, 3]
// 按照索引获取数据
arr[0]
1
```



## 4 数组长度

```js
arr.length
3
```



## 5 数组常用方法

```js
// 概览
// 1.遍历循环 forEach 和 for of
// 2.合并数组 concat
// 3.数组拼接字符串 join
// 4.移除数组第一个元素 shift
// 5.移除数组最后一个元素 pop
// 6.在数组的开头插入元素 unshift
// 7.在数组的结尾追加元素 push
// 8.数组翻转 reverse
// 9.数组元素排序 sort
// 9.数组切片 slice
// 10.切片删除 splice
// 11.数组转字符串 toString
// 12.数组转字符串 格式化 toLocaleString
// 13.查找元素出现的第一个位置 indexOf
// 14.查找元素出现的最后一个位置 lastIndexOf
// 15.每个元素执行指定操作 map
// 16.过滤符合条件的元素 filter
// 17.所有元素满足条件 every
// 18.任一元素满足条件some
// 19.累计数组中的元素 生成单一输出值 reduce
// 20.从末尾开始累计 reduceRight
```

```js
// 1.遍历循环 forEach 和 for of
var arr = [1,2,3,4,5];
// (1)forEach
// 传入1个参数 -- 遍历每一个元素

// 又可以分成3中写法 内写函数、外写函数、ES6箭头
// --forEach(函数)
arr.forEach(function(element) {
    console.log(element);
})
1
2
3
4
5
undefined

// --写好函数再forEach(函数名)
function foo(args){
  console.log(args);
};
arr.forEach(foo);
1
2
3
4
5
undefined

// --ES6箭头语法 (参数)=>{执行代码}
arr.forEach((args)=>{console.log(args);})
1
2
3
4
5
undefined

// 传入2个参数 -- 第一个参数是当前元素 第二个参数是对应的索引
arr.forEach((value,index)=>{console.log(value,index);})
1 0
2 1
3 2
4 3
5 4
undefined

// 传入3个参数 -- 第一个参数是当前元素 第二个参数是对应的索引 第三个参数是当前数组
arr.forEach((value,index,item)=>{console.log(value,index,item)})
1 0 (5) [1, 2, 3, 4, 5]
2 1 (5) [1, 2, 3, 4, 5]
3 2 (5) [1, 2, 3, 4, 5]
4 3 (5) [1, 2, 3, 4, 5]
5 4 (5) [1, 2, 3, 4, 5]
undefined


// (2)for of
// for(遍历语句){执行代码}
for (let item of arr){
    console.log(item);
}
1
2
3
4
5
undefined
```

```js
// 2.合并数组 concat
var arr1 = [1,2,3];
var arr2 = [4,5,6];

// 两个列表相加是隐式转换 先将两个列表都转换为字符串然后字符串拼接
arr1+arr2;
'1,2,34,5,6'

arr1.concat(arr2);
(6) [1, 2, 3, 4, 5, 6]
```

```js
// 3.数组拼接字符串 join
// join:可以使用特殊字符将数组中的每一个数据拼接到一起
arr.join("|")
'1|2|3|4|5'
```

```js
// 4.移除数组第一个元素 shift
// 删除第一个元素并弹出
arr.shift();
1
arr
(3) [2, 3, 4]
```

```js
// 5.移除数组最后一个元素 pop
// 删除最后一个元素并弹出
arr.pop();
5
```

```js
// 6.在数组的开头插入元素 unshift
arr.unshift(999);
6
arr
(6) [999, 1, 2, 3, 4, 5]
```

```js
// 7.在数组的结尾追加元素 push
arr.push(888);
7
arr
(7) [999, 1, 2, 3, 4, 5, 888]
```

```js
// 8.数组翻转 reverse
arr.reverse()
(7) [888, 5, 4, 3, 2, 1, 999]
arr
(7) [888, 5, 4, 3, 2, 1, 999]
```

```js
// 9.数组元素排序 sort
arr.sort()
(7) [1, 2, 3, 4, 5, 888, 999]
arr
(7) [1, 2, 3, 4, 5, 888, 999]
```

```js
// 9.数组切片 slice
// 数组.slice(起始索引, 结束索引)
arr.slice(2,5)
(3) [3, 4, 5]
// 原本的列表不受影响
arr
(7) [1, 2, 3, 4, 5, 888, 999]
```

```js
// 10.切片删除 splice
// 数组.splice(起始索引,删除元素个数,补充元素)
// 从索引为2的元素开始向后删除3个元素
arr.splice(2,3)
(3) [3, 4, 5]
arr
(4) [1, 2, 888, 999]

// 从索引为2的元素开始向后删除2个元素，然后追加指定元素
arr.splice(2,2,3,4,5)
(2) [888, 999]
arr
(5) [1, 2, 3, 4, 5]
```

```js
// 11.数组转字符串 toString
arr.toString()
'1,2,3,4,5'
```

```js
// 12.数组转字符串 格式化 toLocaleString
arr.toLocaleString()
'1,2,3,4,5'
// toLocaleString()
// 里面的数组元素会根据系统的区域设置进行格式化
// 一般千位数会被格式化 8888->8,888 1234567->1,234,567
```

```js
// 13.查找元素出现的第一个位置 indexOf
var arr =  [1,2,3,4,5,888,999,5,5,5];
// 查找目标元素出现的第一个位置 如果没有找到就返回-1
arr.indexOf(5)
4
arr.indexOf(123)
-1
```

```js
// 14.查找元素出现的最后一个位置 lastIndexOf
// 查找目标元素最后一次出现的索引位置
arr.lastIndexOf(5)
9
```

```js
// 15.每个元素执行指定操作 map
// 数组.map(函数)
function foo(item){
    console.log(item)
    return item * item
}
undefined
arr.map(foo)
1
2
3
4
5
888
999
5
5
5
(10) [1, 4, 9, 16, 25, 788544, 998001, 25, 25, 25]

// 也可以仿箭头函数 ES6
arr.map(item => item * item)
(10) [1, 4, 9, 16, 25, 788544, 998001, 25, 25, 25]
```

```js
// 16.过滤符合条件的元素 filter
// 按照指定条件进行过滤 
// 遍历数组中的每一个元素 用每一个元素 /2 取余 如果为 0 则保存
arr.filter(item => item % 2 == 0)
(3) [2, 4, 888]
arr
(10) [1, 2, 3, 4, 5, 888, 999, 5, 5, 5]
```

```js
// 17.所有元素满足条件 every
var arr = [true,false,1,0]
undefined
arr.every(item => item)
false

var arr = [true,1]
undefined
arr.every(item => item)
true
```

```js
// 18.任一元素满足条件some
// 任一元素为 true 返回true
var arr = [true,false,1,0]
undefined
arr.some(item => item)
true

var arr = [false,0]
undefined
arr.some(item => item)
false
```

```js
// 19.累计数组中的元素 生成单一输出值 reduce
// 按照规则累计数组生成单个值
var arr = [1,2,3,4,5];

function add(total,num){
  console.log("total:>>>",total )
  console.log("num:>>>",num )
  total += num
  return total
};
arr.reduce(add)
VM1032:5 total:>>> 1
VM1032:6 num:>>> 2
VM1032:5 total:>>> 3
VM1032:6 num:>>> 3
VM1032:5 total:>>> 6
VM1032:6 num:>>> 4
VM1032:5 total:>>> 10
VM1032:6 num:>>> 5

arr.reduce((total,num)=> total + num,0)
15
```

```js
// 20.从末尾开始累计 reduceRight
function add(total,num){
  console.log("total:>>>",total )
  console.log("num:>>>",num )
  total += num
  return  total
};
arr.reduceRight(add)
VM1059:2 total:>>> 5
VM1059:3 num:>>> 4
VM1059:2 total:>>> 9
VM1059:3 num:>>> 3
VM1059:2 total:>>> 12
VM1059:3 num:>>> 2
VM1059:2 total:>>> 14
VM1059:3 num:>>> 1
15
```





