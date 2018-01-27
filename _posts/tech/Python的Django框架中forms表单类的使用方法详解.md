# Python的Django框架中forms表单类的使用方法详解



用户表单是Web端的一项基本功能,大而全的Django框架中自然带有现成的基础form对象,Python的Django框架中forms表单类的使用方法详解

**Form表单的功能**

- 自动生成HTML表单元素
- 检查表单数据的合法性
- 如果验证错误，重新显示表单（数据不会重置）
- 数据类型转换（字符类型的数据转换成相应的Python类型）

**Form相关的对象包括**

- Widget：用来渲染成HTML元素的工具，如：forms.Textarea对应HTML中的<textarea>标签
- Field：Form对象中的一个字段，如：EmailField表示email字段，如果这个字段不是有效的email格式，就会产生错误。
- Form：一系列Field对象的集合，负责验证和显示HTML元素
- Form Media：用来渲染表单的CSS和JavaScript资源。

**Form Objects**

`Form`对象封装了一系列Field和验证规则，`Form`类都必须直接或间接继承自`django.forms.Form`，定义`Form`有两种方式:

方法一：直接继承`Form`

```python
from django import forms
class ContactForm(forms.Form):
 subject = forms.CharField(max_length=100,label='主题')
 message = form.CharField(widget=forms.TextArea)
 sender = form.EmailField()
 cc_myself = forms.BooleanField(required=False)

```

方法二：结合`Model`，继承`django.forms.ModelForm`

```
#models.py
class Contact(models.Model):
 title = models.CharField(max_length=30)
 content = models.CharField(max_length=20)

#form.py
class ConotactForm(ModelForm):
 class Meta:
 model = Contact
 field = ('title','content') #只显示model中指定的字段


```

**在视图（view）中使用form**

在view函数中使用form的一般情景是：

view.py:

```
form django.shortcuts import render
form django.http import HttpResponseRedirect

def contact(request):
 if request.method=="POST":
  form = ContactForm(request.POST)
  if form.is_valid(): #所有验证都通过
   #do something处理业务
   return HttpResponseRedirect('/')
 else:
  form = ContactForm()
 return render(request,'contact.html',{'form':form})


```

contact.html:

```
<form action='/contact/' method='POST'>
 {% for field in form %}
  <div class = 'fieldWrapper'>
   {{field.label_tag}}:{{field}}
   {{field.errors}}
  </div>
 {% endfor %}
 <div class='fieldWrapper'> <p><input type='submit' value='留言'></p></div>
</form>

```

**处理表单数据**

`form.is_valid()`返回true后，表单数据都被存储在`form.cleaned_data`对象中（字典类型，意为经过清洗的数据），而且数据会被自动转换为Python对象，如：在form中定义了`DateTimeField`，那么该字段将被转换为`datetime`类型，还有诸如：`IntegerField、FloatField`

```
if form.is_valid():
 subject = form.cleaned_data['subject']
 message = form.cleaned_data['message']
 sender = form.cleaned_data['sender']
 cc_myself = form.cleaned_data['cc_myself']

 recipients = ['info@example.com']
 if cc_myself:
  recipients.append(sender)

 from django.core.mail import send_mail
 send_mail(subject, message, sender, recipients)
 return HttpResponseRedirect('/thanks/') # Redirect after POST


```

Form的简单使用方法就这些。 另：

在模版中显示表单的几种方式：

显示form找template中的方法多种多样，也可以自定义：

```
<form action="/contact/" method="post">{% csrf_token %}
{{ form.as_p }}
<input type="submit" value="Submit" />
</form>

```

还可以使用form.as_table、form.as_ul，分别表示用<p>标签，<table>标签和<ul>表示显示表单。如果要自定义，你只要获取到每个元素的值就行：

```html
<form action="/contact/" method="post">
 {{ form.non_field_errors }}
 <div class="fieldWrapper">
  {{ form.subject.errors }}
  <label for="id_subject">Email subject:</label>
  {{ form.subject }}
 </div>
 <div class="fieldWrapper">
  {{ form.message.errors }}
  <label for="id_message">Your message:</label>
  {{ form.message }}
 </div>
 <div class="fieldWrapper">
  {{ form.sender.errors }}
  <label for="id_sender">Your email address:</label>
  {{ form.sender }}
 </div>
 <div class="fieldWrapper">
  {{ form.cc_myself.errors }}
  <label for="id_cc_myself">CC yourself?</label>
  {{ form.cc_myself }}
 </div>
 <p><input type="submit" value="Send message" /></p>
</form>

```

每个`form`字段都可以使用` {{form.name_of_field}}`得到。

也可以通过迭代form，每个迭代元素对应的是form里面的field

```
<form action="/contact/" method="post">
 {% for field in form %}
  <div class="fieldWrapper">
   {{ field.errors }}
   {{ field.label_tag }}: {{ field }}
  </div>
 {% endfor %}
 <p><input type="submit" value="Send message" /></p>
</form>

```

{{field}}有如下属性：

```
{{field.lable}}，如：Email address
{{field.label_tag}}，如： <label for=id_email>Email address</label>
{{field.value}} 如：someone.@gmail.com
{{field.errors}}

```

**实例：构建表单 **第一步:首先在models.py中定义一个表单模型

```
class RemarkForm(forms.Form):
  subject = forms.CharField(max_length=100 ,label='留言标题')
  mail = forms.EmailField(label='电子邮件')
  topic = forms.ChoiceField(choices=TOPIC_CHOICES,label='选择评分') 
  message = forms.CharField(label='留言内容',widget=forms.Textarea)
  cc_myself = forms.BooleanField(required=False ,label='订阅该贴')


```

那个topic中的choices需要在models.py中定义一个数组.

```
TOPIC_CHOICES = (
  ('leve1', '差评'),
  ('leve2', '中评'),
  ('leve3', '好评'),
)

```

这样，之后在html显示的表单就采用这个模型的数据了。

另外还有一种定义表单模型的方式，那就是直接继承另一个models。如果我们在models里设计数据库时，已经设计好了一个类(就是数据库的表)之后想复用这个类的信息来作为表单的模型，那么很简单，同样是在models中的一个类

```
class Advertisement(models.Model): 
 #订单编号
 OrderID =  models.ForeignKey(OrderInfo)
 #//广告标题#
 Title = models.CharField(max_length = 36) 
 #//广告内容#
 Content = models.CharField(max_length = 600)

```

注意，他的类型是models.Model，是做数据库ORM用的。

然后怎么关联呢?
需要导入一个新的类 (ModelForm)

```
from django.forms import ModelForm

class ContactForm(ModelForm):
 class Meta:
 model = Advertisement
 fields = ('Title', 'CustomerID')


```

这里的Advertisement就是之前那个ORM的模型。
第二步：OK，继续我们的表单，下一步需要做什么呢？开始在views.py里写对表单的调用吧.
def remark(request):

```
  if request.method == 'POST': # 如果表单被提交
    form = ContactForm(request.POST) # 获取Post表单数据
    if form.is_valid(): # 验证表单
      return HttpResponseRedirect('/') # 跳转
  else:
    form = ContactForm() #获得表单对象
    
  return render_to_response('message.html', {
    'form': form,
  })

```

整个代码很简单，我就不过多的解释了。

第三步：我们知道django里任何访问都是通过urls.py来管理的。所以下面我们需要配置一个路径。

```
 (r'^message', 'iring.views.remark'),
```

第四步：最后应该是建立一个模板的时候了，因为我们最终是输出到html上的.注意views的remark函数最后一行

```
  return render_to_response('message.html', {
    'form': form,
  })

```

就是讲当前的表单对象输出到message.html里，自动生成一个表单。
所以，先构建一个html.
这个html模板很简单，除去不必要的CSS，我只给出核心部分好啦。

```
 <form action="/message/" method="POST">
 {% for field in form %}
  <div class="fieldWrapper">
   {{ field.label_tag }}:{{ field }} 
   {{ field.errors }}
  div>
 {% endfor %}
 <div class="fieldWrapper"><p><input type="submit" value="留言" />p>div>
 form>

```

```
{% for field in form %}

{% endfor %}


```

用于对表单对象里的元素进行遍历，然后通过

```
{{ field.label_tag }}
{{ field }} 
{{ field.errors }}

```

这三个标签来输出，注意{{ field.errors }}默认情况下是不会输出的，只有当验证表单正确性时才会输出内容。

最终我们通过:
http://youdjangourl/message来访问这个例子

![2016621145313568.jpg (463×521)](http://files.jb51.net/file_images/article/201606/2016621145313568.jpg?2016521145332)

![2016621145344436.jpg (433×383)](http://files.jb51.net/file_images/article/201606/2016621145344436.jpg?2016521145355)

![2016621145405787.jpg (434×350)](http://files.jb51.net/file_images/article/201606/2016621145405787.jpg?2016521145415)