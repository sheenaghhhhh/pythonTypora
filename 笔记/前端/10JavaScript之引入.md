# 10 JavaScript之引入

## 1 JavaScript介绍

### 1.1 JavaScript简介

```python
# 1.什么是JS
# JS就是 JavaScript 可以用来写前端的事件操作

# 2.什么是node.js
# 在js开始的时候很受欢迎 于是 js 就有点膨胀
# 不仅想用 js 做前端 还想做后端
# 于是 node.js 诞生
# 市面上使用不多

# 3.和Java的关系
# 没有关系 蹭热度

# 4.JS 不是一门完善的语言
# 最开始只用了7天开发出来
# 里面bug多
# 但是重写太复杂 打补丁
# 补丁自己的bug 再打补丁
# 最后导致JS 中 不合理的地方很多
# 但是也都继续用
```

### 1.2 JavaScript 和 ECMAScript

```python
# 1.历史
# 1996年11月 公司将JavaScript提交给估计语言组织 希望能通过认证
# 次年 ECMAScript 发布了标准文件第一版 规定了浏览器脚本语言的标准
# 并且 将 ECMAScript 作为所有浏览器语言的标准

# 2.为什么不叫JavaScript
# 一开始针对 JavaScript 标准 做的
# 原因1: Java名字有版权
# 原因2: 想体现出 创造者 ECMA 而不是公司 Netscape 有利于保持中立和开发性

# 但是后面随着大众都叫 JavaScript 就保持了这个名字
```

### 1.3 ES6语法

```python
# ● ES6 既是一个历史名词，也是一个泛指，含义是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等等，而 ES2015 则是正式名称，特指该年发布的正式版本的语言标准。
# ● 提到 ES6 的地方，一般是指 ES2015 标准，但有时也是泛指“下一代 JavaScript 语言”。

# ES6语法 箭头函数
```

### 1.4 JavaScript的组成

```python
# ECMAScript标准 + DOM + BOM
# DOM - document object model 页面源码文档对象
# BOM - browser object model 浏览器对象
```



## 2 注释语法

```python
# 单行 //
# 多行 /**/
```



## 3 书写JS代码的方式

```python
# 1.head内部的 script 标签内部写 js代码
# 2.body内部的 script 标签内部写 js代码
# 3.创建一个 js 文件 引入 用 script标签
# 4.打开开发者模式切换到控制台
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--  方式1: 在head里的script  -->
    <script>
        alert("head里的script!")
    </script>
    <script src="08JavaScript语法.js">
        <!--    方式3:  创建js代码文件并引入  -->
    </script>
</head>
<body>
<!-- 方式2: 在body里的script -->
<script>
    alert("body里的script!")
</script>
</body>
</html>

<!-- 方式4: 开发者模式中的控制台 ctrl + shift + i -->
```



## 4 语法结构

```python
"""
<script>
// 自己的js代码
// 每一句js代码都携带分隔符 ; 但是不带也行
</script>
"""
```



## 5 JS学习路线

```python
# 变量-常量
# 基本数据类型
# 流程控制语句
# 函数
# 对象 - Python中的类
# 内置方法 和 模块
```













