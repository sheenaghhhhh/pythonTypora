# 22 jQuery进阶

## 1 操作标签

```js
// 1.JS版本
// 两种方法拿classlist
divEle = document.getElementById("d1")
classValue = divEle.getAttribute("class")
// 'name username password pwd '
classList = divEle.classList
// DOMTokenList(4) ['name', 'username', 'password', 'pwd', value: 'name username password pwd ']

// 添加一或多个类名
classList.add("class3", "class4")
// 移除一或多个类名
classList.remove("class3", "class4")
// 检查是否包含指定类名
classList.contains("name")
// 切换类名 有则删除无则添加
classList.toggle("red")

// 2.jQuery版本
// 向当前标签中增加class值
$("#d1").addClass("red")
// 在当前标签删除属性值
$("#d1").removeClass("red")
// 检测当前 class 属性是否包含某个属性
$("#d1").hasClass("red")
// 有则删除无则添加
$("#d1").toggleClass("red")
```



## 2 jQuery操作CSS

```js
// 1.添加 CSS 类名
// 您可以使用 addClass() 方法来添加一个或多个 CSS 类到元素中。

$("#myElement").addClass("myClass");
// 将在具有 id 为 "myElement" 的元素上添加 "myClass" 类。

// 2.移除 CSS 类名
// 您可以使用 removeClass() 方法来从元素中移除一个或多个 CSS 类。

$("#myElement").removeClass("myClass");
// 将从具有 id 为 "myElement" 的元素中移除 "myClass" 类。

// 3.切换 CSS 类名
// 您可以使用 toggleClass() 方法来在元素上切换一个或多个 CSS 类。
// 如果元素已经具有该类，则该方法会将其移除；否则，它会添加该类。

$("#myElement").toggleClass("myClass");
// 将在具有 id 为 "myElement" 的元素上切换 "myClass" 类。

// 4.设置 CSS 属性
// 您可以使用 css() 方法来设置元素的 CSS 属性。
// 该方法接受两个参数，第一个参数是要设置的 CSS 属性名称，第二个参数是属性的值。

$("#myElement").css("color", "red");
// 将将具有 id 为 "myElement" 的元素的颜色设置为红色。

// 5.获取 CSS 属性
// 您可以使用 css() 方法来获取元素的 CSS 属性值。
// 该方法接受一个参数，即要获取的 CSS 属性名称。

var color = $("#myElement").css("color");
// 将返回具有 id 为 "myElement" 的元素的颜色值。
```



## 3 jQuery链式操作

```js
divEle = document.getElementById("d1")
divEle.setAttribute(
    "style", "color:red;"
)

// 链式操作 一连串的操作
$("#d1").first().css("color", "red").next().css("color", "green")
```



## 4 jQuery位置操作

```js
// 控制当前浏览器窗口的滑动
$("p").offset();

// 返回第一个匹配元素的位置
$("p").position();

// 先获取到当前的浏览器对象 --- window
$(window).scroll()
// 返回或设置元素垂直方向的滚动条偏移值
$(window).scrollTop()
// 返回或设置元素水平方向的滚动条偏移值
$(window).scrollBottom()
$(window).scrollLeft()
$(window).scrollRight()

// 不写参数就是返回的默认窗口高度和宽度
// 写了就是按照指定的高度和宽度变化
$(window).height(400)
$(window).width(400)
```



## 5 jQuery尺寸操作

```js
// 1..height()和.width()
$(window).height(400)
$(window).width(400)

// 2..innerHeight()和.innerWidth()
// 获取当前对象的高度和宽度
$("#d1").css("margin", 60).css("padding", "90")
$("#d1").innerHeight()
$('#d1').innerWidth()

// 3..outerHeight()和.outerWidth()
// 获取当前对象的外高度和宽度
$("#d1").outerHeight()
$("#d1").outerWidth()

// 4..outerHeight(true) 和 .outerWidth(true)
// 用于获取元素的完整的外部高度和宽度，包括内容区域、内边距、边框和外边距
```





## 6 jQuery文本操作

```js
// 1.JS版本
dEle = document.getElementById("d1")

// 获取值
dEle.innerText
// 设置值
dEle.innerText = "666"
dEle.innerHTML

// input 操作
usernameEle = document.getElementById("username")

// 获取值
username = usernameEle.value
// 修改值
usernameEle.value = "dream"

// 向当前标签内部添加一段文本内容
dEle.append("<a href=\"\">点我有惊喜</a>")

dEle.insertBefore("<a href=\"\">点我有惊喜</a>", dEle)

// 2.jQuery版本
// 获取标签中间的文本
$("#d1").text()
// 设置标签中间的文本
$("#d1").text("999")
// 获取 标签和标签中间的 html 代码
$("#d1").html()
// 设置 标签和标签中间的 html 代码
$("#d1").html(" <a href=\"\">点我有惊喜</a>")

// 获取 input 框输入的文本
$("#username").val()
// 修改 input 框输入的文本
$("#username").val("666")

// 在当前标签内部追加一个 标准的标签 或者文本
$("#d1").append("<a href=\"\">点我有惊喜</a>")
$("#d1").append("你真棒")

// 在当前标签内部的第一个位置追加 标准的标签 或者文本
$("#d1").prepend("<a href=\"\">点我有惊喜</a>")

// 在指定标签后面追加新的标签
$("#d1").after("<a href=\"\">点我有惊喜</a>")

// 在指定标签前面追加新的标签
$("#d1").before("<a href=\"\">点我有惊喜</a>")
```



## 7 jQuery属性操作

```js
// 1.JS版本
divEle = document.getElementById("d1")

// 获取当前标签属性
divEle.getAttribute("class")

// 设置当前标签的属性值
divEle.setAttribute("name", "dream")
divEle.setAttribute("name", "dream01")
// 完全覆盖将属性值变更
divEle.setAttribute("class", "name")

// 删除当前标签的属性
divEle.removeAttribute("name")

// 2.jQuery版本
// 获取当前标签属性
$("#d1").attr("class")

// 设置当前标签的属性值
$("#d1").attr("username", "username")
$("#d1").attr("username", "username01")
// 完全覆盖将属性值变更
$("#d1").attr("class", "name")

// 删除当前标签的属性
$("#d1").removeAttr("username")
```



## 8 针对checked的特殊方法prop

```js
// 【六】针对 多选框/单选框/复选框/下拉框 的特殊方法
// 不加报错
$("#gender_male").prop()
// 加了一个参数 checked 选中的意思

// 检查当前标签是否被选中 选中就是 true
$("#gender_male").prop("checked")

// 重置当前标签选中状态
$("#gender_male").prop("checked", false)
// 选中当前标签
$("#gender_male").prop("checked", true)
```



## 9 jQuery文档处理

```js
// 1.JS版本
// 创建空的节点
divEleEmpty = document.createElement("div")
// 追加到某个节点后面
divEle = document.getElementById("d1")
divEle.appendChild(divEleEmpty)

// 2.jQuery
// 追加在前面
$("#d1").prepend("<a href=\"\">点我有惊喜1</a>").prepend("<a href=\"\">点我有惊喜1</a>")
// 追加到后面
$("#d1").append("<a href=\"\">点我有惊喜1</a>").append("<a href=\"\">点我有惊喜1</a>")
// 将当前标签追加到指定标签内部
$("#d1").appendTo($("#d2"))

// 指定追加到某个后面
$("#d1").insertAfter($("#d2"))

// 指定追加到某个前面
$("#d1").insertBefore($("#d2"))

$("#d1").before($("#d2"))

$("#d1").after($("#d2"))

// 删除当前元素及当前元素的子元素
$("#d1").remove()

// 清空当前标签内的内容
$("#d2").empty()
```





