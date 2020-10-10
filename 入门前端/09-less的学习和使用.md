less是一种动态样式语言，属于css预处理器的范畴，它扩展了CSS语言，增加了变量，Minxin，函数等特性，使CSS更易于维护和扩展，LESS既可以在客户端上运行，也可以借助Node.js在服务端运行。

less的中文官网为：http://lesscss.cn/

bootstrap中less教程：http://www.bootcss.com/p/lesscss/

## less编译工具

koala官网：http://www.koala-app.com

less的cdn：

```html
<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/2.5.3/less.min.js"></script>
```

## 使用less文件的方式

目前使用less文件的方式有两种，一种是在html中使用，还有种使用koala编译，如果后面学到了node.js，优先使用node.js编译。

### 在html中使用

需要在style标签中指定type属性，还有在文件尾部引入cdn链接。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style type="text/less">             <!-- 记得要修改type属性为text/less -->
        @color : red;
        body{
            background-color: @color;
        }
    </style>
</head>
<body>
    
</body>  <!-- cdn一定要放在style标签之后 -->
<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/2.5.3/less.min.js"></script>
</html>
```

### 使用koala编译

下载好koala使用即可。

## less中的注释

以//开头的注释不会编译到css文件中，以/**/包裹的注释会被编译到css文件中。我们先写好一个less文件：

```less
// 这是见不得人的注释
/* 这是想暴露出去的注释 */
```

然后编译，css文件中的内容为：

```css
/* 这是想暴露出去的注释 */
```

## less中的变量

普通声明变量用@符号，如果是属性或者选择器需要用{}包裹。

```less
@color: red;
@m:margin;
@selector:#wrap;
h1{
    color: @color;
}
*{
    @{m}: 0;
    padding: 0;
}
@{selector}{
    color: yellow;
}
```

编译后的css文件样式：

```css
h1 {
  color: #ff0000;
}
* {
  margin: 0;
  padding: 0;
}
#wrap {
  color: yellow;
}

```

变量延迟加载：会读完所有代码才会加载变量。

## less中的嵌套规则

```less
.div{
    color: red;
    .wrap{
        width: 100px;
    }
}
```

编译后代码如下：

```css
.div {
  color: red;
}
.div .wrap {
  width: 100px;
}
```

此时，我们的需求来了，我想给div添加一个hover属性，然后这样写less文件：

```less
.div{
    color: red;
    .wrap{
        width: 100px;
    }
    :hover{
        color: green;
    }
}
```

然后编译：

```css
.div {
  color: red;
}
.div .wrap {
  width: 100px;
}
.div :hover {
  color: green;
}
```

很明显，.div与hover之间有一段空格标记，这样不符合我们的需求，如果要符合我们的需求，则要加上一个&符号即可！所以原less文件应该修改为：

```less
.div{
    color: red;
    .wrap{
        width: 100px;
    }
    &:hover{
        color: green;
    }
}
```

编译为：

```css
.div {
  color: red;
}
.div .wrap {
  width: 100px;
}
.div:hover {
  color: green;
}
```

即可，满足条件。

## less中的混合Minxin

### 不带参数的混合

```less
.juzhong(){
    margin: 0;
    padding: 0;
}
*{
    .juzhong;
}
```

### 带参数的混合

```less
.juzhong(@a, @b){
    margin: @a;
    padding: @b;
}
*{
    juzhong(0, 0);
}
```

### 带参数且有默认值的混合

```less
.juzhong(@a:10px, @b:10px){
    margin: @a;
    padding: @b;
}
*{
    juzhong();
}
```

### 命名参数

```less
.juzhong(@a:10px, @b:10px){
    margin: @a;
    padding: @b;
}
*{
    juzhong(@b:100px);
}
```

### 匹配模式

先在一个less文件中写入：

```less
@import "./traingle.less";

#wrap .sjx{
    .striangle(L, 400px, red);
}
```

第二个less文件中写入：

```less
.triangle(@_,@w,@c){
    width: 0px;
    height: 0px;
    overflow: hidden;
}

.triangle(L,@w,@c){
    border-width: @w;
    border-style: dashed solid dashed dashed;
    border-color: transparent @c transparent transparent;
}
```

第一个参数是匹配符，如果第一个参数是@_，后面形参数量要相同。那么在调用之前都会事先调用一次该混合。最后文件的css文件的显示结果为：

```css
#wrap .sjx {
  width: 0px;
  height: 0px;
  overflow: hidden;
  border-width: 400px;
  border-style: dashed solid dashed dashed;
  border-color: transparent #ff0000 transparent transparent;
}
```

### argument变量

```less
.border(@1, @2, @3){
    border: @arguments;
}
#warp .sjx{
    .border(1px, solid, black);
}
```

## less运算

```less
#wrap .sjx{
    width: (100+100px);
}
```

## 继承

继承不允许出现参数。

```less
.juzhong{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: 0 auto;
}
#wrap{
    position: relative;
    width: 300px;
    height: 300px;
    inner:extend(.juzhong){
        &:nth-child(1){
            width: 100px;
            height: 100px;
            background: pink;
        }
        &:nth-child(2){
            width: 50px;
            height: 50px;
            background: yellow;
        }
    }
}
```

然后css代码如下：

```css
.juzhong,
#wrap inner,
#wrap inner:nth-child(1),
#wrap inner:nth-child(2) {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  margin: 0 auto;
}
#wrap {
  position: relative;
  width: 300px;
  height: 300px;
}
#wrap inner:nth-child(1) {
  width: 100px;
  height: 100px;
  background: pink;
}
#wrap inner:nth-child(2) {
  width: 50px;
  height: 50px;
  background: yellow;
}
```

其中继承还可以这样写：

```less
.juzhong{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: 0 auto;
}
#wrap{
    position: relative;
    width: 300px;
    height: 300px;
    inner{
        &:extend(.juzhong);
        &:nth-child(1){
            width: 100px;
            height: 100px;
            background: pink;
        }
        &:nth-child(2){
            width: 50px;
            height: 50px;
            background: yellow;
        }
    }
}
```

继承编译出来的css代码性能比混合高，如果没有参数的混合推荐用继承的形式。

如果继承代码有hover等其他伪类选择器，则需要在后面加上all，使用如下：

```less
.juzhong{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: 0 auto;
}
.juzhong:hover{                       // hover属性
    background-color: red;  
}
#wrap{
    position: relative;
    width: 300px;
    height: 300px;
    inner{
        &:extend(.juzhong all);  // 加上了all
        &:nth-child(1){
            width: 100px;
            height: 100px;
            background: pink;
        }
        &:nth-child(2){
            width: 50px;
            height: 50px;
            background: yellow;
        }
    }
}
```

## 避免编译

使用方法如下：

```less
*{
    margin: ~"calc(100+100px)";  // 这样就不会被less编译变成calc(200px);会原封不动变成calc(100+100px)
    padding: 0;
}
```