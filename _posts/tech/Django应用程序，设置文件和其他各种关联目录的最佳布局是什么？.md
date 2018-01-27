## Django应用程序，设置文件和其他各种关联目录的最佳布局是什么？

当Django 1.4发布的时候，它包含了一个[更新的项目布局](https://docs.djangoproject.com/en/dev/releases/1.4/#updated-default-project-layout-and-manage-py)，这对改进默认的Django项目布局有很大的帮助，对此本文档将提供一些改进的方法，可轻松应用于1.4版本之后的任何Django版本。

##为什么这个布局更好

我们在这里推荐的项目布局有几个优点，即：

- 允许提取、封装和重新使用各个Django应用程序以用于其他项目，如果从一开始就以这种方式构建，以后需要，将更容易运用到其它项目中去。
- 特定的环境设置。单一的配置文件中无需出现过多的`if DEBUG==True`，这可以很容易地看到哪些设置是共享的，哪些是覆盖的。
- 设置特定的PIP需求
- 项目级模板和静态文件，如果需要，可以覆盖默认应用程序默认。
- 更小的更具体的测试文件，更容易阅读和理解。

假设你有两个应用程序：*博客*和*用户*，2个环境：两个工作环境，`dev`（开发）和`prod`（生产），那么你的项目布局应该具备像下面这样的结构：

```ba&amp;#39;s
myproject/
    manage.py
    myproject/
        __init__.py
        urls.py
        wsgi.py
        settings/
            __init__.py
            base.py
            dev.py
            prod.py
    blog/
        __init__.py
        models.py
        managers.py
        views.py
        urls.py
        templates/
            blog/
                base.html
                list.html
                detail.html
        static/
           …
        tests/
            __init__.py
            test_models.py
            test_managers.py
            test_views.py
    users/
        __init__.py
        models.py
        views.py
        urls.py
        templates/
            users/
                base.html
                list.html
                detail.html
        static/
            …
        tests/
            __init__.py
            test_models.py
            test_views.py
     static/
         css/
             …
         js/
             …
     templates/
         base.html
         index.html
     requirements/
         base.txt
         dev.txt
         test.txt
         prod.txt

```

