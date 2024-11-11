[Requests: 让 HTTP 服务人类 — Requests 2.18.1 文档](https://requests.readthedocs.io/projects/cn/zh-cn/latest/)

# 1 初识

## 1.1 Requests模块简介
1. 简介

Requests 是用 Python 语言编写，基于urllib，采用Apache2 Licensed开源协议的 HTTP 库。 

它比urllib 更加方便，可以节约我们大量的工作，完全满足HTTP测试需求。 

是一个功能强大、简洁易用的第三方Python库，用于发送HTTP请求。

2. 来源

可以模拟发送http请求

urlib2：内置库，不太好用，繁琐

封装出requests模块，应用非常广泛（公司，人）

requests模块最初由Kenneth Reitz于2010年创建并开源，旨在提供一种更人性化的方式来发送HTTP请求。

它的出现填补了Python标准库中urllib和urllib2模块使用起来不够友好的问题。

## 1.2 基础知识储备

1. **http协议特点**

参考链接：https://www.cnblogs.com/dream-ze/p/17337378.html

- 应用层协议

HTTP是一种应用层协议，用于定义客户端和服务器之间传输数据的规则。

与其他应用层协议如MySQL、Redis、MongoDB等不同，它使用的是基于请求-响应模式的交互方式。

- 基于请求-响应模式

HTTP采用客户端主动发起请求，并等待服务器响应的模式。

这意味着服务端不能主动向客户端推送消息。

要实现主动推送消息，可以使用轮询、长轮询或WebSocket协议。

- 无状态保存

HTTP协议本身是无状态的，它不会保存客户端的状态信息。

为了在多个请求之间共享状态，常用的解决方案包括使用Cookie、Session和Token等机制来维持用户会话。

- 无连接

每次请求-响应周期都需要建立和断开连接，这会对性能产生影响。

但从HTTP协议的设计角度来看，它是无连接的，即在收到响应后就断开连接。

然而，HTTP 1.1引入了持久连接，允许在一个TCP连接上发送多个HTTP请求，从而提高了性能效率。

HTTP 2.0更进一步，可以在一个TCP连接上同时发送多个HTTP请求和响应。

2. **HTTP请求**

参考博客：https://www.cnblogs.com/dream-ze/p/17176477.html

<u>请求首行</u> 

请求首行指的是在发送HTTP请求时的第一行

包括 

​	请求方法（GET、POST等）

​	请求URI（Uniform Resource Identifier）。

<u>请求头</u> 

请求头包含了关于请求的附加信息，以键值对的形式表示。

常见的请求头 

​	Content-Type

​	User-Agent

​	Cookie等
它们用来描述请求的相关属性和数据。

<u>请求体</u> 

请求体可选，通常用于POST请求，在请求体中携带参数和数据。

请求体的格式可以是 

​	urlencode形式的键值对

​	JSON格式

​	formdata格式。

3. **HTTP**响应

<u>响应首行</u>

响应首行指的是服务器返回响应时的第一行

其中包含了状态码，表示服务器对请求的处理结果。

常见的状态码有200（成功）、301（永久重定向）、302（临时重定向）等。

<u>响应头</u>

响应头包含了关于响应的附加信息，以键值对的形式表示。

常见的响应头有Set-Cookie、Cache-Control等，它们用来描述响应的相关属性和数据。

<u>响应体</u>

响应体是服务器返回给客户端的实际数据。

它可以是HTML格式的网页内容，也可以是JSON格式的数据等。

4. **浏览器调试**

<u>调试模式</u>

通过右键浏览器页面并选择调试模式，可以打开开发者工具，方便进行调试和查看页面的相关信息。

<u>Elements</u>

Elements面板用于查看和编辑网页的结构和内容，其中包括响应体中的HTML格式数据。

<u>Console</u>

Console面板是开发者用于在JavaScript中输出调试信息的窗口，在这里可以查看通过console.log()等方法输出的内容。

<u>Network</u>

Network面板用于监视和查看浏览器发送和接收的网络请求，包括AJAX请求。

可以查看所有请求或者仅显示XHR（XMLHttpRequest）请求。

## 1.3 Requests模块基础使用

1. 安装

```python
pip install requests
```

2. 简单使用

```python
# 导入 requests 模块
import requests

# 使用 requests 模块发起简单的 get  请求
response = requests.get('https://www.baidu.com/')
# 对页面进行转码 ( 页面可能需要 utf-8 转码也可能是 gbk 转码 )
response.encoding = "utf-8"
# 查看请求到的页面数据
print(response.text)
```



# 2 请求

## 2.1 发送GET请求

1. 引言

HTTP默认的请求方法就是GET

- 没有请求体
- 数据必须在1K之内！
- GET请求数据会暴露在浏览器的地址栏中

GET请求常用的操作

- 在浏览器的地址栏中直接给出URL，那么就一定是GET请求
- 点击页面上的超链接也一定是GET请求
- 提交表单时，表单默认使用GET请求，但可以设置为POST

小结

- 在Python中，我们可以使用requests模块来发送HTTP请求。
- 其中，GET请求是最常用的一种请求方式，用于从服务器获取数据。

2. 导入模块

首先，我们需要通过import语句导入requests模块：

```python
import requests
```

3. 发送请求

然后，我们可以使用get方法发送一个GET请求，并指定请求的URL

```python
# 导入模块
import requests

# 定义请求地址
url = "https://www.baidu.com/"

# 发送请求获取响应数据
response = requests.get(url)
```

在上述代码中，url是我们要请求的目标URL。

发送GET请求后，服务器会返回一个响应对象，我们可以通过访问这个响应对象的属性和方法来获取服务器返回的数据。

其中，一些常用的属性和方法包括：

status_code: 响应的状态码，例如200表示请求成功，404表示页面不存在等。

text: 响应的内容，通常是服务器返回的HTML文本。

json(): 将响应的内容解析为JSON格式。

4. 示例

演示如何发送GET请求并获取服务器返回的数据：

```python
# 导入请求库
import requests

# 定义目标路由地址
url = "https://www.baidu.com"

# 发起GET请求，获取响应内容
response = requests.get(url)

# 判断当前请求是否成功，如果成功应该返回 200 OK 
if response.status_code == 200:
    # 打印响应内容 --- 文本类型即 text 类型的数据
    response_text = response.text
    print(response_text)
else:
    print(f"当前请求数据失败!错误码: {response.status_code}")
```

## 2.2 Get请求之携带参数

1. 携带请求体

需要注意的是，发送GET请求时，可以通过URL传递参数 

例如：https://www.baidu.com/s?wd=周杰伦。

如果需要传递参数，可以使用params参数来指定，例如：

```python
# 发送带有参数的GET请求
url = "http://example.com/api/data"

params = {
    'param1': 'value1',
    'param2': 'value2'
}

response = requests.get(url, params=params)
```

在上述示例中，我们通过params参数指定了要传递的参数。

2. 携带请求头

常见的HTTP头部字段包括：

Host：指定目标服务器的域名或IP地址。

User-Agent：标识发送请求的用户代理（通常是浏览器）。

PC浏览器/APP浏览器/Linux/macOS

Accept：指定客户端能够接收的内容类型。

Content-Type：指定请求或响应中实体的媒体类型。

Content-Length：指定实体主体的长度（以字节为单位）。

Cookie：向服务器传递保存在客户端的cookie信息。

Cache-Control：指定如何缓存和重新验证响应。

Referer：大型网站通常都会根据该参数判断请求的来源

（1）携带请求载体headers

- headers又称载体标识
- 添加headers(浏览器会识别请求头,不加可能会被拒绝访问,
- 比如访问https://www.zhihu.com/explore)

不携带请求载体标识 User-Agent

```python
import requests

response = requests.get('https://www.zhihu.com/explore')
response.status_code  # 500
```

主动携带请求载体标识 User-Agent

手动携带 User-Agent

```python
import requests

headers = {
    'User-Agent': 'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2490.76 Mobile Safari/537.36',
}
respone = requests.get('https://www.zhihu.com/explore',
                       headers=headers)
print(respone.status_code)  # 200
```

随机生成

```python
# 安装模块 pip install fake-useragent

# 导入模块
from fake_useragent import UserAgent

# 随机生成UA标识
user_agent = UserAgent().random
import requests
# 导入模块
from fake_useragent import UserAgent

headers = {
    'User-Agent': UserAgent().random,
}
respone = requests.get('https://www.zhihu.com/explore',
                       headers=headers)
print(respone.status_code)  # 200
```

（2）携带 请求参数params

方式一：使用urlencode对参数进行编码

```python
import requests
from urllib.parse import urlencode

keyword = {
    'wd': '美女'
}

# 如果查询关键词是中文或者有其他特殊符号，则不得不进行url编码
# 指定成关键字进行编码
params = urlencode(keyword, encoding='utf-8')
print(params)  # wd=%E7%BE%8E%E5%A5%B3

# 然后拼接成url
url = 'https://www.baidu.com/s?wd=%s' % params

# 定义请求头参数
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.75 Safari/537.36',
}

# 对目标地址发起 GET 请求，获取响应数据
response = requests.get(url, headers=headers)

# 获取响应数据的文本类型
page_text = response.text
print(page_text)

# 打开本地的 HTML 文件写入当前请求到的源码数据
with open('a.html', 'w', encoding='utf-8') as f:
    f.write(page_text)
```

方式二：使用requests模块内置的方法

本质上也是调用urlencode

```python
import requests

# 指定目标地址
url = 'https://www.baidu.com/s'
# 构造请求参数字典
params = {
    'wd': '美女',
}

# 构造请求字典
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.75 Safari/537.36',
}

# 对指定的目标地址发起 GET 请求 ， 并携带请求参数和请求头
response = requests.get(
    # 指定路由地址
    url,
    # 指定GET请求携带的参数：一定要注意是字典类型的键值对数据才能完成自动编码，内部原理还是 urlencode 但是自动帮我们完成
    params=params,
    # 携带请求头参数
    headers=headers
)

# 获取当前请求获取到的响应数据 --- 文本类型
page_text = response.text
print(page_text)

# 打开文件，保存请求到的页面源码数据
with open('b.html', 'w', encoding='utf-8') as f:
    f.write(page_text)
```

验证结果，打开a.html与b.html页面内容一样

（3）携带标识 **cookies**

登录京东，然后从浏览器中获取cookies

以后就可以直接拿着cookie登录了，无需输入用户名密码

方式一：携带在请求中

切分cookies

```python
def split_cookies(cookies):
    first_data_start = cookies.split(";")
    return {key: value for key, value in [item.split("=") for item in first_data_start]}
# 导入requests模块
import requests

# 伪装请求头
from fake_useragent import UserAgent


# 对 cookies 参数进行切分成字典格式键值对
def split_cookies(cookies):
    first_data_start = cookies.split(";")
    return {key: value for key, value in [item.split("=") for item in first_data_start]}


# 定义请求头参数
headers = {
    "User-Agent": UserAgent().random,
}

# 指定目标地址
tag_url = "https://search.jd.com/Search?keyword=%E6%89%8B%E6%9C%BA"

# 【1】不携带 cookie 发起请求
response_no_cookie = requests.get(url=tag_url, headers=headers)
# 结果为请求不到指定的数据
print(response_no_cookie.text)

# 【2】携带 cookies 发起请求

# （1）从浏览器中获取cookie
cookies = "..."

# （2）切分成 cookie 为字典格式
cookies = split_cookies(cookies=cookies)
# （3）发起请求
response_cookie = requests.get(url=tag_url, headers=headers, cookies=cookies)
# 可以请求到详细的数据
print(response_cookie.text)
```

方式二：携带在请求参数中

```python
# 导入requests模块
import requests

# 伪装请求头
from fake_useragent import UserAgent

# 定义请求头参数
# 携带 cookies 发起请求
headers = {
    "User-Agent": UserAgent().random,
    # （1）从浏览器中获取cookie
    "Cookie": "..."
}

# （2）指定目标地址
tag_url = "https://search.jd.com/Search?keyword=%E6%89%8B%E6%9C%BA"

# （3）发起请求
response_cookie = requests.get(url=tag_url, headers=headers)
# 可以请求到详细的数据
print(response_cookie.text)
```

## 2.3 发送POST请求

- 数据不会出现在地址栏中
- 数据的大小没有上限
- 有请求体
-  请求体中如果存在中文，会使用URL编码！

requests.post()用法与requests.get()完全一致

特殊的是requests.post()有一个data参数，用来存放请求体数据

## 2.4 POST请求之携带参数

1. 目标网址

http://www.aa7a.cn/user.php

2. 抓包分析

抓包分析可以得到当前登录的请求地址和请求方式

3. 第一次尝试

携带UA，目标地址

```python
# 导入requests模块
import requests

# 伪装请求头
from fake_useragent import UserAgent

# 定义目标路由地址
tag_url = "http://www.aa7a.cn/user.php"

# 定义请求头参数
headers = {
  'User-Agent': UserAgent().random
}

# 定义请求体数据
data = {
  # 自己的用户名
  'username': 'username',
  # 自己的密码
  'password': 'password',
  # 页面验证码
  'captcha': 'captcha',
  'remember': 1,
  'ref': 'http://www.aa7a.cn',
  'act': 'act_login'
}

# 对目标地址发起请求
# post 请求携带请求体可以有两种方式
# 一种是 data=data
response = requests.post(url=tag_url, data=data, headers=headers)
# 一种是 json=data
# response = requests.post(url=tag_url, data=data, headers=headers)

# 打印响应数据
print(response.text)
# {"error":0,"ref":"http://www.aa7a.cn/user.php"}

# 观察发现响应的数据应该为验证码错误 ，而不是当前的错误
```

4. 二次分析包

请求头中增加参数

5. 二次尝试

```python
# 导入requests模块
import requests

# 伪装请求头
from fake_useragent import UserAgent

# 定义目标路由地址
tag_url = "http://www.aa7a.cn/user.php"

# 定义请求头参数
headers = {
    'User-Agent': UserAgent().random,
    'Referer': 'http://www.aa7a.cn/user.php?&ref=http%3A%2F%2Fwww.aa7a.cn',
    # 通过尝试，只携带上面的参数即可
    # 'Host': 'www.aa7a.cn',
    # 'Origin': 'http://www.aa7a.cn',
}

# 定义请求体数据
data = {
    # 自己的用户名
    'username': 'username',
    # 自己的密码
    'password': 'password',
    # 页面验证码
    'captcha': 'captcha',
    'remember': 1,
    'ref': 'http://www.aa7a.cn',
    'act': 'act_login'
}

# 对目标地址发起请求
response = requests.post(url=tag_url, data=data, headers=headers)

# 打印响应数据
print(response.text)
# {"error":5}

# 与页面上的数据一致
```

6. 小结：POST请求携带数据的两种方式

json=data

- 当使用requests.post方法时
- 如果将data参数设置为一个字典，并同时将headers参数中的Content-Type设置为application/json，那么data字典将被自动序列化为JSON字符串，并作为请求的主体数据发送。
- 这样的请求方式常用于与服务器交互时，需要使用JSON格式进行数据传输的情况。

data=data

- 默认情况下，requests.post方法将会将data参数以application/x-www-form-urlencoded格式进行编码。
- 这种编码方式将字典数据转换成键值对的形式，并使用&符号进行连接。
- 然后，将生成的字符串作为请求的主体数据发送到服务器。这种方式常用于处理表单提交的场景。

## 2.5 POST请求应用之登陆github

对于登录来说，应该输错用户名或密码然后分析抓包流程

用脑子想一想，输对了浏览器就跳转了，还分析个毛线，累死你也找不到包

1. 目标站点分析

（1）浏览器输入

https://github.com/login

（2）然后输入错误的账号密码

抓包发现登录行为是post提交到

https://github.com/session

（3）分析请求体

请求头包含cookie

请求体包含

```python
commit: Sign in
authenticity_token: mk40HAZuIAeMeBvz0O8fd5c3Y5Q4VHUsz/6h17BLyuDO070ieKIBU/LpjziBKiMnggpRk0G3NPqDKM2/18/1sA==
login: username
password: password
webauthn-conditional: undefined
javascript-support: true
webauthn-support: supported
webauthn-iuvpaa-support: supported
return_to: https://github.com/login
allow_signup: 
client_id: 
integration: 
required_field_fd11: 
timestamp: 1711094788024
timestamp_secret: 1e14e74fc401d1278cb3efaa8db5edb0f480fb4b9d6a47e001c6501d3f65f817
```

2. 流程分析

（1）先GET

- https://github.com/login
- 拿到初始cookie与authenticity_token

（2）再POST

- https://github.com/session
- 带上初始cookie，带上请求体（authenticity_token，用户名，密码等）
- 最后拿到登录cookie

ps：如果密码时密文形式，则可以先输错账号，输对密码，然后到浏览器中拿到加密后的密码，github的密码是明文

3. 代码实现

（2）第一次请求

```python
# 导入requests模块
import requests

# 伪装请求头
from fake_useragent import UserAgent

import re

# 定义目标路由地址
tag_url = "https://github.com/login"

# 定义请求头参数
headers = {
    'User-Agent': UserAgent().random,
}


def get_authenticity_token():
    # 对目标地址发起请求
    response = requests.get(url=tag_url, headers=headers, verify=False)

    # 拿到初始cookie(未被授权)
    response_cookie = response.cookies.get_dict()

    # 从页面中拿到CSRF TOKEN
    authenticity_token = re.findall(r'name="authenticity_token".*?value="(.*?)"', response.text)[0]
    return authenticity_token, response_cookie
```

（2）第二次请求

- 带着初始cookie和TOKEN发送POST请求给登录页面 

- 带上账号密码

```python
# 导入requests模块
import requests

# 伪装请求头
from fake_useragent import UserAgent

import re

# 定义目标路由地址
tag_url = "https://github.com/login"

# 定义请求头参数
headers = {
    'User-Agent': UserAgent().random,
}


def get_authenticity_token():
    # 对目标地址发起请求
    response = requests.get(url=tag_url, headers=headers, verify=False)

    # 拿到初始cookie(未被授权)
    response_cookie = response.cookies.get_dict()

    # 从页面中拿到CSRF TOKEN
    authenticity_token = re.findall(r'name="authenticity_token".*?value="(.*?)"', response.text)[0]
    return authenticity_token, response_cookie


def get_login_cookie():
    authenticity_token, response_cookie = get_authenticity_token()
    # 定义请求参数
    data = {
        'commit': 'Sign in',
        'utf8': '✓',
        'authenticity_token': authenticity_token,
        'login': 'username',
        'password': 'password'
    }

    # 发起 post 请求
    response = requests.post(tag_url,
                             data=data,
                             cookies=response_cookie
                             )

    # 获取登陆成功后的cookie
    login_cookie = response.cookies.get_dict()
    return login_cookie
```

（3）第三次请求

- 以后的登录，拿着login_cookie就可以 

- 比如访问一些个人配置

```python
def get_response():
    login_cookie = get_login_cookie()
    response = requests.get('https://github.com/settings/emails',
                            cookies=login_cookie)

    print('username' in response.text)  # True
```

（4）完整

```python
# 导入requests模块
import requests

# 伪装请求头
from fake_useragent import UserAgent

import re

# 定义目标路由地址
tag_url = "https://github.com/login"

# 定义请求头参数
headers = {
    'User-Agent': UserAgent().random,
}


# 第一次请求
def get_authenticity_token():
    # 对目标地址发起请求
    response = requests.get(url=tag_url, headers=headers, verify=False)

    # 拿到初始cookie(未被授权)
    response_cookie = response.cookies.get_dict()

    # 从页面中拿到CSRF TOKEN
    authenticity_token = re.findall(r'name="authenticity_token".*?value="(.*?)"', response.text)[0]
    return authenticity_token, response_cookie


# 第二次请求：带着初始cookie和TOKEN发送POST请求给登录页面，带上账号密码
def get_login_cookie():
    authenticity_token, response_cookie = get_authenticity_token()
    # 定义请求参数
    data = {
        'commit': 'Sign in',
        'utf8': '✓',
        'authenticity_token': authenticity_token,
        'login': 'username',
        'password': 'password'
    }

    # 发起 post 请求
    response = requests.post(tag_url,
                             data=data,
                             cookies=response_cookie
                             )

    # 获取登陆成功后的cookie
    login_cookie = response.cookies.get_dict()
    return login_cookie


# 第三次请求：以后的登录，拿着login_cookie就可以,比如访问一些个人配置
def get_response():
    login_cookie = get_login_cookie()
    response = requests.get('https://github.com/settings/emails',
                            cookies=login_cookie)

    print('username' in response.text)  # True
```

4. POST请求补充

```python
# 没有指定请求头,#默认的请求头:application/x-www-form-urlencoed
requests.post(url='xxxxxxxx',
              data={'xxx': 'yyy'})  

# 如果我们自定义请求头是application/json,并且用data传值, 则服务端取不到值
requests.post(url='',
              data={'': 1, },
              headers={
                  'content-type': 'application/json'
              })

# 默认的请求头:application/json
requests.post(url='',
              json={'': 1, },
              )
```

## 2.6 自动携带cookie 的session对象

- 参考博客：[https://www.cnblogs.com/dream-ze/p/17176494.html](https:_www.cnblogs.com_dream-ze_p_17176494)

（1）不使用session

```python
# 导入 requests 模块
import requests
# 导入伪装UA请求头
from fake_useragent import UserAgent

# 定义请求头
headers = {
    "User-Agent": UserAgent().random,
}


def get_cookies():
    # 定义目标地址
    tag_url = "https://xueqiu.com/"

    # 发起请求获取响应对象
    response = requests.get(url=tag_url, headers=headers)

    # 获取Cookie参数
    cookie_params = dict(response.cookies)

    # print(cookie_params)
    # {'cookiesu': '531711090956851', 'u': '531711090956851', 'xq_a_token': '01b99d82fffd2faf8b614e98a00cbb35d6c7ddcf', 'xq_id_token': 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1aWQiOi0xLCJpc3MiOiJ1YyIsImV4cCI6MTcxMzY2MDQyOCwiY3RtIjoxNzExMDkwOTAxODEyLCJjaWQiOiJkOWQwbjRBWnVwIn0.l_DjE-AuBBfHd9jusczif8BDef5ApTqEY3k1HQlZuG4GchCnNoegnoltyKERKzXOz3rMYggOamBc2g5M3kvM8tDvnfixHXatCrVaq0h8sygh3rqzGraoaPaCT5tFoalGOldhNzBdViXBjWQi7zKJvHj8B7Qdl-vtnjCnAUtKtr989lMbpjMLs0_XrVbx6a-PiLHn9X37pgdya-UQ3sTT0mRYeOv1q7StVRh_tGU829SIFlKvyYWRRmU4bo3KOutN2sdxixpr_w-GMEvSSQHWG26R7uGSVOGbWP0MSB4QRTEY6VoVSHdaoYPpsHZvRlexA9ZQ4kvzhOACHHAkQE033A', 'xq_r_token': '7fe9b3213c399b15eee3c5bca4841433a03128a6', 'xqat': '01b99d82fffd2faf8b614e98a00cbb35d6c7ddcf', 'acw_tc': '2760826617110909568441159ed1c3d4d3756a8dcb29b019731e2f7b1878e1'}
    return cookie_params


def get_data():
    # 【一】获取 cookies 参数
    cookie_params = get_cookies()
    # 【二】定义目标网址
    tag_url = "https://stock.xueqiu.com/v5/stock/batch/quote.json?symbol=SH000001,SZ399001,SZ399006,SH000688,SH000016,SH000300,BJ899050,HKHSI,HKHSCEI,HKHSTECH,.DJI,.IXIC,.INX"
    # 【三】携带 Cookie 获取指定数据
    # response = requests.get(url=tag_url, headers=headers)
    # （1）当我们不携带 cookies 时，返回的数据是
    # {'error_description': '遇到错误，请刷新页面或者重新登录帐号后再试', 'error_uri': '/v5/stock/batch/quote.json', 'error_data': None, 'error_code': '400016'}

    # （2）携带 cookie  请求数据
    response = requests.get(url=tag_url, headers=headers, cookies=cookie_params)
    # [1] json 格式的数据
    # data_json = response.json()
    # print(data_json)
    # [2] 二进制格式的数据
    data_bytes = response.content
    # 对二进制数据进行解码
    data_bytes = data_bytes.decode('utf-8')
    print(data_bytes)


get_data()
```

（2）使用session

```python
# 导入 requests 模块
import requests
# 导入伪装UA请求头
from fake_useragent import UserAgent

# 定义请求头
headers = {
    "User-Agent": UserAgent().random,
}

# 创建 session 对象
session = requests.Session()


def get_cookies():
    # 定义目标地址
    tag_url = "https://xueqiu.com/"

    # 发起请求获取响应对象
    response = session.get(url=tag_url, headers=headers)

    # 获取Cookie参数
    cookie_params = dict(response.cookies)

    # print(cookie_params)
    return cookie_params


def get_data():
    # 【一】获取 cookies 参数
    cookie_params = get_cookies()
    # 【二】定义目标网址
    tag_url = "https://stock.xueqiu.com/v5/stock/batch/quote.json?symbol=SH000001,SZ399001,SZ399006,SH000688,SH000016,SH000300,BJ899050,HKHSI,HKHSCEI,HKHSTECH,.DJI,.IXIC,.INX"
    # 【三】不用手动携带 cookie 即可获取指定的数据
    response = session.get(url=tag_url, headers=headers)

    # [1] json 格式的数据
    # data_json = response.json()
    # print(data_json)
    # [2] 二进制格式的数据
    data_bytes = response.content
    # 对二进制数据进行解码
    data_bytes = data_bytes.decode('utf-8')
    print(data_bytes)


get_data()
```

【补充】url编码和解码

- 参考博客：[https://www.cnblogs.com/dream-ze/p/17418964.html](https:_www.cnblogs.com_dream-ze_p_17418964)

## 2.7 长链转短链

（1）简解

- 长链转短链是一种将长URL转换为短URL的过程。
- 通常，通过将长URL映射到一个较短的标识符或者域名来实现。
- 这种转换可以提供一些实际的好处，如更美观的URL、节省空间和更方便的分享。

（2）示例

- 原始长链：https://www.cnblogs.com/******g/p/16005866.html
- 转短链服务：申请短域名（例如：m.tb.cn）
- 生成随机字符串：假设生成的字符串为9QqPdHKXc2n
- 在数据库中建立映射关系：将随机字符串与原始长链对应存储在数据库中，例如：

```plain
id   随机字符串     真正地址
1    9QqPdHKXc2n   https://www.cnblogs.com/********g/p/16005866.html
```

- 返回短链给用户：https://m.tb.cn/h.5bZAfFS?tk=9QqPdHKXc2n
- 用户使用短链访问：当用户点击短链（https://m.tb.cn/h.5bZAfFS?tk=9QqPdHKXc2n）时，请求会发送到短链服务。
- 获取随机字符串：短链服务从URL中提取随机字符串（9QqPdHKXc2n）。
- 查询数据库：短链服务在数据库中查找对应的真正地址。
- 重定向到真正地址：短链服务将用户重定向到与随机字符串对应的真正地址（https://www.cnblogs.com/********g/p/16005866.html），实现长链到短链的跳转。

- 通过这个过程，可以将原始长链转换为一个更短、易于分享和记忆的短链，并且当用户点击短链时能够实现跳转到原始长链。

（3）注意事项

- 长链转短链需要使用合法的短域名，并在后台通过数据库进行映射管理以确保正确的跳转。
- 此外，短链服务需要处理并发请求，有效地检索数据库并进行重定向操作，确保系统的性能和稳定性。



# 3 响应

## 3.1 响应体数据格式

1. 字符串格式

`response.text`: 将响应体转换为字符串形式。

```python
import requests

response = requests.get('https://pic.netbian.com/uploads/allimg/240322/232300-171112098057a5.jpg')

# `response.text`: 将响应体转换为字符串形式。
data = response.text
print(data, type(data))
# 很多数据，但是是文本类型的
```

2. 二进制格式

`response.content`: 获取响应体的二进制内容，适用于处理图像、视频等非文本类型的响应。（默认是16进制）

```python
import requests

response = requests.get('https://pic.netbian.com/uploads/allimg/240322/232300-171112098057a5.jpg')

# `response.content`: 获取响应体的二进制内容，适用于处理图像、视频等非文本类型的响应。（默认是16进制）
data = response.content
print(data, type(data))
# 很多数据，但是是二进制类型的
```

3. json格式

要解析 JSON 数据，可以使用 `requests` 模块的 `json()` 方法。

该方法将响应内容解析为 JSON 格式，并返回一个 Python 字典对象。

```python
import requests

# 定义目标地址
tag_url = "https://jsonplaceholder.typicode.com/users"

# 发送请求，获取响应数据
response = requests.get(tag_url)

# 获取文本类型的响应数据
page_text = response.text

# 打印响应内容的数据类型
print(type(page_text))

# 打印原始响应内容
print(page_text)

# 将响应内容解析为 JSON 格式
data = response.json()

# 打印响应内容的数据类型
print(type(data))

# 打印解析后的 JSON 数据
print(data)
```

在上述代码中，假设 tag_url 是您要发送请求的目标 URL。

首先，我们使用 `requests` 模块的 `get()` 方法发送请求并获取响应。

然后，我们打印响应内容的数据类型和原始内容。

接下来，我们使用 `json()` 方法对响应内容进行解析并将其存储在变量 `data` 中。

最后，我们打印解析后的 JSON 数据。

请注意，如果响应内容不符合 JSON 格式，或者您尝试解析的响应内容不是有效的 JSON 数据，那么调用 `json()` 方法时将引发 `JSONDecodeError` 异常。

因此，在实际使用中，建议添加适当的错误处理来处理可能发生的异常情况。

## 3.2 响应体编码格式

`response.encoding`：获取响应的编码方式

```python
import requests

response = requests.get('https://www.baidu.com/')

# `response.encoding`：获取响应的编码方式
print(response.encoding)
# ISO-8859-1
# 查看到当前页面的默认编码格式为 ISO-8859-1
```

## 3.3 响应状态码

`response.status_code`: 获取响应的状态码。

```python
import requests

response = requests.get('https://www.baidu.com/')

# `response.status_code`: 获取响应的状态码。
print(response.status_code)
# 200
```

## 3.4 响应头

`response.headers`: 获取响应头信息，返回一个字典对象。

```python
import requests

response = requests.get('https://www.baidu.com/')

# `response.headers`: 获取响应头信息，返回一个字典对象。
print(response.headers)
# {'Cache-Control': 'private, no-cache, no-store, proxy-revalidate, no-transform', 'Connection': 'keep-alive', 'Content-Encoding': 'gzip', 'Content-Type': 'text/html', 'Date': 'Fri, 22 Mar 2024 07:14:04 GMT', 'Last-Modified': 'Mon, 23 Jan 2017 13:23:55 GMT', 'Pragma': 'no-cache', 'Server': 'bfe/1.0.8.18', 'Set-Cookie': 'BDORZ=27315; max-age=86400; domain=.baidu.com; path=/', 'Transfer-Encoding': 'chunked'}
```

## 3.5 响应Cookie

`response.cookies`: 获取服务器返回的cookie信息。

`response.cookies.get_dict()`: 将cookie信息转换为字典形式。

`response.cookies.items()`: 获取cookie信息并以列表形式返回。

```python
import requests

response = requests.get('https://www.baidu.com/')

# `response.cookies`: 获取服务器返回的cookie信息。
print(response.cookies)
# <RequestsCookieJar[<Cookie BDORZ=27315 for .baidu.com/>]>

# `response.cookies.get_dict()`: 将cookie信息转换为字典形式。
print(response.cookies.get_dict())
# {'BDORZ': '27315'}

# `response.cookies.items()`: 获取cookie信息并以列表形式返回。
print(response.cookies.items())
# [('BDORZ', '27315')]
```

## 3.6 当前请求/响应对象的URL

`response.url`: 获取当前响应的URL。

- 这是在完成HTTP请求并接收到服务器响应后，实际返回的资源URL。
- 在重定向发生时，这个属性会反映最终页面的实际URL。
- 例如，如果你发起一个请求到某个网站A，但该网站随后重定向到了网站B，那么`response.url`将显示网站B的URL。

`response.request.url`: 获取当前请求的URL。

- 这是发送HTTP请求时使用的原始URL，即你在发出请求时指定的URL。
- 无论是否发生重定向，这个属性始终保持不变。
- 也就是说，它反映了你最初尝试访问的地址。

总结来说

- 没有重定向：两者通常是一致的。

- 有重定向：

  `response.url` 是重定向后的最终URL

  `response.request.url` 则是最初的请求URL。

```python
import requests

response = requests.get('https://www.baidu.com/')

# `response.url`: 获取当前请求的URL。
print(response.url)
print(response.request.url)
```

## 3.7 当前请求的重定向URL

`response.history`: 如果有重定向，返回一个列表，包含所有经过的重定向URL。

```python
import requests

response = requests.get('https://www.baidu.com/')

# `response.history`: 如果有重定向，返回一个列表，包含所有经过的重定向URL。
print(response.history)
# []
```

## 3.8 迭代获取二进制数据

`response.iter_content()` ：迭代获取响应内容（适用于处理视频、图片等二进制数据）

```python
# 导入requests库
import requests

# 定义目标地址：该地址是一张图片，图片属于二进制数据
tag_url = "https://pic.netbian.com/uploads/allimg/240322/232300-171112098057a5.jpg"

# 向目标地址发起请求
response = requests.get(tag_url)

# 打开本地的文件路径,写入二进制数据必须用 wb 模式
file_path = "./a.jpg"
with open(file_path, 'wb') as f:
    # 循环获取响应数据中的二进制数据，每次只获取 1024 个字节
    for chunk in response.iter_content(chunk_size=1024):
        # 如果存在数据，则进行文件数据的写入
        if chunk:
            f.write(chunk)
```



# 4 session

## 4.1 目标网址

http://download.java1234.com/

## 4.2 目标包分析

1. 请求登陆接口

打开开发者工具

选择网络，输入用户名和密码

观察数据包

2. 分析登陆接口

观察发现，点击登陆后会有一个 login 的登录包发送过去

在登录包的载荷中携带者我们的用户名和密码

观察这个登陆的数据包我们会发现在数据包的右侧有一个 Cookie 参数

而在 Cookie 会有一个响应的Cookie `Reponse Cookie`

## 4.3 模拟登陆

1. 未携带Cookie

当我们登陆之后我们可以在页面上看到个人中心对应的按钮

点击个人中心后会切换到个人中心页面

在未携带Cookie信息请求个人中心页面时我们回去到的页面源码是首页的源码

```python
import requests

target_url = "http://download.java1234.com/toUserCenterPage"

response = requests.get(url=target_url)

page_text = response.text

with open("self_page.html", "w", encoding="utf-8") as fp:
    fp.write(page_text)
```

2. 携带Cookie

使用 session 对象保持会话 Cookie 信息 

再次请求个人中心页面时，会得到登陆之后才能看到的页面源码

```python
import requests

# 创建会话保持对象 Session
session = requests.Session()

# 定义目标网址
target_url = "http://download.java1234.com/user/login"

# 携带请求体数据
data = {
    'userName': 'dm2068946849',
    'password': 'dm2068946849'
}

# 使用 session 对象 模拟登陆发起请求 获取 Cookie 信息
# 并将 Cookie 信息保存到 Session 会话对象中
session.post(url=target_url, data=data)

# 使用携带有 Cookie 信息的 session 对象请求 网页的个人中心数据

target_url = "http://download.java1234.com/toUserCenterPage"

response = session.get(url=target_url)

page_text = response.text

with open("self_page.html", "w", encoding="utf-8") as fp:
    fp.write(page_text)
```



# 5 代理

## 5.1 什么是代理

在网络爬虫和数据抓取的过程中，我们经常需要发送HTTP请求来获取网页内容或与远程服务器进行通信。

然而，在某些情况下，直接发送请求可能会受到限制或被阻止，这时就需要借助代理来完成任务。

代理在网络通信中起到中间人的作用，它代表我们与目标服务器建立连接并传递请求和响应。

## 5.2 代理服务器的作用

通过使用代理，我们可以隐藏真实的IP地址、绕过访问限制，并增加请求的匿名性。

Python中的requests库提供了便捷且强大的功能来处理HTTP请求，并且支持代理的配置。

## 5.3 为什么要使用代理

有些时候，需要对网站服务器发起高频的请求，网站的服务器会检测到这样的异常现象，则会讲请求对应机器的ip地址加入黑名单，则该ip再次发起的请求，网站服务器就不在受理，则我们就无法再次爬取该网站的数据。

使用代理后，网站服务器接收到的请求，最终是由代理服务器发起，网站服务器通过请求获取的ip就是代理服务器的ip，并不是我们客户端本身的ip。

一般网站都有屏蔽的限制策略，用自己的IP去爬，被封了那该网站就访问不了，这时候就得用代理IP来解决问题了。

反正封的不是本机IP，封的代理IP。

## 5.4 代理的匿名度

1. 透明

网站的服务器知道你使用了代理，也知道你的真实IP

2. 匿名

网站服务器知道你使用了代理，但是无法知道你的真实IP

3. 高匿(推荐)

网站服务器不知道你使用了代理，也不知道你的真实IP

## 5.5 代理的类型

1. HTTP

该类型的代理服务器只可以转发 http 请求

2. HTTPS

可以转发 https 协议的请求

## 5.6 代理配置方法

1. 使用proxies参数设置全局代理

requests库提供了一个名为proxies的参数，通过它可以设置全局代理。

我们可以将代理配置作为一个字典传递给该参数，字典的键是代理类型（如'http'、'https'等），值是代理的地址和端口号。

proxies参数语法格式

```python
proxies={"协议":"协议://IP:PORT"}
```

示例

```python
import requests
proxies = {
    'http': 'http://127.0.0.1:8080',
    'https': 'https://127.0.0.1:8080'
}
 
response = requests.get(url, proxies=proxies)
```

2. 使用session对象设置会话级别的代理

若你需要在多个请求之间保持相同的代理设置，可以使用requests库中的Session对象。

Session对象允许你在会话级别上保持一些参数，包括代理设置。

```python
import requests
 
session = requests.Session()
session.proxies = {
    'http': 'http://proxy.example.com:8080',
    'https': 'https://proxy.example.com:8080'
}
 
response = session.get(url)
```

在这种情况下，创建的Session对象将会持续保持代理设置，直到我们显式地修改或重置它。

对于需要在多个请求中使用相同代理的情况非常有用，例如爬取网站的多个页面时。

请注意，无论是使用全局代理还是会话级别的代理，都要确保代理地址和端口号的正确性，并根据实际情况选择http或https类型的代理。

此外，如果代理需要验证身份，你还需要提供相应的用户名和密码。

## 5.7 携带代理示例

1. 测试本机IP

- 访问测试网站 http://www.cip.cc 即可看到自己的本机IP

```python
import requests
from lxml import etree
from fake_useragent import UserAgent

target_url = 'http://www.cip.cc'

headers = {
    "User-Agent": UserAgent().random
}

response = requests.get(url=target_url, headers=headers, timeout=1)

page_text = response.text

tree = etree.HTML(page_text)

text = tree.xpath('/html/body/div/div/div[3]/pre/text()')[0]

ip = [item.strip().split(":") for item in [i.strip() for i in text.split("\n")] if item][0][1].strip()

print(ip)
```

- 访问测试网站 http://httpbin.org/get 也可以看到自己的本机IP

```python
import requests
from fake_useragent import UserAgent

# 定义目标地址
url = 'http://httpbin.org/get'

# 定义请求头参数
headers = {
    "User-Agent": UserAgent().random
}

# 获取响应对象
response = requests.get(url=url, headers=headers)

# 检查请求是否成功
if response.status_code == 200:
    # 处理响应内容
    response.encoding = 'utf-8'  # 设置响应内容的编码格式为utf-8
    # 解析JSON结果
    data = response.text  # 获取响应信息
    print(data)
else:
    print("请求失败:", response.status_code)
{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Host": "httpbin.org", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0", 
    "X-Amzn-Trace-Id": "Root=1-6602ae76-293b7ac268c637c025f19eb4"
  }, 
  "origin": "116.237.218.139", 
  "url": "http://httpbin.org/get"
}
```

2. 付费代理 - 芝麻HTTP

参考平台芝麻HTTP

https://jahttp.zhimaruanjian.com/getapi/

填参数获取 IP 代理

访问指定链接 即可提取到 IP 数据

携带IP请求目标网址

3. 免费代理

封的快 每次去找新的

```python
import requests
from fake_useragent import UserAgent

# 定义目标地址
url = 'http://httpbin.org/get'

# 定义请求头参数
headers = {
    "User-Agent": UserAgent().random
}

# 设置代理地址
proxies = {
    'http': 'http://117.69.236.184:8089'
}

# 获取响应对象
response = requests.get(url=url, headers=headers, proxies=ip_item)

# 检查请求是否成功
if response.status_code == 200:
    # 处理响应内容
    response.encoding = 'utf-8'  # 设置响应内容的编码格式为utf-8
    # 解析JSON结果
    data = response.text  # 获取响应信息
    print(data)
else:
    print("请求失败:", response.status_code)
```

```json
{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Host": "httpbin.org", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0", 
    "X-Amzn-Trace-Id": "Root=1-6602ae76-293b7ac268c637c025f19eb4"
  }, 
  "origin": "117.69.236.184", 
  "url": "http://httpbin.org/get"
}
```

## 5.8 批量抓取代理脚本

```python
# 导入模块
# 发起requests请求
import requests  

# 使用XPath解析页面数据
from lxml import etree  

 # 使用你的程序来计算随机的UserAgent
from fake_useragent import UserAgent 

# UA 伪装 headers
headers = {
    'User-Agent': UserAgent().random
}


def get_proxy():
    '''
    url : {89免费代理的url：https://www.89ip.cn/index_2.html}
    :return: 返回所有爬取到的ip列表
    '''
    # 存放所有代理存在的页码
    page_url_lists = []
    # 存放爬取到的所有代码的列表
    ip_lists = []
    # 循环获取每一页的代理页面的url
    for i in range(1, 5):
        if i == 1:
            # 首页的url
            page_url = 'https://www.89ip.cn/'
            # 将代理页url存储到类可用列表里
            page_url_lists.append(page_url)
        else:
            # 处理第二页及以后所有页码的规则
            page_url = f'https://www.89ip.cn/index_{i}.html'
            # 将代理页url存储到类可用列表里
            page_url_lists.append(page_url)
    # 从存储所有代理存在的页面的列表，循环发起请求获得代理ip和端口
    for pages_url in page_url_lists:
        response = requests.get(pages_url, headers=headers)

        response.encoding = 'utf-8'
        page_text = response.text
        tree = etree.HTML(page_text)
        # xpath提取到所有的代理ip列表
        tr_lists = tree.xpath('//div[3]/div[1]/div/div[1]/table/tbody/tr')
        # 循环获取每一个代理ip和端口
        for tr in tr_lists:
            # 获取ip并做数据清洗
            ip = tr.xpath('./td[1]/text()')[0].strip()
            # 获取端口并做数据清洗
            port = tr.xpath('./td[2]/text()')[0].strip()
            # 将获取到的代理ip和端口拼接
            ip_port = 'http://' + ip + ':' + str(port)
            # 将获取到的代理ip和端口存储到类可用列表里
            ip_lists.append({'http': ip_port})
    # 返回所有代理ip列表
    return ip_lists


def text_ip():
    '''
    url：{百度首页，}
    :return: 做测试返回状态码为200正常可用保留，否则再次执行验证
    '''
    url = "https://www.baidu.com/"
    # 获取到所有ip列表
    ip_lists = get_proxy()
    # 循环测试每一个ip和端口是否可用
    for ip_item in ip_lists:
        response = requests.get(url, proxies=ip_item, headers=headers, timeout=10)
        ret = response.status_code
        # 做判断是否返回值为200可用
        if ret == 200:
            print(f'当前状态码为:::{ret},ip可用,返回ip:::{ip_item}')
            return ip_item
        else:
            print(f'当前状态码为:::{ret},当前ip不可用,重新测试ip中')
            text_ip()


if __name__ == '__main__':
    ip_lists = get_proxy()
    ip_item = text_ip()
    url = 'https://www.baidu.com/'
    response = requests.get(url=url, headers=headers, proxies=ip_item)
    print(response.text)
```



# 6 ssl认证

## 6.1 引言

有些网站没有设置好 HTTPS 证书，或者 HTTPS 证书不被 CA 机构认可，这些网站就会出现 SSL 证书错误提示。

在 `requests` 模块中进行 SSL 认证时，可以通过 `verify` 参数来控制是否验证服务器的 SSL 证书。

默认情况下，`verify` 参数被设置为 `True`，表示会验证 SSL 证书。

当 `verify` 设置为布尔值 `False` 时，会忽略证书验证，但会引发警告。

## 6.2 验证证书报错演示

```python
import requests
url = 'https://ssr2.scrape.center/'
response = requests.get(url)
print(response.status_code)
SSLError：
requests.exceptions.SSLError: HTTPSConnectionPool(host='ssr2.scrape.center', port=443): Max retries exceeded with url: / (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1051)')))
```

## 6.3 不验证证书

```python
import requests
url = 'https://ssr2.scrape.center/'
response = requests.get(url=url, verify=False)
print(response.status_code)
InsecureRequestWarning: Unverified HTTPS request is being made to host 'ssr2.scrape.center'. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#tls-warnings
  warnings.warn(
200
```

报出 InsecureRequestWarning 警告，但得到了请求成功的状态码

## 6.4 自定义证书路径

如果您希望使用自定义的证书进行验证，可以将 `verify` 参数设置为证书文件的路径。例如：

```python
import requests

cert_file = "/path/to/my_certificate.pem"

# 发送请求，并使用自定义证书进行 SSL 验证
response = requests.get(url, verify=cert_file)

# 打印响应状态码
print(response.status_code)

# 打印响应内容
print(response.text)
```

在上述代码中，将 `verify` 参数设置为自定义证书文件的路径。`requests` 将使用指定的证书进行验证。

请注意，在实际使用中，如果从未提供过自定义证书，并且要禁用验证，则最好提供证书路径。这样可以避免不必要的错误和警告。

## 6.5 小结

```python
import requests

# 发送请求，并禁用 SSL 证书验证
response = requests.get(url, verify=False)

# 打印响应状态码
print(response.status_code)

# 打印响应内容
print(response.text)
```

在上述代码中，我们使用 `requests` 模块的 `get()` 方法发送带有 URL 的请求。

将 `verify` 参数设置为 `False`，以禁用 SSL 证书验证。

注意：禁用 SSL 证书验证存在安全风险，因为它允许您的请求受到中间人攻击。

在生产环境中，强烈建议启用 SSL 证书验证，以确保通信的安全性。



# 7 超时

## 7.1 引言

网络请求不可避免会遇上请求超时的情况，在 requests 中，如果不设置你的程序可能会永远失去响应。

超时又可分为连接超时和读取超时。

在使用requests模块时，可以通过设置超时参数来控制请求的超时时间。

超时参数可以是一个浮点数或一个元组。

如果超时参数是一个浮点数，它表示接收数据的超时时间，单位为秒。

例如，`timeout=0.1`代表接收数据的超时时间为0.1秒。

如果超时参数是一个元组，它包含两个值，分别表示连接超时时间和接收数据的超时时间，单位同样为秒。

例如，`timeout=(0.1, 0.2)`代表连接超时时间为0.1秒，接收数据的超时时间为0.2秒。

## 7.2 连接超时

连接超时指的是在你的客户端实现到远端机器端口的连接时（对应的是`connect()`），Request等待的秒数。

```python
import time
import requests

# 定义目标地址
url = 'http://www.google.com.hk'

# 打印开始时间
print(time.strftime('%Y-%m-%d %H:%M:%S'))

# 异常捕获
try:
    # 尝试获取目标地址响应
    response = requests.get(url, timeout=5).text
    print('success')
# 如果遇到超时异常，则捕获并打印异常信息
except requests.exceptions.RequestException as e:
    print(e)

# 打印结束时间
print(time.strftime('%Y-%m-%d %H:%M:%S'))
```

因为 google 被墙了，所以无法连接，错误信息显示 connect timeout（连接超时）。

```python
# 2024-03-26 19:27:58
# HTTPConnectionPool(host='www.google.com.hk', port=80): Max retries exceeded with url: / (Caused by ConnectTimeoutError(<urllib3.connection.HTTPConnection object at 0x11c4a83d0>, 'Connection to www.google.com.hk timed out. (connect timeout=5)'))
# 2024-03-26 19:28:03
```

就算不设置，也会有一个默认的连接超时时间（测试大概是21秒）。

## 7.3 读取超时

读取超时指的就是客户端等待服务器发送请求的时间。（特定地，它指的是客户端要等待服务器发送字节之间的时间。在 99.9% 的情况下这指的是服务器发送第一个字节之前的时间）。

简单的说，连接超时就是发起请求连接到连接建立之间的最大时长，读取超时就是连接成功开始到服务器返回响应之间等待的最大时长。

如果你设置了一个单一的值作为 timeout，如下所示：

```python
response = requests.get('https://github.com', timeout=5)
```

这一 timeout 值将会用作 connect 和 read 二者的 timeout。

如果要分别制定，就传入一个元组：

```python
response = requests.get('https://github.com', timeout=(3.05, 27))
```

读取超时是没有默认值的，如果不设置，程序将一直处于等待状态。

我们的爬虫经常卡死又没有任何的报错信息，原因就在这里了。

```python
import requests

# 设置接收数据超时时间为0.1秒
response = requests.get('https://www.baidu.com', timeout=0.1)
print(response.status_code)

# 设置连接超时时间为0.1秒，接收数据超时时间为0.2秒
response = requests.get('https://www.baidu.com', timeout=(0.1, 0.2))
print(response.status_code)
```

通过这样的超时设置，可以限制请求的响应时间，避免长时间阻塞等待结果导致程序无法继续执行。

根据具体的需求，可以灵活调整超时时间来适应不同的网络环境和请求场景。

## 7.4 超时重试

一般超时我们不会立即返回，而会设置一个三次重连的机制。

```python
def gethtml(url):
    i = 0
    while i < 3:
        try:
            html = requests.get(url, timeout=5).text
            return html
        except requests.exceptions.RequestException:
            i += 1
```

其实 requests 已经帮我们封装好了。

```python
import time
import requests
from requests.adapters import HTTPAdapter

session = requests.Session()
# 设置 HTTP 请求最大重试次数为3
session.mount('http://', HTTPAdapter(max_retries=3))
# 设置 HTTPS 请求最大重试次数为3
session.mount('https://', HTTPAdapter(max_retries=3))

# 记录开始时间
print(time.strftime('%Y-%m-%d %H:%M:%S'))
try:
    # 发送 GET 请求，获取响应对象
    response = session.get('http://www.google.com.hk', timeout=5)
    page_text = response.text
    print(page_text)
except requests.exceptions.RequestException as e:
    print(e)
print(time.strftime('%Y-%m-%d %H:%M:%S'))
```

`max_retries` 为最大重试次数，重试3次，加上最初的一次请求，一共是4次，所以上述代码运行耗时是20秒而不是15秒

```python
# 2024-03-26 21:50:42
# HTTPConnectionPool(host='www.google.com.hk', port=80): Max retries exceeded with url: / (Caused by ConnectTimeoutError(<urllib3.connection.HTTPConnection object at 0x122d4d0c0>, 'Connection to www.google.com.hk timed out. (connect timeout=5)'))
# 2024-03-26 21:51:02
```



# 8 身份认证

## 8.1 官网链接

http://docs.python-requests.org/en/master/user/authentication/

## 8.2 认证设置

登陆网站是,弹出一个框，要求你输入用户名密码（与alter很类似），此时是无法获取html的

但本质原理是拼接成请求头发送

` r.headers['Authorization'] = _basic_auth_str(self.username, self.password)`

一般的网站都不用默认的加密方式，都是自己写

那么我们就需要按照网站的加密方式，自己写一个类似于_basic_auth_str的方法

得到加密字符串后添加到请求头

` r.headers['Authorization'] =func('.....')`

在使用requests模块进行认证设置时，可以通过HTTPBasicAuth类或简写方式来提供用户名和密码进行认证。

## 8.3 基本身份认证

许多需要身份验证的 Web 服务都接受 HTTP 基本身份验证。

这是最简单的类型，Requests 开箱即用地支持它。

1. 完整语法

可以使用requests.auth模块中的HTTPBasicAuth类来提供用户名和密码进行认证。

```python
from requests.auth import HTTPBasicAuth
import requests

# 创建一个使用 'user' 和 'pass' 进行基本身份验证的函数。
basic = HTTPBasicAuth('user', 'pass')

# 然后使用该函数进行请求。
response = requests.get('https://httpbin.org/basic-auth/user/pass', auth=basic)

# 输出响应的文本。
print(response.text)
# {
#   "authenticated": true, 
#   "user": "user"
# }

# 输出响应的状态码。
print(response.status_code)
# <Response [200]>
```

2. 简写语法

可以直接在auth参数中以元组的形式提供用户名和密码进行认证，无需导入HTTPBasicAuth类。

```python
import requests

# 携带认证参数，使用该函数进行请求。
response = requests.get('https://httpbin.org/basic-auth/user/pass', auth=('user', 'pass'))

# 输出响应的文本。
print(response.text)
# {
#   "authenticated": true, 
#   "user": "user"
# }

# 输出响应的状态码。
print(response.status_code)
# <Response [200]>
```

需要注意的是，上述示例代码中的'xxx'应替换为具体的URL地址，用于发送请求进行认证。

3. 小结

默认情况下，这两种方式都会使用HTTP基本认证（Basic Authentication），即将用户名和密码以base64编码形式拼接到请求头的Authorization字段中发送给服务器。

然而，大多数网站都会使用自定义的加密方式，所以需要根据网站的加密方式进行相应的处理，在认证之前生成正确的认证字符串，并将其添加到请求头的Authorization字段中。

例如，如果网站使用了自定义的加密方式，可以按照该方式编写一个自定义函数，并将生成的认证字符串通过r.headers['Authorization'] = 函数名('...')的方式添加到请求头中。

需要根据具体网站的加密方式和认证机制进行处理，可以参考相关网站的文档或联系网站开发人员获取详细的认证设置信息。

## 8.4 netrc身份验证

如果参数未给出身份验证方法，则请求将 尝试从 用户的 NETRC 文件。

netrc 文件覆盖原始 HTTP 身份验证标头 使用 headers= 设置。`auth`

如果找到主机名的凭据，则使用 HTTP Basic 发送请求 认证。

## 8.5 摘要式身份验证

另一种非常流行的 HTTP 身份验证形式是摘要式身份验证

```python
from requests.auth import HTTPDigestAuth
import requests

url = 'https://httpbin.org/digest-auth/auth/user/pass'
response = requests.get(url, auth=HTTPDigestAuth('user', 'pass'))

print(response.status_code)
# <Response [200]>
```



# 9 异常

https://docs.python-requests.org/en/latest/api/#requests.RequestException

在使用requests模块时，可以通过异常处理机制来捕获和处理可能出现的异常情况。

通过对不同类型的异常进行处理，可以增加代码的健壮性和容错性。

```python
import requests
from requests.exceptions import *

try:
    response = requests.get('http://www.baidu.com', timeout=0.00001)

# 服务器未在分配的时间内发送任何数据。
except ReadTimeout:
    print('ReadTimeout Exception')

# 尝试连接到远程服务器时请求超时。
# 产生此错误的请求可以安全地重试。
except ConnectionError:
    print('ConnectionError Exception')

# 请求超时。
except Timeout:
    print('Timeout Exception')

# 在处理您的 请求。
except RequestException:
    print('RequestException')

# 重定向过多。
except TooManyRedirects:
    print('TooManyRedirects Exception')

# 发出请求需要有效的 URL。
except UnicodeError:
    print('UnicodeError Exception')

# 发生 HTTP 错误
except HTTPError:
    print('HTTPError Exception')

# 无法将文本解码为 json
except JSONDecodeError:
    print('JSONDecodeError Exception')
```



# 10 上传文件

## 10.1 引言

在使用requests模块进行文件上传时，可以通过向post请求中添加files参数来实现。



## 10.2 语法

files参数是一个字典，其中键为表单中的字段名（比如`<input type="file" name="file">`中的`name`），值为打开的文件对象。

以下是关于使用requests模块上传文件的示例代码：

```python
import requests

files = {'file': open('a.jpg', 'rb')}
response = requests.post('http://httpbin.org/post', files=files)
print(response.status_code)
```

在上述示例代码中，我们首先导入了requests模块。

然后，定义了一个files字典，其中键为'file'，值为使用open函数打开的文件对象。

这里假设要上传的文件名为'a.jpg'，使用二进制读取模式打开文件（'rb'）。

接下来，使用requests.post函数发送post请求，传递了files参数，该参数指定了要上传的文件。

这里的URL使用了一个测试服务器'http://httpbin.org/post'，你可以将其替换为你要上传文件的目标服务器URL。'http://httpbin.org/post'，你可以将其替换为你要上传文件的目标服务器URL。

最后，打印response的status_code属性，即可获取请求的响应状态码。



## 10.3 注意

需要注意的是，根据实际情况，可能需要添加其他参数来完成文件上传，比如身份验证、请求头部信息等。

具体的参数设置可以根据目标服务器的要求进行调整。

另外，还需要注意文件路径的正确性，确保上传的文件存在，并且有合适的权限进行读取。

