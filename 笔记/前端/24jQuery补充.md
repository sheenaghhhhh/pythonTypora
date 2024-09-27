# 24 jQuery补充

## 1 阻止标签后续执行

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





## 2 阻止事件冒泡



## 3 事件委托



## 4 页面加载



## 5 jQuery中的动画效果



## 6 补充