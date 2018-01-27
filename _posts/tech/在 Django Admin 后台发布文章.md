# 在 Django Admin 后台发布文章

在此之前我们完成了 Django 博客首页视图的编写，我们希望首页展示发布的博客文章列表，但是它却抱怨：暂时还没有发布的文章！如它所言，我们确实还没有发布任何文章，本节我们将使用 Django 自带的 Admin 后台来发布我们的博客文章。

## 创建 Admin 后台管理员账户

要想进入Django Admin 后台，首先需要创建一个超级管理员账户。我们在 [让 Django 完成翻译：迁移数据库](http://zmrenwu.com/post/6/) 中已经创建了一个后台账户，但如果你没有按照前面的步骤创建账户的话，可以运行 `python manage.py createsuperuser` 命令新建一个：

```
python manage.py createsuperuser

Username (leave blank to use 'zmrenwu@163.com'):  admin
Email address:  admin@example.com
Warning: Password input may be echoed.
Password:  ******
Warning: Password input may be echoed.
Password (again):  ******
Superuser created successfully.

```

**注意：在命令行输入密码时可能不会显示输入的字符，不要以为键盘坏了，照正常的方式输入密码即可。**

## 在 Admin 后台注册模型

要在后台注册我们自己创建的几个模型，这样 Django Admin 才能知道它们的存在，注册非常简单，只需要在 blog\admin.py 中加入下面的代码：

```
blog/admin.py

from django.contrib import admin
from .models import Post, Category, Tag

admin.site.register(Post)
admin.site.register(Category)
admin.site.register(Tag)

```

**激活虚拟环境**，运行开发服务器，访问 http://127.0.0.1:8000/admin/ ，就进入了到了Django Admin 后台登录页面，输入刚才创建的管理员账户密码就可以登录到后台了。

![Django Admin 后台](https://bkt.zmrenwu.com/Django%20Admin%20%E5%90%8E%E5%8F%B0.png)

可以看到我们刚才注册的三个模型了，点击 Posts 后面的**增加**按钮，将进入添加 Post 的页面，也就是新增博客文章。然后在相关的地方输入一些测试用的内容，增加完后点击保存，这样文章就添加完毕了，你也可以多添加几篇看看效果。注意每篇文章必须有一个分类，在添加文章时你可以选择已有分类。如果数据库中还没有分类，在选择分类时点击 Category 后面的 + 按钮新增一个分类即可。

![Django Admin 后台新增文章](https://bkt.zmrenwu.com/Django%20Admin%20%E5%90%8E%E5%8F%B0%E6%96%B0%E5%A2%9E%E6%96%87%E7%AB%A0.png)

**你可能想往文章内容中添加图片，但目前来说还做不到。在支持 Markdown 语法部分中将介绍如何在文章中插入图片的方法。**

访问 http://127.0.0.1:8000/ 首页，你就可以看到你添加的文章列表了，下面是我所在环境的效果图：

![博客首页显示的文章列表](https://bkt.zmrenwu.com/%E5%8D%9A%E5%AE%A2%E9%A6%96%E9%A1%B5%E6%98%BE%E7%A4%BA%E7%9A%84%E6%96%87%E7%AB%A0%E5%88%97%E8%A1%A8.png)

## 定制 Admin 后台

在 admin post 列表页面，我们只看到了文章的标题，但是我们希望它显示更加详细的信息，这需要我们来定制 Admin 了，在 admin.py 添加如下代码：

```
blog/admin.py

from django.contrib import admin
from .models import Post, Category, Tag

class PostAdmin(admin.ModelAdmin):
    list_display = ['title', 'created_time', 'modified_time', 'category', 'author']

# 把新增的 PostAdmin 也注册进来
admin.site.register(Post, PostAdmin)
admin.site.register(Category)
admin.site.register(Tag)

```

由于如何定制 Admin 后台不是本教程的重点，这里只是为了简单地到达期望效果，因此不再对上述代码做过多解释。

刷新 Admin Post 列表页面，可以看到显示的效果好多了。

![定制后的 Admin 文章列表页](https://bkt.zmrenwu.com/%E5%AE%9A%E5%88%B6%E5%90%8E%E7%9A%84%20Admin%20%E6%96%87%E7%AB%A0%E5%88%97%E8%A1%A8%E9%A1%B5.png)