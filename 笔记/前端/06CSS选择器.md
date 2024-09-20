# 06 CSS选择器

## 1 分组与嵌套

### 1.1 分组

```css
selector1, selector2, selector3 {
    property: value;
}
```

```css
h1, h2, p {
    color: blue;
    font-family: Arial, sans-serif;
}
```

### 1.2 嵌套

```css
parentSelector childSelector {
    property: value;
}
```

```css
div p {
    color: red;
}
```

即前面的后代选择器



## 2 伪类选择器

```python
# 选择符	版本	描述
# E:link	CSS1	设置超链接a在未被访问前的样式。
# E:visited	CSS1	设置超链接a在其链接地址已被访问过时的样式。
# E:hover	CSS1/2	设置元素在其鼠标悬停时的样式。
# E:active	CSS1/2	设置元素在被用户激活（在鼠标点击与释放之间发生的事件）时的样式。
# E:focus	CSS1/2	设置元素在成为输入焦点（该元素的onfocus事件发生）时的样式。
# E:lang(fr)	CSS2	匹配使用特殊语言的E元素。
# E:not(s)	CSS3	匹配不含有s选择符的元素E。
# E:root	CSS3	匹配E元素在文档的根元素。
# E:first-child	CSS2	匹配父元素的第一个子元素E。
# E:last-child	CSS3	匹配父元素的最后一个子元素E。
# E:only-child	CSS3	匹配父元素仅有的一个子元素E。
# E:nth-child(n)	CSS3	匹配父元素的第n个子元素E。
# E:nth-last-child(n)	CSS3	匹配父元素的倒数第n个子元素E。
# E:first-of-type	CSS3	匹配父元素下第一个类型为E的子元素。
# E:last-of-type	CSS3	匹配父元素下的所有E子元素中的倒数第一个。
# E:only-of-type	CSS3	匹配父元素的所有子元素中唯一的那个子元素E。
# E:nth-of-type(n)	CSS3	匹配父元素的第n个子元素E。
# E:nth-last-of-type(n)	CSS3	匹配父元素的倒数第n个子元素E。
# E:empty	CSS3	匹配没有任何子元素（包括text节点）的元素E。
# E:checked	CSS3	匹配用户界面上处于选中状态的元素E。(用于input type为radio与checkbox时)
# E:enabled	CSS3	匹配用户界面上处于可用状态的元素E。
# E:disabled	CSS3	匹配用户界面上处于禁用状态的元素E。
# E:target	CSS3	匹配相关URL指向的E元素。
# @page:first	CSS2	设置页面容器第一页使用的样式。仅用于@page规则
# @page:left	CSS2	设置页面容器位于装订线左边的所有页面使用的样式。仅用于@page规则
# @page:right	CSS2	设置页面容器位于装订线右边的所有页面使用的样式。仅用于@page规则


# 【需要记住的】
# 访问连接相关的一组
# # E:link	CSS1	设置超链接a在未被访问前的样式。
# # E:visited	CSS1	设置超链接a在其链接地址已被访问过时的样式。
# # E:hover	CSS1/2	设置元素在其鼠标悬停时的样式。
# # E:active	CSS1/2	设置元素在被用户激活（在鼠标点击与释放之间发生的事件）时的样式。
# # E:focus	CSS1/2	设置元素在成为输入焦点（该元素的onfocus事件发生）时的样式

# 子元素
# # E:first-child	CSS2	匹配父元素的第一个子元素E。
# # E:last-child	CSS3	匹配父元素的最后一个子元素E。
# # E:only-child	CSS3	匹配父元素仅有的一个子元素E。
# # E:nth-child(n)	CSS3	匹配父元素的第n个子元素E。

# 状态
# # E:empty	CSS3	匹配没有任何子元素（包括text节点）的元素E。
# # E:checked	CSS3	匹配用户界面上处于选中状态的元素E。(用于input type为radio与checkbox时)
# # E:enabled	CSS3	匹配用户界面上处于可用状态的元素E。
# # E:disabled	CSS3	匹配用户界面上处于禁用状态的元素E。
```



## 3 伪元素选择器 

```python
# 选择符	版本	描述
# E:first-letter/E::first-letter	CSS1/3	设置对象内的第一个字符的样式。
# E:first-line/E::first-line	CSS1/3	设置对象内的第一行的样式。
# E:before/E::before	CSS2/3	设置在对象前（依据对象树的逻辑结构）发生的内容。用来和content属性一起使用
# E:after/E::after	CSS2/3	设置在对象后（依据对象树的逻辑结构）发生的内容。用来和content属性一起使用
# E::placeholder	CSS3	设置对象文字占位符的样式。
# E::selection	CSS3	设置对象被选择时的颜色。
```



## 4 选择器优先级

- **!important** **> 行内 style 属性** **>** **行内选择器 > id选择器 > 类选择器 > 标签选择器**
- 精确度越高，优先级越高

- `!important` 是一个 **CSS 优先级规则**，它用于强制应用某条 CSS 样式，即使有其他优先级更高的规则。
