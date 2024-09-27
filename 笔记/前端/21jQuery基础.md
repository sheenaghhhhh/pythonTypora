# 21 jQuery基础

## 1 jQuery基本语法

```js
jQuery(选择器).action()

// 秉承jQuery宗旨，jQuery 简写成 $
jQuery(选择器) ---->  $(选择器)
$(选择器)
```



## 2 jQuery与原生js代码比较

```js
// 1.原生
document.getElementById("d1")
document.getElementsByClassName("div_d")
document.getElementsByTagName("div")

// 2.jQuery
// css 语法 ：
// class值 ： . + class值
// id 值 ： # + id 值
// 标签名 ： 标签名
$("#d1") // document.getElementById("d1")
$(".div_d") // document.getElementsByClassName("div_d")
$("div") // document.getElementsByTagName("div")
```



## 3 基本选择器

```js
// 1.元素选择器（Element Selector）：
// 使用元素名称作为选择器，选取所有匹配该元素名称的元素。
// 示例：选择所有段落元素
$('p');

// 2.ID选择器（ID Selector）：
// 使用ID属性值作为选择器，选取具有相同ID的唯一元素。
// 示例：选择具有 "myElement" ID的元素
$('#myElement');

// 3.类选择器（Class Selector）
// 使用类名作为选择器，选取具有相同类名的元素。
// 示例：选择所有具有 "myClass" 类的元素
$('.myClass');

// 4.属性选择器（Attribute Selector）：
// 使用元素的属性和属性值进行选择。
// 示例：选择所有具有 "target" 属性的元素
$('[target]');
// 示例：选择具有 "href" 属性值以 "https://" 开头的所有链接
$('a[href^="https://"]');

// 5.选择器组合（Multiple Selectors）：
// 通过逗号分隔多个选择器，同时选择多个元素。
// 示例：选择所有段落元素和具有 "myClass" 类的元素
$('p, .myClass');

// 6.后代选择器（Descendant Selector）
// 选择某个元素的后代元素。
// 示例：选择所有 "myElement" 元素内的段落元素
$('#myElement p');

// 7.子元素选择器（Child Selector）：
// 选择某个元素的直接子元素。
// 示例：选择所有 "myElement" 元素的直接子元素中的段落元素
$('#myElement > p');

// 8.下一个兄弟元素选择器（Next Adjacent Selector）
// 选择紧接在指定元素后的下一个兄弟元素。
// 示例：选择紧接在 "myElement" 元素后的下一个兄弟元素
$('#myElement + div');

// 9.后续所有兄弟元素选择器（Following Siblings Selector）
// 选择指定元素之后的所有兄弟元素。
// 示例：选择紧接在 "myElement" 元素后的所有兄弟元素中具有 "myClass" 类的元素
$('#myElement ~ .myClass');
```



## 4 组合选择器

```js
// 在jQuery中，"组合选择器"是一种使用多个选择器来选择元素的方法。通过组合不同的选择器，可以更精确地选取需要的元素。

// 1.并集选择器（Union Selector）：
// 使用逗号分隔，可以同时选择多个选择器所匹配的所有元素。
$("selector1, selector2")

// 2.后代选择器（Descendant Selector）：
// 选择某个元素的后代元素。
$("parent descendant")

// 3.子元素选择器（Child Selector）：
// 选择某个元素的直接子元素。
$("parent > child")

// 4.相邻兄弟选择器（Adjacent Sibling Selector）：
// 选择某个元素的下一个相邻兄弟元素。
$("element + sibling")

// 5.兄弟选择器（General Sibling Selector）：
// 选择某个元素之后的所有兄弟元素。
$("element ~ siblings")

// 6.过滤选择器（Filter Selector）：
// 根据特定的条件过滤选取元素。
$("selector").filter(condition)
```



## 5 分组与嵌套

```js
// 在jQuery中，"分组"和"嵌套"也是常见的用法，用于选择和操作DOM元素。
// 1.分组（Grouping）
// 使用逗号将多个选择器组合在一起，可以同时选择匹配这些选择器的所有元素。
$('.selector1, .selector2').doSomething();

// 通过分组选择器，可以在一个操作中同时对多个元素进行相同的操作。

// 2.嵌套（Nesting）
// 在一个选择器内部嵌套另一个选择器，用于表示某个选择器的子元素或特定关系的元素。
$('parent').find('.child').doSomething();

// 通过嵌套选择器，可以在指定的父元素下选择特定的子元素进行操作。
// 嵌套选择器在jQuery中非常有用，可以通过链式调用来实现更复杂的选择和操作。
$('parent')
  .find('.child')
  .filter('.special')
  .doSomething();

// 上述代码首先选择父元素
// 然后在父元素下找到特定的子元素
// 并进一步筛选出特定的具有".special"类名的元素
// 最后对这些元素执行相应的操作。
```



## 6 基本筛选器

```js
// 1.:first
// 选择第一个匹配元素。
// 2.:last
// 选择最后一个匹配元素。
// 3.:even
// 选择索引为偶数的匹配元素（索引从开始）。
// 4.:odd
// 选择索引为奇数的匹配元素（索引从开始）。
// 5.:eq(index)
// 选择索引为指定值index的元素（索引从开始）。
// 6.:gt(index)
// 选择索引大于指定值index的元素（索引从开始）。
// 7.:lt(index)
// 选择索引小于指定值index的元素（索引从开始）。
// 8.:header
// 选择所有标题元素（例如h1、h2等）。
// 9.:animated
// 选择当前正在执行动画的元素。
// 10.:focus
// 选择当前获得焦点的元素。
// 11.:contains(text)
// 选择包含指定文本text的元素。
// 12.:empty
// 选择没有子元素或文本的空元素。
// 13.:parent
// 选择有子元素或文本的元素。
// 可以使用:even筛选器选择表格中的偶数行，并使用:contains筛选器选择包含特定文本的元素。


// 获取当前 ul 标签下的所有 li 标签
$("#ul_li li")
// 选择第一个匹配元素
$("#ul_li :first") // p1
$("#ul_li :last") // p5
// 索引从 0 开始
$("#ul_li :even")// p1 p3 p5
$("#ul_li :odd")// p2 p4
$("#ul_li :eq(1)")// p2
$("#ul_li :gt(1)")// p3 p4 p5
```



## 7 属性选择器

```js
// 按照标签的属性获取标签
// css 按照指定属性获取标签
// E[attr]
// E[attr="value"]
$("[name]")
$("div[name]")

// 1.选择具有特定属性的元素：$("[attribute]")
// 例如，选择所有具有"src"属性的图片元素：$("img[src]")

// 2.选择具有特定属性值的元素：$("[attribute=value]")
// 例如，选择所有"href"属性值为"https://example.com"的链接元素：$("a[href='https://example.com']")

// 3.选择具有包含特定属性值的元素：$("[attribute*=value]")
// 例如，选择所有"src"属性值包含"image"的图片元素：$("img[src*='image']")

// 4.选择具有以特定属性值开头的元素：$("[attribute^=value]")
// 例如，选择所有"href"属性值以"https://"开头的链接元素：$("a[href^='https://']")

// 5.选择具有以特定属性值结尾的元素：$("[attribute$=value]")
// 例如，选择所有"src"属性值以".jpg"结尾的图片元素：$("img[src$='.jpg']")

// 6.选择具有以特定属性值开头且以特定字符串结尾的元素：$("[attribute^=value][attribute$=value]")
// 例如，选择所有"src"属性值以"images/"开头且以".jpg"结尾的图片元素：$("img[src^='images/'][src$='.jpg']")
```