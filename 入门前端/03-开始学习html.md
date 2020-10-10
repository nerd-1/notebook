## 开始！

### 什么是浏览器？

浏览器是安装在电脑里面的一个软件，能够让网页内容呈现给用户查看，并让用户与网页交互的一种软件。

### 常见主流浏览器：

chrome, ie, firefox, safari, opera，不同的浏览器内核不同，因为内核不同从而产生了兼容问题。

### 浏览器访问网站的原理

1. 当我们利用浏览器访问网页时，其实是有真实的物理文件传输的，浏览器会先将网页上的内容缓存到本地文件夹中，然后再渲染出来呈现给用户查看。
2. 平时我们在上网时会感觉到第二次访问网页会比第一次要快，就是因为第一次访问已经将这个网页上的信息缓存到了本地
3. 缓存文件夹除了缓存图片以外还缓存了一些例如.js .css .html等文件，所以一个网页不是一个文件，而是一堆文件，网页越复杂那么组成这个网页的文件就越多。

### 什么是html？

`html`是`HyperText Markup Language`的缩写，超文本标记语言。

### html的作用

给纯文本添加语义。一定要记住，`html`是专门给文本添加语义的而不是用来修改样式的！

### 标签

1. h标签：用来给文本添加标题语义的，最多1~6，超过6无效，`h1`最大`h6`最小，一个网页最多只有一个`h1`标签。

2. p标签：告诉浏览器哪些是一个段落。

3. hr标签：在浏览器上显示一条分隔线。

4. img标签：告诉浏览器显示一张图片。
```html
<img src="">
<!--
属性列表：
src：资源链接
width：宽度
height：高度
title：鼠标悬停后会弹出提示文本
alt：告诉浏览器需要显示的图片找不到时会显示什么内容
-->
```

5. br标签：在`html`中换行

6. a标签：用于控制页面与页面之间的跳转

```html
<a href=""></a>
<!-- 
属性列表：
href: 需要指定跳转的路径，一个标签必须要有href属性
target：_blank新标签跳转，默认是_self
title：鼠标悬停后会弹出提示文本
-->
```

7. base标签：同一指定网页所有的超链接如何打开

```html
<base target="_blank">
<!--
写在head标签之间
-->
```

8. 列表标签：告诉浏览器这堆数据是一个整体，分为无序列表，有序列表，定义列表

```html
<!-- 无序列表 -->
<ul>
    <li></li>
</ul>
<!-- 有序列表 -->
<ol>
    <li></li>
</ol>
<!-- 自定义列表 -->
<dl>
    <dt></dt>
    <dd></dd> <!--推荐使用一个dt对应一个dd-->
    <dt></dt>
    <dd></dd>
</dl>
```

常用`css`修改样式：

```css
ul{
    list-style: none;
}
```

### 注释

```html
<!-- 这是一条注释 -->
```

### 假链接

```html
<a href="#">点我呀</a>
<a href="javascript:;">点我呀</a>
```

### 锚点

```html
<a href="#ida">点击跳转</a>
<h2 id="ida">目标位置</h2>
```

如果想锚点加上过渡效果，只需要添加如下`css`代码：

```css
html{
    scroll-behavior: smooth;
}
```

## 表格

其实在过去表格标签使用的非常非常多，绝大部分标签都是根据表格标签来制作的，也就是说表格标签是一个时代的代表。

```html
<table>
    <tr>
        <td></td>
    </tr>
</table>
```

要求：写一个两行三列的表格：

```html
<table border="1"> <!-- 默认情况下表格标签的线条省略，加上border属性显示线条 -->
    <tr>
    	<td>1.1</td>
        <td>1.2</td>
        <td>1.3</td>
    </tr>
    <tr>
    	<td>2.1</td>
        <td>2.2</td>
        <td>2.3</td>
    </tr>
</table>
```

### 表格标签的属性

```html
<!--
1. 宽度和高度：
可以给table和td标签使用(width，height)

2. 水平对齐和垂直对齐的属性：
水平对齐只能给table，tr，td标签使用（align: left, right, center)
垂直对齐只能给tr标签和td标签使用(valign: center, bottom, top)

3. 外间距和内间距
只能给table标签使用(cellsapcing, cellpadding)
-->
<table border="1" cellspacing="0"> <!--默认情况下单元格与单元格外边距是2--> 
    <tr>                           <!--默认情况下单元格与单元格内边距是1--> 
    	<td>1.1</td>
        <td>1.2</td>
        <td>1.3</td>
    </tr>
    <tr>
    	<td>2.1</td>
        <td>2.2</td>
        <td>2.3</td>
    </tr>
</table>
```

### 细线表格

```html
<table bgcolor="black" cellspacing="1px">
    <tr bgcolor="white">
    	<td>1.1</td>
        <td>1.2</td>
    </tr>
    <tr bgcolor="white">
    	<td>2.1</td>
        <td>2.2</td>
    </tr>
</table>
```

### 表格中其他标签

```html
<caption>今日小说排行榜</caption>   
<!--
caption要紧跟在表格table标签的后面，将标题caption写在表格中会自动居中
--->
<tr>
	<th>表头信息</th>
</tr>
<!--自动加粗居中-->
```

### 表格的结构

企业开发中一般不写。

```html
<table>
    <caption>学生信息</caption>
    <thead>
    	<tr>
        	<th>姓名</th>
            <th>年龄</th>
        </tr>
    </thead>
    <tbody>
    	<tr>
            <td>张三</td>
            <td>20</td>
        </tr>
        <tr>
        	<td>李四</td>
            <td>40</td>
        </tr>
    </tbody>
    <tfoot>
    	<tr>
        	<td>2</td>
            <td>30</td>
        </tr>
    </tfoot>
</table>
```

### 单元格的合并

```html
<!-- 
1. 水平方向上单元格的合并 
可以给td标签添加colspan="2"属性，来指定一个单元格当多个单元格看待
2. 垂直方向上的单元格的合并
可以给td标签添加rowspan="2"属性，来指定一个单元格当多个单元格看待
-->
```

## 表单

表单元素用来提交信息，常见表单元素如下：

```html
<input type="text" value="123"><br>
<input type="password" value="123"><br>
性别：
<input type="radio" name="gender" checked>男
<input type="radio" name="gender">女
<input type="radio" name="gender">未知<br>
爱好：
<input type="checkbox">篮球

<!--按钮：-->
<form acrion="" method="GET">
    <!--定义一个普通按钮-->
    <input type="button" value="我是按钮">
    <!-- 定义一个图片按钮 -->
    <input type="image" src="image/register.jpg">
    <!-- 清空表单数据 -->
    <input type="reset">
    <!-- 提交表单数据 -->
    <input type="submit">
    <!-- 隐藏域 -->
    <input type="hidden" name="cc" value="it666">
</form>
```

### label标签

```html
<form acrion="">
    <!-- 第一种写法 -->
    <label for"username">账号：</label><input type="text" id="username">
    <label for"password">账号：</label><input type="text" id="password">
    <!-- 第二种写法 -->
    <label>
    	账号：<input type="text">
    </label>
</form>
```

### 其他标签

`select`标签：

```html
<select name="city">
    <optgroup label="分组">
    	<option value="beijing">北京</option>
    	<option value="shanghai">上海</option>
    </optgroup>
    <optgroup label="城市">
        <option value="guangzhou">广州</option>
        <option value="guangxi">广西</option>
    </optgroup>
    <option value="wuhan" selected>武汉</option>
</select>
```

`textarea`标签：

```html
<textarea name="massage" col="2" row="5">
</textarea>
<!--取消手动拉伸-->
<style>
    textarea{
        resize: none;
    }
</style>
```

### form标签常用属性

"\*"标记的就是一般情况下常用的属性

| 属性         | 描述                                              |
| :----------- | :------------------------------------------------ |
| \*action     | 规定向何处提交表单的地址（URL）（提交页面）。     |
| autocomplete | 规定浏览器应该自动完成表单（默认：on）。          |
| \*method     | 规定在提交表单时所用的 HTTP 方法（默认：GET）。   |
| novalidate   | 规定浏览器不验证表单。（novalidate="novalidate"） |
| target       | 规定 action 属性中地址的目标（默认：_self）。     |

### input常用属性

| 属性          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| \*value       | 规定输入字段的初始值                                         |
| readonly      | 规定输入字段为只读字段。(readonly="readonly")                |
| \*disabled    | 规定输入字段是禁用的。                                       |
| maxlength     | 规定输入字段允许的最大长度                                   |
| autocomplete  | 规定表单或输入字段是否应该自动完成。(默认："on")             |
| novalidate    | 在提交表单时不对表单数据进行验证。(novalidate="novalidate")  |
| \*autofocus   | 下方自动获得焦点                                             |
| width和height | 规定input的宽度与高度                                        |
| multiple      | 允许用户上传多个文件(multiple="multiple")                    |
| \*pattern     | 正则表达式匹配                                               |
| \*placeholder | 规定用以描述输入字段预期值的提示（样本值或有关格式的简短描述）。 |
| \*required    | 规定提交表单之前必须填写。                                   |

## 其他标签

### vidio标签

作用：播放视频

```html
<vidio src=""></vidio>
<!--
相关属性：
src: 视频播放地址
autoplay: 是否自动播放视频，autoplay="autoplay";
controls: 进度条菜单。controls="controls"
poster: 播放之前的占位图片的路径
loop: 视频播放完毕后是否循环播放loop="loop"
preload: 预加载视频，预先下载好视频，如果设置autoplay属性，preload属性会失效preload="preload"
muted:播放禁音功能loop="loop"
width:宽度
height:高度
-->
```

还有第二种格式：

```html
<vidio>
    <source src="" type=""></source>
    <source src="" type=""></source>
	<source src="" type=""></source>
</vidio>
```

### audio标签

```html
<audio src=""></audio>
<audio>
	<source src="" type="">
</audio>
<!--
属性：
除了poster/height/width其他的等同于vidio标签
-->
```