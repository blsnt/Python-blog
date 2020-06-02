.. role:: raw-latex(raw)
   :format: latex
..

.. contents::
   :depth: 3
..

[TOC]

15158881532

hr18367922520

Python入门
==============

第一章 计算机基础
-----------------

1.1 硬件基础
~~~~~~~~~~~~

计算机基本的硬件有：CPU，内存，显卡，主板，电源，键盘，鼠标，网卡等组成，只有硬件但硬件之间无法进行交流和通信。

1.2 操作系统
~~~~~~~~~~~~

操作系统用于协调和控制硬件之间进行工作，常见的操作系统有哪些

-  windows
-  linux

   -  centos
   -  redhat

-  mac

1.3 解释器或编译器
~~~~~~~~~~~~~~~~~~

编程语言的开发者编写的一个工具，将用户写的代码转换成010101交给操作系统去执行

1.3.1 解释和编译型语言
^^^^^^^^^^^^^^^^^^^^^^

解释型语言就类似于：实时翻译，代表：Python/PHP/Ruby/Perl

编译型语言类似于：说完之后，整体再进行翻译，代表：C/C++/Java/Go

1.4 软件(应用程序)
~~~~~~~~~~~~~~~~~~

软件又称为应用程序，就是我们在电脑上使用的工具，类似于：记事本/图片查看/游戏

1.5 进制
~~~~~~~~

对于计算机而言无论是文件存储/网络传输输入本质都是：二进制，如：电脑上存储视频/图片/文件，都是二进制

进制：

-  二进制，计算机内部
-  8进制
-  10进制，人来进行使用一般情况下计算机可以获取10进制，然后再内部会自动转换成二进制并操作
-  16进制，一般应用于表示二进制(用更短的内容表示更多的数据)

第二章 Python入门
-----------------

2.1 环境的安装
~~~~~~~~~~~~~~

-  解释器：py2/py3 (环境变量)
-  开发工具：pycharm

2.2 编码
~~~~~~~~

2.2.1 编码基础
^^^^^^^^^^^^^^

-  unicode 万国码 32位 4字节 所有的都能表示出来还有剩余
-  ascii 8位4字节 只能表示英文字母 数字下划线
-  utf-8 万国码的压缩版 从左到右每8位为0就舍去 汉字占3字节
-  gbk 汉字占2字符
-  gb2312

2.2.2 python编码相关
^^^^^^^^^^^^^^^^^^^^

对于python默认解释器编码

-  py2默认ascii编码，在文件头部加

   .. code:: python

      #!/usr/bin/env python
      # -*- coding:utf-8 -*-

-  py3默认utf-8编码

..

   注意: 对于操作文件时，要按照：以什么编写写入，就要用什么编码打开

2.3 变量
~~~~~~~~

2.3.1 为什么要有变量？
^^^^^^^^^^^^^^^^^^^^^^

为某个值创建一个外号，以后再使用的时候通过此外号就可以直接调用

2.3.2 变量命名规范
^^^^^^^^^^^^^^^^^^

-  变量名只能包含：字母数字下划线
-  数字不能开头
-  不能是python关键字
-  见名知意

2.4 pycharm 自动生成头部代码
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|image0|

|image1|

2.4 python2和3区别
~~~~~~~~~~~~~~~~~~

-  py2默认解释器编码格式 ascii 修改头文件加

::

   # -*- coding:utf-8 -*-

-  py3默认解释器编码格式 utf-8
-  py2输出 print加空格
-  py3输出print()
-  py2输入 raw_input()
-  py3输入 input()
-  字典的keys,items,values 拿到的数据类型都不一样

   -  py2：拿到的都是列表
   -  py3：迭代器

-  字符串类型不同

   -  py3 ：str bytes
   -  py2 ：unicode str

2.5 条件判断
~~~~~~~~~~~~

2.5.1 if 判断
^^^^^^^^^^^^^

.. code:: python

   if 表达式: 
        语句块 1 
   else: 
       语句块 2 

.. code:: python

   # 让用户输入一个数字，猜：如果数字 > 50，则输出：大了，如果数字<=50 ,则输出：小了
   num = input("请输入数字")
   number = int(num)
   if number > 50:
     print("大了")
   else:
     print("小了")
     
   # 用户名密码登录
   username = input("请输入用户名：")
   password = input("请输入密码：")

   if username == 'alex' and password == 123:
     print("登录成功")
   else:
     print("用户名或者密码错误")

2.6 循环
~~~~~~~~

2.6.1 while 循环
^^^^^^^^^^^^^^^^

.. code:: python

   while True:
     print("人生苦短，我用python")

break

.. code:: python

   while True:
     print("人生苦短，我用python")
     break

continue

打印1234568910

.. code:: python

   count = 1
   while count <= 10:
       if count == 7:
           count += 1
           continue
       print(count)
       count += 1

while else

.. code:: python

   count = 1
   while count < 10:
     print(count)
     count += 1
   else: # 不在满足while候的条件时触发，或者条件等于False
     print("else代码块")
   print("结束")

2.6.2 for 循环
^^^^^^^^^^^^^^

.. code:: python

   name = 'alex'
   for i in name:
     print(i)

break

.. code:: python

   name = 'alex'
   for i in name:
     print(i)
     break
     print('123')

continue

.. code:: python

   name = 'alex'
   for i in name:
     print(i)
     continue
     print('123')

range

.. code:: python

   for i in range(1,11):
     print(i)
     
   # 打印1234568910
   for i in range(1,11):
     if i != 7:
       print(i)

2.7 注释
~~~~~~~~

.. code:: python

   # 当行注释
   """""" # 多行注释
   # 快捷键 ctrl + / 选中多行注释

2.8 字符串格式化
~~~~~~~~~~~~~~~~

.. code:: python

   msg = "我是%s,年龄%s" %('alex',19,)
   print(msg)

   msg = "我是%(n1)s,年龄%(n2)s" % {'n1': 'alex', 'n2': 123, }
   print(msg)

.. code:: python

   # v1 = "我是{0},年龄{1}".format('alex',19)
   v1 = "我是{0},年龄{1}".format(*('alex',19,))
   print(v1)

   # v2 = "我是{name},年龄{age}".format(name='alex',age=18)
   v2 = "我是{name},年龄{age}".format(**{'name':'alex','age':18})
   print(v2)

第三章 数据类型
---------------

3.1 整形(int)
~~~~~~~~~~~~~

3.1.1 整形的长度
^^^^^^^^^^^^^^^^

py2中有：int、long

py3中有：int

3.1.2 整除
^^^^^^^^^^

注意：在python2中使用除法时，只能保留整数位，如果想要保留小数位，可以先导入一个模块。

.. code:: python

   from __future__ import division 
   value = 3/2
   print(value)

3.2 布尔(bool)
~~~~~~~~~~~~~~

布尔值就是用于表示真假，True和False

转换：

-  数字转布尔：0是False，其他都是True
-  字符串转布尔：""是False，其他都是True

3.3 字符串(str)
~~~~~~~~~~~~~~~

字符串是写代码中最常见的，python内存中的字符串是按照:unicode编码存储，对于字符串是不可变

3.3.1 字符串的私有方法
^^^^^^^^^^^^^^^^^^^^^^

-  upper() / lower() 大小写

-  isupper() 是否全部是大写

-  isdigit() 是否是数字

-  isnumeric() 推荐使用，判断是否是10进制的数

-  strip() /lstrip()/rstrip() ，去空白、:raw-latex:`\t、`:raw-latex:`\n`

-  replace(‘被替换的字符’,‘要替换的内容’,可选次数)

-  split(‘根据什么东西分割’,‘分割次数’) / rsplit() 从右切割

-  startswith() 以什么开头

-  endswith() 以什么结尾

-  capitalize() 首字母变大写

-  find 找索引位置

   .. code:: python

      v = 'alex'
      index = v.find('e') # 存在则返回索引位置，不存在则返回-1

-  center 居中

   .. code:: python

      v = 'alex'
      v1 = v.center(20,'*')
      print(v1)  # ********alex********

-  count 计算个数

   .. code:: python

      v = 'alex'
      v1 = v.count('a')

-  format 格式化

   .. code:: python

      name = "我叫:{0},年龄:{1}".format('老男孩',74)

-  encode 编码

   .. code:: python

      name = '杨世义' # 解释器读取到内存后，按照unicode编码存储，8个字节
      v1 = name.encode('utf-8')

-  join 拼接

   .. code:: python

      name = 'alex'
      v1 = "_".join(name) # a_l_e_x

3.4 列表
~~~~~~~~

.. code:: python

   v1 = ['blsnt','alex',99]

-  append 追加

   .. code:: python

      name = []
      name.append('name')

-  insert 插入指定位置

   .. code:: python

      name = [1,2,3,4,5]
      name.insert(0,9)
      print(name)
      [9, 1, 2, 3, 4, 5]

-  remove 删除

   .. code:: python

      name = [1,2,3,4,5]
      name.remove(1) # 1是元素名字

-  pop

   .. code:: python

      name = [1,2,3,4,5]
      name.pop(1) # 1是索引位置，不加索引默认删除最后一个

-  del

   .. code:: python

      del name[1] # 1是索引  

-  修改(字符串/数字/布尔除外)

   .. code:: python

      users = ['李烧起','李思','王五']
      user[2] = 66
      user[0] = '李杰'

-  列表嵌套

   .. code:: python

      users = ['alex',0,True,[11,22,33],[1,['alex','oldboy'],2,3]]

-  extend

   .. code:: python

      name = ['alex',123,12]
      s = 'qwert'
      li.extend(s)

-  reverse

   .. code:: python

      v1 = [1,2,3,4,5]
      v1.reverse()

-  sort

   .. code:: python

      v1 = [1,2,3,4,5]
      v1.sort(reverse=False) # 从小到大（默认False）
      v1.sort(reverse=True)  # 从大到小

反转案例

.. code:: python

   name = 'xadgasdgsdg'
   name_len = len(name) - 1
   value = ""
   for index in range(name_len,-1,-1):
     value += name[index]
   print(value)

..

   注意：删除功能数字布尔字符串除外

   字符串本身不能修改或删除【不可变类型】

..

   列表是可变类型

3.5 元祖
~~~~~~~~

.. code:: python

   # 元祖格式
   users = (11,22,33,44)

公共功能

1.索引（排除：int/bool）

.. code:: python

   users = (11,22,33,44)
   user[0]

2.切片（排除：int/bool）

.. code:: python

   users = (11,22,33,44)
   user[0:2]

3.步长（排除：int/bool）

.. code:: python

   users = (11,22,33,44)
   user[0:2:2]

4.删除（排除：tuple/str/int/bool）

5.修改（排除：tuple/str/int/bool）

6.for循环（排除：int/bool）

.. code:: python

   users = (11,22,33,44)
   for i in users:
     print(i)

7.len（排除：int/bool）

.. code:: python

   users = (11,22,33,44)
   print(len(users))

独有功能(无)

3.6 字典
~~~~~~~~

帮助用户去表示一个事物的信息(事物是有多个属性)

.. code:: python

   info = {"name":"伟大","age":18,"gender":"男"}
   info['name'] # 取值

字典的独有功能

-  keys

.. code:: python

   info = {"name":"伟大","age":18,"gender":"男"}
   v1 = info.keys() # 获取字典中的所有键

-  values

.. code:: python

   info = {"name":"伟大","age":18,"gender":"男"}
   v1 = info.values() # 获取字典中的所有值

-  items

.. code:: python

   info = {"name":"伟大","age":18,"gender":"男"}
   v1 = info.items() # 获取字典中的所有键值
   for v1,v2 in info.items():
     print(v1,v2)

-  get

.. code:: python

   v = {'user':'blsnt','k2':'v2'}
   print(v.get('k2'，666))           # key存在返回值，如果不存在None，后面可以修改默认返回666

-  pop

.. code:: python

   v = {'user':'blsnt','k2':'v2'}
   result = v.pop('user') 

-  update

.. code:: python

   v = {'user':'blsnt','k2':'v2'}
   v.update({'k3':'v3','k4':'v4'}) # 存在就覆盖，不存在就添加

判断一个字符串中是否有敏感字符？

-  str

   .. code:: python

      v = "Python全栈21期"

      if "全栈" in v:
          print('含敏感字符')

-  list/tuple

   .. code:: python

      v = ['alex','oldboy','藏老四','利奇航']

      if "利奇航" in v:
          print('含敏感')

-  dict

   .. code:: python

      v = {'k1':'v1','k2':'v2','k3':'v3'}

      # 默认按照键判断，即：判断x是否是字典的键。
      if 'x' in v:
          pass 

      # 请判断：k1 是否在其中？
      if 'k1' in v:
          pass
      # 请判断：v2 是否在其中？
      # 方式一：循环判断
      flag = '不存在'
      for v in v.values():
          if v == 'v2':
              flag = '存在'
      print(flag)
      # 方式二：
      if 'v2' in list(v.values()): # 强制转换成列表 ['v1','v2','v3']
            pass
      # 请判断：k2:v2 是否在其中？
      value = v.get('k2')
      if value == 'v2':
          print('存在')
      else:
          print('不存在')

-  练习题

   .. code:: python

      # 让用户输入任意字符串，然后判断此字符串是否包含指定的敏感字符。

      char_list = ['利奇航','堂有光','炸展会']
      content = input('请输入内容：') # 我叫利奇航  / 我是堂有光  / 我要炸展会

      success = True

      for v in char_list:
          if v in content:
              success = False
              break

      if success:
        print(content)
      else:
          print('包含铭感字符')

      # 示例：
      # 1. 昨天课上最后一题
      # 2. 判断 ‘v2’ 是否在字典的value中 v = {'k1':'v1','k2':'v2','k3':'v3'} 【循环判断】
      # 3. 敏感字

3.7 集合
~~~~~~~~

.. code:: python

   v = {1,2,3,4,5,6}

   # 空集合
   v1 = set()

独有功能

.. code:: python

   # 添加
   v1 = {1,2}
   v.add('blsnt')

   # 删除
   v1 = {1,2,'blsnt'}
   v1.discard('blsnt') # 无序，不能使用索引删除，不存在不会报错

   # 批量添加
   v1 = {1,2,'blsnt'}
   v1.update({11,22,33})

   # 交集
   v1 = {1,2,'blsnt'}
   restult = v1.intersection({1,'blsnt'})  # 会重新生成值，也可以是列表[1,'blsnt']
   print(restult)

   # 并集
   v1 = {1,2,'blsnt'}
   restult = v1.union({1,3,'blsnt'})
   print(restult)

   # 差集
   v1 = {1,2,'blsnt'}
   restult = v1.difference({1,3,'blsnt'}) # v1里面有的，这里面没有的
   print(restult)

公共功能

-  len

   ::

      v = {1,2,'李邵奇'}
      print(len(v))

-  for循环

   ::

      v = {1,2,'李邵奇'}
      for item in v:
          print(item)

-  索引【无】

-  步长【无】

-  切片【无】

-  删除【无】

-  修改【无】

元祖嵌套

.. code:: python

   # 1.列表/字典/集合 -> 不能放在集合中+不能作为字典的key 

   # 特殊情况
   info = {0, 2, 3, 4, False, "国风", None, (1, 2, 3)}
   print(info) # {0, 2, 3, 4, '国风', None, (1, 2, 3)}

   info = {
       1:'alex',
       True:'oldboy'
   }
   print(info) # {1: 'oldboy'}

3.8 内存相关(小数据池)
~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   v1 = [11,22,33]
   v2 = [11,22,33]

   v1 = 666
   v2 = 666


   # 按理 v1 和 v2 应该是不同的内存地址。特殊：
   1. 整型：  -5 ~ 256 
   2. 字符串："alex",'asfasd asdf asdf d_asdf '       ----"f_*" * 3  - 重新开辟内存。

.. code:: python

   v1 = [11,22,33]
   v2 = v1 

   # 练习1 (内部修改)
   v1 = [11,22,33]
   v2 = v1 
   v1.append(666)
   print(v2) # 含 666

   # 练习2：（赋值）
   v1 = [11,22,33]
   v2 = v1 
   v1 = [1,2,3,4]
   print(v2)

   # 练习3：(重新赋值)
   v1 = 'alex'
   v2 = v1
   v1 = 'oldboy'
   print(v2)

3.9 ==和is区别？
~~~~~~~~~~~~~~~~

-  == 用于比较值是否相等
-  is用于比较内存地址是否相等

.. code:: python

   v1 = [1,2,3,4]
   v2 = [1,2,3,4]
   print(v1 == v2)  # True
   print(v1 is v2)  # False

3.10 运算符
~~~~~~~~~~~

3.10.1 算数运算
^^^^^^^^^^^^^^^

以下假设变量：a=10，b=20

|image2|

.. code:: python

   1-100 之间所有的数相加
   total = 0
   count = 1
   while count <= 100:
     total = total + count
     count = count + 1
   print(total)

3.10.2 比较运算
^^^^^^^^^^^^^^^

以下假设变量：a=10，b=20

|image3|

3.10.3 赋值运算
^^^^^^^^^^^^^^^

以下假设变量：a=10，b=20

|image4|

3.10.4 逻辑运算
^^^^^^^^^^^^^^^

|image5|

针对逻辑运算的进一步研究：

在没有()的情况下not 优先级高于 and，and优先级高于or，即优先级关系为(
)>not>and>or，同一优先级从左往右计算。

例题：

判断下列逻辑语句的True，False。

x or y , x为真，值就是x，x为假，值是y；

x and y, x为真，值是y,x为假，值是x。

.. code:: python

   3>4 or 4<3 and 1==1
   1 < 2 and 3 < 4 or 1>2 
   2 > 1 and 3 < 4 or 4 > 5 and 2 < 1
   not 2 > 1 and 3 < 4 or 4 > 5 and 2 > 1 and 9 > 8 or 7 < 6

.. code:: python

   < or >
   v1 = 0 or 1 # 1
   v2 = 8 or 10 # 8
   v3 = 0 or 9 or 8 # 9
   第一个值如果是转换成布尔值如果是真，则value=第一值
   第一个值如果是转换成布尔值如果是假，则value=第二值
   如果有多个or条件，则从左到右依次进行比较

   < and >
   v1 = 1 and 9 # 9
   v2 = 1 and 0 # 0
   v3 = 0 and 7 # 0
   v4 = 0 and "" # 0
   v5 = 1 and 0 and 9 # 0
   如果第一个值转换成布尔值是True，则value=第二个值
   如果第一个值转换成布尔值是False，则value=第一个值
   多个and条件，则从左到右依次进行比较

   < 综合 >
   v1 = 1 and 9 or 0 and  6 # 9

3.11 in & not in
~~~~~~~~~~~~~~~~

-  in

.. code:: python

   value = "我是中国人"
   v1 = "中国" in value  # 判断中国是否在value所代指的字符串中，得到布尔类型

   # 示列
   content = input("请输入内容：")
   if '退钱' in content:
     print("包含敏感字符")

-  not in

练习题

.. code:: python

   # 三次登录失败就退出
   count = 1
   while count <= 3:
       print(count)
       user = input("请输入用户名：")
       pwd = input("请输入密码：")

       if user == 'blsnt' and pwd == 'blsnt':
           print("登录成功")
           break
       else:
           print("登录失败")
       if count ==3:break
       count += 1
       
   # 需求：允许用户最多尝试三次登录，没尝试三次过后，如果还没输入正确，就问用户是否还想继续玩，如果回答Y，就继续让其猜3次，以此往复
   count = 1
   while count <= 3:
       print(count)
       user = input("请输入用户名：")
       pwd = input("请输入密码：")

       if user == 'blsnt' and pwd == 'blsnt':
           print("登录成功")
           break
       else:
           print("登录失败")
       if count ==3:
           choice = input("请输入是否继续(Y/N):")
           if choice == 'N':
               break
           elif choice == 'Y':
               count = 1
               continue
           else:
               print("输入错误")
               break
       count += 1

   # 三次登录，并打印剩余登录次数
   count = 2
   while count >= 0:
       user = input("请输入用户名：")
     pwd = input("请输入密码：")
     if user == 'alex'  and pwd == 'alex':
       print("登录成功")
       break
     template = "用户名或密码输入错误，剩余%s次机会" %(count,)
     print(template)
     count -= 1
   else:
     print("三次机会用完")

3.12 深浅拷贝
~~~~~~~~~~~~~

.. code:: python

   # 浅拷贝
   # 应该每次都会拷贝一会(但由于小数据池,未拷贝)
   v1 = 'alex'
   import copy
   v2 = copy.copy(v1)
   print(id(v1),id(v2))

   v1 = [1,2,3,4,[11,22,33]]
   v2 = copy.copy(v1)
   print(id(v1),id(v2))
   print(id(v1[4]),id(v2[4]))

   # 深拷贝，有嵌套的时候才有意义
   import copy
   v1 = [1,2,3,4,[11,22,33]]
   v2 = copy.deepcopy(v1)
   print(id(v1),id(v2))
   print(id(v1[4]),id(v2[4]))

练习

.. code:: python

   # 浅拷贝
   import copy

   v1 = [1,2,3]

   v2 = copy.copy(v1)
   print(v1 == v2) # True
   print(v1 is v2) # False
   print(v1[0] is v2[0])  # True

   # 深拷贝
   import copy

   v1 = [1,2,3]

   v2 = copy.deepcopy(v1)
   print(v1 == v2) # True
   print(v1 is v2) # False
   print(v1[0] is v2[0])  # True ,因为都是不可变数据类型，如果没有小数据池就会是False

   # 案例
   import copy

   v1 = [1,2,3,{'k1':123}]
   v2 = copy.deepcopy(v1)
   print(v1 == v2) # True
   print(v1 is v2) # False
   print(v1[3] is v2[3])  # False

   # 特殊情况
   v1 = (1,2,3,4)  # 如果元祖是不可变类型，深浅拷贝都是一样的内存地址。里面如果有可变类型，内存地址会不一样

   import copy
   v2 = copy.copy(v1)
   print(id(v1),id(v2))
   v3 = copy.deepcopy(v1)
   print(id(v1),id(v3))

..

   注意：浅拷贝只拷贝第一层，深拷贝只拷贝层次里面的所有可变类型

第四章 文件操作
---------------

4.1 文件基本操作
~~~~~~~~~~~~~~~~

.. code:: python

   obj = open('路径',mode='模式',encoding='编码')
   obj.write()
   obj.read()
   obj.close()

4.2 打开模式
~~~~~~~~~~~~

-  r / w / a
-  r+ / w+ /a+
-  rb / wb /ab
-  r+b / w+b / a+b

4.3 操作
~~~~~~~~

-  read(),全部读到内存

-  read(1)

   -  1表示一个字符

      .. code:: python

         obj = open('a.txt',mode='r',encoding='utf-8')
         data = obj.read(1) # 1个字符
         obj.close()

   -  1表示一个字节

      .. code:: python

         obj = open('a.txt',mode='rb')
         data = obj.read(1) # 1个字节 
         obj.close()

-  write(字符串)

-  seek() ,无论模式是否带b，都是按照字节处理

-  tell(),获取光标当前所在字节位置

-  flush()，刷新到磁盘

   .. code:: python

      v = open('a.txt','w',encoding='utf-8')
      while True:
          val = input('请输入：')
          v.write(val)
          v.flush() # 强制刷到硬盘上
      v.close()  # 数据会一直写在内存中

4.4 关闭文件
~~~~~~~~~~~~

文艺青年

.. code:: python

   v = open('a.txt','w',encoding='utf-8')
   v.close()

二逼

.. code:: python

   with open('a.txt','w',encoding='utf-8') as f:
       data = v.read()
       # 缩进中的代码执行完毕后，自动关闭文件

4.5 文件修改
~~~~~~~~~~~~

.. code:: python

   with open('a.txt',mode='r',encoding='utf-8') as f1:
       data = f1.read()
   new_data = data.replace('飞洒','666')

   with open('a.txt',mode='w',encoding='utf-8') as f1:
       data = f1.write(new_data)

大文件修改

.. code:: python

   f1 = open('a.txt',mode='r',encoding='utf-8')
   f2 = open('b.txt',mode='w',encoding='utf-8')

   for line in f1:
       new_line = line.replace('阿斯','死啊')
       f2.write(new_line)
   f1.close()
   f2.close()

.. code:: python

   with open('a.txt',mode='r',encoding='utf-8') as f1, open('c.txt',mode='w',encoding='utf-8') as f2:
       for line in f1:
           new_line = line.replace('阿斯', '死啊')
           f2.write(new_line)

第五章 函数
-----------

5.1 三元运算
~~~~~~~~~~~~

.. code:: python

   v = 前面 if 条件 else 后面

   if 条件：
       v = '前面'
   else:
       v = '后面'

5.2 函数的基本结构
~~~~~~~~~~~~~~~~~~

.. code:: python

   # 定义函数
   def 函数名():
       pass

   # 函数执行
   函数名()

.. code:: python

   def get_list_first_data():
       v = [11,22,33,44]
       print(v[0])
       
   get_list_first_data()

5.3 函数的参数
~~~~~~~~~~~~~~

什么是形参？顾名思义，形参就是形式上的参数，可以理解为数学的X，没有实际的值，通过别人赋值后才有意义。相当于变量。

什么是实参？顾名思义，实参就是实际意义上的参数，是一个实际存在的参数，可以是字符串或是数字等

5.3.1 形参
^^^^^^^^^^

.. code:: python

   def get_list_first_data(a): # 形参
       v = [11,22,33,44]
       print(v[a])
       
   get_list_first_data(1) # 实参

练习题

.. code:: python

   # 1.写一个函数，函数计算列表info = [11,22,33,44,55]中所有元素的和
   def get_num():
       info = [11, 22, 33, 44, 55]
       count = 0
       for i in info:
           count += i
       print(count)
   get_num()

5.3.1.1 万能参数
''''''''''''''''

.. code:: python

   def func(*args,**kwargs):
       print(args,kwargs)
   func(1,2,3,4,5,a=2,b=6) # (1, 2, 3, 4, 5) {'a': 2, 'b': 6}

5.3.2 实参
^^^^^^^^^^

5.3.2.1 位置传参
''''''''''''''''

.. code:: python

   def func(a1,a2):
       print(a1,a2)
   func(1,2)

5.3.2.2 关键字传参
''''''''''''''''''

.. code:: python

   def func(a1,a2):
       print(a1,a2)
   func(a1=1,a2=2)

   # 关键字传参和位置传参混合使用
   def func(a1,a2,a3):
       print(a1,a2,a3)
   func(1,2,a3=9)

..

   注意: 关键字参数不能放在位置参数前面

5.3.2.3 默认参数
''''''''''''''''

.. code:: python

   def func(a1,a2=9):
       pass

   # func函数接收两个参数，调用函数进行船只时
   func(1,2) 
   func(1,a2=10)
   func(1)  # 不传第二个参数的话，就会引用a2=9这个默认参数

5.4 函数的返回值
~~~~~~~~~~~~~~~~

.. code:: python

   def get_num(num):
       count = 0
       for i in num:
           count += i
       return count # 返回值，可以添加,返回多个，默认返回None
   count = get_num([11, 22, 33, 44, 55])

5.5 作用域
~~~~~~~~~~

py文件：全局作用域

函数：局部作用域

.. code:: python

   a = 1
   def s1():
       x1 = 666
       print(x1)
       print(a)
       print(b)

   b = 2
   print(a)
   s1()
   a = 88888
   def s2():
       print(a,b)
       s1()

   s2()

总结：

-  一个函数是一个作用域

   ::

      def func():
          x = 9
          print(x)
      func()
      print(x)

-  作用域中查找数据规则：优先在自己的作用域找数据，自己没有就去 “父级”
   -> “父级” ->
   直到全局，全部么有就报错。注意：父级作用域中的值到底是什么？

   .. code:: python

      x = 10
      def func():
          x = 9
          print(x)

      func()

-  子作用域只能在父级找到值，默认无法赋值

   .. code:: python

      name = 'oldboy'
      def func():
          name = 'alex'
          print(name)
      func() # 这个只能找到alex，在自己作用域中创建了一个name='alex'

5.5.1 global
^^^^^^^^^^^^

.. code:: python

   name = 'oldboy'
   def func():
       global name
       name = 'alex'
       print(name)
   func()
   print(name) # alex  global 关键字修改全局name值

5.5.2 nonlocal
^^^^^^^^^^^^^^

.. code:: python

   name = 'oldboy'
   def func():
       name = 'alex'
       def inner():
           nonlocal name # 找到上一级的name
           name = 999
       inner()
       print(name)
   func()
   print(name)

5.6 函数嵌套
~~~~~~~~~~~~

.. code:: python

   def func():
       name = 'oldboy'
       def inner():
           print(name)
       inner()
       print(name)
   func()

5.6 函数小高级
~~~~~~~~~~~~~~

5.6.1 函数可以当做变量来使用
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   def func():
       print(123)
   v1 = func
   func()
   v1()

   # 函数放在列表当元素
   def func():
       print(123)

   v1 = [func,func,func]
   for i in v1:
       i()

5.6.2 函数可以当做参数进行传递
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   def func(arg):
       print(arg)

   func(1)
   func([1,2,3,4])

   def show():
       return 000
   func(show)

.. code:: python

   def func(arg):
       arg()
   def show():
       print(666)
   func(show)

5.7 lambda 表达式
~~~~~~~~~~~~~~~~~

用于表示简单的函数的

.. code:: python

   if 2 > 1:
       v1 = 3
   else:
       v1 = 5
   # 三元运算    
   v1 = 3 if 2>1 else 5

   # lambda表达式，为了解决简单函数的情况
   def func(a1,a2):
       return a1 + a2

   func = lambda a1,a2:a1+a2  # 隐藏了一个return
   v = func(1,2)

lambda表达式种类

.. code:: python

   func1 = lambda :100 # 没有参数返回100

   func2 = lambda *args,**kwargs: len(args)
   func2(1,2,3,4)

   DATA = 100
   def func():
       DATA = 1000
       func4 = lambda a1: a1+DATA
       v = func4(1)
       print(v)
   func()

   func5 = lambda n1,n2: n1 if n1 > n2 else n2

..

   列表所有方法基本都是返回None，字符串的所有方法基本都是返回新值

5.8 内置函数
~~~~~~~~~~~~

-  自定义函数

-  内置函数

   -  len

   -  open

   -  range

   -  id

   -  输入输出

      -  print
      -  input

   -  强制转换

      -  dict
      -  list
      -  tuple
      -  int
      -  str
      -  bool
      -  set

   -  数学相关

      -  abs，绝对值

      -  float, 浮点型

         .. code:: python

            v = 55
            v1 = float(v)
            print(v1) # 55.0

      -  max, 找到最大值

         .. code:: python

            v = [1,2,3,455]
            result = max(v)
            print(result) # 455

      -  min，找到最小值

      -  sum，求和

      -  divmod，两个数相除，得商和余数

         .. code:: python

            a,b = divmod(1001,5)
            print(a,b)

         练习题

         .. code:: python

            # 通过分页对数据进行显示
            #!/usr/bin/env python
            # -*- coding:utf-8 -*-
            USER_LIST = []
            for i in range(1,836):
                temp = {'name':'blsnt-%s' %i}
                USER_LIST.append(temp)

            # 数据总条数
            total_count = len(USER_LIST)

            # 每页显示10条
            per_page_count = 10

            # 总页码数
            max_page_num,a = divmod(total_count,per_page_count)
            if a>0:
                max_page_num += 1
            while True:
                pager = int(input("请输入要查看的页码："))
                if pager < 1 or pager > max_page_num:
                    print('页码输入不合法，必须是1~%s' %max_page_num)
                else:
                    start = (pager - 1) * 10
                    end = pager * per_page_count
                    data = USER_LIST[start:end]
                    for item in data:
                        print(item)

   -  进制相关

      -  bin，将其他进制转换成二进制

         .. code:: python

            num = 13
            v1 = bin(num)
            print(v1)

      -  oct，将其他进制转换成八进制

         .. code:: python

            num = 7
            v1 = oct(num)
            print(v1)

      -  int，将其他进制转换成十进制

         .. code:: python

            # 二进制转十进制，其他的也是一样
            v1 = '0b1101'
            result = int(v1,base=2)
            print(result)

      -  hex，将其他进制转换成十六进制

         .. code:: python

            num = 16
            v1 = hex(num)
            print(v1)

   -  编码相关

      -  chr,将十进制数字转换成unicode编码中的对应字符串

         .. code:: python

            v = chr(99)
            print(v) # c

      -  ord,根据字符找到在unicode中找到其对应的十进制

         .. code:: python

            v = ord('A')
            print(v) # 65

         随机验证码练习题

         .. code:: python

            import random

            def get_random_data(length=6):
                DATA = []
                for i in range(length):
                    v = random.randint(65,90)
                    DATA.append(chr(v))
                return ''.join(DATA)

            num = get_random_data()
            print(num)

   -  高级内置函数

      -  filter

         .. code:: python

            result = filter(lambda x: type(x) == int,v1) # 函数返回True添加到列表中，返回False则丢弃
            print(list(result))

      -  map，循环每个元素(第二个参数)，然后让每个元素执行函数(第一个参数)，将每个函数的返回值添加到列表中

         .. code:: python

            v1 = [11,22,33,44]
            res = map(lambda args: args + 100,v1) # 第一个参数必须是一个函数,第二个参数必须是可迭代的类型,循环列表拿到的每个值当做参数传递给函数，会将函数的返回值添加到空列表中
            print(list(res)) # [111, 122, 133, 144]

      -  reduce，会将列表的元素相加

         .. code:: python

            import functools
            v1 = [1,2,3,4,5,6]
            result = functools.reduce(lambda x,y: x+y,v1) # x第一次为1，y为2，第二次x为1+2=3，y为3
            print(result) # 21

5.9 函数中高级
~~~~~~~~~~~~~~

5.9.1 函数当返回值
^^^^^^^^^^^^^^^^^^

.. code:: python

   def func():
       print(123)
       
   def bar()：
       return func

   v = bar()
   v()

.. code:: python

   def bar():
       def inner():
           print(123)
       return inner
   v = bar()
   v()

.. code:: python

   name = 'olboyd'
   def bar():
       name = 'alex'
       def inner():
           print(123)
       return inner
   v = bar()
   v()

5.9.2 闭包
^^^^^^^^^^

闭包概念：为函数创建一块区域并为其维护自己数据，以后执行时方便调用【应用场景：装饰器/SQLAlchemy源码】

.. code:: python

   def func(name):
       def inner():
           print(name)
       return inner 

   v1 = func('alex')
   v1()
   v2 = func('eric')
   v2()

..

   注意name 是需要调用的，才能达到闭包的效果

5.10 装饰器
~~~~~~~~~~~

目的：在不改变原函数的基础上，在函数执行前后自定义功能

5.10.1 装饰器基本格式
^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   def x(func):
       def inner():
           return func()
       return inner 

   @x
   def index():
       pass

5.10.2 关于于参数
^^^^^^^^^^^^^^^^^

.. code:: python

   def x1(func):
       def inner(*args,**kwargs):
           return func(*args,**kwargs)
       return inner 

   @x1
   def f1():
       pass

   @x1
   def f2(a1):
       pass
   @x1
   def f3(a1,a2):
       pass 

5.10.3 关于返回值
^^^^^^^^^^^^^^^^^

.. code:: python

   def x1(func):
       def inner(*args,**kwargs):
           data = func(*args,**kwargs)
           return data
       return inner 

   @x1
   def f1():
       print(123)
       
   v1 = f1()
   print(v1)

.. code:: python

   def x1(func):
       def inner(*args,**kwargs):
           data = func(*args,**kwargs)
           return data
       return inner 

   @x1
   def f1():
       print(123)
       return 666
   v1 = f1()
   print(v1)

.. code:: python

   def x1(func):
       def inner(*args,**kwargs):
           data = func(*args,**kwargs)
       return inner 

   @x1
   def f1():
       print(123)
       return 666

   v1 = f1()
   print(v1)

5.10.4 装饰器推荐写法
^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   装饰器建议写法：
   def x1(func):
       def inner(*args,**kwargs):
           data = func(*args,**kwargs)
           return data
       return inner 

5.10.5 关于前后
^^^^^^^^^^^^^^^

.. code:: python

   def x1(func):
       def inner(*args,**kwargs):
           print('调用原函数之前')
           data = func(*args,**kwargs) # 执行原函数并获取返回值
           print('调用员函数之后')
           return data
       return inner 

   @x1
   def index():
       print(123)
       
   index()

5.10.6 带参数的装饰器
^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   # 第一步：执行 ret = xxx(index)
   # 第二步：将返回值赋值给 index = ret 
   @xxx
   def index():
       pass

   # 第一步：执行 v1 = uuu(9)
   # 第二步：ret = v1(index)
   # 第三步：index = ret 
   @uuu(9)
   def index():
       pass

.. code:: python

   # ################## 普通装饰器 #####################
   def wrapper(func):
       def inner(*args,**kwargs):
           print('调用原函数之前')
           data = func(*args,**kwargs) # 执行原函数并获取返回值
           print('调用员函数之后')
           return data
       return inner 

   @wrapper
   def index():
       pass

   # ################## 带参数装饰器 #####################
   def x(counter):
       def wrapper(func):
           def inner(*args,**kwargs):
               data = func(*args,**kwargs) # 执行原函数并获取返回值
               return data
           return inner 
       return wrapper 

   @x(9)
   def index():
       pass

练习题

.. code:: python

   # 写一个带参数的装饰器，实现：参数是多少，被装饰的函数就要执行多少次，把每次结果添加到列表中，最终返回列表。
   def xxx(counter):
       print('x函数')
       def wrapper(func):
           print('wrapper函数')
           def inner(*args,**kwargs):
               v = []
               for i in range(counter):
                   data = func(*args,**kwargs) # 执行原函数并获取返回值
                   v.append(data)
               return v
           return inner
       return wrapper

   @xxx(5)
   def index():
       return 8

   v = index()
   print(v)

   # 写一个带参数的装饰器，实现：参数是多少，被装饰的函数就要执行多少次，并返回最后一次执行的结果【面试题】
   def xxx(counter):
       print('x函数')
       def wrapper(func):
           print('wrapper函数')
           def inner(*args,**kwargs):
               for i in range(counter):
                   data = func(*args,**kwargs) # 执行原函数并获取返回值，data一直被覆盖
               return data
           return inner
       return wrapper

   @xxx(5)
   def index():
       return 8

   v = index()
   print(v)
   # 写一个带参数的装饰器，实现：参数是多少，被装饰的函数就要执行多少次，并返回执行结果中最大的值。
   def xxx(counter):
       print('x函数')
       def wrapper(func):
           print('wrapper函数')
           def inner(*args,**kwargs):
               value = 0
               for i in range(counter):
                   data = func(*args,**kwargs) # 执行原函数并获取返回值
                   if data > value:
                       value = data 
               return value
           return inner
       return wrapper

   @xxx(5)
   def index():
       return 8

   v = index()
   print(v)

   # 不同需求的装饰器通过True和False实现不同的功能
   def x(counter):
       print('x函数')
       def wrapper(func):
           print('wrapper函数')
           def inner(*args,**kwargs):
               if counter:
                   return 123
               return func(*args,**kwargs)
           return inner
       return wrapper

   @x(True)
   def fun990():
       pass

   @x(False)
   def func10():
       pass

示例

.. code:: python

   def func(arg):
       def inner():
           print('before')
           v = arg()
           print('after')
           return v 
       return inner 

   def index():
       print('123')
       return '666'


   # 示例一
   """
   v1 = index() # 执行index函数，打印123并返回666赋值给v1.
   """
   # 示例二
   """
   v2 = func(index) # v2是inner函数，arg=index函数
   index = 666 
   v3 = v2()
   """
   # 示例三
   """
   v4 = func(index)
   index = v4  # index ==> inner 
   index()
   """

   # 示例四
   index = func(index)
   index()

.. code:: python

   def func(arg):
       def inner():
           v = arg()
           return v 
       return inner 

   # 第一步：执行func函数并将下面的函数参数传递，相当于：func(index)
   # 第二步：将func的返回值重新赋值给下面的函数名。 index = func(index)
   @func 
   def index():
       print(123)
       return 666

   print(index)

应用：

.. code:: python

   # 计算函数执行时间

   def wrapper(func):
       def inner():
           start_time = time.time()
           v = func()
           end_time = time.time()
           print(end_time-start_time)
           return v
       return inner

   @wrapper
   def func1():
       time.sleep(2)
       print(123)
   @wrapper
   def func2():
       time.sleep(1)
       print(123)

   def func3():
       time.sleep(1.5)
       print(123)

   func1()

装饰器编写格式

.. code:: python

   def 外层函数(参数): 
       def 内层函数(*args,**kwargs):
           return 参数(*args,**kwargs)
       return 内层函数

装饰器应用格式

.. code:: python

   @外层函数
   def index():
       pass

   index()

5.11 推导式
~~~~~~~~~~~

5.11.1 列表推导式
^^^^^^^^^^^^^^^^^

.. code:: python

   """
   目的：方便的生成一个列表。
   格式：
       v1 = [i for i in 可迭代对象 ]
       v2 = [i for i in 可迭代对象 if 条件 ] # 条件为true才进行append
   """
   v1 = [ i for i in 'alex' ]  
   v2 = [i+100 for i in range(10)]
   v3 = [99 if i>5 else 66  for i in range(10)]

   def func():
       return 100
   v4 = [func for i in range(10)]

   v5 = [lambda : 100 for i in range(10)]
   result = v5[9]()

   def func():
       return i
   v6 = [func for i in range(10)]
   result = v6[5]()

   v7 = [lambda :i for i in range(10)]
   result = v7[5]()


   v8 = [lambda x:x*i for i in range(10)] # 新浪微博面试题
   # 1.请问 v8 是什么？
   # 2.请问 v8[0](2) 的结果是什么？

   # 面试题
   def num():
       return [lambda x:i*x for i in range(4)]
   # num() -> [函数,函数,函数,函数]
   print([ m(2) for m in num() ]) # [6,6,6,6]

   # ##################### 筛选 #########################
   v9 = [i for i in range(10) if i > 5]

5.11.2 集合推导式
^^^^^^^^^^^^^^^^^

.. code:: python

   v1 = { i for i in 'alex' }

5.11.3 字典推导式
^^^^^^^^^^^^^^^^^

.. code:: python

   v1 = { 'k'+str(i):i for i in range(10) }

第六章 模块
-----------

模块的概念：一系列功能的集合体，可以给其他文件提供功能

6.1 模块基本知识
~~~~~~~~~~~~~~~~

-  内置模块，python内部提供的功能

   .. code:: python

      import os
      os.path.listdir()

-  自定义模块

-  第三方模块，下载/安装/使用

python 找模块的地方

.. code:: python

   import sys
   print(sys.path)

   # 添加目录路径
   sys.path.append('D:\test')

定义模块时，可以把一个py文件或一个文件夹（包）当做一个模块，以方便以后其他py文件的调用

对于包的定义：

-  py2：文件夹中必须有__init__.py
-  py3：不需要__init__.py

6.1.1 导入模块方式
^^^^^^^^^^^^^^^^^^

导入模块,加载此模块中的所有的值到内存

.. code:: python

   import xxx 

   # import 所做的事情,xxx名字就是模块xxx的文件对象，存放的是xxx文件的地址
   1. 将被导入的模块编译成模块名对应的pyc文件
   2. 从上至下执行被调用模块的所有代码
   3. 形成模块的名称空间,将模块中的所有名字存放在模块的名称空间中
   4. 在要使用模块的文件(当前文件)的名称空间中产生一个与模块名同名的名字指向模块的名称空间

   from xxx import xxx
   from xxx import *
   from xxx import xxx as f  # 模块起别名
   1. 模块名与当前文件中名字发生冲突，用起名字解决冲突
   2. 优化模块名

   from xxx import xxx,zzz

   # 相对导入
   form . import xxx # 相对导入必须要有一个父级目录，在根目录不行

6.1.2 配置BASE_DIR
^^^^^^^^^^^^^^^^^^

.. code:: python

   # 默认只有运行的py文件的目录会导入到系统path中,要排除pycharm导入
   import os
   import sys
   print(__file__)  # 当前运行脚本的路径,要配置abspath获取绝对路径
   BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
   sys.path.append(BASE_DIR)

6.1.3 项目常用目录
^^^^^^^^^^^^^^^^^^

-  bin 程序入口 可执行文件

-  config 配置文件

-  db 数据

-  lib 公共代码

-  src 业务代码

-  脚本

|image6|

-  单执行文件

|image7|

-  多执行文件

|image8|

6.1.4 模块实现单例模式
^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   # jd.py
   class Foo(object):
       pass

   obj = Foo()

.. code:: python

   # app.py
   import jd # 加载jd.py，加载最后会实例化一个Foo对象并赋值给obj
   print(jd.obj)

6.1.5 重新加载模块
^^^^^^^^^^^^^^^^^^

.. code:: python

   import jd # 第一次加载：会加载一遍jd中所有的内容。
   import jd # 由已经加载过，就不在加载。
   print(456)

.. code:: python

   import importlib
   import jd
   importlib.reload(jd)
   print(456)

6.1.6 模块的多次导入
^^^^^^^^^^^^^^^^^^^^

.. code:: python

   # m1.py
   print('导入模块')

   # test.py
   # 第一次导入模块已经完成导入模块的三步，编译，运行(产生名称空间存放名字)，执行文件产生名字指向模块
   import m1
   # 再次导入：前两步是重复的，所以只会在当前文件再产生一个名字指向模块的名称空间 
   import m1 as m

6.1.7 模块导入的执行过程
^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   #　执行文件.py
   print('加载')
   import m1   #　进入m1,m1全部走完回到这里
   print('结束')

   #　m1.py
   print('m1 开始')
   y = 10
   import m2   # 进入m2，m2全部走完回到这里
   print('m1 结束')

   # m2.py
   print('m2 开始')
   y = 20
   print('m2 结束')

执行过程结果

.. code:: python

   加载
   m1 开始
   m2 开始
   m2 结束
   m1 结束
   结束

..

   注意在执行文件中访问20

   print(m1.m2.y)

6.1.8 py文件的两种执行方式
^^^^^^^^^^^^^^^^^^^^^^^^^^

-  自执行

   -  在模块中的__name_\_ = \__main_\_

-  模块方式导入执行

   -  作为模块导入执行__name_\_ = ‘模块名’

.. code:: python

   # 执行的py
   import m1
   print('模块导入执行',num)

   # m1.py
   num = 100
   print('模块自执行',num)

..

   模块导入执行的__name_\_ 是模块名，自执行的时候是__main_\_

共存

.. code:: python

   # 模块文件
   # 先写所有的模块资源(数据与函数)
   # 模块最下方
   if __name__ == '__main__'
       # 自执行的逻辑代码

6.2 常用内置模块
~~~~~~~~~~~~~~~~

-  logging

基本应用

日志处理本质：Logger/FileHandler/Formatter

推荐处理日志方式

.. code:: python

   import logging

   file_handler = logging.FileHandler(filename='x1.log', mode='a', encoding='utf-8',)
   logging.basicConfig(
       format='%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
       datefmt='%Y-%m-%d %H:%M:%S %p',
       handlers=[file_handler,],
       level=logging.ERROR
   )

   logging.error('你好')

推荐处理日志方式 + 日志分割

.. code:: python

   import time
   import logging
   from logging import handlers
   # file_handler = logging.FileHandler(filename='x1.log', mode='a', encoding='utf-8',)
   file_handler = handlers.TimedRotatingFileHandler(filename='x3.log', when='s', interval=5, encoding='utf-8')
   logging.basicConfig(
       format='%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
       datefmt='%Y-%m-%d %H:%M:%S %p',
       handlers=[file_handler,],
       level=logging.ERROR
   )

   for i in range(1,100000):
       time.sleep(1)
       logging.error(str(i))

注意事项：

.. code:: python

   # 在应用日志时，如果想要保留异常的堆栈信息。
   import logging
   import requests

   logging.basicConfig(
       filename='wf.log',
       format='%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
       datefmt='%Y-%m-%d %H:%M:%S %p',
       level=logging.ERROR
   )

   try:
       requests.get('http://www.xxx.com')
   except Exception as e:
       msg = str(e) # 调用e.__str__方法
       logging.error(msg,exc_info=True)

settings配置

.. code:: python

   # settings.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-
   import os
   BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
   LOG_FILE_PATH = os.path.join(BASE_DIR,'log','cmdb.log')
   LOG_WHEN = "s"
   LOG_INTERVAL = 5

   # log.py配置
   import os
   import logging
   from logging import handlers
   from config import settings


   def get_logger():
       file_handler = handlers.TimedRotatingFileHandler(filename=settings.LOG_FILE_PATH,
                                                        when=settings.LOG_WHEN,
                                                        interval=settings.LOG_INTERVAL,
                                                        encoding='utf-8')
       logging.basicConfig(
           format='%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
           datefmt='%Y-%m-%d %H:%M:%S %p',
           handlers=[file_handler],
           level=logging.ERROR
       )
       return logging

   logger = get_logger()

logging高级用法

默认日志级别30

日志模块的详细用法：

.. code:: python

   import logging
   # 1.Logger: 产生日志
   # 2.Filter: 几乎不用
   # 3.Handler：接收Logger传过来的日志，进行日志格式化，可以打印到终端，也可以打印到文件(可以有多个)
   # 4.Formatter：日志格式

   '''
   critical=50
   error =40
   warning =30
   info = 20
   debug =10
   '''

   #1、logger对象：负责产生日志，然后交给Filter过滤，然后交给不同的Handler输出
   logger=logging.getLogger(__file__)

   #2、Filter对象：不常用，略

   #3、Handler对象：接收logger传来的日志，然后控制输出
   h1=logging.FileHandler('t1.log') #打印到文件
   h2=logging.FileHandler('t2.log') #打印到文件
   h3=logging.StreamHandler() #打印到终端

   #4、Formatter对象：日志格式
   formmater1=logging.Formatter('%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
                       datefmt='%Y-%m-%d %H:%M:%S %p',)

   formmater2=logging.Formatter('%(asctime)s :  %(message)s',
                       datefmt='%Y-%m-%d %H:%M:%S %p',)

   formmater3=logging.Formatter('%(name)s %(message)s',)


   #5、为Handler对象绑定格式
   h1.setFormatter(formmater1)
   h2.setFormatter(formmater2)
   h3.setFormatter(formmater3)

   #6、将Handler添加给logger并设置日志级别
   logger.addHandler(h1)
   logger.addHandler(h2)
   logger.addHandler(h3)
   logger.setLevel(10)

   #7、测试
   logger.debug('debug')
   logger.info('info')
   logger.warning('warning')
   logger.error('error')
   logger.critical('critical')

python 之 logger日志 字典配置文件

.. code:: python

   # settings.py
   import os
   import sys

   BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
   sys.path.append(BASE_DIR)


   # 定义日志文件的路径
   LOG_PATH=os.path.join(BASE_DIR,'logs','access.log')

   # 定义三种日志输出格式 开始
   standard_format = '[%(asctime)s][%(threadName)s:%(thread)d][task_id:%(name)s][%(filename)s:%(lineno)d]' \
                     '[%(levelname)s][%(message)s]' #其中name为getlogger指定的名字

   #simple_format = '[%(asctime)s] - [%(name)s] - %(levelname)s -%(module)s:  %(message)s'

   simple_format =  '[%(levelname)s][%(asctime)s][%(filename)s:%(lineno)d]%(message)s'

   id_simple_format = '[%(levelname)s][%(asctime)s] %(message)s'

   # log配置字典
   LOGGING_DIC = {
       'version': 1,
       # 禁用已经存在的logger实例
       'disable_existing_loggers': False,
       # 定义日志 格式化的 工具
       'formatters': {
           'standard': {
               'format': standard_format
           },
           'simple': {
               'format': simple_format
           },
           'id_simple': {
               'format': id_simple_format
           },
       },
       # 过滤
       'filters': {},  # jango此处不同
       'handlers': {
           #打印到终端的日志
           'stream': {
               'level': 'DEBUG',
               'class': 'logging.StreamHandler',  # 打印到屏幕
               'formatter': 'standard'
           },
           #打印到文件的日志,收集info及以上的日志
           'access': {
               'level': 'DEBUG',
               'class': 'logging.handlers.RotatingFileHandler',  # 保存到文件
               'formatter': 'standard',
               'filename': LOG_PATH,       # 日志文件路径
               'maxBytes': 1024*1024*5,  # 日志大小 5M
               'backupCount': 5,
               'encoding': 'utf-8',  # 日志文件的编码，再也不用担心中文log乱码了
           },
       },
       # logger实例
       'loggers': {
           # 默认的logger应用如下配置
           '': {
               'handlers': ['stream', 'access'],  # 这里把上面定义的两个handler都加上，即log数据既写入文件又打印到屏幕
               'level': 'DEBUG',
               'propagate': True,  # 向上（更高level的logger）传递
           },
           # logging.getLogger(__name__)拿到的logger配置
           # 这样我们再取logger对象时logging.getLogger(__name__)，不同的文件__name__不同，这保证了打印日志时标识信息不同，
           # 但是拿着该名字去loggers里找key名时却发现找不到，于是默认使用key=''的配置
       },
   }


   # common.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import logging.config
   from config import settings
   def load_my_logging_cfg(log_name):
       logging.config.dictConfig(settings.LOGGING_DIC)  # 导入上面定义的logging配置
       logger = logging.getLogger(log_name)  # 生成一个log实例
       return  logger

   # run.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-
   from config import settings
   from lib import common

   try:
       int('adgdsg')
   except Exception as e:
       msg = str(e)
       logger = common.load_my_logging_cfg('吃饭功能')
       logger.info(msg,exc_info=True)

-  importlib

.. code:: python

   import importlib
   # 用字符串的形式导入模块。
   redis = importlib.import_module('utils.redis')
   # 用字符串的形式去对象（模块）找到他的成员。
   getattr(redis,'func')()


   #!/usr/bin/env python
   # -*- coding:utf-8 -*-
   from utils import redis
   import importlib

   middleware_classes = [
       'utils.redis.Redis',
       # 'utils.mysql.MySQL',
       'utils.mongo.Mongo'
   ]
   for path in middleware_classes:
       module_path,class_name = path.rsplit('.',maxsplit=1)
       module_object = importlib.import_module(module_path)# from utils import redis
       cls = getattr(module_object,class_name)
       obj = cls()
       obj.connect()

..

   补充：开放封闭原则（源代码不改变，改变输入信息，输出信息随即改变）

-  shutil

.. code:: python

   import shutil

   # 删除目录
   # shutil.rmtree('test')

   # 重命名
   # shutil.move('test','ttt')

   # 压缩文件
   # shutil.make_archive('zzh','zip','D:\code\s21day16\lizhong')

   # 解压文件
   # shutil.unpack_archive('zzh.zip',extract_dir=r'D:\code\xxxxxx\xxxx',format='zip')

示例

.. code:: python

   import os
   import shutil
   from datetime import datetime
   ctime = datetime.now().strftime('%Y-%m-%d-%H-%M-%S')

   # 1.压缩lizhongwei文件夹 zip
   # 2.放到到 code 目录（默认不存在）
   # 3.将文件解压到D:\x1目录中。

   if not os.path.exists('code'):
       os.makedirs('code')
   shutil.make_archive(os.path.join('code',ctime),'zip','D:\code\s21day16\lizhongwei')

   file_path = os.path.join('code',ctime) + '.zip'
   shutil.unpack_archive(file_path,r'D:\x1','zip')

-  random

.. code:: python

   # randint 随机
   import random

   def get_random_data(length=6):
       DATA = []
       for i in range(length):
           v = random.randint(65,90)  # 随机取65-90之间的数字
           DATA.append(chr(v))
       return ''.join(DATA)

   num = get_random_data()
   print(num)

-  hashlib

.. code:: python

   # md5 将指定的字符串进行加密
   import hashlib

   def get_md5(data):
       obj = hashlib.md5()
       obj.update(data.encode('utf-8'))
       result = obj.hexdigest()
       return result

   val = get_md5('122453535')
   print(val)

   # 加盐
   import hashlib

   def get_md5(data):
       obj = hashlib.md5('adgfsdgesgasdg'.encode('utf-8'))
       obj.update(data.encode('utf-8'))
       result = obj.hexdigest()
       return result

   val = get_md5('123')
   print(val)

   # 

-  getpass

.. code:: python

   # 密码不显示
   pwd = getpass.getpass('请输入密码')
   print(pwd)

-  os

和操作系统相关的数据。

.. code:: python

   # 文件或目录是否存在
   os.path.exists(path)  如果path存在，返回True；如果path不存在，返回False

   # 文件大小
   os.stat('20190409_192149.mp4').st_size  获取文件大小

   # 文件绝对路径
   os.path.abspath()   获取一个文件的绝对路径

   # 获取路径的上级目录
   os.path.dirname 获取路径的上级目录

   # 路径的拼接 
   # os.path.join
   import os
   path = "D:\code\s21day14" # user/index/inx/fasd/
   v = 'n.txt'

   result = os.path.join(path,v)
   print(result)
   result = os.path.join(path,'n1','n2','n3')
   print(result)

   # 查看一个目录下所有的文件【第一层】
   import os

   result = os.listdir(r'D:\code\s21day14')
   for path in result:
       print(path)
       
   # 查看一个目录下所有的文件【所有层】
   import os

   result = os.walk(r'D:\code\s21day14')
   for a,b,c in result:
       # a,正在查看的目录 b,此目录下的文件夹  c,此目录下的文件
       for item in c:
           path = os.path.join(a,item)
           print(path)
           
   # 创建文件夹，可以多级创建
   os.makedirs()

   import os
   file_path = r'db\xx\xo\xxxxx.txt'

   file_folder = os.path.dirname(file_path)
   if not os.path.exists(file_folder):
       os.makedirs(file_folder)

   with open(file_path,mode='w',encoding='utf-8') as f:
       f.write('asdf')

-  sys

.. code:: python

   # python解释器相关的数据
   import sys

   # python默认支持的递归数量
   print(sys.getrecursionlimit())

   # 获取一个值的应用计数
   a = [11,22,33]
   b = a
   print(sys.getrefcount(a))

   # 进度条
   import os
   # 读取文件大小
   file_path = 'CentOS-7-x86_64-DVD-1810.iso'
   file_size = os.stat(file_path).st_size

   chunk_size = 1024
   read_size = 0
   with open(file_path,mode='rb') as f,open('centos.iso',mode='wb') as f1:
       while read_size < file_size:
           chunk = f.read(1024) # 每次读最多1024个字节
           f1.write(chunk)
           read_size += len(chunk)
           val = int(read_size / file_size * 100)
           print('%s%%\r' %val,end='')
           
   # 获取脚本参数
   获取用户执行脚本时，传入的参数。
   C:\Python36\python36.exe D:/code/s21day14/7.模块传参.py D:/test
   sys.argv = [D:/code/s21day14/7.模块传参.py, D:/test]

   path = sys.argv[1]
   #删除目录
   import shutil
   shutil.rmtree(path)

   # sys.path ,默认python查找模块的目录
   import sys
   sys.path.append('路径')

-  json

.. code:: python

   # json.dumps() 序列化
   v = [12,3,4,5,{'k1':'v1'},True,'asf']
   v_json = json.dumps(v,ensure_ascii=False) # 序列化保留中文显示
   print(v_json) # [12, 3, 4, 5, {"k1": "v1"}, true, "asf"]


   # json.loads()  反序列化
   v = '[12, 3, 4, 5, {"k1": "v1"}, true, "asf"]'
   v_list = json.loads(v)
   print(v_list) #  [12, 3, 4, 5, {'k1': 'v1'}, True, 'asf']

-  pickle

.. code:: python

   import pickle

   # #################### dumps/loads ######################
   """
   v = {1,2,3,4}
   val = pickle.dumps(v)
   print(val)
   data = pickle.loads(val)
   print(data,type(data))
   """

   """
   def f1():
       print('f1')

   v1 = pickle.dumps(f1)
   print(v1)
   v2 = pickle.loads(v1)
   v2()
   """

   # #################### dump/load ######################
   # v = {1,2,3,4}
   # f = open('x.txt',mode='wb')
   # val = pickle.dump(v,f)
   # f.close()

   # f = open('x.txt',mode='rb')
   # data = pickle.load(f)
   # f.close()
   # print(data)

-  logging

6.3 异常处理
~~~~~~~~~~~~

.. code:: python

   try:
       pass
   except Exception as e:
       pass 

.. code:: python

   # finally 的运用
   def:
       try:
           int('asdf')
       except Exception as e:
           print(e)
           return 123
       finally:
           print('1')
   func()
   #即使return  finally最后也会执行

主动抛出异常

.. code:: python

   try:
       int('123')
       raise Exception('dsfsegsdg') #主动触发异常
       except Exception as e:
           print('111')

自定义异常

.. code:: python

   class MyException(Exception):
       def __init__(self,message):
           super().__init__()
           self.message = message

   try:
       raise MyException('asdf')
   except MyException as e:
       print(e.message)

6.4 第三方模块
~~~~~~~~~~~~~~

6.5 主文件
~~~~~~~~~~

.. code:: python

   if __name__ == '__main__':

第七章 面向对象
---------------

7.1 迭代器
~~~~~~~~~~

自己不会写迭代器，只用

任务：展示列表中所有的数据

-  while + 索引 + 计数器
-  迭代器

   -  列表转迭代器

7.3 生成器（函数的变异）
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   # 生成器函数（内部是否包含yield）
   def func(arg):
       arg = arg+1
       yield 1
       yield 2
       yield 100
   # 函数内部代码不会执行，返回一个生成器对象
   func(200)

from

.. code:: python

   def base():
       yield 88
       yield 99

   def func():
       yield 1
       yield 2
       yield from base()
       yield 3

   result = func()

   for item in result:
       print(item)

生成器推导式

.. code:: python

   # def func():
   #     result = []
   #     for i in range(10):
   #         result.append(i)
   #     return result
   # v1 = func()
   v1 = [i for i in range(10)] # 列表推导式，立即循环创建所有元素。
   print(v1)


   # def func():
   #     for i in range(10):
   #         yield i
   # v2 = func()
   v2 = (i for i in range(10)) # 生成器推导式，创建了一个生成器，内部循环为执行。
   print(v2)

7.4 面向对象基本格式
~~~~~~~~~~~~~~~~~~~~

.. code:: python

   # ###### 定义类 ###### 
   class 类名:
       def 方法名(self,name):
           print(name)
           return 123
       def 方法名(self,name):
           print(name)
           return 123
       def 方法名(self,name):
           print(name)
           return 123
   # ###### 调用类中的方法 ###### 
   # 1.创建该类的对象
   obj = 类名()
   # 2.通过对象调用方法
   result = obj.方法名('alex')
   print(result)

应用场景：遇到很多函数，需要给函数进行归类和划分。 【封装】

7.5 类成员
~~~~~~~~~~

-  类

   -  类变量
   -  绑定方法
   -  类方法
   -  静态方法
   -  属性

-  实例（对象）

   -  实例变量

7.5.1 实列变量（属于对象成员）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|image9|

7.5.2 类变量
^^^^^^^^^^^^

|image10|

-  定义：写在类的下一级和方法同一级。

-  访问：

   .. code:: python

      类.类变量名称
      对象.类变量名称

..

   总结：找变量优先找自己，自己没有找 类 或
   基类；修改或赋值只能在自己的内部设置。

|image11|

7.5方法
~~~~~~~

7.5.1 绑定方法/普通方法
^^^^^^^^^^^^^^^^^^^^^^^

-  定义：至少有一个self参数
-  执行：先创建对象，由对象.方法

.. code:: python

   # 普通方法 第一个方法是没有使用self，比较浪费内存
   class Foo:
       def func(self,a,b):
           print(a,b)
           
   obj = Foo()
   obj.func(1,2)

   # 绑定方法 使用到了self，所以是有意义的
   class Foo:
       def __init__(self):
           self.name = 123

       def func(self, a, b):
           print(self.name, a, b)

   obj = Foo()
   obj.func(1, 2)

7.5.2 静态方法
^^^^^^^^^^^^^^

-  定义：

   -  @staticmethod装饰器
   -  参数无限制

-  执行：

   -  类.静态方法名 ()
   -  对象.静态方法() (不推荐)

.. code:: python

   class Foo:
       def __init__(self):
           self.name = 123

       def func(self, a, b):
           print(self.name, a, b)

       @staticmethod
       def f1():
           print(123)

   obj = Foo()
   obj.func(1, 2)

   Foo.f1()
   obj.f1() # 不推荐

7.5.3 类方法
^^^^^^^^^^^^

-  定义：

   -  @classmethod装饰器
   -  至少有cls参数，当前类。

-  执行：

   -  类.类方法()
   -  对象.类方法() （不推荐）

.. code:: python

   class Foo:
       def __init__(self):
           self.name = 123

       def func(self, a, b):
           print(self.name, a, b)

       @classmethod
       def f2(cls,a,b):
           print('cls是当前类',cls)
           print(a,b)

   obj = Foo()
   obj.func(1, 2)

   Foo.f2(1,2)

7.6 嵌套
~~~~~~~~

-  函数：参数可以是任意类型。
-  字典：对象和类都可以做字典的key和value
-  继承的查找关系

.. code:: python

   class StarkConfig(object):
       pass

   class AdminSite(object):
       def __init__(self):
           self.data_list = []
           
       def register(self,arg):
           self.data_list.append(arg)
           
   site = AdminSite()

   obj = StarkConfig()
   site.register(obj)

.. code:: python

   class StarkConfig(object):
       def __init__(self,name,age):
           self.name = name
           self.age = age

   class AdminSite(object):
       def __init__(self):
           self.data_list = []
           self.sk = None

       def set_sk(self,arg):
           self.sk = arg
           
           
   site = AdminSite() # data_list = []  sk = StarkConfig
   site.set_sk(StarkConfig)
   site.sk('alex',19)

.. code:: python

   class StackConfig(object):
       pass

   class Foo(object):
       pass

   class Base(object):
       pass

   class AdminSite(object):
       def __init__(self):
           self._register = {}

       def registry(self,key,arg):
           self._register[key] = arg

   site = AdminSite()
   site.registry(1,StackConfig)
   site.registry(2,StackConfig)
   site.registry(3,StackConfig)
   site.registry(4,Foo)
   site.registry(5,Base)

   for k,v in site._register.items():
       print(k,v() )

7.7 特殊成员
~~~~~~~~~~~~

7.7.1 ``__init__``
^^^^^^^^^^^^^^^^^^

.. code:: python

   # 初始化方法
   class Foo:
       def __init__(self.al):
           self.a1 = a1
   obj = Foo('alex')

7.7.2 ``__new__``
^^^^^^^^^^^^^^^^^

.. code:: python

   class Foo(object):
       def __init__(self):
           """
           用于给对象中赋值，初始化方法
           """
           self.x = 123
       def __new__(cls, *args, **kwargs):
           """
           用于创建空对象，构造方法，在__init__之前执行
           :param args: 
           :param kwargs: 
           :return: 
           """
           return object.__new__(cls)

   obj = Foo()

7.7.3 ``__cal__``
^^^^^^^^^^^^^^^^^

.. code:: python

   class Foo(object):
       def __call__(self, *args, **kwargs):
           print('执行call方法')

   # obj = Foo()
   # obj()
   Foo()()

7.7.4 ``__getitem__   __setitem__ __delitem__``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   class Foo(object):

       def __setitem__(self, key, value):
           pass

       def __getitem__(self, item):
           return item + 'uuu'

       def __delitem__(self, key):
           pass


   obj1 = Foo()
   obj1['k1'] = 123  # 内部会自动调用 __setitem__方法
   val = obj1['xxx']  # 内部会自动调用 __getitem__方法
   print(val)
   del obj1['ttt']  # 内部会自动调用 __delitem__ 方法

7.7.5 ``__str__``
^^^^^^^^^^^^^^^^^

.. code:: python

   class Foo(object):
       def __str__(self):
           """
           只有在打印对象时，会自动化调用此方法，并将其返回值在页面显示出来
           :return: 
           """
           return 'asdfasudfasdfsad'

   obj = Foo()
   print(obj)

.. code:: python

   class User(object):
       def __init__(self,name,email):
           self.name = name
           self.email = email
       def __str__(self):
           return "%s %s" %(self.name,self.email,)
   user_list = [User('二狗','2g@qq.com'),User('二蛋','2d@qq.com'),User('狗蛋','xx@qq.com')]
   for item in user_list:
       print(item)

7.7.6 ``__dict__``
^^^^^^^^^^^^^^^^^^

.. code:: python

   class Foo(object):
       def __init__(self,name,age,email):
           self.name = name
           self.age = age
           self.email = email

   obj = Foo('alex',19,'xxxx@qq.com')
   print(obj)
   print(obj.name)
   print(obj.age)
   print(obj.email)
   val = obj.__dict__ # 去对象中找到所有变量并将其转换为字典
   print(val)

7.7.7 上下文管理
^^^^^^^^^^^^^^^^

.. code:: python

   class Foo(object):
       def do_something(self):
           print('内部执行')
   class Context:
       def __enter__(self):
           print('进入')
           return Foo()

       def __exit__(self, exc_type, exc_val, exc_tb):
           print('推出')

   with Context() as ctx:
       print('内部执行')
       ctx.do_something()

7.7.8 对象相加
^^^^^^^^^^^^^^

-  ``__add__``

.. code:: python

   class Foo(object):
       def__add__(self,other):
           return 13
   obj1 = Foo()
   obj2 = Foo()
   val  = obj1 + obj2
   print(val)

7.8 内置函数补充
~~~~~~~~~~~~~~~~

-  type 判断对象是否是类的实例

.. code:: python

   class Foo:
       pass

   obj = Foo()

   if type(obj) == Foo:
       print('obj是Foo类的对象')

-  issubclass 判断派生类和基类的从属关系

.. code:: python

   class Base:
       pass

   class Base1(Base):
       pass

   class Foo(Base1):
       pass

   class Bar:
       pass

   print(issubclass(Bar,Base))  #如果是，则返回True,否则返回False
   print(issubclass(Foo,Base))

-  isinstance 判断对象是否是类或基类的实例

.. code:: python

   class Base(object):
       pass

   class Foo(Base):
       pass

   obj = Foo()

   print(isinstance(obj,Foo))  # 判断obj是否是Foo类或其基类的实例（对象）
   print(isinstance(obj,Base)) # 判断obj是否是Foo类或其基类的实例（对象）

-  super
   按照self对象所属类的继承关系，按照顺序挨个找func方法并执行（只找第一个）

.. code:: python

   class Base(object): # Base -> object
       def func(self):
           super().func()
           print('base.func')

   class Bar(object):
       def func(self):
           print('bar.func')

   class Foo(Base,Bar): # Foo -> Base -> Bar
       pass

   obj = Foo()
   obj.func()

7.9 可迭代对象
~~~~~~~~~~~~~~

-  只要能被for循环，内部就有iter方法
-  在类中实现\ ``__iter__``\ 方法且返回一个迭代器（或者是生成器）

.. code:: python

   class Foo(object):
       def __iter__(self):
           return iter([1,2,3,4])

   class Foo(object):    #生成器是特殊的迭代器
       def __iter__(self): 
           yield 1
           yield 2
           ...
           
   obj = Foo()

7.10 约束
~~~~~~~~~

-  寻找方便：在源代码或者别人写的代码中加入代码，根据查看基类代码，要在子类中都要添加此send
-  别人也方便：从上往下看，可以看到基类中存在raise中的约束，也方便掌握代码结构
-  更加规范：代码不会轻易出BUG
-  **没有也可以，只要不调用这个send也不会出错，只是为了规范，同上。**

.. code:: python

   class base(object):
       def send(self):
           raise NotImplementedError('子类中必须有方法')
   class Foo(base):
       def send(self):
           print('我是真的')
   class Fooo(base):
       def send(self):
           print('我也是真的')
   class Foooo(base):
       def func(self):
           print('我不知道发生了什么')
   obj = Foooo()
   obj.send()    #报错 NotImplementedError 提醒

7.11 反射
~~~~~~~~~

-  实现了根据输入的字符查找元素并操作

-  根据字符串形式去操作对象中的元素

-  getattr(对象,“字符串”) 根据字符粗的形式去某个对象中 获取 对象的成员。

.. code:: python

   class Foo(object):
       def __init__(self,name):
           self.name = name
   obj = Foo('alex')

   # 获取变量
   v1 = getattr(obj,'name')
   # 获取方法
   method_name = getattr(obj,'login')
   method_name()

-  getattr 中的第三个值是如果获取不到的返回值

.. code:: python

   class Foo(object):
       def get(self):
           pass
   obj = Foo()
   if hasattr(obj,'post'):
       getattr(obj,'post')

   v1 = getattr(obj,'get',None) # 推荐
   print(v1)

-  hasattr(对象,‘字符串’) 根据字符粗的形式去某个对象中判断是否有该成员。

.. code:: python

   #!/usr/bin/env python
   # -*- coding:utf-8 -*-
   from wsgiref.simple_server import make_server

   class View(object):
       def login(self):
           return '登陆'

       def logout(self):
           return '等处'

       def index(self):
           return '首页'


   def func(environ,start_response):
       start_response("200 OK", [('Content-Type', 'text/plain; charset=utf-8')])
       #
       obj = View()
       # 获取用户输入的URL
       method_name = environ.get('PATH_INFO').strip('/')
       if not hasattr(obj,method_name):
           return ["sdf".encode("utf-8"),]
       response = getattr(obj,method_name)()
       return [response.encode("utf-8")  ]

   # 作用：写一个网站，用户只要来方法，就自动找到第三个参数并执行。
   server = make_server('192.168.12.87', 8000, func)
   server.serve_forever()

-  setattr(对象,‘变量’,‘值’) 根据字符粗的形式去某个对象中设置成员。

.. code:: python

   class Foo:
       pass


   obj = Foo()
   obj.k1 = 999
   setattr(obj,'k1',123) # obj.k1 = 123

   print(obj.k1)

-  delattr(对象,‘变量’) 根据字符粗的形式去某个对象中删除成员。

.. code:: python

   class Foo:
       pass

   obj = Foo()
   obj.k1 = 999
   delattr(obj,'k1')
   print(obj.k1)

7.12 单例模式

   无论实例化多少次，永远用的都是第一次实例化出的对象。优点是可以增加效率，防止链接太多导致的崩溃

.. code:: python

   class Singleton(object):
       instance = None
       def __new__(cls,*args,**kwargs):
           if not cls.instance:
               cls.instance = object.__new__(cls)
           return cls.instance
   #当运行第一次，创建new的对象，赋值给类变量instance,之后在使用这个类，对象相同
   obj1 = Singleton()
   obj2 = Singleton()

文件的连接池

.. code:: python

   class FileHelper(object):
       instance = None
       def __init__(self, path):
           self.file_object = open(path,mode='r',encoding='utf-8')

       def __new__(cls, *args, **kwargs):
           if not cls.instance:
               cls.instance = object.__new__(cls)
           return cls.instance

   obj1 = FileHelper('x')
   obj2 = FileHelper('x') # 无论打开多少次都是同一个链接

反射当前文件的变量

.. code:: python

   getattr(sys.modules[__name__],'变量名')

第八章 网络编程
---------------

TCP协议

.. code:: python

   # server.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import socket

   sk = socket.socket()
   sk.bind(('127.0.0.1',9000))
   sk.listen()

   conn,addr = sk.accept()  # 等待连接,阻塞状态
   conn.send(b'hello')      # 发送信息，必须是字节类型
   msg = conn.recv(1024)    # 接收1024 字节
   print(msg)

   conn.close()             # 关闭连接
   sk.close()               # 关闭整个socket

   # client.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import socket

   sk = socket.socket()
   sk.connect(('127.0.0.1',9000))

   msg = sk.recv(1024)
   print(msg)
   sk.send(b'nihao')

   sk.close()

通信循环

.. code:: python

   # client.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import socket

   sk = socket.socket()
   sk.connect(('127.0.0.1',9000))
   while True:
       inp = input('请输入内容：')
       sk.send(inp.encode('utf-8'))
       msg = sk.recv(1024)
       print(msg)

   sk.close()

   # server.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import socket

   sk = socket.socket()
   sk.bind(('127.0.0.1',9000))
   sk.listen()
   conn,addr = sk.accept()  # 等待连接
   while True:
       msg = conn.recv(1024)    # 接收1024 字节
       conn.send(msg.upper())
       print(msg)

   conn.close()             # 关闭连接
   sk.close()               # 关闭整个socket

链接循环

.. code:: python

   # client.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import socket

   sk = socket.socket()
   sk.connect(('127.0.0.1' ,9000))
   while True:
       inp = input('请输入命令：').strip()
       if not inp:continue                     # 解决发送为空
       sk.send(inp.encode('utf-8'))
       msg = sk.recv(1024)
       print(msg)

   sk.close()

   # server.py
   #!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import socket

   sk = socket.socket()
   sk.bind(('127.0.0.1',9000))
   sk.listen()
   while True:
       conn,addr = sk.accept()  # 等待连接
       while True:
           try:
               msg = conn.recv(1024)    # 接收1024 字节
               if not msg:break
               conn.send(msg.upper())
               print(msg)
           except ConnectionResetError:
               break

       conn.close()             # 关闭连接
   sk.close()               # 关闭整个socket

基于udp协议的套接字

.. code:: python

   #!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import socket

   sk = socket.socket()
   sk.bind(('127.0.0.1',9000))
   sk.setblocking(False) # 设置为非阻塞
   sk.listen()

   conn,addr = sk.accept()  # 等待连接,阻塞状态
   conn.send(b'hello')      # 发送信息，必须是字节类型
   msg = conn.recv(1024)    # 接收1024 字节
   print(msg)

   conn.close()             # 关闭连接
   sk.close()               # 关闭整个socket#!/usr/bin/env python
   # -*- coding:utf-8 -*-

   import socket

   sk = socket.socket()
   sk.bind(('127.0.0.1',9000))
   sk.setblocking(False) # 设置为非阻塞
   sk.listen()

   conn,addr = sk.accept()  # 等待连接,阻塞状态
   conn.send(b'hello')      # 发送信息，必须是字节类型
   msg = conn.recv(1024)    # 接收1024 字节
   print(msg)

   conn.close()             # 关闭连接
   sk.close()               # 关闭整个socket

非阻塞io模型

验证客户端合法性

socketserver模块

第九章 并发编程
---------------

第十章 数据库
-------------

10.1 初识sql语句
~~~~~~~~~~~~~~~~

.. code:: python

   #进入mysql客户端
   mysql
   mysql> select user();  #查看当前用户
   mysql> exit     # 也可以用\q quit退出

   # 默认用户登陆之后并没有实际操作的权限
   # 需要使用管理员root用户登陆
   mysql -uroot -p   # mysql5.6默认是没有密码的
   #遇到password直接按回车键
   mysql> set password = password('root'); # 给当前数据库设置密码

   # 创建账号
   mysql> create user 'eva'@'192.168.10.%' IDENTIFIED BY '123';# 指示网段
   mysql> create user 'eva'@'192.168.10.5';   # 指示某机器可以连接
   mysql> create user 'eva'@'%';                   #指示所有机器都可以连接  
   mysql> show grants for 'eva'@'192.168.10.5';查看某个用户的权限 
   # 远程登陆
   mysql -uroot -p123 -h 192.168.10.3

   # 给账号授权
   mysql> grant all on *.* to 'eva'@'%';
   mysql> flush privileges;    # 刷新使授权立即生效

   # 创建账号并授权
   mysql> grant all on *.* to 'eva'@'%' identified by '123';

   # 查看配置项
   mysql> show variables like '%engine%';

   # 查看表结构
   mysql> desc t1;
   mysql> describe t1; # 只能查看表的字段信息，基础信息
   mysql> show create table t1; # 能够看到和这张表相关的所有信息

1、DDL语句 数据库定义语言： 数据库、表、视图、索引、存储过程，例如CREATE
DROP ALTER

2、DML语句 数据库操纵语言：
插入数据INSERT、删除数据DELETE、更新数据UPDATE、查询数据SELECT

3、DCL语句 数据库控制语言： 例如控制用户的访问权限GRANT、REVOKE

.. code:: python

   # 操作文件夹（库）
      增：create database db1 charset utf8;
      查：show databases;
      改：alter database db1 charset latin1;
      删除: drop database db1;


   # 操作文件（表）
      先切换到文件夹下：use db1
      增：create table t1(id int,name char);
      查：show tables;
      改：alter table t1 modify name char(3);
         alter table t1 change name name1 char(2);
      删：drop table t1;
       

   # 操作文件中的内容（记录）
      增：insert into t1 values(1,'egon1'),(2,'egon2'),(3,'egon3');
      查：select * from t1;
      改：update t1 set name='sb' where id=2;
      删：delete from t1 where id=1;

   # 清空表：
          delete from t1; #如果有自增id，新增的数据，仍然是以删除前的最后一样作为起始。
          truncate table t1;数据量大，删除速度比上一条快，且直接从零开始，

   *auto_increment 表示：自增
   *primary key 表示：约束（不能重复且不能为空）；加速查找

10.2 MySQL的表的操作
~~~~~~~~~~~~~~~~~~~~

.. code:: python

   # 创建库
   mysql> create database staff charset=utf-8;
   # 创建表
   mysql> create table staff_info (id int,name varchar(50),age int(3),sex enum('male','female'),phone bigint(11),job varchar(11));
   # 查看表
   mysql> show tables;

10.2.1 表的约束
^^^^^^^^^^^^^^^

.. code:: mysql

   # 约束
       not null 某一个字段不能为空
       default 给某一个字段设置默认值
       unique 设置某一个字段不能重复
       auto_increment 设置某一个int类型的字段，自增
       primary key 设置某一个字段非空且不能重复
       foreign key 外键

.. code:: mysql

   # not null
   mysql> create table t12 (id int not null);
   #不能向id列插入空元素。 
   mysql> insert into t12 values (null);
   ERROR 1048 (23000): Column 'id' cannot be null

   # default


   # unique
   create table t3(
       id int unique,
       username char(12) unique,
       password char(18)
   );

   # 联合唯一
   create table t4(
       id int,
       ip char(15),
       server char(10),
       port int,
       unique(ip,port)
   );
   mysql> show create table t4;

   # 自增，自增字段必须是数字，且必须是唯一的
   create table t3(
       id int unique auto_increment,
       username char(12) unique,
       password char(18)
   ); 

   # primary key，一张表只能设置一个主键
   create table t6(
       id int not null unique, # 你指定的第一个非空且唯一的字段会被定义为主键                   
       name char(12) not null unique
   );

   # 联合主键
   create table t4(
       id int,
       ip char(15),
       server char(10),
       port int,
       primary key(ip,port)
   );

   # foreign key
   create table staff(
       id int primary key auto_increment,
       age int,
       gender enum('male','female')
       salary float(8,2),
       hire_date date,
       post_id int
   );

10.2.2 修改表
^^^^^^^^^^^^^

::

   alter table 表名 add 添加字段
   alter table 表名 drop 删除字段
   alter table 表名 modify 修改已经存在的字段

10.3 单表查询
~~~~~~~~~~~~~

.. code:: python

   SELECT DISTINCT 字段1,字段2... FROM 表名
                                 WHERE 条件
                                 GROUP BY field
                                 HAVING 筛选
                                 ORDER BY field
                                 LIMIT 限制条数

10.3.1 where约束
^^^^^^^^^^^^^^^^

.. code:: python

   # 测试数据准备
   create table employee(
   id int not null unique auto_increment,
   emp_name varchar(20) not null,
   sex enum('male','female') not null default 'male', #大部分是男的
   age int(3) unsigned not null default 28,
   hire_date date not null,
   post varchar(50),
   post_comment varchar(100),
   salary double(15,2),
   office int, #一个部门一个屋子
   depart_id int
   );

   #三个部门：教学，销售，运营
   insert into employee(emp_name,sex,age,hire_date,post,salary,office,depart_id) values
   ('egon','male',18,'20170301','老男孩驻沙河办事处外交大使',7300.33,401,1), #以下是教学部
   ('alex','male',78,'20150302','teacher',1000000.31,401,1),
   ('wupeiqi','male',81,'20130305','teacher',8300,401,1),
   ('yuanhao','male',73,'20140701','teacher',3500,401,1),
   ('liwenzhou','male',28,'20121101','teacher',2100,401,1),
   ('jingliyang','female',18,'20110211','teacher',9000,401,1),
   ('jinxin','male',18,'19000301','teacher',30000,401,1),
   ('成龙','male',48,'20101111','teacher',10000,401,1),

   ('歪歪','female',48,'20150311','sale',3000.13,402,2),#以下是销售部门
   ('丫丫','female',38,'20101101','sale',2000.35,402,2),
   ('丁丁','female',18,'20110312','sale',1000.37,402,2),
   ('星星','female',18,'20160513','sale',3000.29,402,2),
   ('格格','female',28,'20170127','sale',4000.33,402,2),

   ('张野','male',28,'20160311','operation',10000.13,403,3), #以下是运营部门
   ('程咬金','male',18,'19970312','operation',20000,403,3),
   ('程咬银','female',18,'20130311','operation',19000,403,3),
   ('程咬铜','male',18,'20150411','operation',18000,403,3),
   ('程咬铁','female',18,'20140512','operation',17000,403,3)

.. _比较运算-1:

10.3.2 比较运算 > < = >= <= !=
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   # 查询薪资大于1000的人
   mysql> select * from employee where salary > 10000;
   # 查询薪资小于10000的人
   mysql> select * from employee where salary < 10000;
   # 薪资是30000或者薪资是20000的人
   mysql> select * from employee where salary=30000 or salary=20000;
   # 性别为女并且年龄为18岁的人
   mysql> select * from employee where sex='female' and age=18;

10.3.3 范围筛选
^^^^^^^^^^^^^^^

.. code:: python

   1.多选一
   # 查找薪资为20000,30000,3000,19000的人
   mysql> select * from employee where salary in(20000,30000,3000,1900);
   # 查找薪资不在20000,30000,3000,19000的人
   mysql> select * from employee where salary not in(20000,30000,3000,1900);
   # 查找薪资为20000,30000,3000,19000的人的名字和职业
   mysql> select emp_name,post from employee where salary in (20000,30000,3000,1900);

   2.在一个模糊的范围里
   # 在一个数值区间1w-2w之间的所有人的名字
   mysql> select emp_name from employee where salary between 10000 and 20000;

   3.字符串的模糊查询
   # 找一个名字是以程字开头的(%为通配符，匹配任意长度的任意内容)
   mysql> select * from employee where emp_name like '程__';
   select * from employee where emp_name like '程__';  # _代表一个字符，也是一个通配符，匹配一个字符长度的任意内容
   # 查找以n结尾的所有人的名字
   mysql> select * from employee where emp_name like '%n';
   # 查找名字里面含有n的
   mysql> select * from employee where emp_name like '%n%';

   4.正则匹配
   # 查找以j开头后面为5个字母的名字
   mysql> select * from employee where emp_name regexp '^j[a-z]{5}';

10.3.4 分组 group by
^^^^^^^^^^^^^^^^^^^^

.. code:: python

   mysql> select * from employee group by post;

10.3.5 聚合
^^^^^^^^^^^

.. code:: python

   # count 统计
   mysql> select count(id) from employee;
   # ave 平均值
   mysql> select avg(salary) from employee;
   # sum 统计这个字段对应的数值的和
   mysql> select sum(salary) from employee;
   # max 最大值
   mysql> select emp_name,max(salary) from employee; # 名字对应不上金额，显示的是以分组为代价
   # min 最小值

10.3.6 分组聚合
^^^^^^^^^^^^^^^

.. code:: python

   # 求各个部门的人数
   mysql> select post,count(*) from employee group by post;
   # 求公司里男生和女生的人数
   mysql> select count(id) from employee group by sex;
   # 各部门的平均薪资
   mysql> select post,avg(salary) from  employee group by post;

注意：

.. code:: python

   在 my.cnf 里面设置:
   [mysqld]
   sql_mode=’STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION’

10.3.7 having
^^^^^^^^^^^^^

.. code:: python

   # 过滤组，总是与group by一起
   # 部门人数大于3的部门
   mysql> select post  from employee group by post having count(*) > 3;
   # 部门平均薪资大于10000的部门
   mysql> select post from employee group by post having avg(salary)>10000;

10.3.8 order by
^^^^^^^^^^^^^^^

.. code:: python

   # 薪资从大到小排序
   mysql> select * from employee order by salary desc;
   # 入职时间从小到大排序
   mysql> select * from employee order by hire_date;
   # 年龄相同的情况下，按薪资排序
   mysql> select * from employee order by age,salary desc;

10.3.9 limit
^^^^^^^^^^^^

.. code:: python

   # 取薪资最高前三名
   mysql> select * from employee order by age,salary desc limit 3;

10.3.10 pymysql操作数据库
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

   import pymysql

   conn = pymysql.connect(host='192.168.56.30',user='root',password="123",database='blsnt')

   cur = conn.cursor(pymysql.cursors.DictCursor) # 游标,字典格式显示,默认元祖
   cur.execute('select * from employee where id > 10')
   ret = cur.fetchall()
   print(ret)
   conn.commit()

10.4 多表查询
~~~~~~~~~~~~~

10.4.1 连表查询
^^^^^^^^^^^^^^^

.. code:: python

   #建表
   create table department(
   id int,
   name varchar(20) 
   );

   create table emp(
   id int primary key auto_increment,
   name varchar(20),
   sex enum('male','female') not null default 'male',
   age int,
   dep_id int
   );

   #插入数据
   insert into department values
   (200,'技术'),
   (201,'人力资源'),
   (202,'销售'),
   (203,'运营');

   insert into emp(name,sex,age,dep_id) values
   ('egon','male',18,200),
   ('alex','female',48,201),
   ('wupeiqi','male',38,201),
   ('yuanhao','female',28,202),
   ('liwenzhou','male',18,200),
   ('jingliyang','female',18,204)
   ;
   # 查看表结构
   mysql> desc department;
   mysql> desc employee;

   1.交叉连接：不适用任何匹配条件。生成笛卡尔积
   mysql> select * from employee,department;
   +----+------------+--------+------+--------+------+--------------+
   | id | name       | sex    | age  | dep_id | id   | name         |
   +----+------------+--------+------+--------+------+--------------+
   |  1 | egon       | male   |   18 |    200 |  200 | 技术         |
   |  1 | egon       | male   |   18 |    200 |  201 | 人力资源     |
   |  1 | egon       | male   |   18 |    200 |  202 | 销售         |
   |  1 | egon       | male   |   18 |    200 |  203 | 运营         |
   |  2 | alex       | female |   48 |    201 |  200 | 技术         |
   |  2 | alex       | female |   48 |    201 |  201 | 人力资源     |
   |  2 | alex       | female |   48 |    201 |  202 | 销售         |
   |  2 | alex       | female |   48 |    201 |  203 | 运营         |
   |  3 | wupeiqi    | male   |   38 |    201 |  200 | 技术         |
   |  3 | wupeiqi    | male   |   38 |    201 |  201 | 人力资源     |
   |  3 | wupeiqi    | male   |   38 |    201 |  202 | 销售         |
   |  3 | wupeiqi    | male   |   38 |    201 |  203 | 运营         |
   |  4 | yuanhao    | female |   28 |    202 |  200 | 技术         |
   |  4 | yuanhao    | female |   28 |    202 |  201 | 人力资源     |
   |  4 | yuanhao    | female |   28 |    202 |  202 | 销售         |
   |  4 | yuanhao    | female |   28 |    202 |  203 | 运营         |
   |  5 | liwenzhou  | male   |   18 |    200 |  200 | 技术         |
   |  5 | liwenzhou  | male   |   18 |    200 |  201 | 人力资源     |
   |  5 | liwenzhou  | male   |   18 |    200 |  202 | 销售         |
   |  5 | liwenzhou  | male   |   18 |    200 |  203 | 运营         |
   |  6 | jingliyang | female |   18 |    204 |  200 | 技术         |
   |  6 | jingliyang | female |   18 |    204 |  201 | 人力资源     |
   |  6 | jingliyang | female |   18 |    204 |  202 | 销售         |
   |  6 | jingliyang | female |   18 |    204 |  203 | 运营         |
   +----+------------+--------+------+--------+------+--------------+

   2.内连接：只连接匹配的行
   #找两张表共有的部分，相当于利用条件从笛卡尔积结果中筛选出了正确的结果
   #department没有204这个部门，因而emp表中关于204这条员工信息没有匹配出来
   mysql> select * from emp inner join department on emp.dep_id = department.id;
   +----+-----------+--------+------+--------+------+--------------+
   | id | name      | sex    | age  | dep_id | id   | name         |
   +----+-----------+--------+------+--------+------+--------------+
   |  1 | egon      | male   |   18 |    200 |  200 | 技术         |
   |  2 | alex      | female |   48 |    201 |  201 | 人力资源     |
   |  3 | wupeiqi   | male   |   38 |    201 |  201 | 人力资源     |
   |  4 | yuanhao   | female |   28 |    202 |  202 | 销售         |
   |  5 | liwenzhou | male   |   18 |    200 |  200 | 技术         |
   +----+-----------+--------+------+--------+------+--------------+

   3.外链接之左连接：优先显示左表全部记录
   #以左表为准，即找出所有员工信息，当然包括没有部门的员工
   #本质就是：在内连接的基础上增加左边有右边没有的结果
   mysql> select * from emp left join department on emp.dep_id = department.id;
   +----+------------+--------+------+--------+------+--------------+
   | id | name       | sex    | age  | dep_id | id   | name         |
   +----+------------+--------+------+--------+------+--------------+
   |  1 | egon       | male   |   18 |    200 |  200 | 技术         |
   |  5 | liwenzhou  | male   |   18 |    200 |  200 | 技术         |
   |  2 | alex       | female |   48 |    201 |  201 | 人力资源     |
   |  3 | wupeiqi    | male   |   38 |    201 |  201 | 人力资源     |
   |  4 | yuanhao    | female |   28 |    202 |  202 | 销售         |
   |  6 | jingliyang | female |   18 |    204 | NULL | NULL         |
   +----+------------+--------+------+--------+------+--------------+

   4.外链接之右连接：优先显示右表全部记录
   mysql> select * from emp right join department on emp.dep_id = department.id;
   +------+-----------+--------+------+--------+------+--------------+
   | id   | name      | sex    | age  | dep_id | id   | name         |
   +------+-----------+--------+------+--------+------+--------------+
   |    1 | egon      | male   |   18 |    200 |  200 | 技术         |
   |    2 | alex      | female |   48 |    201 |  201 | 人力资源     |
   |    3 | wupeiqi   | male   |   38 |    201 |  201 | 人力资源     |
   |    4 | yuanhao   | female |   28 |    202 |  202 | 销售         |
   |    5 | liwenzhou | male   |   18 |    200 |  200 | 技术         |
   | NULL | NULL      | NULL   | NULL |   NULL |  203 | 运营         |
   +------+-----------+--------+------+--------+------+--------------+

   5.全外连接：显示左右两个表全部记录
   #全外连接：在内连接的基础上增加左边有右边没有的和右边有左边没有的结果
   #注意：mysql不支持全外连接 full JOIN
   #强调：mysql可以使用此种方式间接实现全外连接
   mysql> select * from emp left join department on emp.dep_id = department.id
   union
   select * from emp right join department on emp.dep_id = department.id
   ;
   +------+------------+--------+------+--------+------+--------------+
   | id   | name       | sex    | age  | dep_id | id   | name         |
   +------+------------+--------+------+--------+------+--------------+
   |    1 | egon       | male   |   18 |    200 |  200 | 技术         |
   |    5 | liwenzhou  | male   |   18 |    200 |  200 | 技术         |
   |    2 | alex       | female |   48 |    201 |  201 | 人力资源     |
   |    3 | wupeiqi    | male   |   38 |    201 |  201 | 人力资源     |
   |    4 | yuanhao    | female |   28 |    202 |  202 | 销售         |
   |    6 | jingliyang | female |   18 |    204 | NULL | NULL         |
   | NULL | NULL       | NULL   | NULL |   NULL |  203 | 运营         |
   +------+------------+--------+------+--------+------+--------------+

   # 查找技术部人的名字
   mysql> select emp.name from emp inner join department on emp.dep_id = department.id where department.name='技术';
   mysql> select emp.name from emp inner join department as dep on emp.dep_id = dep.id where dep.name='技术';
   +-----------+
   | name      |
   +-----------+
   | egon      |
   | liwenzhou |
   +-----------+

10.4.2 子查询
^^^^^^^^^^^^^

.. code:: python

第十一章 前端开发
-----------------

第十二章 Django框架
-------------------

第十三章 爬虫
-------------

第十四章 CMDB
-------------

附录 常见错误和单词
-------------------

.. |image0| image:: http://images.dregs.top/images/20200302221200.png
.. |image1| image:: http://images.dregs.top/images/20200302221241.png
.. |image2| image:: http://images.dregs.top/images/20200220182511.png
.. |image3| image:: http://images.dregs.top/images/20200220182630.png
.. |image4| image:: http://images.dregs.top/images/20200220183529.png
.. |image5| image:: http://images.dregs.top/images/20200220185115.png
.. |image6| image:: http://images.dregs.top/images/20200302210656.png
.. |image7| image:: http://images.dregs.top/images/20200302210831.png
.. |image8| image:: http://images.dregs.top/images/20200302210909.png
.. |image9| image:: http://images.dregs.top/images/20200301042126.png
.. |image10| image:: http://images.dregs.top/images/20200301042337.png
.. |image11| image:: http://images.dregs.top/images/20200301042849.png
