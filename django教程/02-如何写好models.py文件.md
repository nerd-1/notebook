我们都知道models.py文件是和数据库相关的，models.py文件里面储存的是数据，是建立网站的基石。对于如何写好models.py文件是我们学习过程中必不可少的。

## 字段类型

详细请查看：[https://docs.djangoproject.com/en/3.1/ref/models/fields/](https://docs.djangoproject.com/en/3.1/ref/models/fields/)

### BooleanField

布尔类型。为一个复选框。

### CharField

字符串，为文本输入框。

### DateField

日期。有两个属性：auto_now和auto_now_add。设置auto_now=True即每次保存的时间。设置auto_now_add=True，即每次创建对象的时间。同时还可以设置为：

```python
from django.utils import timezone

class Post(models.Model):
    publish = models.DateTimeField(default=timezone.now)
```

这样会显示日期表单框。

### DateTimeField

同上。

### FileField

必须要写一个参数upload_to，来指定保存文件的位置，使用方法如下：

```python
class MyModel(models.Model):
    # file will be uploaded to MEDIA_ROOT/uploads
    upload = models.FileField(upload_to='uploads/')
    # or...
    # file will be saved to MEDIA_ROOT/uploads/2015/01/30
    upload = models.FileField(upload_to='uploads/%Y/%m/%d/')
```

此时，必须要设置MEDIA_ROOT属性。

一般使用如下：

```shell
>>> f = open("../课程表.txt")
>>> from django.core.files import File
>>> myfile = File(f)
>>> from myapp.models import MyModel
>>> a = MyModel()
>>> a.upload = myfile
>>> a
<MyModel: MyModel object (None)>
>>> a.upload
<FieldFile: ../课程表.txt>
>>> a.upload.url
'%E8%AF%BE%E7%A8%8B%E8%A1%A8.txt'
```

一般调用它的url属性来返回图片资源的指定位置。

### ImageField

和FileField差不多，就是多了两个参数，一个是height_field，还有一个是width_field。用于指定图片的宽和高。不过使用ImageField字段需要安装有pillow库。

### IntegerField

整数类型。

### TextField

文本类型。

### URLField

url类型。

### ForeignKey

用法如下：

```python
from django.db import models

class Car(models.Model):
    manufacturer = models.ForeignKey('Manufacturer', on_delete=models.CASCADE, related_name="car")
    # ...

class Manufacturer(models.Model):
    # ...
    pass
```

第一个参数是多对一关联的类，第二个参数是删除后的方式，第三个参数是反向关联。

### OneToOneField

和ForeignKey类似。

### ManyToManyField

编辑models.py文件为：

```python
from django.db import models

class People(models.Model):
    CHOICES = (("men", "男"), ("women", "女"))
    name = models.CharField(max_length=20)
    grade = models.CharField(choices=CHOICES, max_length=20)

class Hobby(models.Model):
    person = models.ManyToManyField("People", related_name="hobby")
```

然后两者分别创建一个对象：

```shell
>>> from myapp.models import People, Hobby
>>> a = People.objects.create(name="catfish", grade="men")
>>> b = Hobby().save()
>>> b.person.add(a)       # 把a关联到b
>>> b.person.remove(a)     # 移除ab之间的关联
```

## 字段选项

详细请查看：[https://docs.djangoproject.com/en/3.1/ref/models/fields/](https://docs.djangoproject.com/en/3.1/ref/models/fields/)

我这里只写一些常用的。

### null

数据库中该值是否能为空值，默认为false。

### blank

表单验证中是否为空值，默认为false。

### choices

枚举类型，小括号内第一个为机器可读语言，第二个为人类可读语言。使用如下：

```python
from django.db import models

class People(models.Model):
    CHOICES = (("men", "男"), ("women", "女"))
    name = models.CharField(max_length=20)
    grade = models.CharField(choices=CHOICES, max_length=20)
```

### default

设置默认值。

### help_text

后台表单的提示文本

### primary_key

主键。设置primary_key=True来指定主键，如果不指定django默认有个id为主键。

### unique

是否是唯一的，默认False。

### verbose_name

汉化后台。

## Meta选项

### verbose_name

汉化表名

### verbose_name_plural

汉化表名

### ordering

排序规则

## 处理objects对象

### 重写objects内置方法

```python
class PublishedManager(models.Manager):
    def get_queryset(self):
        return super(PublishedManager, self).get_queryset().filter(status='published')
    
class Article(models.Model):
    published = PublishedManager()
```

### 自定义objects方法

```python
class BookManager(models.Manager):
    def create_book(self, title):
        book = self.create(title=title)
        # do something with the book
        return book

class Book(models.Model):
    title = models.CharField(max_length=100)
    objects = BookManager()

book = Book.objects.create_book("Pride and Prejudice")
```

## 自定义的模型方法

就是自己写一个带有返回值的模型方法，还可以使用修饰器。

## 内置可调用的模型方法

### delete

### save

## 重写的模型方法

### clean

```python
from django.core.exceptions import ValidationErrors

def clean(self):
    if self.comment_type == 0 and len(self.content)== 0 :
        raise ValidationError(u'评论不能为空')
```

### _\_str\_\_

### get_absolute_url

```python
def get_absolute_url(self):
    return "/people/%i/" % self.id
```

一般与reverse方法一起使用：

```python
def get_absolute_url(self):
    from django.urls import reverse
    return reverse('people.views.details', args=[str(self.id)])
```

### save

```python
def save(self, *args, **kwargs):
        # do_something()
        super().save(*args, **kwargs)  # Call the "real" save() method.
        # do_something_else()
```

有些是models里面的自带方法，有些是objects里面自带的方法