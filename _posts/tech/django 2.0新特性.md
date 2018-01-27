

Django 是 Python Web 开发最常用的框架之一，跟进它的最新变化绝对是必须的。下面这篇文章主要给大家介绍了关于django 2.0更新的10条注意事项，文中通过示例代码介绍的非常详细，需要的朋友可以参考借鉴，下面来一起看看吧。

**前言**

备受期待的django 2.0已经发布了，最大的一个变化就是不再支持python2.x版本了，这也为我们还在保守使用的2.x的同学们敲响了警钟，赶紧学习python3.x吧，虽然大同小异，但是python3.x还是提供了很多更高级的用法。

**Python兼容性**

Django 2.0支持Python3.4、3.5和3.6。Django官方强烈推荐每个系列的最新版本。

最重要的是Django 2.0不再支持Python2！

Django 1.11.x是支持Python2.7的最后版本。

**django2.0 弃用的特性**

下面这些是我在升级到Django 2.0后遇到的最常见的一些错误：

**1. url**

```
from django.core.urlresolvers import reverse
```

变成了

```
from django.urls import reverse
```

**2. MIDDLEWARE**

settings.py文件，`MIDDLEWARE_CLASSES`更改成了`MIDDLEWARE`，这个一定要注意。

**3. django.shortcuts.render()**

`django.shortcuts.render_to_response()`方法已经被弃用了，现在使用`django.shortcuts.render()`方法。

**4. User.is_authenticated 和User.is_anonymous**

之前的`User.is_authenticated()`和`User.is_anonymous()`方法更改成了属性：`User.is_authenticated`和`User.is_anonymous`。

**5. SessionAuthenticationMiddleware**

删除了`SessionAuthenticationMiddleware`类，不再需要该中间件，在Django 1.10+中已经默认开启了。

**6. assignment_tag 改成simple_tag**

`@register.assignment_tag`改成了`@register.simple_tag`。

**django2.0 更新的特性**

下面的是更新到Django 2.0遇到的一些新特性

**7. on_delete=models.CASCADE**

为`model`的`ForeignKey`和`OneToOne`的属性增加`on_delete=models.CASCADE`。

**8. URL编写进行了简化**

Django以前的URL规则是正则规则，写起来是有点反人类的，一点都不Pythonic。开发者们一直在被迫写类似这样的匹配表达式：

```python
url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),

```

python2.0简化了url路由方法，现在可以这样写：

```python
path('articles/<int:year>/', views.year_archive),
```

新语法支持强制定义参数类型。例子中只接收整数型年份参数，不再接收字符串类型，同时“10000”年也是合法的（虽然是5位数字），而不像先前正则里只能接收4位数字。

以前版本的`django.conf.urls.url()`方法变成了`django.urls.re_path()` ，但为了向后兼容，旧的依然保留，而不是立刻废弃。`django.conf.urls.include()`方法现在可以从`django.urls`导入，也就是你可以使用`from django.urls import include, path, re_path`。

另外如果使用`include('comments.urls',namespace='comment')`这种方式来引用`url`文件的话，需要在`url`文件中加上`app_name`，如：
`app_name = "comments"`。

**9. 聚合操作**

数据库查询的聚合操作annotate中，增加了一个叫Window的操作，和一个叫Frame的条件。

**10. 首页**

除此之外，首次启动的欢迎页面也重做了，感觉高大上了很多，有没有 django2-index

**总结**

有关Django 2.0的新功能和更改的完整列表，请参阅官方文档。

好了，以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家对脚本之家的支持。