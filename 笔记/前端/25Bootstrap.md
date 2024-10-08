# 25 Bootstrap

## 1 引入

- 该框架已经帮我们写好了很多页面样式，如果需要使用，只需要下载对应文件
- 直接CV拷贝即可
- 在使用Bootstrap的时候，所有的页面样式只需要通过修改class属性来调节即可



## 2 什么是Bootstrap

- Bootstrap是一个开源的前端框架，用于快速构建响应式和移动设备优先的网站或应用程序。 

- 它包含了HTML、CSS和JavaScript的模板和工具集，使开发人员能够快速地创建具有一致性和现代外观的页面布局和UI组件。

- Bootstrap最初由Twitter的一些工程师创建，旨在简化Web开发的过程。 

- 它提供了一个广泛的预定义样式和组件库，可以轻松自定义和扩展，以满足各种需求。

- 使用Bootstrap，开发人员可以更加专注于网站或应用程序的功能和逻辑，而不必从头开始编写CSS样式和设计页面布局。 

- 它提供了响应式布局的支持，使得页面能够自适应不同的设备和屏幕尺寸。
- 此外，Bootstrap还提供了丰富的UI组件（如导航栏、按钮、表单、模态框等），使开发人员能够轻松地在项目中使用这些现成的组件。

- 总而言之，Bootstrap是一个强大的开发工具，可帮助开发人员快速搭建出现代化和具备自适应能力的网站或应用程序。



## 3 Bootstraps引入

中文文档查询：https://www.bootcss.com/

### 3.1 CDN加速链接

[twitter-bootstrap (v5.2.3) - Bootstrap 是全球最受欢迎的前端组件库，用于开发响应式布局、移动设备优先的 WEB 项目。 | BootCDN - Bootstrap 中文网开源项目免费 CDN 加速服务](https://www.bootcdn.cn/twitter-bootstrap/)

- 压缩文档链接引入

https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.4.1/css/bootstrap.min.css

https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.4.1/js/bootstrap.min.js

### 3.2 注意

- 网络连接引入在本地不会提示相关的补充
- 下载本地文档较为友好

**Bootstrap的js代码是基于jQuery的**

**在使用bootstrap做动态效果时一定要引入jQuery!!!**



## 4 布局容器

- 布局容器是指用于组织和排列其他元素的容器或容器类。
- 在前端开发中，常用的布局容器有以下三种

### 4.1 块级布局容器（Block-Level Layout Container）

- 块级布局容器会生成一个块级框，它可以使用display属性设置为"block"、"flex"或者"grid"。
- 常见的HTML元素如<div>、<p>、<section>等都是块级布局容器。
- 块级容器会独占一行，并通过CSS属性进行自上而下的垂直排列。

### 4.2 行内布局容器（Inline Layout Container）

- 行内布局容器会生成一个行内框，它可以使用display属性设置为"inline"或者"inline-block"。
- 常见的HTML元素如<span>、<a>、<em>等都是行内布局容器。
- 行内容器会水平地从左到右排列，直到行的边缘，然后自动换行。

### 4.3 弹性布局容器（Flexbox Layout Container）

- 弹性布局容器是CSS3中的一种新型布局技术，通过定义父元素作为弹性容器，将其子元素称为弹性项。
- 弹性容器通过一系列的属性灵活地控制和调整弹性项的尺寸和位置。
- 常见的属性包括：flex-direction、justify-content、align-items等。
- 弹性布局容器的主要好处是可以实现响应式布局和灵活的排列方式。

### 4.4 留白

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.4/jquery.min.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.4.1/css/bootstrap.min.css"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.4.1/js/bootstrap.min.js"></script>

   <style>
      .c1 {
         height: 1000px;
         width: 1000px;
         background-color: red;
      }
      
      .c2 {
         height: 1000px;
         width: 1000px;
         background-color: red;
      }
   </style>

</head>
<body>
<div class="container c1"></div> /*左右两侧有留白*/
<div class="container-fluid c2"></div> /*左右两侧没有留白*/
</body>
</html>
```



## 5 栅格系统

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--  本地 链接 引入方法  -->
    <!--  Websource 文件夹 拷贝到当前文件夹下即可使用  -->
    <script src="Websource\jQuery\node_modules\jquery\dist\jquery.min.js"></script>
    <script src="Websource\Bootstrap\js\bootstrap.min.js"></script>
    <link rel="stylesheet" href="Websource\Bootstrap\css\bootstrap.min.css">
    <!--  CDN 链接 引入方法  -->
    <link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.4.1/css/bootstrap.min.css">
    <script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.4.1/js/bootstrap.min.js">// bootstrap </script>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.4/jquery.min.js"> // jquery</script>

    <style>
        .c1 {
            height: 100px;
            background-color: red;
            border: 5px solid black;

        }

        .c2 {
            height: 100px;
            background-color: green;
        }
    </style>


</head>
<body>

<div class="container">
    <div class="row">
        <div class="col-md-6 c1"></div>
        <div class="col-md-6 c1"></div>

        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
        <div class="col-md-1 c1"></div>
    </div>

</div>

</body>
</html>
```

```html
<div class="row"></div>
```

- 写一个 row 就是将所在区域划分成 12 份

```html
<div class="col-md-6"></div>
```

- 写一个 col-md-6 获取想要的份数



## 6 栅格参数

| 超小屏幕 手机 (<768px) | 小屏幕 平板 (≥768px)       | 中等屏幕 桌面显示器 (≥992px)                        | 大屏幕 大桌面显示器 (≥1200px) |          |
| ---------------------- | -------------------------- | --------------------------------------------------- | ----------------------------- | -------- |
| 栅格系统行为           | 总是水平排列               | 开始是堆叠在一起的，当大于这些阈值时将变为水平排列C |                               |          |
| .container 最大宽度    | None （自动）              | 750px                                               | 970px                         | 1170px   |
| 类前缀                 | .col-xs-                   | .col-sm-                                            | .col-md-                      | .col-lg- |
| 列（column）数         | 12                         |                                                     |                               |          |
| 最大列（column）宽     | 自动                       | ~62px                                               | ~81px                         | ~97px    |
| 槽（gutter）宽         | 30px （每列左右均有 15px） |                                                     |                               |          |
| 可嵌套                 | 是                         |                                                     |                               |          |
| 偏移（Offsets）        | 是                         |                                                     |                               |          |
| 列排序                 | 是                         |                                                     |                               |          |

针对不同的显示器，要加上不同的参数



## 7 列偏移

- 使用 .col-md-offset-* 类可以将列向右侧偏移。
- 这些类实际是通过使用 * 选择器为当前元素增加了左侧的边距（margin）。
- 例如，.col-md-offset-4 类将 .col-md-4 元素向右侧偏移了4个列（column）的宽度。



## 8 表格样式

- bootstrap在设置页面的时候将字体设置成了肉眼可以接受的好看一点的字体。

### 8.1 条纹状表格

- 通过 .table-striped 类可以给 <tbody> 之内的每一行增加斑马条纹样式。

```html
<table class="table table-striped">
  <thead>
    <tr>
      <th>#</th>
      <th>First Name</th>
      <th>Last Name</th>
      <th>Username</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>Mark</td>
      <td>Otto</td>
      <td>@mdo</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>Jacob</td>
      <td>Thornton</td>
      <td>@fat</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td>Larry</td>
      <td>the Bird</td>
      <td>@twitter</td>
    </tr>
  </tbody>
</table>
```

### 8.2 带边框的表格

- 添加 .table-bordered 类为表格和其中的每个单元格增加边框。

```html
<table class="table table-bordered">
  <thead>
    <tr>
      <th>#</th>
      <th>First Name</th>
      <th>Last Name</th>
      <th>Username</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>Mark</td>
      <td>Otto</td>
      <td>@mdo</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>Jacob</td>
      <td>Thornton</td>
      <td>@fat</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td>Larry</td>
      <td>the Bird</td>
      <td>@twitter</td>
    </tr>
  </tbody>
</table>
```

### 8.3 鼠标悬停

- 通过添加 .table-hover 类可以让 <tbody> 中的每一行对鼠标悬停状态作出响应。

```html
<table class="table table-hover">
  <thead>
    <tr>
      <th>#</th>
      <th>First Name</th>
      <th>Last Name</th>
      <th>Username</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>Mark</td>
      <td>Otto</td>
      <td>@mdo</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>Jacob</td>
      <td>Thornton</td>
      <td>@fat</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td>Larry</td>
      <td>the Bird</td>
      <td>@twitter</td>
    </tr>
  </tbody>
</table>
```

### 8.4 紧缩表格

- 通过添加 .table-condensed 类可以让表格更加紧凑，单元格中的内补（padding）均会减半。

```html
<table class="table table-condensed">
      <thead>
        <tr>
          <th>#</th>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Username</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">1</th>
          <td>Mark</td>
          <td>Otto</td>
          <td>@mdo</td>
        </tr>
        <tr>
          <th scope="row">2</th>
          <td>Jacob</td>
          <td>Thornton</td>
          <td>@fat</td>
        </tr>
        <tr>
          <th scope="row">3</th>
          <td colspan="2">Larry the Bird</td>
          <td>@twitter</td>
        </tr>
      </tbody>
    </table>
```

### 8.5 状态类

通过这些状态类可以为行或单元格设置颜色。

| Class    | 描述                                 |
| -------- | ------------------------------------ |
| .active  | 鼠标悬停在行或单元格上时所设置的颜色 |
| .success | 标识成功或积极的动作                 |
| .info    | 标识普通的提示信息或动作             |
| .warning | 标识警告或需要用户注意               |
| .danger  | 标识危险或潜在的带来负面影响的动作   |

```html
<table class="table">
  <thead>
    <tr>
      <th>#</th>
      <th>Column heading</th>
      <th>Column heading</th>
      <th>Column heading</th>
    </tr>
  </thead>
  <tbody>
    <tr class="active">
      <th scope="row">1</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr class="success">
      <th scope="row">3</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr>
      <th scope="row">4</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr class="info">
      <th scope="row">5</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr>
      <th scope="row">6</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr class="warning">
      <th scope="row">7</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr>
      <th scope="row">8</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr class="danger">
      <th scope="row">9</th>
      <td>Column content</td>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
  </tbody>
</table>
```



## 9 表单样式

- 针对form表单，加样式优先考虑form-control
- 针对radio和checkbox不能加!!!



## 10 按钮组

### 10.1 颜色

```html
<!-- Standard button -->
<button type="button" class="btn btn-default">（默认样式）Default</button>

<!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
<button type="button" class="btn btn-primary">（首选项）Primary</button>

<!-- Indicates a successful or positive action -->
<button type="button" class="btn btn-success">（成功）Success</button>

<!-- Contextual button for informational alert messages -->
<button type="button" class="btn btn-info">（一般信息）Info</button>

<!-- Indicates caution should be taken with this action -->
<button type="button" class="btn btn-warning">（警告）Warning</button>

<!-- Indicates a dangerous or potentially negative action -->
<button type="button" class="btn btn-danger">（危险）Danger</button>

<!-- Deemphasize a button by making it look like a link while maintaining button behavior -->
<button type="button" class="btn btn-link">（链接）Link</button>
```

### 10.2 大小

```html
<p>
  <button type="button" class="btn btn-primary btn-lg">（大按钮）Large button</button>
  <button type="button" class="btn btn-default btn-lg">（大按钮）Large button</button>
</p>
<p>
  <button type="button" class="btn btn-primary">（默认尺寸）Default button</button>
  <button type="button" class="btn btn-default">（默认尺寸）Default button</button>
</p>
<p>
  <button type="button" class="btn btn-primary btn-sm">（小按钮）Small button</button>
  <button type="button" class="btn btn-default btn-sm">（小按钮）Small button</button>
</p>
<p>
  <button type="button" class="btn btn-primary btn-xs">（超小尺寸）Extra small button</button>
  <button type="button" class="btn btn-default btn-xs">（超小尺寸）Extra small button</button>
</p>
```



## 11 图片

### 11.1 形状

```html
<img src="..." alt="..." class="img-rounded">
<img src="..." alt="..." class="img-circle">
<img src="..." alt="..." class="img-thumbnail">
```

### 11.2 颜色

```html
<p class="bg-primary">...</p>
<p class="bg-success">...</p>
<p class="bg-info">...</p>
<p class="bg-warning">...</p>
<p class="bg-danger">...</p>
```

### 11.3 清除浮动

```html
<!-- Usage as a class -->
<div class="clearfix">...</div>
```

```js
// Mixin itself
.clearfix() {
  &:before,
  &:after {
    content: " ";
    display: table;
  }
  &:after {
    clear: both;
  }
}

// Usage as a mixin
.element {
  .clearfix();
}
```



## 12 图标

span标签

```html
<button type="button" class="btn btn-default" aria-label="Left Align">
  <span class="glyphicon glyphicon-align-left" aria-hidden="true"></span>
</button>

<button type="button" class="btn btn-default btn-lg">
  <span class="glyphicon glyphicon-star" aria-hidden="true"></span> Star
</button>
```

可以改颜色 - 操作普通文本方法相同

```html
<script>
.c {color:red;}
</script>
```

图标参考网站：https://fontawesome.com.cn/
