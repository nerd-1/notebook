## CSS三大特性

### 继承性

给父元素设置一些属性，子元素也可以使用，被称为继承性。只有以color，text-，line-，font-，开头的属性才可以继承。不仅仅只有儿子可以继承，后代都可以继承。

### 层叠性（覆盖性）

作用：层叠性就是css处理冲突的一种属性。层叠性只有在多个选择器选中同一个标签，然后又设置了相同的属性才会发生层叠性。

### 优先级

层叠性的优先级：内联>id>类>标签>通配符>继承>浏览器默认

### 优先级之important

important只能用于直接选中，不能用于间接选中。用法入下：

```css
*{
    color: blue !important;
}
```
## 文字相关属性

### font-style

normal（默认），itanic

### font-weight

bold，bolder，lighter（默认）

### font-size

16px（默认）

### font-family

"宋体"（默认），"微软雅黑"，"楷体"

## 文本属性

### text-decoration

underline，line-through，overline，none（默认）

eg：超链接可以用`text-decoration`来去掉下划线。

### text-align

left（默认），right，center

### text-indent

2em；0px（默认）

## 颜色控制属性

### color

red，rgb(255, 0, 0)，#FF0000，#F00，rgba(255, 0, 0, 1)

## 背景

### background-color

和color一样

### background-image

url(路径);

### background-repeat

repeat（默认），no-repeat，repeat-x，repeat-y

###  background-position

规定背景图片的位置。

```css
div{
    width: 500px;
    height: 500px;
    background-image: url("1.jpg");
    background-position: 10px 10px;
}
```

第一个取值是水平方向，第二个取值是垂直方向，其中还能添加方位名词：

```css
div{
    width: 500px;
    height: 500px;
    background-image: url("1.jpg");
    background-position: right bottom;
}
```

可选的方位名词有：top，right，left，bottom，center。

还可以使用百分数：

```css
div{
    width: 500px;
    height: 500px;
    background-image: url("1.jpg");
    background-position: 50% 50%;
}
```

### background-attachment

<b>注意</b>：背景图片不会随着滚动条的滚动而滚动，但是元素会随着滚动条滚动而滚动 

scroll（默认），fixed

### background-size

cover（背景图片最短的一边填满整个元素），contain（背景图片最长的一边填满整个元素），还可以设置宽高：

```css
div{
    background-image: url("1.jpg");
    background-repeat: no-repeat;
    background-size: 200px 100px;
}
```

还可以设置百分比：

```css
div{
    background-image: url("1.jpg");
    background-repeat: no-repeat;
    background-size: 50% 50%;
}
```

然后等比拉伸：

```css
div{
    background-image: url("1.jpg");
    background-repeat: no-repeat;
    background-size: auto 100px;
}
```

### background-origin

padding-box（默认），border-box，content-box

### background-clip

border-box（默认），padding-box，content-box

### 添加多个背景图片

第一种写法：

```css
div{
    background: url("1.jpg") no-repeat left top, url("2.jpg") no-repeat right top;
}
```

第二种写法：

```css
div{
    background-image: url("1.jpg"), url("2.jpg");
    background-repeat: no-repeat, no-repeat;
	background-position: left top, right top;
}
```



### 精灵图/雪碧图

第一步：获取图片的宽高像素

第二步：设置background-position值

第三步：调试器调试并修改

## 边框

### border

（同时设置四条边）border：边框的宽度 边框的样式 边框的颜色；

```css
div{
    width: 500px;
    height: 500px;
    border: blue solid 5px;
}
```

### border-top，border-right，border-left，border-bottom

同上。

## 内边距和外边距

### padding

内边距像素px

### padding-top，padding-bottom，padding-left，padding-right

内边距像素px

### margin

外边距

### margin-top，margin-bottom，margin-left，margin-right

外边距

### 外边距的合并

外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。

## 盒子模型

### width和height

值为像素。

### box-sizing

content-box（默认），border-box

增加border，padding和margin后，盒子元素的宽高发生改变。如果想不变需要添加该属性。

### 盒子水平居中

```css
div{
    margin: 0 auto;
}
```

### 清空样式

常用方式：

```css
*{
    margin: 0;
    padding: 0;
}
```

标准方式：

```html
<link rel="stylesheet" href="http://yui.yahooapis.com/3.18.1/build/cssreset/cssreset-min.css">
```

### line-height

行高，值为像素px。

## 网页布局方式

常见的排版方式有三种：标准流（文档流/普通流(/≧▽≦)/），浮动流，定位流。

### 标准流display

div是块级元素（设置宽高），span是行内元素（同时占一行）。

inline，block，inline-block，none

## 浮动流float

浮动流元素都可以设置宽高。

left，right，none（默认）

### 浮动流脱标

浮动流元素会相当于从标准流删除了一样，这就是浮动流脱标。

### 用clear属性清除浮动

none（默认），right，left，both。

left：表式左侧不能有浮动元素

right：表示右侧不能有浮动元素

both：表示两侧不能有浮动元素

浮动还需要搞清楚先放置和后放置，这样才能完美的理解clear属性的作用。在开发中，如果想清除浮动都给我用both！

**注意**：当我们使用clear属性后，`margin`会失效！

### 使用伪类选择器清除浮动

这是一种使用css来清除浮动且不会使margin属性失效！

```css
.hello1::after{
    content: "";
    display: block;
    clear: both;
}
/* 由于伪元素选择器使css3出来的，所以ie6不兼容，于是可以添加如下代码来使ie6兼容 */
.hello1{
    *zoom: 1;
}
```

### 使用overflow清除浮动

```css
.hello1{
    overflow: hidden;
    /* 以下代码可以使ie6兼容 */
    *zoom: 1;
}
```

## 定位流position

定位流分类：相对定位，绝对定位，固定定位，静态定位(static)（默认）

### 相对定位relative

相对定位不脱离标准流，也占用一份空间。需要配合top，left，bottom，right使用。

```css
.box2{
    width: 100px;
    height: 100px;
    background-color: red;
    position: relative;
    top: 10px;
    left: 20px;
}
```

相对定位来微调某个元素。通常相对定位和绝对定位一起使用。

### 绝对定位absolute

相对定位是脱离标准流，不占用空间。需要配合top，left，bottom，right使用。

### 小结

相对定位：根据自己原来位置进行微调，top就是距离元素上部分的距离，right就是距离元素右部分的距离（占用空间）

绝对定位：根据body的距离进行微调，top就是距离body顶部的距离，right就是距离body右边的距离（不占用空间）

如果父元素是相对定位，那么就会根据父元素属性进行微调。即：子绝父相

### 固定定位fixed

固定定位是脱离标准流，不占用空间。需要配合top，left，bottom，right使用。

### z-index

默认情况下所有元素都有一个默认的z-index属性，取值是0。

默认情况下定位流的元素会覆盖标准流的元素。后面编写的定位流元素会盖住前面编写的元素。

如果设置了z-index，谁的值比较大谁就会显示在前面。其中z-index受父元素影响。子元素中哪个父元素的z-index值大，那么哪个子元素将会优先展示。

```css
div{
    z-index: 1;
}
```