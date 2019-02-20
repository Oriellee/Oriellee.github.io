---
layout: post
title:  vue+element+python+django搭建后台管理系统。
date:   2018-10-13 11:00:00 +0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: vueFlame/4.png # Add image post (optional)
tags: [DEMO]
author: Oriel # Add name author (optional)
---


   **因为工作需要，在空闲的时候搭建了一个vue的管理框架，因为平时也没太多时间，只有晚上有点时间，整个搭建流程大概花了不到十天的样子，整体来说vue用起来还是比angular轻便很多的。其中的难点有两个，一个是涉及了前后端的跨域问题，另一个是登录系统。**

------------

##### 在搭建过程中主要用到了下面的技术：    在搭建过程中主要用到了下面的技术：
    vue
    element
    python
    django
    redis
    mysql

下面我们正式开搭建框架。

##### 使用pycharm专业版，new一个新的django项目。
   ![截图]({{site.baseurl}}/assets/img/vueFlame/1.png)

选择项目目录，第三个地方选择创建一个app的目录，并输入app的名称。porject可以包含多个app，在一般应用中，一个app表示一个服务类文件。


在myproject下的settings.py配置文件中，把默认的sqllite3数据库换成我们的mysql数据库：
```
python
DATABASES = {
'default': {
'ENGINE': 'django.db.backends.mysql',
'NAME': 'collectbits',
'USER': 'root',
'PASSWORD': 'root',
'HOST': '127.0.0.1',
}
}
```
django新版本会主动将默认的app添加到installed_app中，所以无需再次添加，但是之后新建的app需要手动添加。

   ![截图]({{site.baseurl}}/assets/img/vueFlame/2.png)
   
在app目录下的models下写一个model：
```python
class Menus(models.Model):
id = models.BigIntegerField(primary_key=True)
name = models.CharField(max_length=100, null=True)

class Meta:
db_table = 'menus'
```
在view文件夹下写一个接口：
```python
from django.views import View
from unit import unit as unit
from . import models

# Create your views here.
class Menu(View):
def get(self, request):
menu_obj = models.Menus.objects.all().values()
return unit.json_response(list(menu_obj))
```
在url文件下写一个url链接。
```python
from django.conf.urls import url
from . import views

urlpatterns = [
url(r'menus$', views.Menu.as_view()),
]
```
url文件中的as_view()函数是指直接使用Class下的默认函数。
需要在本文件头部，引入的view文件中引入from django.views import View。

- 使用建库语句，修改建库语句中的库名，用户名和用户名，和用户密码，创建项目所需数据库。
```python
CREATE DATABASE
IF
    NOT EXISTS wit_agro DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
CREATE USER 'wit_agro' @'%' IDENTIFIED BY 'wit_agro';
GRANT ALL PRIVILEGES ON wit_agro.* TO 'wit_agro' IDENTIFIED BY 'wit_agro';
FLUSH PRIVILEGES;
```

- 在项目的根目录，分别输入命令：
```python
python manage.py makemigrations
python manage.py migrate
```
第一行代码是将项目下所有文件的的sql语句写入本项目下的migrations的文件夹。
第二行代码是将上面的语句，插入项目所连接的数据库中。
- django，setting页面中，添加 django-cors-headers的中简件，解决django的跨域问题。

```python
    MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',]
    
    CORS_ORIGIN_ALLOW_ALL = True
```

- 配置，django的url连接首页指向哪个页面。在项目中的url文件中配置。

```python
    from django.contrib import admin
    from django.conf.urls import url, include
    from django.views.generic import TemplateView
    import user.url
    
    urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^api/', include(user.url)),
    url(r'^$', TemplateView.as_view(template_name="index.html")),
    ]
```

- 上一步使用了Django的模板系统，所以需要配置一下模板使Django知道从哪里找到index.html。在project目录的settings.py下：

```python
    TEMPLATES = [
    {
    'BACKEND': 'django.template.backends.django.DjangoTemplates',
    'DIRS': ['frontend/dist'],
    'APP_DIRS': True,
    'OPTIONS': {
    'context_processors': [
    'django.template.context_processors.debug',
    'django.template.context_processors.request',
    'django.contrib.auth.context_processors.auth',
    'django.contrib.messages.context_processors.messages',
    ],
    },
    },
    ]
```

- 以及界面的默认路径：

```python
    STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "frontend/dist"),
    ]
```

1. 在项目目录下，打开cmd命令行，输入vue-ui，在可视化界面中创建vue项目。
2. 点击App.vue，修改html代码，为路由。
3. 引入vue-router。
4. 新建router文件夹，创建index路由文件。
5. 在components中创建几个测试页面，其中index为主页，除了login和404页面，其他子页面均在index页面下。
6. template中只能有一个子元素，其他元素放在该子元素下即可。
7. 引入 vuex.
8. 创建store文件。
9. 引入axiox。
10. 创建axios文件。
11. 引入element。
12. 在main.js中调用，ele，store，和router。
13. 后台配置完成跨域以及前后端分离连接以后，需要前端配置前端的路径。
14. 在src文件夹下创建static文件，将除了main.js之外的所有文件全部放入。

   ![截图]({{site.baseurl}}/assets/img/vueFlame/3.png)

- 在前端主目录下，创建vue.config.js文件，在文件中编写如下内容。

```python
    module.exports = {
    baseUrl: '/static/',//打包时在url中寻找的路径。
    outputDir: 'dist',//打包所存放的路径。
    // 配置代理。
    devServer: {
    proxy: {
    '/api': {//代理名称
    target: 'http://127.0.0.1:8000/api',//代理到的地址。
    changeOrigin: true,//是否跨域
    ws: true,// proxy websockets 加上这一句试试看
    pathRewrite: {
    '^/api': '' // 替换target中的请求地址，也就是说，在请求的时候，url用'/proxy'代替'http://ip.taobao.com'
    }
    }
    },
    },
    lintOnSave: false,//编译时，config是否报错。
    };
```

##### 整体框架搭建，大概就结束了，剩下的就是封装编写页面了。具体的完整代码我会在下面放出连接。
   ![截图]({{site.baseurl}}/assets/img/vueFlame/4.png)
   ![截图]({{site.baseurl}}/assets/img/vueFlame/5.gif)

[完整代码](https://github.com/Oriellee/VUE/tree/master/simpleframe "完整代码")
完整代码中我封装了几个前端常用的组件。
运行教程：


    在项目路径下：
    cd frontend
    npm install
    npm run serve
    
    重启动一个命令行
    在项目路径下：
    python manage.py runserver

转载请注明出处。