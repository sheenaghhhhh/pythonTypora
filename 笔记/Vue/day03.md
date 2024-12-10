# 昨日回顾

```python
# 1 选美女小案例
	-属性指令--image标签
    -事件指令--按钮点击
    -定时器：setInterval(匿名函数,1000)
    -Math.random() -> 0--1之间数字
    -Math.random() * 数组大小  -> 0--数组大小之间的数字
    -Math.floor()  取地板整数
    
    
# 2 style和class
	-都可以绑定三种数据类型：字符串，数组，对象
    	-class：数组
        -style：对象
        	<div :style=style_obj>
             style_obj={fontSize:60px}
# 3 条件渲染
	v-if  v-else-if  v-else
    
    <p v-if='ss'></p>
    <div v-else-if='zz'></p>
    <image v-else></p>
    
# 4 v-for 
	-购物车案例：bootstrap5使用
		<p v-for='item in 数组'>{{}}</p>
    -v-for可以循环的变量
    	-数字
        -字符串
        -数组
        -对象
        <p v-for='(item,index)in 数组'>{{}}</p>
    
# 5 v-model : input 标签，双向数据绑定   
    
# 6 v-model事件
	-点击
    -变化：change
    -输入：input
    -获取焦点：focus
    -失去焦点：blur
    
# 7 过滤案例
	-1 数组过滤  数组.filter(function(item){})
    -2 indexOf   判断子字符串在 某个字符串中得位置，返回索引值，只要大于0，就是包含关系
    -3 input -> 输入框，每输入一个字母，就触发过滤 -> 把过滤后的数据使用v-for循环子页面上
```



# 今日内容

# 1 es6箭头函数

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>

<script>
    // 1 普通函数
    function demo01() {
        console.log("我是demo01")
    }

    demo01()

    // 2 匿名函数
    var demo02 = function () {
        console.log("我是demo02")
    }
    demo02()

    // 3 箭头函数写法--->匿名函数简写形式 -> 没有参数，没有返回值
    var demo03 = () => {
        console.log("我是demo03")
    }
    demo03()

    // 4 箭头函数，带两个参数
    var demo04 = (a,b) => {
        console.log("我是demo04")
        console.log(a+b)
    }
    demo04(4,5)

    // 5 箭头函数，带1个参数 -> 简写 把括号省略
    // var demo05 = (a) => {
    var demo05 = a => {  // 简写成
        console.log("我是demo05")
        console.log(a)
    }
    demo05(4)

    // 6 箭头函数，带多个参数，有返回值
    // var demo06 = (a,b) => {
    //     return a+b
    // }
    // 简写成
    var demo06 = (a,b) =>a+b
    console.log(demo06(4,6))


    // 7 一个参数，有返回值
    // var demo07 = (a) => {
    //     return a+100
    // }
    // var demo07 = a => {
    //     return a+100
    // }

    var demo07 = a => a+100
    console.log(demo07(9))

    // 8 匿名化函数就够用了，为什么出箭头函数？  1 代码简洁  2 this指向问题
    // 箭头函数，没有自己的this，如果在箭头函数中使用this，会使用上一层中得this

</script>
</html>
```

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>input事件</title>
    <script src="./vue2/vue.js"></script>

</head>
<body>
<div class="app">
    <h1>过滤案例</h1>
    <input type="text" v-model="search" @input="handleInput">
    <hr>
    <ul>
        <li v-for="item in newDataList">{{item}}</li>
    </ul>
</div>
</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            search: '',
            dataList: ['a', 'at', 'atom', 'atommon', 'be', 'beyond', 'cs', 'csrf'],
            newDataList: ['a', 'at', 'atom', 'atommon', 'be', 'beyond', 'cs', 'csrf']
        },
        methods: {
            //  handleInput(){
            //
            //      console.log('methods普通函数内部,这个this 是：Vue对象',this)
            //     this.newDataList=this.dataList.filter((item)=>{
            //         // console.log('匿名函数内部,这个this 是：window对象',this)
            //         // console.log('箭头函数内部没有自己的this,这个this 是：vue对象',this)
            //         return item.indexOf(this.search)>=0
            //     })
            // },

            handleInput() {
                this.newDataList = this.dataList.filter(item =>item.indexOf(this.search) >= 0)
            }, 
        }
    })
</script>
</html>
```

# 2 js的各种循环

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script
  src="https://code.jquery.com/jquery-3.7.1.min.js"
  integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo="
  crossorigin="anonymous"></script>
</head>
    
<body>
</body>

<script>
    //1 普通，基于索引的循环
    for (var i = 0; i < 10; i++) {
        console.log(i)
    }
    console.log("--------------------")
    // 2 for in 循环--->循环出索引
    var l = [3, 4, 5, 6, 7, 8]
    for (const index in l) {
        console.log(index)
        console.log(l[index])
    }
    console.log("--------------------")
    // 3 for of 循环 -> 循环出值
    var l2 = [3, 4, 5, 6, 7, 8]
    for (const value of l2) {
        console.log(value)
    }
    console.log("--------------------")

    // 4  循环数组  forEach 循环
    var l3 = [3, 4, 5, 6, 7, 8]
    l3.forEach((value, index) => {
        console.log(index)
        console.log(value)
    })

    // 5 对象循环-->使用for in
    console.log("--------------------")
    var obj = {name: 'lqz', age: 19}
    for (const key in obj) {
        console.log(key)
        console.log(obj[key])
    }

    // 6 对象循环-->使用for of -> 对象不能使用of循环
    // console.log("--------------------")
    // var obj1 = {name: 'lqz', age: 19}
    // for (const value of obj1) {
    //     console.log(value)
    // }


    // 7 jq 的循环方法：对象，数组
    console.log("--------------------")
    var obj1 = {name: 'lqz', age: 19}
    // $.each('要循环的对象',匿名函数)
    $.each(obj1, (key, value) => {
        console.log(key)
        console.log(value)
    })
        console.log("--------------------")
    var l4 = [3, 4, 5, 6, 7, 8]
    $.each(l4, (index, value) => {
        console.log(index)
        console.log(value)
    })


</script>
</html>
```



# 2 事件-按键修饰符

## 2.1 事件修饰符

```python
# 1 用来修饰事件的

# 2 常用的，修饰click事件
事件修饰符	           释义
.stop	          只处理自己的事件，父控件冒泡的事件不处理（阻止事件冒泡）
.self	          只处理自己的事件，子控件冒泡的事件不处理
.prevent	      阻止a链接的跳转
.once	          事件只会触发一次（适用于抽奖页面）
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件修饰符</title>
    <script src="./vue2/vue.js"></script>

</head>
<body>
<div class="app">
    <h1>stop事件修饰符</h1>

    <!--    会出现事件冒泡：button的点击事件会冒泡到div上，触发div的点击事件-->
    <!--    处理成，点击button，父标签的事件不触发-->
    <div style="height: 300px;width: 300px;background-color: pink" @click="handleDiv">
        <button style="margin-left: 100px;margin-top: 100px" @click.stop="handleButton">点我</button>
    </div>


    <h1>self事件修饰符</h1>
<!--    只处理自己的事件，冒泡的事件不处理-->
    <div style="height: 300px;width: 300px;background-color: pink" @click.self="handleDiv2">
        <button style="margin-left: 100px;margin-top: 100px" @click="handleButton2">点我</button>
    </div>

    <h1>prevent事件修饰符-a标签要跳转时，触发它,阻止跳转</h1>
    <a href="http://www.baidu.com" @click.prevent="handleA">点我看美女</a>

    <h1>once事件修饰符</h1>
    <button @click.once="handleM">点我秒杀</button>


</div>
</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {},
        methods: {
            handleDiv() {
                console.log('div被点了')
            },
            handleButton() {
                console.log('button被点了')
            },
            handleDiv2() {
                console.log('div被点了')
            },
            handleButton2() {
                console.log('button被点了')
            },
            handleA(){
               console.log('aaaa')
                // 加逻辑判断，决定能不能跳
                var res=Math.floor(Math.random()*2)
                if (res==0){
                    location.href='http://www.baidu.com'
                }else {
                    location.href='http://www.coblogs.com'
                }

            },
            handleM(){
                console.log('开始秒杀')
            }

        }
    })

</script>
</html>


```

## 2.2 按键修饰符

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>按键修饰符</title>
    <script src="./vue2/vue.js"></script>

</head>
<body>
<div class="app">
    <h1>按键事件</h1>
<!--    <input type="text" v-model="text" @keyup="handleUp">====>{{text}}-->
     <h1>按键事件</h1>
<!--    <input type="text" v-model="text" @keyup.enter="handleUp">====>{{text}}-->
<!--    keyCode对照表-->
    <input type="text" v-model="text" @keyup.66="handleUp">====>{{text}}

</div>
</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            text:''
        },
        methods:{
            handleUp(event){
                console.log('按了',event.key)
            }
        }
    })

</script>
</html>


```



# 3 表单控制

```python
# 1 checkbox  v-model 绑定：数组，True和False
	-checkbox 选中和不选中，使用true和false控制
    -checkbox 多选的value值放在数组中
# 2 radio    v-model 绑定：字符串
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js2/vue.js"></script>
</head>
<body>
<div id="app">
    <p>用户名： <input type="text" v-model="username"></p>
    <p>密码： <input type="text" v-model="password"></p>
    <p> <input type="checkbox" v-model="remember">记住密码</p>
    <p> 爱好：
        <input type="checkbox" v-model="hobby" value="1"> 篮球
        <input type="checkbox" v-model="hobby" value="2"> 足球
        <input type="checkbox" v-model="hobby" value="3"> 乒乓球
        <input type="checkbox" v-model="hobby" value="4"> 游泳
    </p>
    <p>性别：
        <input type="radio" v-model="gender" value="1">男
        <input type="radio"  v-model="gender"  value="2">女
        <input type="radio"  v-model="gender"  value="0">未知
    </p>
    <p><input type="submit" value="登录" @click="handleSubmit"></p>
</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            username: '',
            password: '',
            remember: false,
            hobby:[],
            gender:''
        },
        methods: {
            handleSubmit(){
                console.log(this.username)
                console.log(this.password)
                console.log(this.remember)
                console.log(this.hobby)
                console.log(this.gender)
            }
        }
    })
</script>
</html>
```



# 4 购物车案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>购物车案例</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js">
</script>
    <script src="./vue2/vue.js"></script>

</head>
<body>
<div class="app">
    <div class="container-fluid">
        <div class="col align-self-center">
            <div class="text-center">
                <h1>购物车</h1>
            </div>

            <table class="table">
                <thead>
                <tr>
                    <th scope="col">商品id</th>
                    <th scope="col">商品名</th>
                    <th scope="col">价格</th>
                    <th scope="col">数量</th>
                    <th scope="col">
                        全选/全不选
                        <input type="checkbox" v-model="check_all" @change="handleCheckAll">
                    </th>
                </tr>
                </thead>
                <tbody>

                <tr v-for="(item,index) in goods_list" :class="index%2==0?'table-success':'table-danger'">
                    <th scope="row">{{item.id}}</th>
                    <td>{{item.name}}</td>
                    <td>{{item.price}}</td>
                    <td><span class="btn" @click="handleJ(item)">-</span>{{item.number}} <span class="btn"@click="item.number++">+</span></td>
                    <td><input type="checkbox" v-model="goods_check" :value="item" @change="handleOne"></td>
                </tr>

                </tbody>
            </table>


        </div>
        {{check_all}}--{{goods_check}}
        <h2 class="text-right">商品总价格:{{getPrice()}}</h2>
    </div>


</div>
</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            goods_list: [
                {id: 1001, name: '脸盆', price: 9, number: 2},
                {id: 1002, name: '水杯', price: 19, number: 1},
                {id: 1003, name: '电脑', price: 6999, number: 5},
                {id: 1004, name: '电视机', price: 1999, number: 4},
                {id: 1005, name: '收音机', price: 30, number: 3},
            ],
            goods_check:[],
            check_all:false
        },
        methods: {
            getPrice(){
                var total=0
                for (const item of this.goods_check) {
                    total+=item.price * item.number
                }
                return total
            },
            handleCheckAll(){
                if(this.check_all){
                    // 全选了
                    this.goods_check=this.goods_list
                }else{
                    //全不选
                    this.goods_check=[]
                }
            },
            handleOne(){
                // 只要goods_check 长度等于 goods_list 长度 -> 让check_all 变为true
                if(this.goods_check.length==this.goods_list.length){
                    this.check_all=true
                }else {
                    this.check_all=false
                }
            },
            handleJ(item){
                if(item.number>1){
                   item.number--
                }else {
                    alert('不能再少了')
                }


            }
        }
    })

</script>
</html>


```



# 5 v-model-修饰符

```python
# v-model 修饰符
lazy：等待input框的数据绑定时区焦点之后再变化
number：数字开头，只保留数字，后面的字母不保留；字母开头，都保留
trim：去除首位的空格
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js2/vue.js"></script>
</head>
<body>
<div id="app">

    <h1>lazy</h1>
    <input type="text" v-model.lazy="text01">-->{{text01}}
    <h1>number</h1>
    <input type="text" v-model.number="text02">-->{{text02}}
        <h1>trim</h1>
    <input type="text" v-model.trim="text03">-->{{text03}}

</div>

</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            text01: '',
            text02: '',
            text03: '',
        },
    })
</script>
</html>
```



# 6 ajax

```python
# 1 ajax:Asynchronous Javascript And XML（异步JavaScript和XML)


# 2 使用js发送ajax
	-2.1   js原生发送ajax -> XMLHttpRequest -> 浏览器兼容性不好
        var xhr = new XMLHttpRequest();
        xhr.open("GET", "请求地址", true);
        xhr.onreadystatechange = function () {
          if (xhr.readyState == 4 && xhr.status == 200) {
            var response = JSON.parse(xhr.responseText);
            console.log(response);
          }
        };
        xhr.send()
        
     -2.2 jquery 封装了XMLHttpRequest，发送ajax更简单
    	$.ajax({
            url:
            method:get,
            success:function(data){
                
            }
        })   
        
 # 3 在vue中，不会使用jquery 的ajax
	-需要原生发送：XMLHttpRequest -> 第三方基于XMLHttpRequest封装了 -> axios
    
    
 # 4 因为原生：XMLHttpRequest 有兼容性问题
	-后期js新版本又造了一个发送ajax请求的方法：fetch
    -这个东西，用的也少
    
# 5 前端跟后端交互，会有跨域问题（咱们先忽略）
	blocked by CORS policy: No 'Access-Control-Allow-Origin'
    
    响应头中允许
```



## 6.1 使用jq的ajax（基本不用）

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ajax请求</title>
    <script
            src="https://code.jquery.com/jquery-3.7.1.min.js"
            integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo="
            crossorigin="anonymous"></script>
    <script src="./vue2/vue.js"></script>

</head>
<body>
<div class="app">
    <button @click="handleLoad">加载用户信息</button>
    <hr>
    <h3>用户名:{{username}}</h3>
    <h3>年龄:{{age}}</h3>
    <h3>性别:{{gender}}</h3>

</div>
</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            username:'',
            age:'',
            gender:'',
        },
        methods:{
            handleLoad(){
                // 发送ajax请求，获取后端数据，显示在前端
                $.ajax({
                    url:'http://127.0.0.1:5000/info',
                    method:'get',
                    success:data=>{
                        console.log(data)
                        this.username=data.username
                        this.age=data.age
                        this.gender=data.gender

                    }
                })
            }
        }
    })

</script>
</html>


```



```python
from flask import Flask, jsonify

app = Flask(__name__)


@app.get('/user_info')
def user_info():
    res=jsonify({'code': 100, 'msg': '成功', 'username': 'lqz', 'age': 19})
    res.headers['Access-Control-Allow-Origin']='*'
    return res


if __name__ == '__main__':
    app.run(host='127.0.0.1', port=8888)

```



## 6.2 使用js原生fetch（基本不用）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ajax请求</title>
    <script src="./vue2/vue.js"></script>

</head>
<body>
<div class="app">
    <button @click="handleLoad">加载用户信息</button>
    <hr>
    <h3>用户名:{{username}}</h3>
    <h3>年龄:{{age}}</h3>
    <h3>性别:{{gender}}</h3>

</div>
</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            username:'',
            age:'',
            gender:'',
        },
        methods:{
            handleLoad(){
                /*
                fetch('http://example.com/movies.json')
                  .then(response=>response.json())
                  .then(myJson=>{
                    console.log(myJson);}
                    )


                 */
                // fetch
                fetch('http://127.0.0.1:5000/info').then(res=>res.json()).then(res=>{
                    console.log(res)
                    this.username=res.username+'_nb'
                    this.age=res.age
                    this.gender=res.gender
                })

            }
        }
    })

</script>
</html>


```



## 6.3 使用第三方axios（必用）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ajax请求</title>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="./vue2/vue.js"></script>

</head>
<body>
<div class="app">
    <button @click="handleLoad">加载用户信息</button>
    <hr>
    <h3>用户名:{{username}}</h3>
    <h3>年龄:{{age}}</h3>
    <h3>性别:{{gender}}</h3>

</div>
</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            username:'',
            age:'',
            gender:'',
        },
        methods:{
            handleLoad(){
                axios.get('http://127.0.0.1:5000/info').then(res=>{
                    console.log(res) //res 是http响应对象 -> 响应体在res.data中
                    this.username=res.data.username+'_nb'
                    this.age=res.data.age
                    this.gender=res.data.gender
                })
            }
        }
    })

</script>
</html>


```





```python
from flask import Flask,jsonify
app=Flask(__name__)
@app.route('/info')
def info():
    res=jsonify({'username':'张三','age':99,'gender':'男'})
    res.headers['Access-Control-Allow-Origin']='*'  # 解决跨域问题
    return res

if __name__ == '__main__':
    app.run()
```



# 作业

```python
# 1 今天讲的写一遍
	-购物车案例
    -ajax
    
# 2 查询所有图书
	-vue -> 表格 -> bootstrap
    	-隔一个是绿，隔一个是红
        
        
# 3 高级--部分
	-增，删，查，改  都配合成前端
    -跨域：只解决了ge请求跨域
```

