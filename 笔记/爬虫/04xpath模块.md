[语法 | Xpath 学习](https://curder.github.io/xpath-study/guide/syntax.html)

# 1 引言

## 1.1 介绍

xpath在Python的爬虫学习中，起着举足轻重的地位，对比正则表达式 re两者可以完成同样的工作，实现的功能也差不多，但xpath明显比re具有优势，在网页分析上使re退居二线。

xpath 全称为XML Path Language 一种小型的查询语言

## 1.2 优点

可在XML中查找信息

支持HTML的查找

通过元素和属性进行导航

python开发使用XPath条件： 由于XPath属于lxml库模块，所以首先要安装库lxml

```shell
pip install lxml
from lxml import etree

# 将源码转化为能被XPath匹配的格式
selector=etree.HTML(源码) 

# 返回为一列表
selector.xpath(表达式)
```

# 2 路径表达式

| 表达式 | 描述                                                     | 实例         | 解析                                |
| ------ | -------------------------------------------------------- | ------------ | ----------------------------------- |
| /      | 从根节点选取                                             | /body/div[1] | 选取根结点下的body下的第一个div标签 |
| //     | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置 | //a          | 选取文档中所有的a标签               |
| ./     | 当前节点再次进行xpath                                    | ./a          | 选取当前节点下的所有a标签           |
| @      | 选取属性                                                 | //@calss     | 选取所有的class属性                 |

```python
from lxml import etree

html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""


selector = etree.HTML(html_doc)
print(type(selector)) #<class 'lxml.etree._Element'>
results = selector.xpath("//a")

print(results) # [<Element a at 0x2155de71640>, <Element a at 0x2155e3976c0>, <Element a at 0x2155e397800>]
for result in results:
    href = result.xpath('./@href')
    href1 = result.xpath('./@href')[0] #获取标签里的href值
    href2 = result.xpath('./text()')[0] #获取标签里的文本值
    
    print(href)
    #['http://example.com/elsie'] ['http://example.com/lacie'] ['http://example.com/tillie']
    
    print(href1)
    #http://example.com/elsie http://example.com/lacie http://example.com/tillie
    
    print(href2) 
    #Elsie Lacie Tillie
```

# 3 谓语（Predicates）

谓语用来查找某个特定的节点或者包含某个指定的值的节点。

谓语被嵌在方括号中。

在下面的表格中，我们列出了带有谓语的一些路径表达式，以及表达式的结果：

| 路径表达式                  | 结果                                                         |
| --------------------------- | ------------------------------------------------------------ |
| /ul/li[1]                   | 选取属于 ul子元素的第一个 li元素。                           |
| /ul/li[last()]              | 选取属于 ul子元素的最后一个 li元素。                         |
| /ul/li[last()-1]            | 选取属于 ul子元素的倒数第二个 li元素。                       |
| //ul/li[position()<3]       | 选取最前面的两个属于 ul元素的子元素的 li元素。               |
| //a[@title]                 | 选取所有拥有名为 title的属性的 a元素。                       |
| //a[@title='xx']            | 选取所有 a元素，且这些元素拥有值为 xx的 title属性。          |
| //a[@title>10] > < >= <= != | 选取 a元素的所有 title元素，且其中的 title元素的值须大于 10。 |
| /body/div[@price>35.00]     | 选取body下price元素值大于35的div节点                         |

# 4 选取未知节点

## 4.1 语法

XPath 通配符可用来选取未知的 XML 元素。

| 通配符 | 描述                 |
| ------ | -------------------- |
| *      | 匹配任何元素节点。   |
| @*     | 匹配任何属性节点。   |
| node() | 匹配任何类型的节点。 |

## 4.2 实例

在下面的表格中，我们列出了一些路径表达式，以及这些表达式的结果：

| 路径表达式  | 结果                              |
| ----------- | --------------------------------- |
| /ul/*       | 选取 bookstore 元素的所有子元素。 |
| //*         | 选取文档中的所有元素。            |
| //title[@*] | 选取所有带有属性的 title 元素。   |
| //node()    | 获取所有节点                      |

# 5 选取若干路径

## 5.1 语法

通过在路径表达式中使用 | 运算符，您可以选取若干个路径。

## 5.2 实例

在下面的表格中，我们列出了一些路径表达式，以及这些表达式的结果：

| 路径表达式                       | 结果                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| //book/title \| //book/price     | 选取 book 元素的所有 title 和 price 元素。                   |
| //title \| //price               | 选取文档中的所有 title 和 price 元素。                       |
| /bookstore/book/title \| //price | 选取属于 bookstore 元素的 book 元素的所有 title 元素，以及文档中所有的 price 元素。 |

（1）逻辑运算

```python
//div[@id="head" and @class="s_down"] # 查找所有id属性等于head并且class属性等于s_down的div标签
//title | //price # 选取文档中的所有 title 和 price 元素,“|”两边必须是完整的xpath路径
```

（2）属性查询

```python
//div[@id] # 找所有包含id属性的div节点
//div[@id="maincontent"]  # 查找所有id属性等于maincontent的div标签
//@class
//li[@name="xx"]//text()  # 获取li标签name为xx的里面的文本内容
```

（3）获取第几个标签 索引从1开始

```python
tree.xpath('//li[1]/a/text()')  # 获取第一个
tree.xpath('//li[last()]/a/text()')  # 获取最后一个
tree.xpath('//li[last()-1]/a/text()')  # 获取倒数第二个
```

（4）模糊查询

```python
//div[contains(@id, "he")]  # 查询所有id属性中包含he的div标签
//div[starts-with(@id, "he")] # 查询所有id属性中包以he开头的div标签

//div/h1/text()  # 查找所有div标签下的直接子节点h1的内容
//div/a/@href   # 获取a里面的href属性值 
//*  #获取所有
//*[@class="xx"]  #获取所有class为xx的标签

# 获取节点内容转换成字符串
c = tree.xpath('//li/a')[0]
result=etree.tostring(c, encoding='utf-8')
print(result.decode('UTF-8'))
```

# 6 示例

```python
from lxml import etree

doc = '''
<html>
 <head>
  <base href='http://example.com/' />  <!-- 设置基准链接 -->
  <title>Example website</title>  <!-- 设置网页标题 -->
 </head>
 <body>
  <div id='images'>
   <a href='image1' id='lqz'>Name: My image 1 <br /><img src='image1_thumb.jpg' /></a>
   <a href='image2'>Name: My image 2 <br /><img src='image2_thumb.jpg' /></a>
   <a href='image3'>Name: My image 3 <br /><img src='image3_thumb.jpg' /></a>
   <a href='image4'>Name: My image 4 <br /><img src='image4_thumb.jpg' /></a>
   <a href='image5' class='li li-item' name='items'>Name: My image 5 <br /><img src='image5_thumb.jpg' /></a>
   <a href='image6' name='items'><span><h5>test</h5></span>Name: My image 6 <br /><img src='image6_thumb.jpg' /></a>
  </div>
 </body>
</html>
'''

# 将HTML字符串转为可解析的对象
html = etree.HTML(doc)

# 1. 获取所有节点
all_nodes = html.xpath('//*')
print(all_nodes)

# 2. 指定节点（结果为列表）
head_node = html.xpath('//head')
print(head_node)

# 3. 子节点和子孙节点
child_nodes = html.xpath('//div/a')  # 获取div下的所有a标签
descendant_nodes = html.xpath('//body//a')  # 获取body下的所有子孙a标签
print(child_nodes)
print(descendant_nodes)

# 4. 父节点
parent_node = html.xpath('//body//a[1]/..')  # 获取第一个a标签的父节点
print(parent_node)

# 5. 属性匹配
matched_nodes = html.xpath('//body//a[@href="image1"]')  # 获取href属性为"image1.html"的a标签
print(matched_nodes)

# 6. 文本获取
text = html.xpath('//body//a[@href="image1"]/text()')  # 获取第一个a标签的文本内容
print(text)

# 7. 属性获取
href_attributes = html.xpath('//body//a/@href')  # 获取所有a标签的href属性值
print(href_attributes)

# 8. 属性多值匹配
li_class_nodes = html.xpath('//body//a[contains(@class, "li")]')  # 获取class属性包含"li"的a标签
print(li_class_nodes)

# 9. 多属性匹配
matched_nodes = html.xpath('//body//a[contains(@class, "li") and @name="items"]')  # 获取class属性包含"li"和name属性为"items"的a标签
print(matched_nodes)

# 10. 按序选择
second_a_text = html.xpath('//a[2]/text()')  # 获取第二个a标签的文本内容
print(second_a_text)

# 11. 节点轴选择
ancestors = html.xpath('//a/ancestor::*')  # 获取a标签的所有祖先节点
div_ancestor_node = html.xpath('//a/ancestor::div')  # 获取a标签的祖先节点中的div
attribute_values = html.xpath('//a[1]/attribute::*')  # 获取第一个a标签的所有属性值
child_nodes = html.xpath('//a[1]/child::*')  # 获取第一个a标签的所有子节点
descendant_nodes = html.xpath('//a[6]/descendant::*')  # 获取第六个a标签的所有子孙节点
following_nodes = html.xpath('//a[1]/following::*')  # 获取第一个a标签之后的所有节点
following_sibling_nodes = html.xpath('//a[1]/following-sibling::*')  # 获取第一个a标签之后的同级节点

print(ancestors)
print(div_ancestor_node)
print(attribute_values)
print(child_nodes)
print(descendant_nodes)
print(following_nodes)
print(following_sibling_nodes)
```

# 【案例】豆瓣Top250基于xpath解析

```python
import requests
from lxml import etree

url = "https://movie.douban.com/top250?start=0"
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.82 Safari/537.36"
}
resp = requests.get(url, headers=headers)

tree = etree.HTML(resp.text)  # 加载页面源代码

items = tree.xpath('//li/div[@class="item"]/div[@class="info"]')

for item in items:
    title = item.xpath('./div[@class="hd"]/a/span[1]/text()')[0]
    rating_num = item.xpath('./div[@class="bd"]/div[@class="star"]/span[@class="rating_num"]/text()')[0]
    comment_num = item.xpath('./div[@class="bd"]/div[@class="star"]/span[4]/text()')[0]
    print(title, rating_num, comment_num)
```

