# 24 jQuery补充

## 1 阻止标签后续执行 和 阻止事件冒泡

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="query.min.js"></script>
</head>
<body>
<div id="d1">
    我是 div
    <p id="p1">
        我是 p
        <span id="s1">
            我是 span
        </span>
    </p>
</div>

<form action="">
    <input type="submit" id="submit">
</form>

<script>
    // 阻止事件后续执行
    $("#submit").click(function (e) {
        // return false
        // 阻止当前标签的默认行为
        e.preventDefault()
    })


    // 阻止事件冒泡
    // 在不断嵌套的标签内 会发现点击内部标签会持续触发后续的 逻辑代码

    $("#d1").click(
        function (e) {
            alert(`我出来了 ${$(this).text()}`)
            console.log(e)

        }
    )
    $("#p1").click(
        function (e) {
            alert(`我出来了 ${$(this).text()}`)
            e.stopPropagation()
        }
    )
    $("#s1").click(
        function (e) {
            alert(`我出来了 ${$(this).text()}`)
            // return false
            e.stopPropagation()
        }
    )

    // 方案一：阻止后续代码执行 在函数内部 用 return false 来告诉 js 代码后续不需要执行

    // 方案二：内置了一个方法帮助我们阻止事件后续执行
    // 在函数的参数上 有一个对象e 执行  e.stopPropagation()

</script>
</body>
</html>
```



## 3 事件委托

```js
// 针对动态创建的标签 提前写好的事件默认是无法生效的
$('body').on('事件类型','选择器',function(){})

// 将body内所有的点击事件交给button标签处理
$('body').on('click','button',function(){})
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.4/jquery.min.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.2.3/css/bootstrap-grid.min.css"></script>

</head>
<body>

<button>点我有惊喜哦!</button>

<script>
    // 创建button按钮标签
    let $buttonEle = $("<button>")
    $buttonEle.text("你放心的去吧,一切有我!")
    let bodyEle = $("body")
    bodyEle.append($buttonEle)

    // 给页面上的所有button按钮标签绑定点击事件
    // $("button").click(function () { // 无法监控动态创建的按钮标签
    //     alert("button")
    // })

    // 事件委托
    bodyEle.on("click", "button", function () {
        alert("欢迎光临") // 在指定范围内，将事件委托给某个标签，无论是该标签是事先声明的还是动态创建的
    })

</script>

</body>
</html>
```



## 4 页面加载

- jQuery中等待页面加载
- 在jQuery中，可以使用$(document).ready()方法等待页面加载完成。

```js
// 1.$(document).ready()
// $(document).ready()是一个事件处理函数，会在DOM（文档对象模型）树完全构建好后执行。 
//   它确保了JavaScript代码只有在页面完全加载后才开始执行，从而避免了由于DOM元素尚未准备好而导致的错误。

$(document).ready(function() {
  // 在这里编写需要在页面加载完成后执行的代码
  // 例如操作DOM元素、绑定事件等
});

// 你可以将你的JavaScript代码放在$(document).ready()方法内部， 
//   这样就能够确保代码在页面加载完成后执行。


// 2.$(function() { ... })
// $(document).ready()方法的一种简写形式。

$(function() {
  // 在这里编写需要在页面加载完成后执行的代码
});

// 使用$(document).ready()方法或其简化形式，可以保证你的JavaScript代码在页面加载完成后执行，从而避免因为DOM元素未准备好而导致的问题。


// 3.最暴力的方法
// 直接写在 body 标签 内部 ！
```



