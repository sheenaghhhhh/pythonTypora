# 17 JavaScript之Dom

## 1 什么是DOM/BOM

```js
// 【一】什么是DOM / BOM
// 1.DOM - document object model
// 文本对象模型
// ● 文档对象模型（DOM）是一个编程接口，它以树状结构来表示 HTML 或 XML 文档。
// ● 在 DOM 中，每个HTML元素、属性、文本节点等都被视为一个对象，通过JavaScript可以创建、查询、修改和删除这些对象。

// 2.BOM - browser object model
// 浏览器对象模型
// 浏览器对象模型（BOM）则是浏览器提供的API集合，主要用于处理与浏览器环境相关的任务，如窗口管理、导航、cookie、location等。

// 3.用处
// 用来和浏览器及文档对象进行交互
```



## 2 Window对象

```js
// 操作浏览器对象

// 【一】Windows对象
// 代表全局的浏览器窗口的对象操作浏览器的窗口

// 1.open
// 打开新的窗口
window.open("https://www.baidu.com")
// 第一个参数 目标链接地址
// 第二个参数 打开窗口方式
// 第三个窗口 参数设置
window.open(
    "https://www.baidu.com",
    "blank",
    "height=400px,width=400px"
)

// 2.close()
// 关闭当前窗口
window.close()

// 3.设置定时器/延时器
// (1)定时器: 到指定时间就自动执行
// 第一个参数放 延时执行的函数
// 第二个参数放 延时的时间 以毫秒为单位
// 效果就是隔执行时间自动执行
setInterval(
    function () {
        console.log("执行了!")
    },
    2000
)

// (2)延时器: 延迟指定时间后再执行
// 第一个参数 延时执行的函数
// 第二个参数 延时的时间 以毫秒为单位
// 效果 延迟 指定时间自动执行
setTimeout(
    function() {
        window.alert("我被执行!")
    },
    2000
)

// 4.删除定时器/延时器
// (1)删除定时器
// 参数是当前设置定时器时候的 定时器 ID
clearInterval(
    10
)

// (2)删除延时器
// 参数是当前设置延时器时候的 延时器 ID
clearTimeout(
    10
)

// 5.交互语句
// (1)警告框
alert("提示信息")
// (2)输入框
// 弹出一个输入框并且携带默认值如果不输入信息则返回默认值
resultPrompt = prompt("age is ", 30)
// (3)确认框
// 弹出一个确认框并且会提示信息 点击确定则返回 true 否则返回false
resultConfirm = confirm("is still loving?")

// 6.innerHeight 属性
// 获取当前浏览器窗口高度
alert(window.innerHeight)

// 7.innerWidth 属性
// 获取当前浏览器窗口的 宽度
alert(window.innerWidth)

// 8.监听 resize 事件
// 需要注意的是，这两个属性返回的值会随着窗口的大小调整而改变
// 因此如果需要监控窗口大小的改变，可以通过监听 resize 事件来实现。
window.addEventListener(
    "resize", function () {
        let height = window.innerHeight;
        let width = window.innerWidth;
        console.log(`高度是 :>>>> ${height} `)
        console.log(`宽度是 :>>>> ${width} `)
    }
)
```



## 3 Window子对象

### 3.1 location

```js
// 此子对象包含有关当前页面 URL 的信息，并提供了一些与页面导航相关的方法。
// 通过 window.location，可以获取当前页面的 URL、查询字符串参数、路径等信息，并且可以使用一些方法来导航到其他页面。
// 例如，要在新标签页中打开一个 URL

// 1.直接打印 location 对象
window.location
// Location {ancestorOrigins: DOMStringList, href: 'https://www.baidu.com/index.htm', origin: 'https://www.baidu.com', protocol: 'https:', host: 'www.baidu.com', …}

// 2.获取当前浏览器地址栏中的链接
location.href
// 'https://www.baidu.com/index.htm'
// 浏览器中看到中文 而打印的结果 中文看到了乱码
// 原因是 浏览器会对 URL 中的数据进行转码

// 3.获取当前地址的协议
location.protocol
// 'https:'

// 4.获取当前地址的域名 (IP + PORT)
location.host
// 'www.baidu.com'

// 5.获取当前地址的主机名 (IP)
location.hostname
// 'www.baidu.com'

// 6.获取到当前地址的端口 (PORT)
location.port
// ''

// 7.获取当前地址的路径参数
// 没有默认就是 /  根目录
location.pathname
// '/index.htm'

// 8.获取到当前页面的查询参数
// 获取到地址栏中 ? + ? 后面的所有参数
location.search
// '?wd=123&rsv_spt=1&rsv_iqid=0xd96150bd001c0113&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=1&rsv_dl=tb&rsv_sug3=4&rsv_sug1=3&rsv_sug7=100&rsv_sug2=0&rsv_btype=t&prefixsug=12%2526lt%253B&rsp=5&inputT=791&rsv_sug4=1801'

// 9.获取到当前地址的片段标识
location.hash
// ''

// 10.将当前地址重定向到指定的地址
location.assign("https://www.jd.com")

// 11.重载当前页面 （刷新页面）
location.reload()
```

### 3.2 history

```js
// 此子对象用于操作浏览器的历史记录，包括向前和向后导航。
// 通过 window.history，可以使用一些方法来在历史记录中向前或向后导航，以及获取当前历史记录的状态数量。

// 1.打印当前历史记录条数
// 获取到当前页面跳转链接的次数 至少是 1 次 就是当前地址
history.length

// 2.前进
history.forward()

// 3.后退
history.back()

// 4.前往指定页面
// 历史记录的标识
// 向前进
history.go(1)
// 向后退
history.go(-1)

// 5.向浏览器中添加状态
history.pushState({page: 1}, "Page 2", "/page2.html")

// 6.向浏览器中替换状态
history.replaceState({page: 1}, "Page 2", "/page2.html")
```

### 3.3 navigator

```js
// 此子对象提供有关浏览器环境和设备的信息，包括用户代理字符串、浏览器版本、操作系统等。

// 1.获取当前浏览器的标识
navigator.userAgent
// 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/129.0.0.0 Safari/537.36 Edg/129.0.0.0'

// 2.浏览器对应的平台
navigator.platform
// 'Win32'

// 3.获取当前浏览器的供应商或厂商
navigator.vendor
// 'Google Inc.'

// 4.获取当前浏览器的语言首选项
navigator.language
// 'zh-CN'

// 5.获取当前浏览器的Cookie状态
// Cookie 可以保存登陆信息
navigator.cookieEnabled
// true

// 6.获取浏览器的插件列表
navigator.plugins
// PluginArray {0: Plugin, 1: Plugin, 2: Plugin, 3: Plugin, 4: Plugin, PDF Viewer: Plugin, Chrome PDF Viewer: Plugin, Chromium PDF Viewer: Plugin, Microsoft Edge PDF Viewer: Plugin, WebKit built-in PDF: Plugin, …}
```



## 4 查找标签

### 4.1 直接查找标签 getElement 3种

```js
// 1.按照指定ID查找标签 - 返回值是一个标签对象
document.getElementById("d1")

// 2.按照标签名查找标签 - 返回值是一个数组
document.getElementsByTagName("div")

// 3.按照标签的 class 属性值获取标签 - 返回值是一个数组
document.getElementsByClassName("div_d")
```

### 4.2 半间接查找标签 query 2种

```js
// 1.查找单个标签 返回值也是一个单独的标签对象
// 如果是 类属性 .类属性值 / ID值 #id值 / 标签名
document.querySelector(".div_d")
document.querySelector("#d1")
document.querySelector("div")

// 2.查询到所有符合条件的标签
// 如果是 类属性 .类属性值 / ID值 #id值 / 标签名
document.querySelectorAll(".div_d")
document.querySelectorAll("#d1")
document.querySelectorAll("div")
```

### 4.3 间接查找标签 6种

```js
// 1.返回执行元素的父元素 .parentElement
pEle = document.getElementById("p1") // p1
parentEle = pEle.parentElement // d1

// 2.返回当前标签的子元素 .children
divEle = document.getElementById("d1") // d1
sonEles = divEle.children // p1 p2 p3

// 3.返回当前子元素的第一个子元素 .firstElementChild / 最后一个子元素 .lastElementChild
divEle = document.getElementById("d1") // d1
sonEleOne = divEle.firstElementChild // p1
sonEleLast = divEle.lastElementChild // p3

// 4.返回当前元素的下一个元素 .nextElementSibling / 上一个元素 .previousElementSibling
pEle = document.getElementById("p2") // p2
pEleNext = pEle.nextElementSibling // p3
pElePerious = pEle.previousElementSibling // p1
```



## 5 节点操作

```js
// 1.创建节点
// 创建一个指定标签的空节点
divEleEmpty = document.createElement("div") // 创建空的div
// 空文本节点对象
textEle = document.createTextNode("这是一段文本") // 创建空的文本
// 创建一个空的片段节点
frgEle = document.createDocumentFragment() // 创建空的代码片段

// 2.获取节点
document.getElementById(id)
document.getElementsByTagName(tagName)
document.getElementsByClassName(className)
document.querySelector(selector)
document.querySelectorAll(selector)

// 3.修改节点
// 在指定节点后追加子节点
pEle = document.getElementById("p3") // p3
pEle.appendChild(divEle) // p3 里面加了一个 空的div 子元素
// 移除指定节点 --- 从父节点中移除指定节点
pEle.removeChild(divEle) // p3 内部移除 空的div子元素
// 替换指定节点 --- 用新节点去替换指定的节点
// replaceChild(新节点,旧节点)
divEle = document.getElementById("d1") // d1
divEle.replaceChild(divEleEmpty, pEle) // 在d1 内部将 p3 替换成  空的div 子元素
// 插入指定节点 -- 在指定节点后面追加节点
pEleEmpty = document.createElement("p")
pEle = document.getElementById("p2") // p2
divEle = document.getElementById("d1") // d1
divEle.insertBefore(pEleEmpty,pEle) // 在 div 标签内部的 p2 标签前面追加一个新的标签 创建的空的 p标签

// 4.属性操作
// 修改指定标签的属性值
imgEle = document.createElement("img")
// 向标签中增加属性
// src ---> 指向图片地址
// https://pic.netbian.com/uploads/allimg/240924/182402-17271734427f37.jpg
imgEle.setAttribute("src","https://pic.netbian.com/uploads/allimg/240924/182402-17271734427f37.jpg")
divEle = document.getElementById("d1") // d1
divEle.appendChild(imgEle)
// 在指定标签中获取属性
imgEle.getAttribute("src")
// 移除指定标签属性
imgEle.removeAttribute("src")

// 5.文本操作
// 获取指定节点的文本内容或者设置指定节点的文本内容
divEle = document.getElementById("d1") // d1
// 获取当前节点下的所有文本内容
divEle.innerText
// 取当前节点下的所有文本内容
divEle.textContent
// 不仅能够取当前节点下的所有文本内容 还能够获取到当前标签下的所有子标签的标签代码
divEle.innerHTML
```



## 6 获取值操作

```js
// 1.获取属性值
divEle = document.getElementById("d1")
// 获取标签的属性值
value = divEle.getAttribute("属性名")

// 2.获取文本内容
// 通过innerText、textContent、innerHTML来获取文本内容
// 返回的是纯文本的表掐内容 不包括HTML 代码
divEle.innerText
// 返回当前元素及子元素的文本内容 包括 隐藏元素或者注释
divEle.textContent
// 返回元素的文本内容及当前标签下的子标签的 HTML 代码
divEle.innerHTML

// 3.获取用户输入值
// 通过和form表单交互
// 在前端有 form 表单 但是我们校验数据 ---> 发送给后端去校验数据
// 必须在前端能够获取到所有的数据
usernameEle = document.getElementById("username")
usernameEle.value;

// 获取到多选框的标签值
hobbyEles = document.getElementsByClassName("hobby")
for (e of hobbyEles) {
    console.log(e.value)
}

// 4.获取文件选择框
fileEle = document.getElementById("file")
fileObj = fileEle.files[0]
fileObj.name
```



## 7 属性操作

```js
divEle = document.getElementById("d1")

// 1.方式一：获取属性值并切分
classValue = divEle.getAttribute("class") // 'div_d name select dream'
classValue.split(" ") // ['div_d', 'name', 'select', 'dream']

// 2.方式二：获取 class 列表
// 直接将获取到的 class 属性值切分成数组
classValue = divEle.classList

// 通过数组对象操作 class 属性值
// 添加类属性
classValue.add("ooo")
// 删除
classValue.remove("ooo")
// 切换类属性 --- 有则删除无则添加
classValue.toggle("abc")
```



## 8 事件

```js
// 当达到某个条件的时候会自动触发的代码就叫事件
// 1.鼠标事件
// ● click：鼠标点击事件。
// ● mouseover：鼠标悬停在元素上的事件。
// ● mouseout：鼠标离开元素的事件。
// ● mousedown：鼠标按下事件。
// ● mouseup：鼠标松开事件。
// ● mousemove：鼠标移动事件。

// 2.键盘事件
// ● keydown：键盘按下事件。
// ● keyup：键盘松开事件。
// ● keypress：键盘按键被按下并松开事件。

// 3.表单事件
// ● submit：表单提交事件。
// ● change：表单值改变事件。
// ● focus：表单元素获取焦点事件。
// ● blur：表单元素失去焦点事件。

// 4.文档加载事件
// ● load：页面完全加载完成事件。
// ● unload：页面关闭或离开事件。

// 5.定时器事件
// ● setTimeout：在指定的延迟时间后触发事件。
// ● setInterval：每隔一定时间触发事件。
```



## 11 绑定事件的两种方式

```js
// 1.方式一：传统添加
usernameEle = document.getElementById("username")
usernameEle.addEventListener(
    "focus", function () {
        usernameEle.setAttribute(
            "style", "border : 3px solid red"
        )
    }
)

usernameEle.addEventListener(
    "blur", function () {
        usernameEle.setAttribute(
            "style", "background-color: blue"
        )
    }
)

// 2.方式二：提前定义事件 绑定给某个标签
/*
 <input type="text" id="username" name="username" onfocus="focusMethod()" onblur="blurMethod()">
* */
usernameEle = document.getElementById("username")

function focusMethod() {
    usernameEle.setAttribute(
        "style", "border : 3px solid red"
    )
}

function blurMethod() {
    usernameEle.setAttribute(
        "style", "background-color: blue"
    )
}
```



## 12 task

```js
// 【一】开关灯
// 在页面上有一个○内部原本是红色 -- 没开灯
// 在页面上还有一个按钮 控制开关灯
// 点一下 变成绿色 -- 开灯 再点一下 红色 -- 关灯

// 【二】输入框聚焦和失去焦点
// 在用户点击输入框的时候会将边框样式更改
// 在用户离开输入框的时候也会自动增加样式

// 【三】二级菜单
/*
1
    1-1
    1-2
    1-3
2
    2-1
    2-2
    2-3
3
    3-1
    3-2
    3-3
* */

// 点击 1 的时候
/*
1
    1-1
    1-2
    1-3
2
3
* */

// 点击2 的时候
/*
1
2
    2-1
    2-2
    2-3
3
* */
```

