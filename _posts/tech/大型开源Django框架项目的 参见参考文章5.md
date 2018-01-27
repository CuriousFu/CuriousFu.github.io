**经典的Django项目源码目录结构**

 Django在一个项目的目录结构划分方面缺乏必要的规范。在Django的官方文档中并没有给出大型项目的代码建议目录结构，网上的文章也是根据项目的不同结构也有适当的变化。

**样式一 ：强调模块化设计**  参见参考文章二

管理项目的依赖文件requirements文件夹，列出项目中需要的依赖的文件，比如项目使用的所有python包。

分离应用文件和库文件（apps ,libs）新建apps和libs用来保存application和library

创建完善的setting模块

优势：模块化设计适合规模较大的项目

 缺点：这种布局结构没有说明static文件的放置路径。

```
$ tree .
.
├── djangolicious
│   ├── apps
│   │   ├── blog
│   │   │   ├── __init__.py
│   │   │   ├── models.py
│   │   │   ├── tests.py
│   │   │   └── views.py
│   │   ├── __init__.py
│   │   ├── news
│   │   │   ├── __init__.py
│   │   │   ├── models.py
│   │   │   ├── tests.py
│   │   │   └── views.py
│   │   └── reader
│   │       ├── __init__.py
│   │       ├── models.py
│   │       ├── tests.py
│   │       └── views.py
│   ├── __init__.py
│   ├── libs
│   │   ├── display
│   │   │   ├── __init__.py
│   │   │   ├── models.py
│   │   │   ├── tests.py
│   │   │   └── views.py
│   │   ├── __init__.py
│   │   └── management
│   │       ├── __init__.py
│   │       ├── models.py
│   │       ├── tests.py
│   │       └── views.py
│   ├── settings
│   │   ├── common.py
│   │   ├── dev.py
│   │   ├── __init__.py
│   │   ├── prod.py
│   │   └── test.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
├── requirements
│   ├── common.txt
│   ├── dev.txt
│   ├── prod.txt
│   └── test.txt
└── requirements.txt

10 directories, 36 files
```

###样式二，大型开源Django框架项目的 参见参考文章5

 此模板是开源项目常用的结构，适合大型Django项目开发。

Project的目录结构，PROJ_NAME指的是项目名

```
PROJ_NAME/
     __init__.py      这几个文件是django创建project所必须的，不做过多说明
     manage.py
     settings.py
     urls.py  
     apps/               即使是“小”工程，也建议分成多个app，每个app足够简单，只解决某一个方面的问题 （注1）
         myapp1/
         myapp2/
     extra_apps/     引用的其他app。
     libs/                加载第三方模块，可以避免版本冲突，按照标准的site-packages管理（注2）
           python*.*/　　指定python版本号
               site-packages/   
               requirements.pip    #pip的依赖说明文件
     tests/          project级别的测试，对于每个app，还要有自己的测试代码
     static/          静态内容
            css/
            js/
            images/
     uploads/       上传文件所在目录
     templates/    模板目录，覆盖app的模板
            flatpages/
            comments/
            example/
            app1/
            app2/
     templatetags/    tag目录
```



注1：指定app加载，在settings.py中设置：

```
sys.path.insert(0, os.path.join(PROJECT_ROOT, 'apps'))
sys.path.insert(0, os.path.join(PROJECT_ROOT, 'extras'))
sys.path.insert(0, os.path.join(PROJECT_ROOT, 'libs'))  
```

注2：自定义libs的加载，在settings.py中设置：

```
sys.path.insert(0, '/{{MY_LIB)}/site-packages/*****.egg')
sys.path.insert(0, '/{{MY_LIB}} /site-packages/')  
```

### 其中 app目录结构



```
$APP_NAME/
     tests/                    app级别的测试代码
     models/                 注1
          __init__.py
          Amodels.py
          Bmodels.py
     templates/              注2
     templatetags/        tag目录
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

注1：如果很好的控制app的规模，Model类数量少，可以使用惯用的models.py文件中, 否则将models做成一个package
接下来可以有两种做法：

\1. 在__init__.py中import所有的Model类
\2. 指定Model的元类（Meta）的app_label, 参考[这里](http://www.cnblogs.com/holbrook/archive/2012/02/13/2357339.html)

注2：如果extend 工程下的base.html， 使用 !base.html

 

** 我的总结**

 ···样式一中强调的“Think Modular”思想十分适合大型的项目，大部分大型的项目也是遵循模块化目录结构的原则的。

如下图所示： 

```
-project
 ---app1
   --templates
     --statics
   views
   models
   tests
   urls
 ---app2
 ---appn
 statics
   --css
   --js
 settings 
```