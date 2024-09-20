# 07 CSS属性

## 1 长度和宽度

````python
# 【一】属性概览
# 属性	版本	继承性	描述
# width	CSS1	无	定义了元素内容区（Content Area）的宽度
# min-width	CSS2	无	定义了元素内容区（Content Area）的最小宽度
# max-width	CSS2	无	定义了元素内容区（Content Area）的最大宽度
# height	CSS1	无	定义了元素内容区（Content Area）的高度
# min-height	CSS2	无	定义了元素内容区（Content Area）的最小高度
# max-height	CSS2	无	定义了元素内容区（Content Area）的最大高度

# 用来定义当前区域高度和宽度

# 【二】注意事项
# 1.适用范围
# 除非置换内联元素，table-row, table-row-group之外的所有元素
# 内联元素 ： 我们看到的行内标签

# 2.取值
# auto：无特定宽度值，取决于其它属性值
# <length>： 用长度值来定义宽度。不允许负值
# <percentage>：用百分比来定义宽度。百分比参照包含块宽度。不允许负值
````



## 2 字体属性

```python
# 属性	版本	继承性	描述
# font	CSS1/2	有	简写属性。定义元素的文本特性
# font-style	CSS1	有	指定元素的文本是否为斜体
# font-variant	CSS1	有	定义元素的文本是否为小型的大写字母
# font-weight	CSS1	有	定义元素文本字体的粗细
# font-size	CSS1	有	定义元素的字体大小
# font-family	CSS1	有	定义元素文本的字体名称序列
# font-stretch	CSS3	有	定义元素的文字是否横向拉伸变形
# font-size-adjust	CSS3	有	定义小写字母x的高度与对象文字字号的比率。
```



## 3 文本属性

```python
# 属性	版本	继承性	描述
# text-transform	CSS1/3	有	定义元素的文本如何转换大小写
# white-space	CSS1	有	指定元素是否保留文本间的空格、换行；指定文本超过边界时是否换行。
# tab-size	CSS3	有	定义元素内容中制表符的长度
# word-break	CSS3	有	定义元素内容文本的字间与字符间的换行行为
# word-wrap/overflow-wrap	CSS3	有	定义元素内容文本遇到边界时如何换行
# text-align	CSS1/3	有	定义元素内容的水平对齐方式
# text-align-last	CSS3	有	定义块内文本内容的最后一行（包括块内仅有一行文本的情况，这时既是第一行也是最后一行）或者被强制打断的行的对齐方式
# text-justify	CSS3	有	定义使用什么方式实现文本内容两端对齐
# word-spacing	CSS1/3	有	指定单词之间的额外间隙
# letter-spacing	CSS1/3	有	指定字符之间的额外间隙
# text-indent	CSS1/3	有	定义块内文本内容的缩进
# vertical-align	CSS1/2	无	定义行内元素在行框内的垂直对齐方式
# line-height	CSS1	有	定义元素中行框的最小高度
# text-size-adjust	CSS3	有	定义移动端页面中元素文本的大小如何调整

# # text-transform	CSS1/3	有	定义元素的文本如何转换大小写
'''
none：
无转换
capitalize：
将每个单词的第一个字母转换成大写
uppercase：
将每个单词转换成大写
lowercase：
将每个单词转换成小写
full-width：
将所有字符转换成fullwidth形式。如果字符没有相应的fullwidth形式，将保留原样。这个值通常用于排版拉丁字符和数字等表意符号。（CSS3）
'''

# # text-align	CSS1/3	有	定义元素内容的水平对齐方式
'''
left：内容左对齐。
center：内容居中对齐。
right：内容右对齐。
justify：内容两端对齐，但对于强制打断的行（被打断的这一行）及最后一行（包括仅有一行文本的情况，因为它既是第一行也是最后一行）不做处理。（CSS3）
start：内容对齐开始边界。（CSS3）
end：内容对齐结束边界。（CSS3）
match-parent：这个值和inherit表现一致，只是该值继承的start或end关键字是针对父母的direction值并计算的，计算值可以是 left 和 right 。（CSS3）
justify-all：效果等同于justify，不同的是最后一行也会两端对齐。（CSS3）
'''

# # text-align-last	CSS3	有	定义块内文本内容的最后一行（包括块内仅有一行文本的情况，这时既是第一行也是最后一行）或者被强制打断的行的对齐方式

# # text-justify	CSS3	有	定义使用什么方式实现文本内容两端对齐
'''
auto：
允许浏览器用户代理确定使用的两端对齐法则。
none：
禁止两端对齐。
inter-word：
通过增加字之间的空格对齐文本。该行为是对齐所有文本行最快的方法，它的两端对齐行为对段落的最后一行无效。
inter-ideograph：
为表意字文本提供完全两端对齐，增加或减少表意字和词间的空格。
inter-cluster：
调整文本无词间空格的行。这种模式的调整是用于优化亚洲语言文档的
distribute：
通过增加或减少字或字母之间的空格对齐文本，适用于东亚文档，尤其是泰国。
kashida：
通过拉长选定点的字符调整文本。这种调整模式是特别为阿拉伯脚本语言提供的。需要IE5.5+支持
'''
```



## 4 背景属性

```python
# background	CSS1/3	无	简写属性。定义元素的背景特性

# background-color	CSS1	无	定义元素使用的背景颜色
'''
<color>：指定颜色。
'''

# background-image	CSS1/3	无	定义元素使用的背景图像
'''

'''
# background-repeat	CSS1/3	无	定义元素的背景图像如何填充
'''
repeat-x：背景图像在横向上平铺
repeat-y：背景图像在纵向上平铺
repeat：背景图像在横向和纵向平铺
no-repeat：背景图像不平铺
round：当背景图像不能以整数次平铺时，会根据情况缩放图像。（CSS3）
space：当背景图像不能以整数次平铺时，会用空白间隙填充在图像周围。（CSS3）
'''
# background-attachment	CSS1/3	无	定义滚动时背景图像相对于谁固定
'''
fixed：
背景图像相对于视口（viewport）固定。
scroll：
背景图像相对于元素固定，也就是说当元素内容滚动时背景图像不会跟着滚动，因为背景图像总是要跟着元素本身。但会随元素的祖先元素或窗体一起滚动。
local：
背景图像相对于元素内容固定，也就是说当元素随元素滚动时背景图像也会跟着滚动，因为背景图像总是要跟着内容。（CSS3）
'''
# background-position	CSS1/3	无	指定背景图像在元素中出现的位置
'''
<percentage>：
用百分比指定背景图像在元素中出现的位置。可以为负值。参考容器尺寸减去背景图尺寸进行换算。
<length>：
用长度值指定背景图像在元素中出现的位置。可以为负值。
center：
背景图像横向或纵向居中。
left：
背景图像从元素左边开始出现。
right：
背景图像从元素右边开始出现。
top：
背景图像从元素顶部开始出现。
bottom：
背景图像从元素底部开始出现。
'''
# background-origin	CSS3	无	指定的背景图像计算background-position时的参考原点(位置)
# background-clip	CSS3	无	指定对象的背景图像向外裁剪的区域
# background-size	CSS3	无	定义背景图像的尺寸大小
'''
<length>：
用长度值指定背景图像大小。不允许负值。
<percentage>：
用百分比指定背景图像大小。不允许负值。
auto：
背景图像的真实大小。
cover：
将背景图像等比缩放到完全覆盖容器，背景图像有可能超出容器。
contain：
将背景图像等比缩放到宽度或高度与容器的宽度或高度相等，背景图像始终被包含在容器内。
'''
```



## 5 边框属性

```python
# 属性	版本	继承性	描述
# border	CSS1	无	简写属性。定义元素边框的外观特性。参阅outline属性
'''
<line-width> || <line-style> || <color>
'''

# border-width	CSS1	无	简写属性。定义元素的边框厚度
'''
<length>：
用长度值来定义边框的厚度。不允许负值
medium：
定义默认厚度的边框。计算值为3px
thin：
定义比默认厚度细的边框。计算值为1px
thick：
定义比默认厚度粗的边框。计算值为5px
'''
# border-style	CSS1	无	简写属性。定义元素的边框样式
'''
none：
无轮廓。当定义了该值时，border-color将被忽略，border-width计算值为0，除非边框轮廓应用了border-image。
hidden：
隐藏边框。
dotted：
点状轮廓。
dashed：
虚线轮廓。
solid：
实线轮廓
double：
双线轮廓。两条单线与其间隔的和等于指定的border-width值
groove：
3D凹槽轮廓。
ridge：
3D凸槽轮廓。
inset：
3D凹边轮廓。
outset：
3D凸边轮廓。
'''
# border-color	CSS1	无	简写属性。定义元素的边框颜色
'''

'''

# box-shadow	CSS3	无	定义元素的阴影

# border-radius	CSS3	无	简写属性。定义元素的圆角
'''

# 方形变圆形
border-radius: 50%;
'''


# border-image	CSS3	无	简写属性。定义将图像应用到元素的边框上

# border-image-source	CSS3	无	定义元素边框样式所使用的图像。
# border-image-slice	CSS3	无	用以指定从哪 4 个位置分割图像（遵循上右下左的顺序）
# border-image-width	CSS3	无	定义元素边框图像的厚度
# border-image-outset	CSS3	无	定义边框图像从边框边界向外偏移的距离
# border-image-repeat	CSS3	无	定义分割图像怎样填充边框图像区域
```



## 6 display属性

```python
'''
none：隐藏对象。与visibility属性的hidden值不同，其不为被隐藏的对象保留其物理空间
inline：指定对象为内联元素。 --- 行内标签 span 不会自动换行
block：指定对象为块元素。
list-item：指定对象为列表项目。
inline-block：指定对象为内联块元素。（CSS2）
table：指定对象作为块元素级的表格。类同于html标签<table>（CSS2）
inline-table：指定对象作为内联元素级的表格。类同于html标签<table>（CSS2）
table-caption：指定对象作为表格标题。类同于html标签<caption>（CSS2）
table-cell：指定对象作为表格单元格。类同于html标签<td>（CSS2）
table-row：指定对象作为表格行。类同于html标签<tr>（CSS2）
table-row-group：指定对象作为表格行组。类同于html标签<tbody>（CSS2）
table-column：指定对象作为表格列。类同于html标签<col>（CSS2）
table-column-group：指定对象作为表格列组显示。类同于html标签<colgroup>（CSS2）
table-header-group：指定对象作为表格标题组。类同于html标签<thead>（CSS2）
table-footer-group：指定对象作为表格脚注组。类同于html标签<tfoot>（CSS2）
run-in：根据上下文决定对象是内联对象还是块级对象。（CSS3）
box：将对象作为弹性伸缩盒显示。（伸缩盒最老版本）（CSS3）
inline-box：将对象作为内联块级弹性伸缩盒显示。（伸缩盒最老版本）（CSS3）
flexbox：将对象作为弹性伸缩盒显示。（伸缩盒过渡版本）（CSS3）
inline-flexbox：将对象作为内联块级弹性伸缩盒显示。（伸缩盒过渡版本）（CSS3）
flex：将对象作为弹性伸缩盒显示。（伸缩盒最新版本）（CSS3）
inline-flex：将对象作为内联块级弹性伸缩盒显示。（伸缩盒最新版本）（CSS3）
'''
```



