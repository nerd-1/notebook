html已经告一段落了，接下来就是学习css。简单的地方我就略过去，难得地方我就重点写出来。

## css普通选择器

### 标签选择器

```css
p{
    color: red;
}
```

### id选择器

```css
#hello{
    color: red;
}
```

### 类选择器

```css
.hello{
    color: red;
}
```

### 后代选择器

```css
div p{
    color: red;
}
```

### 子元素选择器

```css
div>p{
    color: red;
}
```

### 交集选择器

中间不加任何符号连接起来使用，可以连接多个选择器。

```css
p.paral1{
    color: red;
}
p.hello#world{
    color: red;
}
```

### 并集选择器

逗号分隔

```css
.hello, #world{
    color: red;
}
```

### 相邻兄弟选择器

表示选中紧跟在h1后面的p标签，如果p不紧跟着h1（中间有其他标签），那么就不会被选中

```css
h1+p{
    color: red;
}
```

### 通用兄弟选择器

表示选中与h1为兄弟的所有p标签

```css
h1~p{
    color: red;
}
```

### 序选择器

CSS3新增的选择器

```css
/* 选中同级别的第一个标签，如果不是p标签则不会被选中 */
p:first-child{
    color: red;
}
/* 选中同级别中同类型的第一个 */
p:first-of-type{
    color: red;
}
/* 
倒数第一个元素标签
*/
p:last-child{
    color: red;
}
/* 选中同级别中同类型的最后一个 */
p:last-of-type{
    color: red;
}
/* 同级别第三个标签是否满足，满足就会变红：不区分类型 */
p:nth-child(3){
    color: red;
}
/* 同级别同类型第n个标签被选中 */
p:nth-of-type(3){
    color: red;
}
/* 同级别倒数第三个是否满足，满足就会变红 */
p:nth-last-child(3){
    color: red;
}
/* 同级别中同类型倒数第三个 */
p:nth-last-child(3){
    color: red;
}
/* 同级别只有一个元素 */
p:only-child{
    color: red;
}
/* 同级别一类元素中只有一个元素 */
p:only-of-type{
    color: red;
}
```

序选择器中的其他用法：

```css
/* 选择奇数的所有标签 */
p:nth-child(odd){
    color: red;
}
/* 选择偶数的所有标签 */
p:nth-child(even){
    color: red;
}
/* 根据函数来进行选择 */
p:nth-child(2n+1){
    color: red;
}
```

### 属性选择器

根据指定的属性名称找到对应的标签

```css
/* id属性 */
p[id]{
    color: red;
}
/* 区分input属性 */
[name=username]{
    color: red;
}
/* 取值是否以什么开头 */
[name^=user]{
    color: red;
}
/* 取值是否以什么结尾 */
[name$=user]{
    color: red;
}
/* 取值包含什么什么 */
[name*=nam]{
    color: red;
}
```

### 通配符选择器

```css
*{
    color: red;
}
```

## 伪元素选择器

### 什么是伪元素选择器？

伪元素选择器作用就是给指定标签的内容前面添加一个子元素或者给指定标签的内容后面添加一个子元素

```css
div::before{
    content: "爱你";
}
div::after{
    content: "么么哒";
}
```

如果想要隐藏添加的元素，可以这样：

```css
div::before{
    content: "爱你";
    visibility: hidden;
}
```

这种方式隐藏会给元素留一定的空间，如果不想留空间可以这样隐藏：

```css
div::before{
    content: "爱你";
    display: none;
}
```

### a标签的伪类选择器

a标签的伪类选择器是专门用来修改a标签不同状态的样式的

```css
/* 从未被访问过的状态的样式 */
a:link{
    color: red;
}
/* 被访问过的状态的样式 */
a:visited{
    color: red;
}
/* 长按写的状态的样式 */
a:active{
    color: red;
}
/* 鼠标悬停在a标签上的状态 */
a:hover{
    color: red;
}
```

注意点：a标签伪类选择器一起出现会有顺序要求，遵守lovehate原则。所以顺序为：link，visited，hover，active。