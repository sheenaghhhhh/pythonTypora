[Scrapy 2.11 文档 — Scrapy 2.11.1 文档 - Scrapy 中文](https://docs.scrapy.net.cn/en/latest/)

# 1 介绍

## 1.1 开源和协作的框架

其最初是为了页面抓取 (更确切来说, 网络抓取 )所设计的，

使用它可以以快速、简单、可扩展的方式从网站中提取所需的数据。

但目前Scrapy的用途十分广泛，可用于如数据挖掘、监测和自动化测试等领域，也可以应用在获取API所返回的数据(例如 Amazon Associates Web Services ) 或者通用的网络爬虫。

Scrapy 是基于twisted框架开发而来，twisted是一个流行的事件驱动的python网络框架。

因此Scrapy使用了一种非阻塞（又名异步）的代码来实现并发。

Scrapy框架类似于Django框架

Django框架大而全 类似于航空母舰

scrapy框架也大而全

一旦创建项目 就会创建很多文件 体积比较大 内置功能全

## 1.2 整体架构大致如下

![img](assets/202403261435709.png)

1. 引擎(EGINE)

引擎负责控制系统所有组件之间的数据流，并在某些动作发生时触发事件。

2. 调度器(SCHEDULER)

用来接受引擎发过来的请求, 压入队列中, 并在引擎再次请求的时候返回

可以想像成一个URL的优先级队列, 由它来决定下一个要抓取的网址是什么, 同时去除重复的网址

3. 下载器(DOWLOADER)

用于下载网页内容, 并将网页内容返回给EGINE，下载器是建立在twisted这个高效的异步模型上的

4. 爬虫(SPIDERS)

SPIDERS是开发人员自定义的类，用来解析responses，并且提取items，或者发送新的请求

5. 项目管道(ITEM PIPLINES)

在items被提取后负责处理它们，主要包括清理、验证、持久化（比如存到数据库）等操作

下载器中间件(Downloader Middlewares)位于Scrapy引擎和下载器之间，主要用来处理从EGINE传到DOWLOADER的请求request，已经从DOWNLOADER传到EGINE的响应response，

6. 爬虫中间件(Spider Middlewares)

位于ENGINE和SPIDERS之间，主要工作是处理SPIDERS的输入（即responses）和输出（即requests）



# 2 安装

```python
# 【一】直接安装
# pip install scrapy
# 【二】直接安装失败 --- 间接安装
# 1.将scrapy需要的所有模块都安装一遍
# Requirement already satisfied: scrapy in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (2.10.1)
# Requirement already satisfied: Twisted<23.8.0,>=18.9.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (22.10.0)
# Requirement already satisfied: cryptography>=36.0.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (42.0.5)
# Requirement already satisfied: cssselect>=0.9.1 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (1.2.0)
# Requirement already satisfied: itemloaders>=1.0.1 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (1.1.0)
# Requirement already satisfied: parsel>=1.5.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (1.9.0)
# Requirement already satisfied: pyOpenSSL>=21.0.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (24.1.0)
# Requirement already satisfied: queuelib>=1.4.2 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (1.6.2)
# Requirement already satisfied: service-identity>=18.1.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (24.1.0)
# Requirement already satisfied: w3lib>=1.17.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (2.1.2)
# Requirement already satisfied: zope.interface>=5.1.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (6.1)
# Requirement already satisfied: protego>=0.1.15 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (0.3.0)
# Requirement already satisfied: itemadapter>=0.1.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (0.8.0)
# Requirement already satisfied: setuptools in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (65.5.0)
# Requirement already satisfied: packaging in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (23.2)
# Requirement already satisfied: tldextract in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (5.1.2)
# Requirement already satisfied: lxml>=4.4.1 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (4.9.2)
# Requirement already satisfied: PyDispatcher>=2.0.5 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from scrapy) (2.0.7)
# Requirement already satisfied: cffi>=1.12 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from cryptography>=36.0.0->scrapy) (1.16.0)
# Requirement already satisfied: jmespath>=0.9.5 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from itemloaders>=1.0.1->scrapy) (1.0.1)
# Requirement already satisfied: attrs>=19.1.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from service-identity>=18.1.0->scrapy) (23.2.0)
# Requirement already satisfied: pyasn1 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from service-identity>=18.1.0->scrapy) (0.6.0)
# Requirement already satisfied: pyasn1-modules in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from service-identity>=18.1.0->scrapy) (0.4.0)
# Requirement already satisfied: constantly>=15.1 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from Twisted<23.8.0,>=18.9.0->scrapy) (23.10.4)
# Requirement already satisfied: incremental>=21.3.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from Twisted<23.8.0,>=18.9.0->scrapy) (22.10.0)
# Requirement already satisfied: Automat>=0.8.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from Twisted<23.8.0,>=18.9.0->scrapy) (22.10.0)
# Requirement already satisfied: hyperlink>=17.1.1 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from Twisted<23.8.0,>=18.9.0->scrapy) (21.0.0)
# Requirement already satisfied: typing-extensions>=3.6.5 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from Twisted<23.8.0,>=18.9.0->scrapy) (4.9.0)
# Requirement already satisfied: idna in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from tldextract->scrapy) (2.7)
# Requirement already satisfied: requests>=2.1.0 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from tldextract->scrapy) (2.20.0)
# Requirement already satisfied: requests-file>=1.4 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from tldextract->scrapy) (2.0.0)
# Requirement already satisfied: filelock>=3.0.8 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from tldextract->scrapy) (3.13.3)
# Requirement already satisfied: six in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from Automat>=0.8.0->Twisted<23.8.0,>=18.9.0->scrapy) (1.16.0)
# Requirement already satisfied: pycparser in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from cffi>=1.12->cryptography>=36.0.0->scrapy) (2.21)
# Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from requests>=2.1.0->tldextract->scrapy) (3.0.4)
# Requirement already satisfied: urllib3<1.25,>=1.21.1 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from requests>=2.1.0->tldextract->scrapy) (1.24.3)
# Requirement already satisfied: certifi>=2017.4.17 in /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages (from requests>=2.1.0->tldextract->scrapy) (2023.11.17)

# 2.去 wheel 官网上下载本地的 wheel 文件
# pip install wheel 文件
```



# 3 基本使用

## 3.1 查看帮助

```python
# Terminal输入django-admin
"""
[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations # 生成迁移记录
    migrate   # 生成数据库表
    runserver # 启动Django项目
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp # 创建 app
    startproject # 创建Django项目
    test
    testserver
"""

# Terminal输入scrapy
"""
Usage:
  scrapy <command> [options] [args]

Available commands:
  bench         Run quick benchmark test
  fetch         Fetch a URL using the Scrapy downloader
  genspider     Generate new spider using pre-defined templates # 创建一个爬虫程序用指定的模版
  runspider     Run a self-contained spider (without creating a project) # 启动一个自己创建的项目
  settings      Get settings values # 获取到配置项
  shell         Interactive scraping console
  startproject  Create new project # 创建新项目
  version       Print Scrapy version
  view          Open URL in browser, as seen by Scrapy

  [ more ]      More commands available when run from project directory

Use "scrapy <command> -h" to see more info about a command
"""

# Terminal输入scrapy startproject -h
"""
Create new project

Global Options
--------------
  --logfile FILE        log file. if omitted stderr will be used
  -L LEVEL, --loglevel LEVEL
                        log level (default: DEBUG)
  --nolog               disable logging completely
  --profile FILE        write python cProfile stats to FILE
  --pidfile FILE        write process ID to FILE
  -s NAME=VALUE, --set NAME=VALUE
                        set/override setting (may be repeated)
  --pdb                 enable pdb on failure
"""
```

## 3.2 创建项目

```py
# scrapy startproject 项目名称

# scrapy startproject SpiderScrapy

"""
New Scrapy project 'SpiderScrapy', using template directory 'D:\Program Files\Python310\Lib\site-packages\scrapy/templates\project', created in:
    D:\Program Files\JetBrains\PycharmProjects\Day75\SpiderScrapy

You can start your first spider with:
    cd SpiderScrapy
    scrapy genspider example example.com
"""
```

## 3.3 进入项目

```python
# cd SpiderScrapy
"""
PS D:\Program Files\JetBrains\PycharmProjects\Day75> cd SpiderScrapy
PS D:\Program Files\JetBrains\PycharmProjects\Day75\SpiderScrapy>
"""
```

## 3.4 创建爬虫程序

```python
# scrapy genspider example example.com
# scrapy genspider 爬虫程序的名字 爬虫目标的域名
# scrapy genspider baidu www.baidu.com
# scrapy genspider sougou www.sougou.com
"""
Created spider 'baidu' using template 'basic' in module:
  SpiderScrapy.spiders.baidu
"""
# 在SpiderScrapy/SpiderScrapy/spiders里 会出现刚才新建的爬虫名.py文件 打开
```

## 3.5 创建的爬虫程序解释

```python
"""
# 导入 scrapy 模块
import scrapy


# 自动帮助我们创建的一个爬类 继承了 默认的 Spider 类
# 类似于我们Django项目中的 View  django.views.View
class BaiduSpider(scrapy.Spider):
    # 指当前爬虫的名称 跟创建项目时候写的名字有关 跟当前爬虫文件名是一致的
    name = "baidu"
    # 允许访问的当前域名 们实现多页爬取的时候如果下一页的域名和当前的域名不一致 爬虫就不会生效了
    allowed_domains = ["www.baidu.com"]
    # 起始的 URL 爬虫启动后第一次访问的目标地址
    start_urls = ["https://www.baidu.com"]

    # 处理响应的地方
    def parse(self, response):
        # response 当前 scrapy 帮助我们发起请求 获取到的响应内容 scrapy 内置的响应对象 可以直接使用 xpath .. 
        # response = requests.get("https://www.sougou.com")
        pass
"""
```

## 3.6 scrapy项目文件解释

```python
"""
SpiderScrapy
├── __init__.py # 初始化文件
├── items.py # 管道 类似与Django项目中的 models 模型表 我们需要再里面创建模型类
├── middlewares.py # 中间件 和 Django的 中间件 是一样的 拦截我们的请求和响应
├── pipelines.py # 管道 在这里负责对数据进行清洗和存储
├── settings.py # 项目的配置文件
└── spiders # 存储你的爬虫程序的文件夹
    ├── baidu.py # 你自己需要对响应进行处理的爬虫逻辑
    └── xx.py # 如果创建了不止一个爬虫 就会继续有其他的爬虫逻辑
"""
```

## 3.7 启动爬虫程序

```python
# 先修改
"""
import scrapy


class BaiduSpider(scrapy.Spider):
    name = "baidu"
    allowed_domains = ["www.baidu.com"]
    start_urls = ["https://www.baidu.com"]

    def parse(self, response):
        print(f"response :>>>> {response.text}")
        pass
"""

# Terminal里输入命令进行启动
# scrapy crawl 爬虫文件名
# scrapy crawl baidu

# robots.txt 机器人协议(爬虫协议):君子协议  大家都会被爬虫光顾，于是我们统一规定 用 robots.txt 来声明自己的网站内哪些内容是允许爬的
# 这样就爬不出来了
"""
2024-11-05 16:27:54 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.baidu.com/robots.txt> (referer: None)
2024-11-05 16:27:54 [scrapy.downloadermiddlewares.robotstxt] DEBUG: Forbidden by robots.txt: <GET https://www.baidu.com>
"""
# 一般情况下 大家都不遵循 Forbidden by robots.txt: <GET https://www.baidu.com>

# 不遵守
# 去修改 settings.py 中的配置
# ROBOTSTXT_OBEY = False

# 获取到响应源码
"""
response :>>>> <!DOCTYPE html>
<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http
...省略
...有很多日志相关的信息 影响看response
"""
```

## 3.8 不带日志启动项目

```python
# 1.方式一：命令行
# 在输出源码的时候都会带很多日志信息
# scrapy crawl baidu --nolog

# 2.方式二：修改配置文件的日志等级
# setting.py
# 添加一个日志等级
# LOG_LEVEL = "ERROR"
# 去除 ERROR 等级以外的其他日志
```

## 3.9 文件启动项目
```python
# 最大的SpriderScrapy下创建一个 py 文件

"""
from scrapy.cmdline import execute

# scrapy crawl baidu

# execute(["scrapy", "crawl", "baidu"])

# 无日志启动

execute(["scrapy", "crawl", "baidu", "--nolog"])
"""
```



# 4 数据解析

## 4.1 项目怎么创建、爬虫怎么写

```python
# 1.创建项目
# scrapy startproject SpiderScrapy

# 2.进入项目
# cd SpiderScrapy

# 3.创建爬虫程序
# scrapy genspider cnblog https://www.cnblogs.com/

# 4.去SpiderScrapy/SpiderScrapy/spiders里看文件
# cnblog.py 进行编辑
"""
# 导入 scrapy 模块
import scrapy


class CnblogSpider(scrapy.Spider):
    # 指当前爬虫的名称 跟创建项目时候写的名字有关 跟当前爬虫文件名是一致的
    name = "cnblog"
    # 允许访问的当前域名 在我们实现多页爬取的时候如果下一页的域名和当前的域名不一致 爬虫就不会生效了
    allowed_domains = ["www.cnblogs.com"]
    # 起始的 URL 爬虫启动后第一次访问的目标地址
    start_urls = ["https://www.cnblogs.com/"]

    def parse(self, response):
        print(f"response :>>>> {response.text}")
        pass
"""

# 5.启动
# 用创建的py文件启动
"""
# 无日志启动
execute(["scrapy", "crawl", "cnblog", "--nolog"])
"""

# 或者: 
# scrapy crawl cnblog --nolog
# 这样就可以拿数据
```

## 4.2 爬虫里怎么解析想要的数据

```python
# 【一】解析方式一：通过 css 选择器进行解析数据
    # 拿的方式 在网页里面选想要的元素 复制元素选择复制selector
    # 取这个最小元素的text或者其他的
    def parse(self, response):
        # 获取到页面的源码数据
        # print(f"response :>>>> {response.text}")
        # 直接使用 response 内置的 css 方法就可以仿 css 选择器表达式
        # 1 获取所有的文章列表
        article_list = response.css('#post_list > article')
        # print(article_list,len(article_list))
        # 2 遍历每一篇文章获取到文章详情内容
        article_dict = {}
        for article_obj in article_list:
            try:
                # 0.提取 方法
                # extract_first ： 获取到当前一个标签
                # extract ： 获取到当前所有标签
                # 1.获取到 Selector 对象
                # #post_list > article:nth-child(2) > section > div > a
                # title = article_obj.css('section > div > a')
                # [<Selector query='descendant-or-self::section/div/a' data='<a class="post-item-title" href="http...'>]
                # 2.提取 当前 Selector 对象中的 a 标签对象
                # title = article_obj.css('section > div > a').extract_first()
                '''
                <a class="post-item-title" href="https://www.cnblogs.com/jinjiangongzuoshi/p/18527263" target="_blank">强！34.1K star!  再见Postman，新一代API测试利器，功能强大、颜值爆表！</a>
                '''
                # 3.提取出当前 a 标签的 文本内容
                title = article_obj.css('section > div > a::text').extract_first()
                # 强！34.1K star!  再见Postman，新一代API测试利器，功能强大、颜值爆表！
                # 4.提取出当前 a 标签的 href 属性
                detail_url = article_obj.css('section > div > a::attr("href")').extract_first()
                # 5.文章简介内容
                # #post_list > article:nth-child(2) > section > div > p
                # 发现有的时候 空的 需要跳过
                try:
                    article_desc = article_obj.css('section > div > p::text').extract()[1].strip()
                except Exception as e:
                    article_desc = ""
                # 6.作者名字
                # #post_list > article:nth-child(2) > section > footer > a.post-item-author > span
                author_name = article_obj.css('section > footer > a.post-item-author > span::text').extract_first()
                # 7.作者主页地址
                author_blog = article_obj.css('section > footer > a.post-item-author::attr("href")').extract_first()
                # 8.评论数
                # #post_list > article:nth-child(2) > section > footer > a:nth-child(3) > span
                comment_num = article_obj.css('section > footer > a:nth-child(3) > span::text').extract_first()
                # 9.点赞数
                up_num = article_obj.css('section > footer > a:nth-child(4) > span::text').extract_first()
                # 10.观看数
                visit_num = article_obj.css('section > footer > a:nth-child(5) > span::text').extract_first()
                article_dict[title] = {
                    "title": title,
                    "detail_url": detail_url,
                    "article_desc": article_desc,
                    "author_name": author_name,
                    "author_blog": author_blog,
                    "comment_num": comment_num,
                    "up_num": up_num,
                    "visit_num": visit_num,
                }
            except Exception as e:
                print(f"{title} :>>>> {e}")
        print(article_dict, len(article_dict))


    # 【二】解析方式二：通过 Xpath 选择器进行解析数据
    # 在网页上复制对应的xpath 也是注意一下text href怎么取
    def parse(self, response):
        # 1.获取所有的文章列表
        article_list_all = response.xpath('//*[@id="post_list"]/article')
        # 2.遍历每一篇文章获取到文章详情内容
        article_dict = {}
        for article_obj in article_list_all:
            try:
                # 3.文章标题
                title = article_obj.xpath('./section/div/a/text()').extract_first()
                # 4.文章详情链接
                detail_url = article_obj.xpath('./section/div/a/@href').extract_first()
                # 5.文章简介内容
                try:
                    article_desc = article_obj.xpath('./section/div/p/text()').extract()[1].strip()
                except Exception as e:
                    article_desc = ""
                # 6.作者名字
                author_name = article_obj.xpath('./section/footer/a[1]/span/text()').extract_first()
                # 7.作者主页地址
                author_blog = article_obj.xpath('./section/footer/a[1]/@href').extract_first()
                # 8.评论数
                comment_num = article_obj.xpath('./section/footer/a[2]/span/text()').extract_first()
                # 9.点赞数
                up_num = article_obj.xpath('./section/footer/a[3]/span/text()').extract_first()
                # 10.观看数
                visit_num = article_obj.xpath('./section/footer/a[4]/span/text()').extract_first()
                article_dict[title] = {
                    "title": title,
                    "detail_url": detail_url,
                    "article_desc": article_desc,
                    "author_name": author_name,
                    "author_blog": author_blog,
                    "comment_num": comment_num,
                    "up_num": up_num,
                    "visit_num": visit_num,
                }
            except Exception as e:
                print(f"{title} :>>>> {e}")
        print(article_dict, len(article_dict))
```





# 5 基础案例

## 5.1 博客园



## 5.2 自己写一个



# 6 配置文件

```python
# setting.py
# Scrapy settings for SpiderScrapy project
#
# For simplicity, this file contains only settings considered important or
# commonly used. You can find more settings consulting the documentation:
#
#     https://docs.scrapy.org/en/latest/topics/settings.html
#     https://docs.scrapy.org/en/latest/topics/downloader-middleware.html
#     https://docs.scrapy.org/en/latest/topics/spider-middleware.html
from fake_useragent import UserAgent

# 当前 scrapy 项目的项目名称
BOT_NAME = "SpiderScrapy"

# 爬虫项目存放的位置 SpiderScrapy.spiders
SPIDER_MODULES = ["SpiderScrapy.spiders"]
# 创建新的爬虫项目存在的位置
NEWSPIDER_MODULE = "SpiderScrapy.spiders"

# 当前日志等级
LOG_LEVEL = "ERROR"

# USER_AGENT 浏览器标识 在这里添加自定义的请求头
# Crawl responsibly by identifying yourself (and your website) on the user-agent
# USER_AGENT = "SpiderScrapy (+http://www.yourdomain.com)"
USER_AGENT = UserAgent().random

# 是否遵循 君子协议 (一般不遵守)
# Obey robots.txt rules
ROBOTSTXT_OBEY = False

# 当前请求的最大并发数 默认是 16 个 可以往上加 32 基本够用
# Configure maximum concurrent requests performed by Scrapy (default: 16)
# CONCURRENT_REQUESTS = 32

# 给每一个请求之间增加一些延迟时间 默认延迟时间是 0 s
# Configure a delay for requests for the same website (default: 0)
# See https://docs.scrapy.org/en/latest/topics/settings.html#download-delay
# See also autothrottle settings and docs
# DOWNLOAD_DELAY = 3

# 针对某一个域名的最大并发请求数。
# The download delay setting will honor only one of:
# CONCURRENT_REQUESTS_PER_DOMAIN = 16

# 针对某一ip的最大并发请求数。
# 如果此项非0的话，它的优先级高于CONCURRENT_REQUESTS_PER_DOMAIN。
# 也就是说此时的并发数量限制是基于ip的，而非域名。
# 同时这个设置也会影响到DOWNLOAD_DELAY，如果CONCURRENT_REQUESTS_PER_IP非0，下载延迟会基于ip，而非域名。
# CONCURRENT_REQUESTS_PER_IP = 16

# 是否禁用 Cookie 保存
# Disable cookies (enabled by default)
# COOKIES_ENABLED = False

#  Scrapy提供了一个内置的Telnet终端，允许开发者通过Python交互式环境检查和控制爬虫进程。
#  默认监听6023端口，可以通过Windows或Linux的telnet客户端访问。
#  在终端中，可以使用`est()`方法查看引擎状态，暂停、恢复或停止Scrapy引擎。
#  此外，可以自定义telnet的变量字典进行更深入的定制。
# Disable Telnet Console (enabled by default)
# TELNETCONSOLE_ENABLED = False

# 重写覆盖默认的请求头参数
# Override the default request headers:
# DEFAULT_REQUEST_HEADERS = {
#    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
#    "Accept-Language": "en",
# }

# 爬虫中间件 ： 主要用于拦截 爬虫的请求和响应对象或者异常处理
# Enable or disable spider middlewares
# See https://docs.scrapy.org/en/latest/topics/spider-middleware.html
# SPIDER_MIDDLEWARES = {
#    "SpiderScrapy.middlewares.SpiderscrapySpiderMiddleware": 543,
# }

# 下载中间件 ： 在这里对数据进行分发下载
# Enable or disable downloader middlewares
# See https://docs.scrapy.org/en/latest/topics/downloader-middleware.html
# DOWNLOADER_MIDDLEWARES = {
#    "SpiderScrapy.middlewares.SpiderscrapyDownloaderMiddleware": 543,
# }

# 扩展 可以自己增加
# Enable or disable extensions
# See https://docs.scrapy.org/en/latest/topics/extensions.html
# EXTENSIONS = {
#    "scrapy.extensions.telnet.TelnetConsole": None,
# }

# 管道类 --- Django的模型表
# Configure item pipelines
# See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   "SpiderScrapy.pipelines.CnBlogPipeline": 300,
   # "SpiderScrapy.pipelines.SpiderscrapyPipeline": 300,
}

# 该扩展基于Scrapy服务器负载和当前爬取站点的负载，能够将爬虫的爬取速度自动调整至最佳。
# 你只需要设置请求并发数，扩展就可以完成其他工作师以实现自动限制爬虫请求发起的速度。
# Enable and configure the AutoThrottle extension (disabled by default)
# See https://docs.scrapy.org/en/latest/topics/autothrottle.html
# AUTOTHROTTLE_ENABLED = True
# 每一个爬虫的初始download delay，单位是秒，默认值为5.0
# The initial download delay
# AUTOTHROTTLE_START_DELAY = 5
# 允许的最大download delay，单位是秒，以防出现latency过高而导致download delay跟着变高的情况。默认值是60.0
# The maximum download delay to be set in case of high latencies
# AUTOTHROTTLE_MAX_DELAY = 60
# 针对每个网站的平均并发请求量，默认值是1.0。
# 这是一个平均值，意味着某一时刻的并发量可能高于也可能低于这个值。
# The average number of requests Scrapy should be sending in parallel to
# each remote server
# AUTOTHROTTLE_TARGET_CONCURRENCY = 1.0
# 调试模式，日志将会打印每次响应消耗的时长latency与当前所设置的当前的Download_delay时长。
# 这样就可以实时观察Download_delay参数的调整过程。True：开启调试；False：关闭调试（默认值）
# Enable showing throttling stats for every response received:
# AUTOTHROTTLE_DEBUG = False

# HTTP缓存配置
# Enable and configure HTTP caching (disabled by default)
# See https://docs.scrapy.org/en/latest/topics/downloader-middleware.html#httpcache-middleware-settings
# HTTPCACHE_ENABLED = True
# HTTPCACHE_EXPIRATION_SECS = 0
# HTTPCACHE_DIR = "httpcache"
# HTTPCACHE_IGNORE_HTTP_CODES = []
# HTTPCACHE_STORAGE = "scrapy.extensions.httpcache.FilesystemCacheStorage"

# 版本号
# Set settings whose default value is deprecated to a future-proof value
REQUEST_FINGERPRINTER_IMPLEMENTATION = "2.7"
# 使用默认的 TWISTED 框架地址
TWISTED_REACTOR = "twisted.internet.asyncioreactor.AsyncioSelectorReactor"
# 默认编码格式
FEED_EXPORT_ENCODING = "utf-8"
```



# 7 持久化存储

## 7.1 save和命令行存储

```python
# 保存篇1 xpath+save和函数
    """
    # parse就按照上面第二种xpath的写法来
    def parse(self, response):
        # 1.获取所有的文章列表
        article_list_all = response.xpath('//*[@id="post_list"]/article')
        # 2.遍历每一篇文章获取到文章详情内容
        article_dict = {}
        article_list = []
        for article_obj in article_list_all:
            try:
                # 3.文章标题
                title = article_obj.xpath('./section/div/a/text()').extract_first()
                # 4.文章详情链接
                detail_url = article_obj.xpath('./section/div/a/@href').extract_first()
                # 5.文章简介内容
                try:
                    article_desc = article_obj.xpath('./section/div/p/text()').extract()[1].strip()
                except Exception as e:
                    article_desc = ""
                # 6.作者名字
                author_name = article_obj.xpath('./section/footer/a[1]/span/text()').extract_first()
                # 7.作者主页地址
                author_blog = article_obj.xpath('./section/footer/a[1]/@href').extract_first()
                # 8.评论数
                comment_num = article_obj.xpath('./section/footer/a[2]/span/text()').extract_first()
                # 9.点赞数
                up_num = article_obj.xpath('./section/footer/a[3]/span/text()').extract_first()
                # 10.观看数
                visit_num = article_obj.xpath('./section/footer/a[4]/span/text()').extract_first()
                article_dict[title] = {
                    "title": title,
                    "detail_url": detail_url,
                    "article_desc": article_desc,
                    "author_name": author_name,
                    "author_blog": author_blog,
                    "comment_num": comment_num,
                    "up_num": up_num,
                    "visit_num": visit_num,
                }
                article_list.append({
                    "title": title,
                    "detail_url": detail_url,
                    "article_desc": article_desc,
                    "author_name": author_name,
                    "author_blog": author_blog,
                    "comment_num": comment_num,
                    "up_num": up_num,
                    "visit_num": visit_num,
                })
            except Exception as e:
                print(f"{title} :>>>> {e}")
        print(f"article_list :>>>> {article_list}")
        self.save(article_dict)
        return article_list

    def save(self, data):
        # 不要写死路径 这样写就算文件用在其他设备也在当前文件夹
        file_path = os.path.join(os.path.dirname(__file__), "data.json")
        with open(file_path, "w", encoding="utf-8") as fp:
            json.dump(data, fp, ensure_ascii=False, indent=4)
    """

# ------------------------------------
# 保存篇2 保存到本地(配合命令行)
    # 把parse中的数据形式修改 再加上配合命令行
    # A.我们上面都是把数据写成字典包字典的形式 都是{"title":{}, "title":{}, "title":{}}
    # 前提是 数据要求是 [{"key":"value"}] 列表包字典
    """
    那么写法就要不一样
    # 创建一个列表
    article_list = [] 
    
    # 添加字典格式的数据到列表中
    article_list.append({
                "title": title,
                "detail_url": detail_url,
                "article_desc": article_desc,
                "author_name": author_name,
                "author_blog": author_blog,
                "comment_num": comment_num,
                "up_num": up_num,
                "visit_num": visit_num,
            })
            
    # 将列表返回出去
    return article_list
    """
    def parse(self, response):
        # 1.获取所有的文章列表
        article_list_all = response.xpath('//*[@id="post_list"]/article')
        # 2.遍历每一篇文章获取到文章详情内容
        # **新写一个列表
        article_list = []
        for article_obj in article_list_all:
            try:
                # 3.文章标题
                title = article_obj.xpath('./section/div/a/text()').extract_first()
                # 4.文章详情链接
                detail_url = article_obj.xpath('./section/div/a/@href').extract_first()
                # 5.文章简介内容
                try:
                    article_desc = article_obj.xpath('./section/div/p/text()').extract()[1].strip()
                except Exception as e:
                    article_desc = ""
                # 6.作者名字
                author_name = article_obj.xpath('./section/footer/a[1]/span/text()').extract_first()
                # 7.作者主页地址
                author_blog = article_obj.xpath('./section/footer/a[1]/@href').extract_first()
                # 8.评论数
                comment_num = article_obj.xpath('./section/footer/a[2]/span/text()').extract_first()
                # 9.点赞数
                up_num = article_obj.xpath('./section/footer/a[3]/span/text()').extract_first()
                # 10.观看数
                visit_num = article_obj.xpath('./section/footer/a[4]/span/text()').extract_first()
                # 添加字典格式的数据到列表中
                article_list.append({
                    "title": title,
                    "detail_url": detail_url,
                    "article_desc": article_desc,
                    "author_name": author_name,
                    "author_blog": author_blog,
                    "comment_num": comment_num,
                    "up_num": up_num,
                    "visit_num": visit_num,
                })
            except Exception as e:
                print(f"{title} :>>>> {e}")
        print(article_list, len(article_list))
        # 将列表返回出去
        return article_list
    # B.命令行 terminal里 cd SpiderScrapy
    # 上面是规范 下面是这个例子用的语句
    # 1.保存为json文件
    # scrapy crawl your_spider_name -o output.json
    # scrapy crawl cnblog -o blog_data.json

    # 2.保存为csv文件
    # scrapy crawl your_spider_name -o blog_data.csv -t csv
    # scrapy crawl cnblog -o blog_data.csv -t csv
```



## 7.2 管道

```python
# 【一】第一步 创建一个管道类
# 在 items. py 中创建管道类
"""
# 创建一个管道类 继承 scrapy.Item
class CnBlogItem(scrapy.Item):
    # 定义管道类中的字段
    title = scrapy.Field()
    detail_url = scrapy.Field()
    article_desc = scrapy.Field()
    author_name = scrapy.Field()
    author_blog = scrapy.Field()
    comment_num = scrapy.Field()
    up_num = scrapy.Field()
    visit_num = scrapy.Field()
"""

# 【二】定义管道处理数据函数
# 在 pipelines.py 定义管道类中的数据的处理方式
"""
class CnBlogPipeline:
    def process_item(self, item, spider):
        print(item)
        return item
"""

# 【三】注册到配置文件中 settings.py
# 后面的数字越大 优先级越高
"""
# 管道类 --- Django的模型表
ITEM_PIPELINES = {
   # "SpiderScrapy.pipelines.SpiderscrapyPipeline": 300,
   "SpiderScrapy.pipelines.CnBlogPipeline": 300,
}
"""

# 【四】操作管道类 向管道类中添加数据
"""
# 这样爆红
# from SpiderScrapy.items import CnBlogItem
# 这样运行报错
# from SpiderScrapy.SpiderScrapy.items import CnBlogItem
# 可以写成这样 但是还是报错
# 浪费了一小时 不知道原理是什么 搞不懂 文件目录结构没有问题
from ..items import CnBlogItem
# 补充: 因为如果需要第一种写法 需要把根节点改成大SpiderScrapy 而不是Day75文件夹
# 怎么做:Mart Directory as Resource Root
# 之后就可以用这种写法了

# 用的是解析2 xpath
    def parse(self, response):
        # ***** 0.创建管道类对象
        item = CnBlogItem()

        # 1.获取所有的文章列表
        article_list_all = response.xpath('//*[@id="post_list"]/article')
        # 2.遍历每一篇文章获取到文章详情内容
        for article_obj in article_list_all:
            # 3.文章标题
            title = article_obj.xpath('./section/div/a/text()').extract_first()
            try:
                # 4.文章详情链接
                detail_url = article_obj.xpath('./section/div/a/@href').extract_first()
                # 5.文章简介内容
                try:
                    article_desc = article_obj.xpath('./section/div/p/text()').extract()[1].strip()
                except Exception as e:
                    article_desc = ""
                # 6.作者名字
                author_name = article_obj.xpath('./section/footer/a[1]/span/text()').extract_first()
                # 7.作者主页地址
                author_blog = article_obj.xpath('./section/footer/a[1]/@href').extract_first()
                # 8.评论数
                comment_num = article_obj.xpath('./section/footer/a[2]/span/text()').extract_first()
                # 9.点赞数
                up_num = article_obj.xpath('./section/footer/a[3]/span/text()').extract_first()
                # 10.观看数
                visit_num = article_obj.xpath('./section/footer/a[4]/span/text()').extract_first()

                # ***** 11.将上面解析到 所有数据添加到 管道类中
                # 管道类对象类似于 字典
                item.update({
                    "title": title,
                    "detail_url": detail_url,
                    "article_desc": article_desc,
                    "author_name": author_name,
                    "author_blog": author_blog,
                    "comment_num": comment_num,
                    "up_num": up_num,
                    "visit_num": visit_num,
                })

                # 提交管道数据
                yield item
            except Exception as e:
                print(f"{title} :>>>> {e}")
"""

# 【五】管道类存储本地数据
# 还是pinelines.py 进一步完善
'''
class CnBlogPipeline:
    def open_spider(self, spider):
        """
        每次启动爬虫项目会触发一次
        不是 每一次提交 item 一次
        可以放数据库的创建连接
        可以放文件的打开
        :param spider:
        :return:
        """
        print('爬虫项目启动成功 :>>>> ', spider)

        # 将打开文件的句柄初始化成类的参数
        self.fp = open("./blog_pipline.json", "a", encoding="utf8")

    def close_spider(self, spider):
        """
        每次关闭爬虫项目会启动一次
        # 可以放数据库的关闭
        可以放文件的关闭
        :param spider:
        :return:
        """
        print('爬虫项目结束 :>>>> ', spider)

        # 关闭打开的文件对象
        self.fp.close()

    def process_item(self, item, spider):
        # 不断地更新传过来的 json 数据
        print(item)  # 这里传过来的是 scrapy 的 item 对象所以json 不识别 切换成字典格式
        json.dump(obj=dict(item), fp=self.fp, ensure_ascii=False)
        return item
'''
```

## 7.3 MySQL本地数据库

```python
import pymysql


class CnBlogPipeline:
    def open_spider(self, spider):
        '''
        每次启动爬虫项目会触发一次
        不是 每一次提交 item 一次
        可以放数据库的创建连接
        可以放文件的打开
        :param spider:
        :return:
        '''
        print('爬虫项目启动成功 :>>>> ', spider)

        '''
        create database blog;
        use blog;
        CREATE TABLE blog_article(
            `id` INT NOT NULL AUTO_INCREMENT  COMMENT '主键;主键ID' ,
            `title` VARCHAR(255)    COMMENT '文章标题;文章标题' ,
            `detail_url` VARCHAR(255)    COMMENT '文章详情地址' ,
            `article_desc` VARCHAR(255)    COMMENT '文章简介' ,
            `author_name` VARCHAR(255)    COMMENT '文章作者名字' ,
            `author_blog` VARCHAR(255)    COMMENT '文章作者链接' ,
            `up_num` VARCHAR(255)    COMMENT '文章点赞数' ,
            `comment_num` VARCHAR(255)    COMMENT '文章评论数' ,
            `visit_num` VARCHAR(255)    COMMENT '文章阅读数' ,
            PRIMARY KEY (id)
        )  COMMENT = '';
        '''
        # 初始化MySQL的句柄对象
        self.conn = pymysql.Connect(
            user="root",
            password="123456",
            host="127.0.0.1",
            database="blog",
            port=3306,
            charset="utf8mb4",
        )
        self.cursor = self.conn.cursor()

    def close_spider(self, spider):
        '''
        每次关闭爬虫项目会启动一次
        # 可以放数据库的关闭
        可以放文件的关闭
        :param spider:
        :return:
        '''
        print('爬虫项目结束 :>>>> ', spider)

        # 关闭打开的 数据库对象
        self.cursor.close()
        self.conn.close()

    def process_item(self, item, spider):
        print(dict(item))
        # 不断地向数据库写数据
        sql_head = f"insert into blog_article(title,detail_url,article_desc,author_name,author_blog,comment_num,up_num,visit_num) "
        sql_foot = "values (%(title)s,%(detail_url)s,%(article_desc)s,%(author_name)s,%(author_blog)s,%(comment_num)s,%(up_num)s,%(visit_num)s)"
        sql = sql_head + sql_foot
        print(sql)
        self.cursor.execute(sql, dict(item))
        self.conn.commit()
        return item
```



# 8 全站爬取



# 9 中间件



# 10 集成Selenium框架



# 11 请求头操作



# 12 去重过滤器源码



# 13 布隆过滤器



# 14 分布式爬虫