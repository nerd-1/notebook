## django判断用户是否登陆

### views.py

```python
if request.user.is_authenticated():
    pass
else:
    pass
```

### tempaltes文件中

要确保request对象在templates模板中，这个项目一创建就存在。

```jinja2
{% if request.user.is_authenticated %}
用户已登陆
{% else %}
用户未登陆
{% endif %}
```

### login_require装饰器

这个装饰器就是限制只有登陆才能访问的视图，使用如下：

```python
from django.contrib.auth.decorators import login_required

@login_required(login_url='/login/')
def login(request):
    pass
```

或者在settings.py文件中定义如下语句后可以直接在views.py文件中直接使用login_required：

```python
# settings.py
LOGIN_URL = '/login/'

# views.py
from django.contrib.auth.decorators import login_required

@login_required
def login(request):
    pass
```

### 注册函数

```python
from django.contrib.auth.models import User

User.objects.create_user(username=username, password=password)
```

### 登陆和验证函数

```python
from django.contrib.auth import authenticate, login

def my_view(request):
    username = request.POST['username']
    password = request.POST['password']
    user = authenticate(request, username=username, password=password)
    if user is not None:
        login(request, user)
        # Redirect to a success page.
        ...
    else:
        # Return an 'invalid login' error message.
```

### 登出函数

```python
from django.contrib.auth import logout

def logout_view(request):
    logout(request)
```





