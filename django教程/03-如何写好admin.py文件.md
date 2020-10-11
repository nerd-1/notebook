关于如何写好admin.py文件，我查看了一些资料，仔细阅读了下官方文档。知识的渊博程度让我差点放弃了django。呜呜呜……，终于，我整理出来了一个系统的admin攻略。干货有点多，请系好安全带。

首先我们要编辑admin.py文件都是使用ModelAdmin类来注册admin.py文件。如果我学会了如何使用SimpleAdminConfig类，我以后会再写一篇博客来说明如何使用SimpleAdminConfig类。如果你会的话，请务必教一下我，有偿。

## 相关资料

[Django-admin一些有用的配置-无名小妖-博客园](https://www.cnblogs.com/wumingxiaoyao/p/6928297.html)

[Django官方网站-管理员动作](https://docs.djangoproject.com/en/3.1/ref/contrib/admin/actions/)

[再见紫罗兰](https://www.cnblogs.com/linxiyue/category/569717.html)

## ModelAdmin的基本使用

ModelAdmin有两种注册方式。第一种方式为：

```python
from django.contrib import admin
from myproject.myapp.models import Author

class AuthorAdmin(admin.ModelAdmin):
    pass
admin.site.register(Author, AuthorAdmin)
```

第二种方式为：

```python
from django.contrib import admin
from .models import Author

@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    pass
```

推荐使用第二种，简单，方便，正确。

这样你就可以到后台中看到您的相关模型，然后就可以添加，删除，修改您的数据库数据了。

## 管理员动作

但是上述的操作一次性只能修改一个对象，如果很多个对象要修改，这样改来改去太麻烦了，所以，你可以使用管理原动作的方式，一次性操作多个对象。默认情况下，django会给我们提供一个删除的admin动作，它在这儿：

<img src="https://upload-images.jianshu.io/upload_images/18750334-e82ce22142291f16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

如何给我们自定义设置管理员动作呢，其实非常简单，我们先做一个案例，复制选中的 数据：

```python
@admin.register(People)
class PeopleAdmin(admin.ModelAdmin):
    # 需要到action中指定这个函数才能够有效：
    actions = ["copy_one", ]
    
    def copy_one(self, request, queryset):
        # 判断是否为一条，如果是一条就复制，没有就返回提示：只能获取一条信息
        if queryset.count() == 1:
            old_data = queryset.values()[0]
            old_data.pop("id")
            r_pk = queryset.create(**old_data)
        else:
            self.message_user(request, "只能选取一条数据！", "warning")
    # 给这个函数添加一个中文注释：
    copy_one.short_description = "复制"
```

好了，这是给一个模型添加管理员动作，如果给全局添加管理员动作，则需要这样写：

```python
def copy_one(modeladmin, request, queryset):
    if queryset.count() == 1:
        old_data = queryset.values()[0]
        old_data.pop("id")
        r_pk = queryset.create(**old_data)
    else:
        modeladmin.message_user(request, "只能选取一条数据！", "warning")
copy_one.short_description = "复制"

admin.site.add_action(copy_one)
```

如果这样写，那么你的每一个模型都有这个管理员动作了。

好了，接下来我们再将上一个实例扩展，一个一个选中再复制效率太低了，如果我们有许多对象要复制，那么有没有一个更好的方案呢？答案是有的。我们可以在后台的每条数据后添加一个a标签，然后点击这个a标签就能复制该条数据，这样就可以方便很多了。

### 实现直接点击a标签复制数据

既然要a标签跳转，则需要跳转到路由并引用相关视图函数进行操作。所以我们用到了admin标签的后台视图和后台路由。后台路由需要用到get_urls这个内置函数，实现功能如下：

```python
from django.contrib import admin
from .models import People
from django.utils.html import format_html

@admin.register(People)
class PeopleAdmin(admin.ModelAdmin):
    # 在后台用来展示的列表
    list_display = ["name", "copy_current_data"]

    # 显示在后台的a标签
    def copy_current_data(self, obj):
        return format_html('<a href={}copy/>复制</a>'.format(obj.pk))
    copy_current_data.short_description = "复制"

    # 后台admin路由
    def get_urls(self):
        from django.urls import path
        urlpatterns = [
            path("<pk>copy/", self.admin_site.admin_view(self.copy_one), name="copy_data"),
        ]
        return urlpatterns + super(PeopleAdmin, self).get_urls()

    # 后台admin视图
    def copy_one(self, request, pk):
        from django.shortcuts import get_object_or_404, redirect
        obj = get_object_or_404(People, pk=pk)
        old_data = {
            "name" : obj.name,
            "grade" : obj.grade,
        }
        r_pk = People.objects.create(**old_data)
        co_path = "/".join(request.path.split("/")[:-2])
        self.message_user(request, "复制成功！")
        return redirect(co_path)
```

## ModelAdmin自定义函数的属性

如果想在ModelAdmin中添加一个自定义的内置函数，需要加上一个参数obj。表示当前的模板对象。创建的内置函数有一些方法，这些是我遇到的就总结下来了：

### short_description

给admin函数添加一个注释。

### allow_permissions

给该函数进行授权。使用如下：

```python
make_published.allow_permission = ("change",)
```

相关参数有：add, change, delete, view

### empty_value_display

为空值时显示的值，默认为"-"

### boolean

这个值设置会True后，如果函数返回布尔类型的值，会出现图标。输入如下代码：

```python
from django.contrib import admin
from .models import People

@admin.register(People)
class PeopleAdmin(admin.ModelAdmin):
    list_display = ["name", "hello"]

    def hello(self, obj):
        if obj.id % 2:
            return True
        else : 
            return False
    hello.boolean = True
```

结果如图：

<img src="https://upload-images.jianshu.io/upload_images/18750334-5e3d1687986de761.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

### admin_order_field

通过字段样式设置的字段在展示后无法排序（即无法通过点击字段名排序），这里可以通过指定字段来排序。

## ModelAdmin属性

现在我们来了解ModelAdmin属性，这个属性功能很nb。只需要设置一行或多行代码即可给django后台网站增添功能。我在这儿说几个常用的，剩余的请查看官网：[https://docs.djangoproject.com/en/3.1/ref/contrib/admin/](https://docs.djangoproject.com/en/3.1/ref/contrib/admin/)

### actions

管理员动作，使用方法如上文所述。

### actions_on_top和actions_on_bottom

控制操作栏在页面上的显示位置。默认情况下，管理员更改列表在页面（）的顶部显示操作。`actions_on_top = True; actions_on_bottom = False`，如果没有特殊需求的话，这个选项可以不用设置。

<img src="https://upload-images.jianshu.io/upload_images/18750334-eab47fe64b2e1e76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

### date_hierarchy

指定一个时间列，表示详细时间分层筛选。使用如下：

```python
from django.contrib import admin
from .models import People

@admin.register(People)
class PeopleAdmin(admin.ModelAdmin):
    list_display = ["name"]
    date_hierarchy = "datetime"
```

效果如下图：

<img src="https://upload-images.jianshu.io/upload_images/18750334-7dcc639c7705b342.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

### empty_value_display

此属性将覆盖记录字段为空（None，空字符串等）的默认显示值。默认值为-（破折号）。

### exclude

从表单中排除的字段名称列表。

### fields

从表单中显示字段名称的列表。

### fieldsets

fieldsets是一个二元组的列表，第一个参数是name（自己取名，字符串或者None），第二个参数是field_options，包括有关字段集的信息字典。

使用如下：

```python
fieldsets = (
        (None, {
            'fields': ('url', 'title', 'content', 'sites')
        }),
        ('Advanced options', {
            'classes': ('collapse',),
            'fields': ('registration_required', 'template_name'),
        }),
    )
```

- fields：显示的字段名称的元组，这个关键字是必须的。如果想要在同一行上显示多个表单组件，可以这样写：

```python
{
'fields': (('first_name', 'last_name'), 'address', 'city', 'state'),
}
```

- class：有两个取值，一个是collapse，表示可以折叠。另一个是wide，表示额外的水平空间。
- description：一段在标题下的描述信息

### filetr_horizontal

对于使用ManyToMay字段较多时可以使用该属性，可以使界面更加简洁美观。使用方法如下：

```python
filter_horizontal = ["users",]
```

<img src="https://upload-images.jianshu.io/upload_images/18750334-43cf4cbac11d16e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

###  filter_vertical

对于使用ManyToMay字段较多时可以使用该属性，可以使界面更加简洁美观。不过效果是垂直展示的。

### form

自定义后台模型表单，使用方法如下：

```python
from django import forms
from django.contrib import admin
from myapp.models import Person

class PersonForm(forms.ModelForm):

    class Meta:
        model = Person
        exclude = ['name']

class PersonAdmin(admin.ModelAdmin):
    exclude = ['age']
    form = PersonForm
```

### formfield_overrides

这个方法可以自定义设置django后台field对应的表单小组件。但是django文档中并没有给出自定义表单小组件的教程，所以只能看源码学习如何写出来。现在，让我们应用一下django中内置的表单小组件。比如，将所有CharField字段改为TextInput形式：

```python
from django.contrib import admin
from django.db import models

from myapp.models import People
from django.forms import widgets

class MyModelAdmin(admmin.ModelAdmin):
    formfield_overrides = {
        models.CharField:{'widget': widgets.TextInput}
    }
```

django内置的其他参数还有：TextInput，NumberInput，EmailInput，URLInput，PasswordInput，HiddenInput，FileInput，Textarea，DateInput，DateTimeInput，TimeInput，CheckboxInput，ChoiceWidget等等。详情请查看django.forms.widgets源码。

### inlines

内联外键，可以将互相外键的两个表内联到其中一个表中，使用如下：

```python
from django.db import models

class Author(models.Model):
   name = models.CharField(max_length=100)

class Book(models.Model):
   author = models.ForeignKey(Author, on_delete=models.CASCADE)
   title = models.CharField(max_length=100)
```

您可以在作者页面上编辑作者创作的书籍。通过在中指定内联，可以将内联添加到模型中ModelAdmin.inlines：

```python
from django.contrib import admin

class BookInline(admin.TabularInline):
    model = Book
    extra = 0

class AuthorAdmin(admin.ModelAdmin):
    inlines = [
        BookInline,
    ]
```

InlineModelAdmin提供了两个子类，分别是TabularInline和StackedInline它们的区别只有渲染模板不同。TabularInline占用的空间比较小，StackedInline占用空间比较大。

内联外键还有些相关设置，请参考后文的InlineModelAdmin对象。

### list_display

设置list_display以控制在管理员的更改列表页面上显示哪些字段。

### list_display_links

点击进入的链接的参数，默认情况下是第一列进入。

### list_editable

设置list_editable为模型上字段名称的列表，该列表将允许在更改列表页面上进行编辑。

### list_filter

设置list_filter以激活管理员更改列表页面右侧栏中的过滤器，如以下屏幕截图所示：

<img src="https://docs.djangoproject.com/en/3.1/_images/list_filter.png" width="100%">

### list_per_page

控制在每个分页的管理员更改列表页面上显示多少个项目。默认情况下，此设置为`100`。

### list_select_related

默认值是False，如果为True时select_related()方法将始终被调用，如果设置为False，Django将查看list_display方法并调用select_related()方法。如果传递一个元组或者列表，Django会给指定的外键调用select_related()方法。

说了这么多，select_related()时什么呢？select_related()方法是QuerySet的一个方法，用于提高访问外键的性能。简而言之，就是可以减少从数据库中调用外键的次数，这样对于一些对外键的复杂操作是十分有利的，可以节约性能。

### ordering

排序。

### prepolulated_field

这个是预填充字段，就是根据其他字段的值然后自动填充字段。使用方法如下：

```python
class ArticleAdmin(admin.ModelAdmin):
    prepopulated_fields = {"slug": ("title",)}
```

### radio_fields

用单选按钮代替select选框。

```python
class PersonAdmin(admin.ModelAdmin):
    radio_fields = {"group": admin.HORIZONTAL}
```

有两个可选参数，一个是HORIZONTAL，一个是VERTICAL，分别表示水平和垂直。

### readonly_fields

设置只读字段，为一个元组或列表。

### search_fields

搜索参数，可以用双下划线方法跨表查找。

### view_on_site

默认为True。如果您的模型有get_absolute_url()方法，但您不想显示“现场查看”按钮，则只需将其设置 view_on_sit为False

## ModelAdmin方法

ModelAdmin有丰富的内置方法，就把他们当作钩子函数吧。

### save_model(request, obj,  form, change)

这个字段可以再数据保存时进行一些额外的操作，比如自动添加作者和日期：

```python
from django.contrib import admin

class ArticleAdmin(admin.ModelAdmin):
    def save_model(self, request, obj, form, change):
        from datetime import datetime
        obj.user = request.user
        obj.date = datetime.now().strftime("%Y%m%d-%H%M%S")
        super(ArticleAdmin, self).save_model(request, obj, form, change)
```

change参数可以判断该保存是修改还是新增，如果是修改，change值为True，如果是新增，change值为Flase。

### delete_model(request, obj)

当执行删除操作时会执行一次这个方法，不过注意，使用管理员动作批量删除的时候不会调用这个方法，只有点击那个红色的按钮时才会触发这个方法。使用方法如下：

```python
def delete_model(self, request, obj):
    # do something
    return super(PeopleAdmin, self).delete_model(request, obj)
```

### delete_queryset(request, queryset)

和delete_model一样，不过这个函数是使用管理员动作删除触发，点击那个红色的按钮不会触发。

### get_ordering(request)

定义在后台数据的排列方式，这个方法不会数据库中的ordering排列方式发生变化，仅仅只对后台呈现数据起作用。

```python
def get_ordering(self, request):
    if request.user.is_superuser:
        return ['name', 'rank']
    else:
        return ['name']
```

### get_search_results(request, queryset, search_term)

自定义搜索框，此时你必须设置search_fields属性。使用如下：

```python
class PersonAdmin(admin.ModelAdmin):
    list_display = ('name', 'age')
    search_fields = ('name',)

    def get_search_results(self, request, queryset, search_term):
        queryset, use_distinct = super().get_search_results(request, queryset, search_term)
        try:
            search_term_as_int = int(search_term)
        except ValueError:
            pass
        else:
            queryset |= self.model.objects.filter(age=search_term_as_int)
        return queryset, use_distinct
```

### get_autocomplete_fields(request)

返回一个列表或者元组。这个方法可以直接设置autocomplete_field属性进行控制，所以不用设置。这个方法比属性优先度更高。

### get_readonly_fields(request, obj=None)

这个方法可以让普通用户不能修改文章而超级用户可以修改某些字段。使用方法如下：

```python
class MachineInfoAdmin(admin.ModelAdmin):
 
    def get_readonly_fields(self, request, obj=None):
        if request.user.is_superuser:
            self.readonly_fields = []
        return self.readonly_fields
     
    readonly_fields = ('machine_ip', 'status', 'user', 'machine_model', 'cache', 'cpu', 'hard_disk', 'machine_os', 'idc', 'machine_group')
```

### get_prepopulated_fields(request, obj=None)

返回一个字典。这个方法可以直接设置prepopulated_fields属性进行控制。这个方法比属性优先度更高。

### get_list_display(request)

返回一个列表或元组。这个方法可以直接设置list_display属性进行控制。这个方法比属性优先度更高。

### get_list_display_links(request, list_display)

返回一个列表或元组或None，这个方法可以直接设置list_display_links属性进行控制。这个方法比属性优先度更高。

### get_exclude(request, obj=None)

返回一个列表或元素或None。这个方法可以直接设置exclude属性进行控制。这个方法比属性优先度更高。设置为None则该属性不起作用。

### get_fields(request, obj=None)

返回一个列表或元素或None。这个方法可以直接设置fields属性进行控制。这个方法比属性优先度更高。设置为None则该属性不起作用。

### get_fieldsets(request, obj=None)

返回一个两元组的列表。个方法可以直接设置fieldsets属性进行控制。这个方法比属性优先度更高。设置为None则该属性不起作用。

### get_list_filter(request)

返回一个列表或元组。这个方法可以直接设置list_filter属性进行控制。这个方法比属性优先度更高。

### get_list_select_related(request)

返回值为一个布尔值或者是列表或元组。这个方法可以直接设置list_select_reload属性进行控制。这个方法比属性优先度更高。

### get_search_fields(request)

返回值为一个列表或元组。这个方法可以直接设置search_fields属性进行控制。这个方法比属性优先度更高。和get_search_results不同，这个是指定搜索的字段，get_search_results是指定搜索的结果。

### get_sortable_by(request)

返回一个列表元组或集合。这个方法的行为与get_ordering方法的行为一样，如果同时设置这两个方法，将以get_ordering方法为准。这个方法可以直接设置sortable_by属性进行控制。这个方法比属性优先度更高。

### get_inline_instance(request, obj=None)

返回一个含有InlineModelAdmin对象的元组或列表。如下操作是没有添加，更改，删除，查看权限的默认过滤：

```python
class MyModelAdmin(admin.ModelAdmin):
    inlines = (MyInline,)

    def get_inline_instances(self, request, obj=None):
        return [inline(self.model, self.admin_site) for inline in self.inlines]
```

### formfield_for_foreignkey(db_field, request,**kwargs)

当点开某个有外键的一条数据会被执行。方法可以用来修改外键下拉框里面的数据，使用方法如下：

```python
def formfield_for_foreignkey(self, db_field, request, **kwargs):
	if db_field.name == "hobby":  # 相应字段名称为hobby
	kwages["queryset"] = Hobby.objects.filter(name="打球")
    # 此时，name不等于打球的数据将不会在外键字段下显示
	return super(PeopleAdmin, self).formfield_for_foreignkey(db_field, request, **kwargs)
```

### formfield_for_manytomany(db_field，request，** kwargs)

与formfield_for_foreignkey一样。

### formfield_for_choice_field(db_field，request，** kwargs)

和前两者一样，不过呢有些参数发生了变化，例子如下：

```python
def formfield_for_choice_field(self, db_field, request, **kwages):
    if db_field.name == "grade":
        kwages['choices'] = (("yello", "人妖"), ("alian", "外星人"),)
    return super(PeopleAdmin, self).formfield_for_choice_field(db_field, request, **kwages)。
```

### has_view_permission(request, obj=None)

返回True或者False。无效

### has_add_permission(request, obj=None)

返回True或者False。有效

### has_change_permission(request, obj=None)

返回True或者False。有效

### has_delete_permission(request, obj=None)

返回True或者False。有效

### has_module_permission(request, obj=None)

返回True或者False。无效

### get_queryset(request)

这个方法可以让普通用户只能看到自己的文章，而超级用户可以看到所有的文章。

```python 
class MyModelAdmin(admin.ModelAdmin):
    def get_queryset(self, request):
        qs = super(MyModelAdmin, self).get_queryset(request)
        if request.user.is_superuser:
            return qs
        return qs.filter(author=request.user)
```

### message_user(request, message, level=message.INFO)

发送后台提示信息，分为五个级别：debug，info，success，warning，error。使用方法如下：

```python
self.message_user(request, "hello", "warning")
```

### response_add(request, obj, post_url_continue=None)

添加数据后调用该方法。

### response_change(request, obj)

修改数据后调用该方法

### response_delete(request, obj_display, obj_id)

删除数据后调用该方法get

### get_changeform_initial_data(request)

返回一个字典，新建表单时候会默认填写几个参数到表单中。

```python
def get_changeform_initial_data(self, request):
    return {'name': 'custom_initial_value'}
```

### get_actions方法

这个方法在打开操作数据界面和每次使用管理员动作的时候调用。

```python
def get_actions(self, request):
    return super().get_actions(request)
```

### add_view(request, form_url="", extra_context = None)

模型添加页面的Django视图函数。

### changelist_view(request, extra_context=None)

模型实例更改列表操作页面的django视图。

### delete_view(request, object_id, extra_context="None")

确认删除界面的django视图。

### change_view(request, object_id, form_url="", extra_context=None)

表示对单条数据进行操作的方法。就是每次后台点击单条数据打开准备修改之前会执行的方法：

```python
def change_view(self, request, object_id, form_url="", extra_context=None):
    change_obj = Article.objects.filter(pk=object_id)
    return super(ArticleAdmin, self).change_view(request, object_id, from_url, extra_context)
```

### history_view(request, object_id, extra_context=None)

修改历史记录界面的django视图

## InlineModelAdmin对象

### model

指定内联使用的模型。

### fl_name

模型上的外键名称。在大多数情况下，这将自动处理，但是`fk_name`如果同一个父模型有多个外键，则必须明确指定。

### classes

和fieldsets一样写个类classes。默认取值有collapse和wide。

### extra

默认值为3，表示除了已有字段外额外多三个字段的空间。这个参数也可以通过get_extra()方法来自定义。

### max_num

控制内联显示的最大表格数。这个参数会约束空白的额外字段，不会约束已有字段。

### min_num

控制了要在内联显示的最小表单数。如果已有字段大于等于`min_num`，那么已有字段就会占用已有的空间，如果已有字段由于最小表单数，那么就会占用`min_num`的空间。

### template

指定要渲染表单的文件路径，这样我们可以写自己的表单组建来代替Django的表单组件。

### verbose_name和verbose_name_plural

主要是汉化在区块顶部的显示.

### can_delete

指定是否可以在内联中删除内联对象。默认为`True`。反正就算设置True我也不知道怎么删除相关对象，找不到按钮emmmm

### show_change_link

指定可以在管理员中更改的嵌入式对象是否具有指向更改表单的链接。默认为`False`。

### get_extra(request, obj=None, **kwargs)

返回一个默认数字即可。然后obj参数是点击的那个对象。这个方法可以直接设置extra属性进行控制。这个方法比属性优先度更高。

### get_max_num(request, obj=None, **kwargs)

返回一个数字即可，这个方法可以直接设置max_num属性进行控制。这个方法比属性优先度更高。

### get_min_num(request, obj=None, **kwargs)

返回一个数字即可，这个方法可以直接设置min_num属性进行控制。这个方法比属性优先度更高。

### has_add_permission(request, obj)

返回布尔值，是否有添加权限。

### has_change_permission(request, obj=None)

返回布尔值，是否有修改权限。

### has_delete_permission(request, obj=None)

返回布尔值，是否有删除权限。

## admin全局配置

全局配置更改如下，详情请见官网：详情请见官网：[https://docs.djangoproject.com/en/3.0/ref/contrib/admin/](https://docs.djangoproject.com/en/3.0/ref/contrib/admin/)

### admin.site.register("Blog")

注册字段，在管理界面可编辑，一般用修饰器。

### admin.site.site_header = 'catfish的个人博客'           

设置顶部标题。

### admin.site.site_title = “欢迎光临，我的博客”

设置标签页标题。

### admin.site.site_url = '/'

设置后台网站顶部打开url的相对链接，默认为'/'。

### admin.site.index_title

设置导航栏顶部下面的标题，默认为“站点管理”。

### admin.site.empty_value_display = "这里为空"

设置默认的空值字符串，默认为'-'

### admin.site.enable_nav_sidebar = False

django3.1新增：一个布尔值，该值确定是否在较大的屏幕上显示导航侧栏。默认情况下，它设置为True。

## staff_member_required装饰器

用法如下：

```python
from django.contrib.admin.views.decorators import staff_member_required

@staff_member_required
def my_view(request):
    ...
```

该修饰器用于需要授权的管理员视图。