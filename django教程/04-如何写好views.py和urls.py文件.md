## 视图函数的基本写法

举个简单的视图例子：

```python
from django.shortcuts import render

def index(request):
    return render(request, 'index.html', locals())
```

## 404错误

404是目前为止最常见的错误。如果想要看到404界面，请把debug设置为False。

### 在视图中使用404错误

可以使用抛出异常来使页面返回至404界面。

```python
from django.http import HttpResponse, Http404
from django.shortcuts import render
from myapp.models import People

def index(request):
    try:
        one = People.objects.get(pk=poll_id)
    except:
        raise Http404("Page not found")
    return HttpResponse("这是你想要的页面")

def page_not_found(request, exception):
    return render(request, '404.html')
```

### 在urls.py和模板文件中指定404网页文件

指定方法如下：

```python
from django.conf.urls import handler404

# 记得在根urls.py中指定，否则无效
handler404 = "myapp.views.page_not_found"
```

然后到templates文件夹中指创建相关的模板文件即可。

## 其他Http错误

- server_error()

```python
handler500 = 'myapp.views.server_error'
```

- permission_denied()

```python
handler403 = 'myapp.views.permission_denied'
```

- had_request()

```python
handler400 = 'myapp.views.had_request'
```

用法与404错误差不多。

## django.http

### Http404

用法上面已经说过，这里不赘述了。

### HttpResponse

HttpResponse有个注意点就是可以修改Http响应码。

```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("hello", status=201)
```

### JsonResponse

使用用法和HttpResponse类似。

## django.shortcuts

### render

这个前面举过例子了，这里不再赘述。

### redirect

返回一个`HttpResponseRedirect`适当的URL作为传递的参数。

### get_object_or_404

使用方法如下：

```python
from django.shortcuts import get_object_or_404
from django.http import HttpResponse
from myapp.models import People

def my_view(request):
    # 调用get()函数，如果不存在引发Http404异常
    obj = get_object_or_404(People, id=1)
    return HttpResponse("hello world!")
```

### get_list_or_404

调用filter函数，如果结果为空数组则引发Http404异常。

## django.urls

### reverses

用法如下：

```python
reverse('admin:app_list', kwargs={'app_label': 'auth'})
```

### path

用法如下：

```python
from django.urls import include, path

urlpatterns = [
    path('index/', views.index, name='main-view'),
    path('bio/<username>/', views.bio, name='bio'),
    path('articles/<slug:title>/', views.article, name='article-detail'),
    path('articles/<slug:title>/<int:section>/', views.section, name='article-section'),
    path('weblog/', include('blog.urls')),
    ...
]
```

### re_path

与path的唯一区别即为正则表达式。

### include

使用方法如上所示。

### register_converter

用来注册自定义转换器。使用用法如下：

```python
class FourDigitYearConverter:
    regex = '[0-9]{4}'

    def to_python(self, value):
        return int(value)

    def to_url(self, value):
        return '%04d' % value
    
from django.urls import path, register_converter

from . import converters, views

register_converter(converters.FourDigitYearConverter, 'yyyy')

urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    path('articles/<yyyy:year>/', views.year_archive),
    ...
]
```

<img src="https://upload-images.jianshu.io/upload_images/18750334-49dd2f95b2a2ddc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

## 视图函数的修饰器

### require_http_methods

使用用法如下：

```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    # I can assume now that only GET or POST requests make it this far
    # ...
    pass
```

### require_GET()

只允许get方法。

### require_POST()

只允许post方法。

### require_safe()

只允许head和get方法。

### csrf_exempt

装饰器，取消django的csrf的限制，用法如下：

```python
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def index(request):
    ...
```

这是在函数中使用csrf装饰器，如果是类视图应该这么用：

```python
from django.utils.decorators import method_decorator
from django.views.decorators.csrf import csrf_exempt
from django.views import View
from django.http import HttpResponse

@method_decorator(csrf_exempt, name="dispatch")
class CorseList(View):
    def get(self, request):
        return HttpResponse("你好世界！")
        
    def post(self, request):
        return HttpResponse("你好世界！")
```

或者直接在post函数上装饰：

```python
from django.views.decorators.csrf import csrf_exempt
from django.views import View
from django.http import HttpResponse

class CorseList(View):
    def get(self, request):
        return HttpResponse("你好世界！")
    
    @csrf_exempt  
    def post(self, request):
        return HttpResponse("你好世界！")
```

## HttpRequest对象

详情请参考：[https://docs.djangoproject.com/en/3.1/ref/request-response/#httprequest-objects](https://docs.djangoproject.com/en/3.1/ref/request-response/#httprequest-objects)

## HttpResponse对象

详情请参考：[https://docs.djangoproject.com/en/3.1/ref/request-response/#httpresponse-objects](https://docs.djangoproject.com/en/3.1/ref/request-response/#httpresponse-objects)

## 基于类的视图



