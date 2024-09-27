# 20 jQuery引入

## 1 什么是jQuery

```js
// 1.概述
// jQuery是一个轻量级的、兼容多浏览器的JavaScript库。
// jQuery使用户能够更方便地处理HTML Document、Events、实现动画效果、方便地进行Ajax交互，能够极大地简化JavaScript编程。它的宗旨就是: "Write less, do more."

// 2.小结
// jQuery内部封装了原生的js代码
//   核心代码也就几十KB 加载速度快 极大的提升编写效率
// 能够通过书写更少的代码，完成js操作，类似于Python中的模块
// 前端叫 “类库”
// 兼容多个浏览器
//   IE浏览器:很多时候针对IE浏览器前端需要重新写一份代码操作
// 宗旨
//   "Write less, do more."
// Ajax交互
//   异步提交 局部刷新(django部分再学)
```



## 2 导入模块

```js
// 在Python中导入模块发生了哪些事
// 1.预编译
// 将Python代码文件转换为字节码文件
// 存储到内存中
// 2.运行
// 搜索模块 ： 校验当前这个模块是否存在
// 编译模块 ： 找到模块中的代码并将代码加载到内存中
// 创建模块名称空间 ： 将模块中的所有代码跑一遍生成一个 当前模块的名称空间
// 加载名称空间 ： 在当前调用模块的文件中将 模块的名称空间加载到自己的名称空间中
// 执行代码 ： 运行调用模块中的代码

// jQuery做了什么事
// 加载jQuery文件
// 使用jQuery代码 ----> jQuery 代码已经帮封装好了 复杂的 js 代码所以只需要直接用就可以了
```



## 3 学习的内容

```js
// 选择器
// 筛选器
// 样式操作
// 文本操作
// 属性操作
// 文档处理
// 事件
// 动画效果
// 插件
// each、data、Ajax
```



## 4 jQuery代码加载方式

```js
// 1.jQuery官网
// https://jquery.com/

// 2.如何使用
// (1)基于CDN链接分发 （不给提示）
// 用这个版本 包括 ajax 和 effects 模块
// https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.js // 没有被压缩的版本 比较大
// https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.min.js // 压缩后的版本 小一点
// https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.min.map // 不用看

// slim 版本是一个较小的版本，不包括 ajax 和 effects 模块
// https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.slim.js
// https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.slim.min.js
// https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.slim.min.map // 不用看

/*
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
* */

// (2)方式二：基于本地文件 （本地文件会给提示）
// 基于CDN的链接是由网络延迟的网络不好就会加载失败
// 于是我们使用本地的文件也可以
// 直接访问地址：https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.min.js
// 你会看到有很多源码 ctrl + a 全选 ctrl + c 复制
// 在本地创建一个 jquery.min.js 文件 代码复制进去
/*
<script src="./jquery.min.js"></script>
* */
```

