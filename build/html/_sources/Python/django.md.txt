### HTTP协议

* 超文本传输协议

* 四大特性
  * 基于TCP/IP之上作用于应用层的协议
  * 基于请求响应
  * 无状态
    * cookie
    * session
    * token
  * 无连接
    * websocket
* HTTP数据格式

```python
	http数据格式
		请求格式
			请求首行 GET /index HTTP/1.1 
			请求头(一大堆k,v键值对)
			
			请求体(数据)
		
		
		响应格式
			响应首行 HTTP/1.1 200 OK 
			响应头(一大堆k,v键值对)
			
			响应体(数据)
```

* HTTP响应状态码

```
   10X:服务端已经接受到你的数据了 你可以继续提交数据进行下一步操作
   20X:请求成功(200)
   30X:重定向(301,302)
   40X:请求错误(404)
   50X:服务端错误(500)
```

### python三大主流web框架

```python
	 a:socket服务
	 b:路由与视图函数映射关系
	 c:模板渲染

	django:大而全 类似于航空母舰
		a用的别人的 wsgiref  上线之后会换成uwsgi
		b自己写的
		c自己写的
		
	flask:小而精  类似于游骑兵
		a用的别人的  werkzeug  
		b自己写的
		c用的别人 jinja2
			
	tornado:异步非阻塞
		三者都是自己写的
```

#### Django安装注意事项

**注意事项**

		1.计算机名称不能含有中文
		2.一个pycharm窗口就是一个工程(项目)
		3.项目文件夹不要有中文
	ps:django版本问题
		django 1.X
		django 2.X
**安装**

```python
pip3 install django==1.11.11
```

**命令行创建项目**

```python
django-admin startproject 项目名
```

ps:创建一个应用名的文件夹 里面有一个跟应用名同名的文件夹和一个manage.py文件

**创建应用**

```python
django-admin startapp 应用名
```

**启动django项目**

```python
python3 manage.py runserver
```

注意：

命令创建django项目不会自动创建templates文件夹

settings配置文件中要添加templates的路径

![](http://images.dregs.top/images/20190923235329.png)

浏览器访问：127.0.0.1:8000

注意:
1.在django中创建的应用必须去settings文件中注册才能生效否则django不识别

![](http://images.dregs.top/images/20190923235733.png)

2 确保不要端口冲突

**django配置文件**

![](http://images.dregs.top/images/20190924000255.png)

```python
项目名
	应用名文件夹
		migrations文件夹
			数据库迁移记录
		admin.py
			django admin后台管理相关
		models.py
			模型类
		views.py
			视图函数
		
	项目同名文件夹
		settings.py
			django暴露用户可配置的配置文件
		urls.py
			路由与视图函数映射关系
	templates
		所有的html文件
	manage.py
		django入口文件
```

**刚开始学习时可在配置文件中暂时禁用csrf中间件，方便表单提交测试。**

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

### Django基础必备三件套

```python
from django.shortcuts import HttpResponse, render, redirect
```

定义一个index,编辑urls.py

```python
from django.conf.urls import url
from django.contrib import admin
from app01 import views
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^index/', views.index),

]
```

编辑views.py

```python
from django.shortcuts import render,HttpResponse,redirect

# Create your views here.

def index(request):
    return HttpResponse('hello django index')  # 返回字符串
  
def login(request):
    return render(request,'login.html')        # 返回html页面
  
def home(request):
    return redirect('https://www.baidu.com')   # 重定向
```

浏览器访问

![](http://images.dregs.top/images/20190924001132.png)

### Django静态文件配置

网站所用到的已经写好的文件(css,js,图片)

![](http://images.dregs.top/images/20190924182422.png)

settings.py文件配置静态文件放置目录,添加到文件最下面

```python
STATIC_URL = '/static/'   #这是接口前缀，html里面的静态文件前缀都要是static
STATICFILES_DIRS = [
    os.path.join(BASE_DIR,'static')
]
```

动态监测接口前缀变化同步到html

```python
	{% load static %}
    <link rel="stylesheet" href="{% static 'bootstrap-3.3.7/css/bootstrap.min.css'%}">
    <script src={% static "bootstrap-3.3.7/js/bootstrap.min.js"%}></script>
```

form表单默认是get请求

```
get请求携带的参数是拼接在url后面的以?开头&链接
ps:get请求可以携带参数 但是参数的大小有限制 最大4KB，并且是明文的
http://127.0.0.1:8000/login/?username=jason&password=123
```

action参数有三种写法

	1.什么都不写 默认往当前页面的url地址提交
	2.只写路由后缀(******)
	3.写全路径
登录功能

```python
def login(request):
    if request.method == 'POST':
        # 读取post请求提交的数据

        print(request.POST)
        username = request.POST.get('username')
        password = request.POST.get('password')  # 虽然值是一个列表，但是get方法只会获取到列表最后一个元素
        # hobby = request.POST.getlist('xxx')    # 如果想要获取到列表中到多个值，用getlist方法
    
        if username == 'blsnt' and password == '123':
            return redirect('http://www.xiaohuar.com')
        return HttpResponse('没钱滚')
```

> 获取用户输入的框 都必须要有name属性

### Django连接数据库

1.settings.py配置字段

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'test',
        'USER': 'root',
        'PASSWORD': '123',
        'HOST': 'localhost',
        'PORT': '3306',
        'CHARSET': 'utf-8'
    }
}
```

2.去应用名下的\_\_init\_\_.py或者项目名下的\_\_init\_\_.py文件中 告诉django不要使用默认的mysqld_db模块连接mysql而是使用pymysql

```python
import pymysql
pymysql.install_as_MySQLdb()
```

2.mysql-connector-python

　　这是mysql的官方的驱动包，对于mysql 不同版本的加密方式，不受影响。

　　A. 安装包: pip install mysql-connector-python

　　B. 修改Django 项目中的setting文件中的 ENGINE 的配置：

![](http://images.dregs.top/images/20191223112014.png)

　　C. 然后生成迁移文件，并执行迁移程序。 

　　　　python manage.py makemigrations 

　　　　python mangage.py migrate  

### Django ORM

```python
	对象关系映射
	
	类         	 >>>				数据库的表
	
	对象           >>>				数据库里面的一条条的表记录
	
	对象点属性      >>>				表记录的某个字段对应的值
```

> 能够让一个不会数据库操作的人 也能够通过编程语言轻松的操作数据库,有时候sql语句的查询效率可能偏低

注意事项：

1.models.py中写模型类

2.执行数据库同步命令

```python
python3 manage.py makemigrations   #将数据的更改操作记录到小本本上
python3 manage.py migrate          #将更改真正同步到数据库
```

> 不能创建库，只能创建表

#### ORM 查询操作

views.py将前端传入的用户名和密码拿出来做if判断数据库是否有这个用户

```python
form app01 import models
def login(request):
    print(request.method)  # 获取当前请求方式
    if request.method == 'POST':
        # user_obj = models.User.objects.filter(username=username)
        # print(user_obj.query) # 获取sql语句
        
        # user_obj = models.User.objects.filter(username=username)[0]  # 支持正向索引,不推荐
        
        # user_obj = models.User.objects.filter(username=username)[-1] # 不支持负数索引
        
        # user_obj = models.User.objects.filter(username=username).first()  # 推荐使用first获取对象

        is_alive = models.User.objects.filter(username=username,password=password)  # filter支持传多个参数 并且是and的关系
        if is_alive:
            return HttpResponse('登录成功')
        return HttpResponse('登录失败')
        # select id,username,password from user where username='jason' and password = '123'
        """
        filter方法
        当条件存在的时候 <QuerySet [<User: User object>]>        jQuery对象与原生js对象之间的关系
        条件不存再 <QuerySet []>
        """
        # print(user_obj)
        # print(user_obj.id)
        # print(user_obj.pk)  # pk会自动查找当前对象的主键字段
        # print(user_obj.username)
        # print(user_obj.password)
        # print(hobby,type(hobby))
        # print(username,type(username))
        # print(password,type(password))


    return render(request,'login.html')
```

#### 模型表字段的增删改查

models.py跟数据库相关的操作都在这个文件里面

```python
from django.db import models

# Create your models here.

class User(models.Model):
    username = models.CharField(max_length=32)
    password = models.CharField(max_length=32)
    addr = models.CharField(max_length=32,null=True) # 只要修改了models.py中跟数据库相关的数据，必须重新执行数据库的两条迁移命令
    # email = models.EmailField(null=True)            # 要删除这个数据，直接#然后再执行两条数据库迁移命令即可
```

> python3 manage.py makemigrations  将数据的更改操作记录到小本本上
>
> python3 manage.py migrate  将更改真正同步到数据库

#### 数据的增删改查

```python
def register(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        addr = request.POST.get('addr')

        # orm创建数据
        # 第一种
        # models.User.objects.create(username=username,password=password,addr=addr)

        # 第二种
        user_obj = models.User()
        user_obj.username = username
        user_obj.password = password
        user_obj.addr = addr
        user_obj.save() # 保存到数据库

        return redirect('/user_list')   # 注册完数据跳转到展示页面

    return render(request,'register.html') 

def user_list(request):
    # 获取表所有到数据
    data = models.User.objects.all()  # select id,username,password,addr from user

    # return render(request,'list_html',{'data':data}) # 第一种给页面传值的方式
    return render(request,'list_html',locals()) # 会将当前名称空间中的所有的名字都传递给前端
```

编辑数据

![](http://images.dregs.top/images/20191022024543.png)

```python
def edit(request):
    if request.method == 'POST':
        edit_id = request.GET.get('id')
        username = request.POST.get('username')
        password = request.POST.get('password')
        addr = request.POST.get('addr')
        # queryset对象 可以直接调用update方法进行批量更新  如果queryset对象中有多个数据对象 那么会将多个数据对象全部更新
        # 第一种更新方式
        models.User.objects.filter(pk=edit_id).update(username=username,password=password,addr=addr)
        # 第二种更新方式
        # edit_obj = models.User.objects.filter(pk=edit_id).first()
        # edit_obj.username = username
        # edit_obj.password = password
        # edit_obj.addr = addr
        # edit_obj.save()  # 不推荐使用  它是从头到尾重新写一遍
        return redirect('/user_list')
    # print(request.GET)  # 获取get请求携带参数
    edit_id = request.GET.get('id')
    # 查询该主键对应的数据对象
    edit_obj = models.User.objects.filter(pk=edit_id).first()
    return render(request,'edit.html',locals())
```

删除数据

```python
def delete(request):
    delete_id = request.GET.get('id')
    models.User.objects.filter(pk=delete_id).delete()  # 批量删除
    return redirect('/user_list')
```

### Django URL路由系统

#### 路由基本使用

```python
urlpatterns = [
    url(r'^admin/', admin.site.urls),
		url(r'^test/$', views.test),
  	url(r'testadd/', views.testadd),
]
# url的第一个参数 其实是一个正则表达式
# 获取用户输入的url 然后根据正则匹配 是否对应
# urls中只要匹配到了 就会立刻执行对应的函数 不会在往下继续匹配
# 第一次如果都没有匹配上的话 会自动加/再次匹配 如果还匹配不上直接报错
```

无名分组

```python
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    # 无名分组，会将分组内到结果，当作位置参数自动传递给后面的视图函数
		url(r'^test/([0-9]{4})/$', views.test),
]
```

有名分组

```python
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    # 无名分组，会将分组内到结果，当作关键字参数自动传递给后面的视图函数
		url(r'^test/(?p<id>[0-9]{4})/$', views.test),
]
```

#### 反向解析

根据别名动态解析出可以匹配上视图函数





#### 伪静态

让一个动态页面伪装成一个看似已经写死了的页面

```python
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^register.html', views.register)
]
```

### 虚拟环境

针对不同的项目，只下载对应项目的模块

![](http://images.dregs.top/images/20191023014412.png)



#### 简单实用无名分组来获取URL中的动态参数

URL配置(URLconf)就像Django 所支撑网站的目录。它的本质是URL模式以及要为该URL模式调用的视图函数之间的映射表；你就是以这种方式告诉Django，对于这个URL调用这段代码，对于那个URL调用那段代码。

```python
urlpatterns = [
    url(正则表达式, views视图函数，参数，别名),
]
```

参数说明：

- 一个正则表达式字符串
- 一个可调用对象，通常为一个视图函数或一个指定视图函数路径的字符串
- 可选的要传递给视图函数的默认参数（字典形式）
- 一个可选的name参数

urls.py

```python
from django.conf.urls import url
from django.contrib import admin
from app01 import views
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^index/2003/$', views.index),           #一定要注意路径是否会被前面到捕捉到
    url(r'^articles/([0-9]{4})$', views.year_archive), #year_archive(request,2006)
    url(r'^articles/([0-9]{4})/([0-9]{2})$', views.month_archive), #year_archive(request,2006,12)
    url(r'^articles/([0-9]{4})/([0-9]{2})/([0-9]+)/$', views.article_detail), #year_archive(request,2006,12,99)
]
```

views.py

```python
from django.shortcuts import render,HttpResponse,redirect

# Create your views here.

def index(request):
    return HttpResponse('hello django index')

def year_archive(request,year):
    return HttpResponse(year)

def month_archive(request,year,month):
    return HttpResponse('year:%s month:%s' %(year,month))

def article_detail(request,year,month,article):
    return  HttpResponse('year:%s month:%s article:%s' %(year,month,article))
```

#### 简单实用有名分组来获取URL中的动态参数

urls.py 会把分组内的结果，当做位置参数自动传递给后面的视图函数

```python
from django.conf.urls import url
  
from . import views
  
urlpatterns = [
    url(r'^articles/2003/$', views.special_case_2003),
    url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
    url(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    url(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2})/$', views.article_detail),
]
 #通过?p<>给分组起名字，传参到views里面，跟关键字传参一样
```

views.py 参数名字要跟urls分组里面的名字一样

```python
from django.shortcuts import render,HttpResponse,redirect

# Create your views here.

def index(request):
    return HttpResponse('hello django index')

def year_archive(request,year):
    return HttpResponse(year)

def month_archive(request,year,month):
    return HttpResponse('year:%s month:%s' %(year,month))

def article_detail(request,year,month,article): #不管参数位置怎么变，参数还是对应的，不会走位置
    return  HttpResponse('year:%s month:%s article:%s' %(year,month,article))
```

#### 路由分发

```python
#At any point, your urlpatterns can “include” other URLconf modules. This
#essentially “roots” a set of URLs below other ones.

#For example, here’s an excerpt of the URLconf for the Django website itself.
#It includes a number of other URLconfs:
from django.conf.urls import include, url

urlpatterns = [
   url(r'^admin/', admin.site.urls),
   url(r'^blog/', include('blog.urls')), #以blog开头的分发到blog里面的urls
]
#要先创建这个应用，然后include这个应用的路由
# ^?匹配根目录
```

小结：

```python
    NOTE:
    1 一旦匹配成功则不再继续
    2 若要从URL 中捕获一个值，只需要在它周围放置一对圆括号。
    3 不需要添加一个前导的反斜杠，因为每个URL 都有。例如，应该是^articles 而不是 ^/articles。
    4 每个正则表达式前面的'r' 是可选的但是建议加上。

一些请求的例子：

    /articles/2005/3/ 不匹配任何URL 模式，因为列表中的第三个模式要求月份应该是两个数字。
    /articles/2003/ 将匹配列表中的第一个模式不是第二个，因为模式按顺序匹配，第一个会首先测试是否匹配。
    /articles/2005/03/ 请求将匹配列表中的第三个模式。Django 将调用函数
                       views.month_archive(request, '2005', '03')。
#设置项是否开启URL访问地址后面不为/跳转至带有/的路径
APPEND_SLASH=True
```

#### 反向解析

```python

	反向解析
		注意:在起别名的时候 一定要保证 所有的别名都不能重复  必须是唯一的
		
		根据别名动态解析出可以匹配上视图函数之前的url的一个结果
		url(r'^testxxx/',views.test,name='t')
		url(r'^test/(\d+)/$',views.test,name='ttt'),
		前端
			没有正则表达式的反向解析
			{% url 't' %}
			无名分组反向解析
			{% url 'ttt' 1 %}
			有名分组同上
		
		后端
			from django.shortcuts import render,HttpResponse,redirect,reverse
			没有正则表达式的反向解析
			reverse('t')
			无名分组反向解析
			reverse('ttt',args=(1,))
			有名分组同上
			
		
		ps:数字通常是数据库中查出来的数据的主键值
```

### 伪静态

```python
让一个动态页面伪装成一个看似数据已经写死了的静态页面
让搜索引擎加大对你这个页面的搜藏力度
```

### 虚拟环境

```python
虚拟环境就类似于你又下载了一个python解释器
```

### Django版本区别

```python
1.x 路由里面用的是url()
2.x 路由里面用的是path(),url第一个参数放的是正则表达式，而path第一个参数写什么就是什么，不支持郑泽如果你还想用正则，django2.x版本中有一个re_path(),等价于1.x中的url 
```

### JsonResponse

```python
from django.http import JsonResponse
import json

def index(request):

    d = {'name':'json','password':'123','hobby':'读书'}
    # 方式一:
    # return HttpResponse(json.dumps(d))
    # 方式二:
    return JsonResponse(d,json_dumps_params={'ensure_ascii':False}) # 返回给前端就能显示中文读书
```

注意：

```python
import json

d = {'name': 'json', 'password': '123', 'hobby': '读书'}
print(json.dumps(d,ensure_ascii=False))  # 原样输出中文读书
```

### Django Views（视图函数）

一个视图函数，简称视图，是一个简单的Python 函数，它接受Web请求并且返回Web响应。响应可以是一张网页的HTML内容，一个重定向，一个404错误，一个XML文档，或者一张图片. . . 是任何东西都可以。无论视图本身包含什么逻辑，都要返回响应。代码写在哪里也无所谓，只要它在你的Python目录下面。除此之外没有更多的要求了——可以说“没有什么神奇的地方”。为了将代码放在某处，约定是将视图放置在项目或应用程序目录中的名为`views.py`的文件中。

#### **request属性** 　　

```python
request.method #请求方式
request.path   #请求路径
request.POST   #POST的请求数据 字典格式
request.GET    #GET的请求数据 字典格式
request.META.  #请求头
request.get_full_path()  # 
```

#### HttpResponse对象

Django对于一定最后响应一个httpResponse的实例对象

三种形式：

1. httpResponse('字符串')
2. render('页面')
3. redirect(重定向)

传递要重定向的一个硬编码的URL

```python
def my_view(request):
    ...
    return redirect('/some/url/')
```

也可以是一个完整的URL

```python
def my_view(request):
    ...
    return redirect('http://example.com/')　
```

两次请求

```python
1）301和302的区别。
　　301和302状态码都表示重定向，就是说浏览器在拿到服务器返回的这个状态码后会自动跳转到一个新的URL地址，这个地址可以从响应的Location首部中获取
  （用户看到的效果就是他输入的地址A瞬间变成了另一个地址B）——这是它们的共同点。
　　他们的不同在于。301表示旧地址A的资源已经被永久地移除了（这个资源不可访问了），搜索引擎在抓取新内容的同时也将旧的网址交换为重定向之后的网址；
　　302表示旧地址A的资源还在（仍然可以访问），这个重定向只是临时地从旧地址A跳转到地址B，搜索引擎会抓取新的内容而保存旧的网址。 SEO302好于301


2）重定向原因：
（1）网站调整（如改变网页目录结构）；
（2）网页被移到一个新地址；
（3）网页扩展名改变(如应用需要把.php改成.Html或.shtml)。
        这种情况下，如果不做重定向，则用户收藏夹或搜索引擎数据库中旧地址只能让访问客户得到一个404页面错误信息，访问流量白白丧失；再者某些注册了多个域名的
    网站，也需要通过重定向让访问这些域名的用户自动跳转到主站点等。
```

### Django 模板语法

### **模板语法之变量**

```python
def index(request):
    import datetime
    s="hello"
    l=[111,222,333]    # 列表
    dic={"name":"yuan","age":18}  # 字典
    date = datetime.date(1993, 5, 2)   # 日期对象
 
    class Person(object):
        def __init__(self,name):
            self.name=name
 
    person_yuan=Person("yuan")  # 自定义类对象
    person_egon=Person("egon")
    person_alex=Person("alex")
 
    person_list=[person_yuan,person_egon,person_alex]

    return render(request,"index.html",{"l":l,"dic":dic,"date":date,"person_list":person_list})　
```

**template：** 

```python
<h4>{{s}}</h4>
<h4>列表:{{ l.0 }}</h4>
<h4>列表:{{ l.2 }}</h4>
<h4>字典:{{ dic.name }}</h4>
<h4>日期:{{ date.year }}</h4>
<h4>类对象列表:{{ person_list.0.name }}</h4>
```

注意：句点符也可以用来引用对象的方法(无参数方法):

#### 模板传值 

```
第一种
   return render(request,'demo.html',{'xxx':[1,2,3,4]})

第二种
   return render(request,'demo.html',locals())
   
# 如果是函数名，传递到前端，会自动加括号执行，将结果传递到前端
```

#### 模板之过滤器

语法：

```python
{{obj|filter__name:parm}}
```

**default**

如果是一个变量是false或者为空，使用给定的默认值，否则，使用变量的值

```python
{{ value|default:"nothing" }}
```

**Length**

返回值的长度，它对字符串呵列表都起作用

```python
{{ value|length }}
```

如果 value 是 ['a', 'b', 'c', 'd']，那么输出是 4。

**filesizeformat**

将值格式化为一个 “人类可读的” 文件尺寸 （例如 `'13 KB'`, `'4.1 MB'`, `'102 bytes'`, 等等）。例如：

```python
{{ value|filesizeformat }}
```

如果 `value` 是 123456789，输出将会是 `117.7 MB`。

**date**

如果 value=datetime.datetime.now()

```python
{{ value|date:"Y-m-d" }}　　
```

**slice**

如果 value="hello world"

```python
{{ value|slice:"2:-1" }}
```

**truncatechars**

如果字符串字符多于指定的字符数量，那么会被截断。截断的字符串将以可翻译的省略号序列（“...”）结尾。

**参数：**要截断的字符数

例如：

```
{{ value|truncatechars:9 }}
```

**safe**

Django的模板中会对HTML标签和JS等语法标签进行自动转义，原因显而易见，这样是为了安全。但是有的时候我们可能不希望这些HTML元素被转义，比如我们做一个内容管理系统，后台添加的文章中是经过修饰的，这些修饰可能是通过一个类似于FCKeditor编辑加注了HTML修饰符的文本，如果自动转义的话显示的就是保护HTML标签的源文件。为了在Django中关闭HTML的自动转义有两种方式，如果是一个单独的变量我们可以通过过滤器“|safe”的方式告诉Django这段代码是安全的不必转义。比如：

```python
value="<a href="">点击</a>"
{{ value|safe }}
```

#### 模板之标签

标签看起来像是这样的： `{% tag %}`。标签比变量更加复杂：一些在输出中创建文本，一些通过循环或逻辑来控制流程，一些加载其后的变量将使用到的额外信息到模版中。一些标签需要开始和结束标签 （例如`{% tag %} ...`标签 内容 ... {% endtag %}）。

**for标签**

遍历每一个元素

```python
{% for person in person_list %}
    <p>{{ person }}</p>
{% endfor %}
```

可以利用`{% for obj in list reversed %}`反向完成循环。

遍历一个字典：

```python
{% for key,val in dic.items %}
    <p>{{ key }}:{{ val }}</p>
{% endfor %}
```

注：循环序号可以通过｛｛forloop｝｝显示　

```python
forloop.counter            The current iteration of the loop (1-indexed)
forloop.counter0           The current iteration of the loop (0-indexed)
forloop.revcounter         The number of iterations from the end of the loop (1-indexed)
forloop.revcounter0        The number of iterations from the end of the loop (0-indexed)
forloop.first              True if this is the first time through the loop
forloop.last               True if this is the last time through the loop
```

**if  标签**

`{% if %}`会对一个变量求值，如果它的值是“True”（存在、不为空、且不是boolean类型的false值），对应的内容块会输出。

```python
{% if num > 100 or num < 0 %}
    <p>无效</p>
{% elif num > 80 and num < 100 %}
    <p>优秀</p>
{% else %}
    <p>凑活吧</p>
{% endif %}
```

**with**

使用一个简单地名字缓存一个复杂的变量，当你需要使用一个“昂贵的”方法（比如访问数据库）很多次的时候是非常有用的

例如：

```python
{% with total=business.employees.count %}
    {{ total }} employee{{ total|pluralize }}
{% endwith %}
```

**csrf_token**

这个标签用于跨站请求伪造保护,加到form表单即可

#### 模版继承

Django模版引擎中最强大也是最复杂的部分就是模版继承了。模版继承可以让您创建一个基本的“骨架”模版，它包含您站点中的全部元素，并且可以定义能够被子模版覆盖的 blocks 。

通过从下面这个例子开始，可以容易的理解模版继承：



### Django 模型层

单独测试django里面某个功能

将manage.py里面的四段拷贝到tests.py里面

```python
import os
import sys

if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "mysite.settings")
    import django
    django.setup()
```

针对单表操作

```python
from django.test import TestCase

# Create your tests here.
import json
import os
import sys

if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "mysite.settings")
    import django
    django.setup()

    from app01 import models

    # 增加数据
    models.BookList.objects.create(title='三国演义',price=123.23,publish_date='2019-09-26')

    # 加入时间戳
    import datetime
    ctime = datetime.date.today()
    models.BookList.objects.create(title='红楼梦',price='1666.23',publish_date=ctime)

    # 修改
    models.BookList.objects.filter(title='三国演义').update(price=1123.23)

    # 查询
    models.BookList.objects.all()
    models.BookList.objects.filter(pk=1) # 推荐使用
    models.BookList.objects.get(pk=2)  # get获取到到就是数据对象本身，但是当条件不满足的时候，会直接报错，不推荐使用

    # 删除
    models.BookList.objects.filter(pk=1).delete()

    # 反向查找
    models.BookList.objects.exists(pk=1) # 取反

    # 只拿数据的某几个字段
    models.BookList.objects.values('title','price')
        # vales返回的是列表套字典
    models.BookList.objects.values_list('title','price')
        # vales_list返回的是列表套元祖

    # 对数据进行排序
    models.BookList.objects.order_by('price')
        # 默认是升序，从小到大
    models.BookList.objects.order_by('price').reverse()
        # 反转，从大到小

    # 去重(前提是：数据必须一样)
    models.BookList.objects.filter(title='三国演义').values('price','title').distinct()

    # 统计
    models.BookList.objects.all().count()

    # 如果QuerySet包含数据，就返回True，否则返回False
    models.BookList.objects.filter(pk=2).exists()

    # 查询价格大于2000的
    models.BookList.objects.filter(price__gt=2000)

    # 查询价格小于2000的
    models.BookList.objects.filter(price__lt=2000)

    # 查询价格大于等于2000的
    models.BookList.objects.filter(price__gte=2000)

    # 查询价格小于等于2000的
    models.BookList.objects.filter(price__lte=2000)

    # 价格1000-2000之间的
    models.BookList.objects.filter(price__range=[1000,2000]) # 两边都包含

    # 查询主键在指定到条件内
    models.BookList.objects.filter(pk__in=[1,2,3])
```

### 图书管理系统表设计

表关系

* 一对一
* 一对多
* 多对多

表关系判断：站在两边是否可以同时有多个对方，如果都可以，那么就是一个多对多，如果是单向的一对多，那就是一对多，如果都不是，要么没有任何关系，要么就是一对一

Book

Publish

Author

AuthorDetail

书和出版社就是一个一对多

书和作者就是一个多对多

作者和作者详情就是一个一对一



```python
node('codeCheck') {  
    stage('拉取代码'){
//          git credentialsId: '1dc3c20c-b9c5-4294-93c4-9b0cdbd3cd78', url: 'git@coding.ypsx-internal.com:arch-foundation/ostrich/ostrich-api-gateway.git'
            git credentialsId: '1dc3c20c-b9c5-4294-93c4-9b0cdbd3cd78', url: 'git@coding.ypsx-internal.com:business-platform/ypsx-order.git'
         updateGitlabCommitStatus name: '拉取代码', state: 'success'
    }
    stage('质量扫描') { 
            withSonarQubeEnv('Sonarqube') {
            sh '''
             /usr/local/sonar-scanner/bin/sonar-scanner -X \
            -Dsonar.host.url=${SONAR_HOST_URL} \
            -Dsonar.language=java \
            -Dsonar.projectKey=${JOB_NAME} \
            -Dsonar.projectName=${JOB_NAME} \
            -Dsonar.projectVersion=1.1 \
            -Dsonar.sources=. \
            -Dsonar.sourceEncoding=UTF-8 \
            -Dsonar.java.binaries=. 
            '''
        updateGitlabCommitStatus name: '代码质量扫描', state: 'success'
            
         }
    }
    stage('发送到企业微信'){
        sh '''
        python3 /home/blsnt/scripts/sonar.py
        '''
        updateGitlabCommitStatus name: '发送企业微信', state: 'success'
    }

}


```

