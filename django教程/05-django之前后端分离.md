- 如何使用drf开发restful接口
- 学习内容：序列化，视图集，路由，认证，权限

## 什么是序列化？

把django的queryset/instance类型转换为json/xml/yaml类型。同理，反过来就是反序列化。

## 序列化

### django内置的序列化

下面来看django内置的序列化：

```python
from myapp.models import Book
from django.core import serializers

serializers.serialize('json', Book.objects.all())  # 返回所有字段
serializers.serialize("json", Book.objects.all(), fields=('title',))  # 返回title字段
```

存在的问题：

1. 对前端传过来的代码进行验证处理
2. 验证器的参数
3. 同时序列多个对象
4. 序列化过程中添加上下文
5. 无效的数据异常处理

### django_rest_framework序列化

为了不花大量的时间造轮子，所以要用到第三方的插件django-rest-framework。使用方法如下：

```python
from myapp.models import Course
from rest_framework import serializers

class CourseSerializer(serializers.ModelSerializer):
    # teacher = serializers.CharField(source="teacher.username")
    author = serializers.ReadOnlyField(source="author.username")
    class Meta:
        model = Course
        # fields = ("title",)
        # exclude = ("title",)
        fields = "__all__"
        depth = 2
```

序列化结果带超链接的api：

```python
from myapp.models import Course
from rest_framework import serializers

class CourseSerializer(serializers.HyperlinkedModelSerializer):
    author = serializers.ReadOnlyField(source="author.username")
    class Meta:
        model = Course
        # url是默认值，可以在settings.py中设置URL_FIELD_NAME使全局生效
        fields = ("title", "url")
```

## 视图

django原生的视图在序列化无法满足分页，排序，认证，权限，限流等需求，所以引入了django_rest_framework框架。

### 函数式编程

使用django原生的FBV编写API接口：

```python
import json
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt

course_dict = {
    "name" : "课程名称",
    "introduction" : "课程介绍",
    "price" : 0.11,
}

@csrf_exempt
def course_list(request):
    if request.method == "GET":
        # return HttpResponse(json.dumps(course_dict), content_type="application/json")
        return JsonResponse(course_dict)
    if request.method == "POST":
        course = json.loads(request.body.decode("utf-8"))
        # 如果不是字典是字符串的话，需要设置safe=False，默认为True
        return JsonResponse(course, safe=False)
```

使用django_rest_framework的FBV编写API接口：

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from myapp.serializers import CourseSerializer
from myapp.models import Course

@api_view(["GET", "POST"])
def course_list(reqeust):
    if request.method == "GET":
        # 序列化多个对象需要用到many=True
        s = CourseSerializer(instance=Course.objects.all(), many=True)
        return Response(data=s.data, status=status.HTTP_200_OK)
    elif request.method == 'POST':
        # 如果前端传过来的数据要比django序列化器中的数据要少，则使用partial=True表示部分更新，只适用于非必填字段。
        s = CourseSerializer(instance=request.data, partial=True)
        if s.is_valid():
            # s.save()
            s.save(teacher=request.user)
            return Response(data=s.data, status=HTTP_201_CREATED)
        return Response(s.error, status=status=HTTP_400_BAD_REQUEST)
   
def course_detail(request.pk):
    try:
        course = Course.objects.get(pk=pk)
    except Course.DoseNotExist:
        return Response(data=s.data, status=status.HTTP_200_OK)
    else
        if request.method="GET":
            s = CourseSerializer(instance=course)
            return Response(data=s.data, status=status.HTTP_200_OK)
        elif request.method == "PUT":
            s = CourseSerializer(instance=course, data=request.data)
            if s.is_valid():
                s.save()
                return Response(s.data, status=status.HTTP_200_OK)
        elif request.method == "DELETE":
            course.delete()
            return Response(status=status.HTTP_204_NO_CONTENT)
```

关于status的状态码有：

<details>
	<summary>关于status的状态码</summary>
    <pre>
    HTTP_100_CONTINUE = 100
    HTTP_101_SWITCHING_PROTOCOLS = 101
    HTTP_200_OK = 200
    HTTP_201_CREATED = 201
    HTTP_202_ACCEPTED = 202
    HTTP_203_NON_AUTHORITATIVE_INFORMATION = 203
    HTTP_204_NO_CONTENT = 204
    HTTP_205_RESET_CONTENT = 205
    HTTP_206_PARTIAL_CONTENT = 206
    HTTP_207_MULTI_STATUS = 207
    HTTP_208_ALREADY_REPORTED = 208
    HTTP_226_IM_USED = 226
    HTTP_300_MULTIPLE_CHOICES = 300
    HTTP_301_MOVED_PERMANENTLY = 301
    HTTP_302_FOUND = 302
    HTTP_303_SEE_OTHER = 303
    HTTP_304_NOT_MODIFIED = 304
    HTTP_305_USE_PROXY = 305
    HTTP_306_RESERVED = 306
    HTTP_307_TEMPORARY_REDIRECT = 307
    HTTP_308_PERMANENT_REDIRECT = 308
    HTTP_400_BAD_REQUEST = 400
    HTTP_401_UNAUTHORIZED = 401
    HTTP_402_PAYMENT_REQUIRED = 402
    HTTP_403_FORBIDDEN = 403
    HTTP_404_NOT_FOUND = 404
    HTTP_405_METHOD_NOT_ALLOWED = 405
    HTTP_406_NOT_ACCEPTABLE = 406
    HTTP_407_PROXY_AUTHENTICATION_REQUIRED = 407
    HTTP_408_REQUEST_TIMEOUT = 408
    HTTP_409_CONFLICT = 409
    HTTP_410_GONE = 410
    HTTP_411_LENGTH_REQUIRED = 411
    HTTP_412_PRECONDITION_FAILED = 412
    HTTP_413_REQUEST_ENTITY_TOO_LARGE = 413
    HTTP_414_REQUEST_URI_TOO_LONG = 414
    HTTP_415_UNSUPPORTED_MEDIA_TYPE = 415
    HTTP_416_REQUESTED_RANGE_NOT_SATISFIABLE = 416
    HTTP_417_EXPECTATION_FAILED = 417
    HTTP_418_IM_A_TEAPOT = 418
    HTTP_422_UNPROCESSABLE_ENTITY = 422
    HTTP_423_LOCKED = 423
    HTTP_424_FAILED_DEPENDENCY = 424
    HTTP_426_UPGRADE_REQUIRED = 426
    HTTP_428_PRECONDITION_REQUIRED = 428
    HTTP_429_TOO_MANY_REQUESTS = 429
    HTTP_431_REQUEST_HEADER_FIELDS_TOO_LARGE = 431
    HTTP_451_UNAVAILABLE_FOR_LEGAL_REASONS = 451
    HTTP_500_INTERNAL_SERVER_ERROR = 500
    HTTP_501_NOT_IMPLEMENTED = 501
    HTTP_502_BAD_GATEWAY = 502
    HTTP_503_SERVICE_UNAVAILABLE = 503
    HTTP_504_GATEWAY_TIMEOUT = 504
    HTTP_505_HTTP_VERSION_NOT_SUPPORTED = 505
    HTTP_506_VARIANT_ALSO_NEGOTIATES = 506
    HTTP_507_INSUFFICIENT_STORAGE = 507
    HTTP_508_LOOP_DETECTED = 508
    HTTP_509_BANDWIDTH_LIMIT_EXCEEDED = 509
    HTTP_510_NOT_EXTENDED = 510
    HTTP_511_NETWORK_AUTHENTICATION_REQUIRED = 511
    </pre>
</details>

</details>

### 类视图

使用django原生的FBV编写API接口：

```python
import json
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt, meth_decorator
from django.views import View

course_dict = {
    "name" : "课程名称",
    "introduction" : "课程介绍",
    "price" : 0.11,
}

@method_decorator(scrf_exempt, name="dispatch")
class CourseList(View):
    def get(self, request):
        return JsonResponse(course_dict)
    
    # @csrf_exempt   第一种加装饰器方法，这里采用第二种加装饰器方法
    def post(self, request):
        course = json.loads(request.body.decode('utf-8'))
        return JsonResponse(course)
```

使用django_rest_framework类视图：

```python
from rest_framework.response import Response
from rest_framework import status
from rest_framework.views import APIView
from myapp.serializers import CourseSerializer
from myapp.models import Course

class CourseList(APIView):
    
    def get(self, request):
        s = CourseSerializer(instance=Course.objects.all(), many=True)
        return Response(s.data, status=status.HTTP_200_OK)
    
    def post(self, request):
        s = CourseSerializer(data=request.data)
        if s.is_valid():
            s.save(teacher = self.request.user)
            return Response(s.data, status=status.HTTP_201_CREATED)
        
class CourseDetail(APIView):
    
    def get_object(pk):
        try:
        	return course = Course.objects.get(pk=pk)
        except Course.DoseNotExist:
            return
        
    def get(self, request, pk):
        obj = self.get_object(pk=pk)
        if not obj:
            return Response(data={"msg":"没有此课程信息"}, status=status.HTTP_404_NOT_FOUND)
        s = CourseSerializer(instance=obj, data=request.data)
        return Response(s.data, status=status.HTTP_200_OK)

    def put(self, request, pk):
        obj = self.get_object(pk=pk)
        if not obj:
            return Response(data={"msg":"没有此课程信息"}, status=status.HTTP_404_NOT_FOUND)
        s = CourseSerializer(instance=obj, data=request.data)
        if s.is_valid():
            s.save()
            return Response(data=s.data, status=status.HTTP_201_CREATED)
        return Response(s.errors, status=status.HTTP_400_BAD_REQUEST)
    
    def delete(self, request, pk):
        obj = self.get_object(pk=pk)
        if not obj:
            return Response(data={"msg":"没有此课程信息"}, status=status.HTTP_404_NOT_FOUND)
        obj.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

### drf 通用类视图

通用类视图的写法就十分简单了，实现上面的效果函数如下：

```python
from rest_framework import generics
from myapp.serializers import CourseSerializer
from myapp.models import Course

class GCourseList(generics.ListCreateAPIView):
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    
    def perform_create(self, serializer):
        serializer.save(teacher=self.request.user)
        
class GCourseDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
```

### drf DRF视图集

这个使用更加简单：

```python
from rest_framework import viewsets
from myapp.serializers import CourseSerializer
from myapp.models import Course

class CourseViewSet(viewsets.ModelViewSet):
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    
    def perform_create(self, serializer):
        serializer.save(teacher=self.request.user)
```

但是这个用法也更难，我暂时还没有理解emmm，随后补上。

链接：https://www.cnblogs.com/zhuangyl23/p/11891795.html

### drf Routers路由

