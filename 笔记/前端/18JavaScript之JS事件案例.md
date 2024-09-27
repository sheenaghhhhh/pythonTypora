# 18 JavaScript之JS事件

## 1 开关灯示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        
        
        .c1 {
            height: 400px;
            width: 400px;
            border-radius: 50%;
        }

        .bd_green {
            background-color: green;
        }

        .bd_red {
            background-color: red;
        }


    </style>
</head>
<body>
<div id="d1" class="c1 bd_green bd_red">

</div>

<button id="d2">变色</button>

<script>
    // 找到 按钮标签
    let btnEle = document.getElementById("d2")
    
    // 找到需要变色的标签
    let divEle = document.getElementById("d1")

    // 绑定点击事件
    btnEle.onclick = function () {
        // 动态的修改 div 标签的类属性
        divEle.classList.toggle("bd_red")
    }
</script>
</body>
</html>
```



## 2 input框获取/失去焦点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="text" value="去吗?" id="d1">


<script>
    let inputEle = document.getElementById("d1");

    // 获取焦点事件
    inputEle.onfocus = function (){
        // 将input框内部的值去除
        inputEle.value = '';
        // .value : 获取框内的值
        // = ：赋值

    }


    // 失去焦点事件
    inputEle.onblur = function (){
        // 给input框内的标签重写赋值
        inputEle.value = "再说"
    }


</script>
</body>
</html>
```



## 3 实时展示当前时间

### 3.1 基础版本

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<input type="text" id="d1">

<script>
    // 拿到 input 框所在的位置
    let inputEle = document.getElementById("d1");

    // （1） 访问页面之后，将访问的时间显示到input框当中
    function showTime() {
        // 声明当前时间对象
        let currentTime = new Date();
        // 将时间添加到input框内
        inputEle.value = currentTime.toLocaleString();
    }

    // （2）加载完页面触发事件：打印当前时间
    showTime()
    
</script>

</body>
</html>
```



### 3.2 升级版本

设置触发事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<input type="text" id="d1">

<script>
    // 拿到 input 框所在的位置
    let inputEle = document.getElementById("d1");

    // （1）  访问页面之后，将访问的时间显示到input框当中
    // （2）  动态展示时间
    function showTime() {
        // 声明当前时间对象
        let currentTime = new Date();
        // 将时间添加到input框内
        inputEle.value = currentTime.toLocaleString();
    }

    // 触发事件：打印当前时间
    setInterval(showTime, 100)

</script>

</body>
</html>
```



### 3.3 BUG版本

开启多个定时器时，无法终结每个定时器，只能终结最后定的那个

```html
<!--<!DOCTYPE html>-->
<!--<html lang="en">-->
<!--<head>-->
<!--    <meta charset="UTF-8">-->
<!--    <title>Title</title>-->
<!--</head>-->
<!--<body>-->

<!--<input type="text" id="d1">-->

<!--<script>-->
<!--    // 拿到 input 框所在的位置-->
<!--    let inputEle = document.getElementById("d1");-->

<!--    // （1） 访问页面之后，将访问的时间显示到input框当中-->
<!--    function showTime() {-->
<!--        // 声明当前时间对象-->
<!--        let currentTime = new Date();-->
<!--        // 将时间添加到input框内-->
<!--        inputEle.value = currentTime.toLocaleString();-->
<!--    }-->

<!--    // 触发事件：打印当前时间-->
<!--    showTime()-->

<!--</script>-->

<!--</body>-->
<!--</html>-->


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<input type="text" id="d1" style="display: block;height: 50px;width: 200px;">
<button id="b1">开始计时</button>
<button id="b2">结束计时</button>


<script>

    // 定义一个全局标量存储定时器
    let t = null;


    // 拿到 input 框所在的位置
    let inputEle = document.getElementById("d1");

    // 拿到两个按钮所在的标签
    let startEle = document.getElementById("b1");
    let endEle = document.getElementById("b2");


    // （1）  访问页面之后，将访问的时间显示到input框当中
    // （2）  动态展示时间
    // （3）  页面上加两个控制按钮。开始和结束
    function showTime() {
        // 声明当前时间对象
        let currentTime = new Date();
        // 将时间添加到input框内
        inputEle.value = currentTime.toLocaleString();
    }

    startEle.onclick = function () {
        // 触发事件：打印当前时间
        t = setInterval(showTime, 100)
    }

    endEle.onclick = function () {
        clearInterval(t);
    }

</script>

</body>
</html>
```

解决上述问题

新问题：结束定时器，无法开始

```html
<!--<!DOCTYPE html>-->
<!--<html lang="en">-->
<!--<head>-->
<!--    <meta charset="UTF-8">-->
<!--    <title>Title</title>-->
<!--</head>-->
<!--<body>-->

<!--<input type="text" id="d1">-->

<!--<script>-->
<!--    // 拿到 input 框所在的位置-->
<!--    let inputEle = document.getElementById("d1");-->

<!--    // （1） 访问页面之后，将访问的时间显示到input框当中-->
<!--    function showTime() {-->
<!--        // 声明当前时间对象-->
<!--        let currentTime = new Date();-->
<!--        // 将时间添加到input框内-->
<!--        inputEle.value = currentTime.toLocaleString();-->
<!--    }-->

<!--    // 触发事件：打印当前时间-->
<!--    showTime()-->

<!--</script>-->

<!--</body>-->
<!--</html>-->


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<input type="text" id="d1" style="display: block;height: 50px;width: 200px;">
<button id="b1">开始计时</button>
<button id="b2">结束计时</button>


<script>

    // 定义一个全局标量存储定时器
    let t = null;


    // 拿到 input 框所在的位置
    let inputEle = document.getElementById("d1");

    // 拿到两个按钮所在的标签
    let startEle = document.getElementById("b1");
    let endEle = document.getElementById("b2");


    // （1）  访问页面之后，将访问的时间显示到input框当中
    // （2）  动态展示时间
    // （3）  页面上加两个控制按钮。开始和结束
    function showTime() {
        // 声明当前时间对象
        let currentTime = new Date();
        // 将时间添加到input框内
        inputEle.value = currentTime.toLocaleString();
    }

    startEle.onclick = function () {
        if (!t) {
            // 触发事件：打印当前时间
            t = setInterval(showTime, 100);// 每点击一次生成一个定时器，但是t值复制给最后一个定时器
        }
    }

    endEle.onclick = function () {
        clearInterval(t);
    }

</script>

</body>
</html>
```



### 3.4 改善版本

```html
<!--<!DOCTYPE html>-->
<!--<html lang="en">-->
<!--<head>-->
<!--    <meta charset="UTF-8">-->
<!--    <title>Title</title>-->
<!--</head>-->
<!--<body>-->

<!--<input type="text" id="d1">-->

<!--<script>-->
<!--    // 拿到 input 框所在的位置-->
<!--    let inputEle = document.getElementById("d1");-->

<!--    // （1） 访问页面之后，将访问的时间显示到input框当中-->
<!--    function showTime() {-->
<!--        // 声明当前时间对象-->
<!--        let currentTime = new Date();-->
<!--        // 将时间添加到input框内-->
<!--        inputEle.value = currentTime.toLocaleString();-->
<!--    }-->

<!--    // 触发事件：打印当前时间-->
<!--    showTime()-->

<!--</script>-->

<!--</body>-->
<!--</html>-->


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<input type="text" id="d1" style="display: block;height: 50px;width: 200px;">
<button id="b1">开始计时</button>
<button id="b2">结束计时</button>


<script>

    // 定义一个全局标量存储定时器
    let t = null;


    // 拿到 input 框所在的位置
    let inputEle = document.getElementById("d1");

    // 拿到两个按钮所在的标签
    let startEle = document.getElementById("b1");
    let endEle = document.getElementById("b2");


    // （1）  访问页面之后，将访问的时间显示到input框当中
    // （2）  动态展示时间
    // （3）  页面上加两个控制按钮。开始和结束
    function showTime() {
        // 声明当前时间对象
        let currentTime = new Date();
        // 将时间添加到input框内
        inputEle.value = currentTime.toLocaleString();
    }

    startEle.onclick = function () {
        if (!t) {
            // 触发事件：打印当前时间
            t = setInterval(showTime, 100);// 每点击一次生成一个定时器，但是t值复制给最后一个定时器
        }
    }

    endEle.onclick = function () {
        clearInterval(t);
        // 重置为空
        t = null;
    }

</script>

</body>
</html>
```



## 4 省市联动

后者会根据前者展示对应的内容

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<select name="" id="s1">
    <option value="" selected disabled>--请选择--</option>
</select>
<select name="" id="s2"></select>

<script>
    // 模拟数据
    data = {
        "河北省": ["廊坊", "邯郸", "保定"],
        "北京": ["朝阳区", "海淀区", "遵化市"],
        "山东": ["威海市", "烟台市"],
        "台湾": ["台北市", "高雄市"],
        "香港": ["中西区", "东区"],
        "江西": ["南昌市", "九江市"],
        "黑龙江": ["哈尔滨市", "齐齐哈尔市"]
    };


    // 拿到两个select对象
    let proEle = document.getElementById("s1");
    let cityEle = document.getElementById("s2");

    // for 循环 取键
    for (let key in data) {
        // 将省做成一个个标签，添加到第一个select中
        // (1) 创建option标签
        let opEle = document.createElement("option");
        // (2) 设置属性
        opEle.innerText = key
        // (3) 设置值
        opEle.value = key
        // ----->  <option value="省">省</option>

        // (4) 将创建好的标签添加到第一个选项框中
        proEle.appendChild(opEle)
    }


    // 文本域变化事件 --- change 事件
    proEle.onchange = function () {
        // （1） 先获取到用户选择的省份
        let currentPro = proEle.value;
        // (2) 获取对应的市
        let currentCityList = data[currentPro];
        // 清空市中全部的信息
        cityEle.innerHTML = '';
        // 加一个请选择框
        let oppEle = '<option selected disabled>--请选择--</option>'

        cityEle.innerHTML = oppEle

        // (3) for 循环 将所有的市渲染到第二个标签
        for (let i = 0; i < currentCityList.length; i++) {

            let currentCity = currentCityList[i];

            // (1) 创建option标签
            let proEle = document.createElement("option");
            // (2) 设置属性
            proEle.innerText = currentCity
            // (3) 设置值
            proEle.value = currentCity
            // ----->  <option value="省">省</option>

            // (4) 将创建好的标签添加到第一个选项框中
            cityEle.appendChild(proEle)
        }
    }

</script>
</body>
</html>
```

