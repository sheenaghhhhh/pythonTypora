# 23 jQuery事件

## 1 jQuery绑定事件的两种方式



## 2 克隆事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./query.min.js"></script>

</head>
<body>
<p>
    克隆事件：当我点击页面上的某个按钮的时候会对当前某个标签进行复制
</p>

<button id="btn">
    是兄弟就来砍我!
</button>

<script>
    // 在写 js 绑定的事件的时候建议 用 function 来写
    // function 内部的 this 代表的是当前的 标签对象
    // 箭头函数 写 内部的 this 就是 window 对象

    // $(document).ready 等待当前页面文档对象加载完毕后再执行里面的代码
    // 在 里面写 多个事件 和操作的时候要加 , 分隔开 否则会报错
    $(document).ready(
        $("#btn").on(
            "click",
            function () {
                // 克隆上面的标签
                // clone 默认情况下只克隆 html 和 css 样式 但是不克隆事件本身
                // 加了 true 就可以克隆 自己的事件
                console.log(this)
                $(this).clone(true).insertAfter($("body"))
            }
        )
    )


</script>
</body>
</html>
```





## 3 自定义模态框

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./query.min.js"></script>
    <style>
        .cover {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(128, 128, 128, 0.4);
            z-index: 99;
        }

        .modal {
            position: fixed;
            height: 400px;
            width: 400px;
            background-color: white;
            top: 50%;
            left: 50%;
            margin-left: -300px;
            margin-top: -200px;
            z-index: 100;

        }

        .hide {
            display: none;
        }

    </style>
</head>
<body>

<div>我是最下面的</div>

<button id="d1">点我登陆</button>

<div class="cover hide"></div>
<div class="modal hide">
    <p>username:<input type="text"></p>
    <p>password:<input type="password"></p>
    <input type="button" value="确认">
    <input type="button" value="取消" id="d2">
</div>

<script>
    let $coverEle = $(".cover");
    let $modalEle = $(".modal");

    // 去除hide属性
    $("#d1").click(function () {
        // 将两个div标签的hide属性移除
        $coverEle.removeClass("hide");
        $modalEle.removeClass("hide");
    })

    // 绑定 hide属性
    $('#d2').on('click', function () {
        $coverEle.addClass("hide");
        $modalEle.addClass("hide");
    })

</script>

</body>
</html>
```





## 4 左侧菜单

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.4/jquery.min.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.2.3/css/bootstrap-grid.min.css"></script>
    <style>

        .left {
            float: left;
            background-color: darkgray;
            width: 20%;
            height: 100%;
            position: fixed;
        }

        .title {
            font-size: 24px;
            color: white;
            text-align: center;
        }

        .items {
            border: 1px solid blue;
        }

        .hide {
            display: none;
        }

    </style>
</head>
<body>
<div class="left">
    <dic class="menu">
        <div class="title">菜单一
            <div class="items">1111</div>
            <div class="items">2222</div>
            <div class="items">3333</div>
        </div>

        <div class="title">菜单二
            <div class="items">1111</div>
            <div class="items">2222</div>
            <div class="items">3333</div>
        </div>

        <div class="title">菜单三
            <div class="items">1111</div>
            <div class="items">2222</div>
            <div class="items">3333</div>
        </div>

        <div class="title">菜单四
            <div class="items">1111</div>
            <div class="items">2222</div>
            <div class="items">3333</div>
        </div>
    </dic>

</div>


<script>
    $('.title').click(function () {
        // 先给所有的items 加 hidden
        $('.items').addClass('hide');
        // 再将被点击的 items 标签内部的hidden移除
        $(this).children().removeClass('hide');
    })
</script>


</body>
</html>
```



## 5 返回顶部





## 6 自定义登录校验





## 7 input框实时监控





## 8 hover事件





## 9 键盘按键监控事件









