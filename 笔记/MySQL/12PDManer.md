# 12 PDManer(元数建模)

## 1 介绍

### （1）软件介绍

- PDManer元数建模，是一款多操作系统开源免费的桌面版关系数据库模型建模工具，相对于PowerDesigner，他具备界面简洁美观，操作简单，上手容易等特点。
- 支持Windows,Mac,Linux等操作系统，也能够支持国产操作系统，能够支持的数据库如下： 

- MySQL,PostgreSQL,Oracle,SQLServer等常见数据库
- 支持达梦，GuassDB等国产数据库
- 支持Hive，MaxCompute等大数据方向的数据库
- 用户还可以自行添加更多的数据库扩展

本产品基于 ES6+React+Electron+Java构建

- [PDManer元数建模-4.0]，历时四年，持续升级，工匠精神，做一款简单好用的数据库建模平台。
- [[PDMan-v2](https://gitee.com/robergroup/pdman)] --> [[CHINER-v3](https://gitee.com/robergroup/chiner)] --> [PDManer-v4]，连续四年，产品一直保持很好的传承和延续。
- PDManer=PDMan+er(chiner的er部分，ER也表示关系图的意思)，“元数建模”的中文名称依然延续，名称需要精简，拿掉chi表示中国的前缀部分，使用中文能更加明确这是一个中国小团队的作品，4.0版本之后，产品名称：[PDManer元数建模]就此确定，承接了PDMan以及CHINER的所有功能，并且进行延续精进

### （2）主要功能如下

- **数据表管理：** 数据表，字段，注释，索引等基本功能
- **视图管理：** 实现选择多张表多个字段后，组合一个新的视图对象，视图可生成DDL以及相关程序代码，例如Java的DTO等
- **ER关系图：** 数据表可绘制ER关系图至画布，也支持概念模型等高阶抽像设计
- **数据字典：** 代码映射表管理，例如1表示男，2表示女，并且实现数据字典与数据表字段的关联
- **数据类型：** 系统实现了基础数据类型，基础数据类型在不同数据库下表现为不同数据库类型的方言，这是实现多数据**库支持的基础，为更贴近业务，引入了PowerDesigner的数据域这一概念，用于统一同一类具有同样业务属性字段的批量设置类型，长度等。基础数据类型以及数据域，用户均可自行添加，自行定义。
- **多数据库：** 内置主流常见数据库，如MySQL，PostgreSQL，SQLServer，Oracle等，并且支持用户自行添加新的数据库。
- **代码生成：** 内置Java，Mybatis，MyBatisPlus等常规情况下Controller，Service，Mapper的生成，也添加了C#语言支持，可自行扩展对其他语言的支持，如Python等
- **版本管理：** 实现数据表的版本管理，可生成增量DDL脚本
- **生态对接：** 能够导入PowerDesigner的pdm文件，老版本的PDMan文件，也能导出为word文档，导出相关设置等



## 2 下载和安装(Win)

### （1）软件官网

- https://gitee.com/robergroup/pdmaner/releases

### （2）下载

- 根据官网下载
- 根据百度盘地址下载 

- https://pan.baidu.com/s/1LniAn5_lcKqViwQPHrOKDg?pwd=dm29

### （3）安装

- 傻瓜式安装，一路下一步即可



## 3 使用

### （1）新建项目

![img](assets/202312061714098.png)

- 自定义名称

![img](assets/202312061715581.png)

### （2）创建主题域

![img](assets/202312061716466.png)

- 创建完成

![img](assets/202312061716693.png)

### （3）数据表操作

1. **新建数据表**

![img](assets/202312061717579.png)

- 定义表名和表备注

![img](assets/202312061718852.png)

2. **定义表字段类型**

![img](assets/202312061718452.png)

### （4）关系图操作

1. **新建关系图**

![img](assets/202312061719314.png)

2. **创建表关系和关联关系**

![img](assets/202312061723750.gif)



# 12+ Tabby

## 1 Tabby介绍

- 与Bash相同的行编辑（来自[GNU Readline库](https:_tiswww.case.edu_php_chet_readline_rltop)版本8.1）。
- 会话之间的历史记录持久性。
- 上下文相关完成; 

- 可执行文件（和别名）。
- 目录命令。
- 环境变量。

- 上下文相关的彩色输入文本。
- 来自历史记录和完成的自动建议。
- 新的键盘快捷键; 

- 交互式完成列表 （+）。CtrlSpace
- 增量历史记录搜索（+和+）。CtrlRCtrlS
- 强大的完成 （）。Tab
- 撤消 （+）。CtrlZ
- 环境变量扩展 （++）。CtrlAltE
- Doskey 别名扩展 （++）。CtrlAltF
- 滚动屏幕缓冲区 （+等）。AltUp
- Shift+箭头键选择文本，键入替换所选文本等。
- （按 + 查看更多...AltH

- 目录快捷方式; 

- 键入目录名称后跟路径分隔符是该目录的快捷方式。cd /d
- 键入 or 是 or 的快捷方式（每增加一个 ）。..``...``cd ..``cd ..\..``.``\..
- 键入或更改以前的当前工作目录。-``cd -

- 带有Lua的脚本化自动建议。
- 使用 Lua 完成脚本化。
- 使用 Lua 的可脚本化键绑定。
- 彩色和可编写脚本的提示。
- 自动应答“终止批处理作业？”提示。

默认情况下，Clink 绑定 + 以显示当前的键绑定。更多的功能也可以在GNU的[Readline](https:_tiswww.cwru.edu_php_chet_readline_readline)中找到。AltH



## 2 下载

- 访问可能会有点慢，自己解决

### （1）官网

- 官网：https://tabby.sh/
- Github链接：https://github.com/eugeny/tabby

![img](assets/image-20240125165057654.png)

### （2）下载

- 下载链接：https://github.com/Eugeny/tabby/releases/tag/v1.0.205

![img](assets/image-20240125165254722.png)

- 百度盘链接：https://pan.baidu.com/s/1XwEfhnLo-6_TM9UqsInyQA?pwd=jnuf



## 3 安装

## （1）双击安装程序

![img](assets/image-20240125170044377.png)

## （2）为所有用户安装

![img](assets/image-20240125170111436.png)

## （3）选择安装位置

![img](assets/image-20240125170423609.png)

## （4）安装完成



## 4 使用

## （1）软件主界面

![img](assets/15.png)

## （2）设置语言

1. **开始界面**

- 可以在开始界面选择自己想要的语言版本

![img](assets/image-20240125170658207.png)

2. **设置**

![img](assets/image-20240125170752912.png)

## （3）连接服务器

1. **选择管理配置**

![img](assets/17.png)

2. **选择新配置**

![img](assets/18.png)

3. **选择SSH会话**

![img](assets/19.png)

4. **输入自己的服务器信息**

![img](assets/20.png)

5. **连接成功**



## 5 个人使用评价

- 这个软件感觉比FinalShell还要友好一点，就是没有FinalShell那种监控服务器的可视化界面，其他的功能还是挺全的，功能也有很多
- 有兴趣的朋友可以自己研究研究