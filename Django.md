### Django

> Django是一个开放源代码的Web应用框架，由Python写成。采用了MVC的框架模式，即模型M，视图V和控制器C。它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的，即是CMS（内容管理系统）软件。并于2005年7月在BSD许可证下发布。这套框架是以比利时的吉普赛爵士吉他手Django Reinhardt来命名的。 

#### 安装：

```
pip install Django
```

#### 创建项目：

```
django-admin startproject mysite(项目名称)
```

#### Django项目包中的目录信息

```
manage.py    #一个让你用各种方式管理 Django 项目的命令行工具
mysite/     #目录包含你的项目，它是一个纯 Python 包。它的名字就是当你引用它内部任何东西时需要用到的 Python 包名
__init__.py    #一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包
settings.py    #Django 项目的配置文件
urls.py    #Django 项目的 URL 声明，就像你网站的“目录”
wsgi.py    #作为你的项目的运行在 WSGI 兼容的Web服务器上的入口,服务器文件
```

#### 创建Django应用：

```
#注意：先进入到项目目录  cd mysite
python manage.py startapp polls(应用名称)
```

#### python应用包里的目录信息

```
migrations   #迁移文件包
admin.py    #自定义后台界面
apps.py    #应用的配置，设置应用名称等
models.py    #数据模型（创建数据表）
tests.py     #测试文件
views.py    #视图（MVT中的V，视图处理程序）
```

#### 配置项目：

```
#打开 settings.py
LANGUAGE_CODE = 'zh-hans'     #语言
TIME_ZONE = 'Asia/Shanghai'    #时区
#注册项目，配置应用
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls',
]
#数据库
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

#### 创建第一个视图

- 在应用视图文件中创建视图函数`views.py`

  ```
  from django.shortcuts import render
  from django.http import HttpResponse
  # Create your views here.
  def index(request):
      return HttpResponse('123')
  #JsonResponse({'name':xxxx})   Json
  ```

  

- 在应用中创建路由文件`polls/urls.py`

  ```
  from django.urls import path
  from . import views
  
  urlpatterns = [
      path('',views.index),
  ]
  ```

- 在项目的文件包中导入路由`mysite/urls.py`

  ```
  from django.contrib import admin
  from django.urls import path,include
  
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('polls/', include('polls.urls')),
  ]
  ```

#### 配置数据库

- `mysql`

  ```
  #在mysite/__init.py   替换数据库引擎
  import pymysql
  pymysql.install_as_MySQLdb()
  #在settings.py中配置数据库
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'polls',
          'HOST':'localhost',
          'PORT':'3306',
          'USER':'root',
          'PASSWORD':'123456'
      }
  }
  ```

#### 创建模型   `polls/models.py`

```
from django.db import models

# Create your models here.

class Question(models.Model):
    question_text = models.CharField(max_length=200)    #必须设置字符长度
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    question = models.ForeignKey(to=Question,on_delete=models.CASCADE)
    choice_text=models.CharField(max_length=200)
    votes=models.IntegerField(default=0)
    c = models.ManyToManyFiled(to=xxx)    #多对多的关系
```

#### 创建迁移文件：

```
python manage.py makemigrations polls（应用名称）
```

#### 执行迁移：

```
python manage.py migrate
#python manage.py sqlmigrate polls 0001(版本号)
```

#### 创建站点：

```
#mysite目录下
python manage.py createsuperuser    #根据提示设置用户名密码等
```

#### 自定义的包

```
#加上abstract=True
class xxx(model.Model):
	xxxxx
	class meta:
		abstract=True    #不能被实例化
```

#### 引用静态文件

```
#先加载静态文件的变量
{% load static %}
<script src="{% static 'demo/js/xxx.js' %}"></script>
```

#### 多对多的映射关系

```
from django.core.validators import RegexValidator   # 正则
from django.core.exceptions import ValidationError   # 错误信息提示修改
```

#### 解决跨域问题：

```
# 在表单中加上
{% csrf_token %}     # Django内置的标签，跨域的口令（秘钥）
{{ h2 | safe }}      # 过滤器，一定有返回值
# 设置默认值（前台注册不让输入密码）
form = xxxx(request.POST)
form.is_vail   验证之后加上  
obj = form.save(commit=False)
obj.password = '123456'
obj.save()
# save()  注意事项
form.save()  #可以保存一对一，多对多的数据，如果要与{
    form = xxxx(request.POST)
    form.is_vail   验证之后加上  
    obj = form.save(commit=False)
    obj.password = '123456'
    obj.save()
}一起使用，需要加上form.save_m2m()
```

#### ddddd

```python
readonly_fields = []   # 只读字段
str
{% url 'qiantai:detail' item.id %}   /detail/<cons.id>/

from django.xxxx
@csrf_exempt

captcha

{{ item.cons | safe | striptags }}


form = new FormData($(".form")[0])

data:{csrfmiddlewaretoken: '{{ csrf_token }}' }

{% url 'audit:backstage'%}

static_url = "/static/"
static_root = "os.path.join(base_dir,'static')"
总URL[xxx] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
media = "os.path.join(base_dir,'media')"
序号的显示：
{{ forloop.counter }}   #从1开始计数的
```

#### 验证码

```
INSTALLED_APPS = (
    'captcha',
)
# django_simple_captcha 验证码配置 
# 格式
CAPTCHA_OUTPUT_FORMAT = u'%(text_field)s %(hidden_field)s %(image)s'
# 噪点样式
CAPTCHA_NOISE_FUNCTIONS = ('captcha.helpers.noise_null', # 没有样式
    # 'captcha.helpers.noise_arcs', # 线
    # 'captcha.helpers.noise_dots', # 点
)
# 图片大小
CAPTCHA_IMAGE_SIZE = (100, 25)
CAPTCHA_BACKGROUND_COLOR = '#ffffff'
CAPTCHA_CHALLENGE_FUNCT = 'captcha.helpers.random_char_challenge' # 图片中的文字为随机英文字母，如 mdsh
# CAPTCHA_CHALLENGE_FUNCT = 'captcha.helpers.math_challenge'    # 图片中的文字为数字表达式，如1+2=</span>
 
CAPTCHA_LENGTH = 4 # 字符个数
CAPTCHA_TIMEOUT = 1 # 超时(minutes)
```

#### ajax上传图片

```
$.ajax({
    url:"{% url 'self:upload' %}",
    type:"post",
    data:new FormData($("#uploadForm")[0]),
    processData:false,
    contentType:false,
    success:function(){
        
    }
})

img = request.FILES.get("img",None)
name = os.path.join("upload","userimg",img.name)
if img:
	with open(name,"wb+") as f:
		for item in img.chunks():
			f.write(item)
return 
```

