8.12

admin 的 login 同 user

只是要在admin里写一样的东西

admin_interface里也写一样的东西

非公共方法就要重新写

非公共的部分需要重新写

各种logger都写common里去

admin在logger的设置里没有弄 要补

装饰器 迁移到 common里 user要用 admin也要用 加一个参数 判定给user还是admin



初始化商品这一块

想一下如果改了京东页面 不是手机了会不会影响脚本？

讲道理所有页面的ul li元素都是一样的应该就没问题

但是也要类似的格式 名字取3位都能区分开来的商品



有一个生成id的函数 generate_product_id

问题 为何当重复时调用的是create_bank_id的函数 如果出的随机数应该也不是------的确应该是调用自己--已经修正了



admin

del_one_good

拿商品 没有商品报错误让初始化 选id 没有id报错误 有删掉pop 输出商品信息

check_one_good

拿商品 没有商品报错误让初始化 选id 没有id报错误 有展示商品信息

check_all_good

拿商品 没有商品文件报错误让初始化 有就全部格式化展示商品信息
