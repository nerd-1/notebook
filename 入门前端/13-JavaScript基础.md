## JavaScript概述

JavaScript简称js，是前端开发的一门脚本语言（解释性语言）。JavaScript作用是控制网页行为。JavaScript由ECMAScript，DOM，BOM组成。

### JavaScript历史

- 网景公司开发livescript语言，为了蹭Java热度，改名为JavaScript。

- 微软开发Jscript语言。
- 欧盟统一ECMAScript语言。ESMAScript是JavaScript语言的规范，在某种程度上来说等同于JavaScript语言。

## JavaScript代码的书写

### 外联式

```html
<script src="a.js"></script>
```

### 内嵌式

```html
<script>
	// javascript代码
</script>
```

### 内联式

```html
<button onclick="alert('讨厌，你点我干嘛')">点我一下试试</button>
<a href="javascript:alert('让你点你就点')">点我一下试试</a>
```

如果需要在一对script标签中编写JavaScript代码，那么就不能同时通过script标签再导入其他的.js文件，否则JavaScript代码无效。错误事例如下：

```html
<script src="1.js">
alert("hello world!");
</script>

```

此时，script标签包裹的代码不再生效。

## 延迟执行JavaScript文件

由于JavaScript代码由上至下执行，所以为了使JavaScript代码获取到页面的相关元素，等页面刷新完毕后执行JavaScript代码成为了一种需求。目前有三种解决方案：

1. 将JavaScript代码放到html文件末尾
2. 给外链式文件添加defer属性，使用如下：

```html
<script defer src="a.js"></script>
```

3. 写入函数window.onload=function(){}

```html
<script>
	window.onload = function(){
        // 里面写入JavaScript代码
    }
</script>
```

## JavaScript基本语法

### 常量

常量储存一些固定不变的数据。分类：整形常量（整数），实型常量（小数），字符串常量（字符串），布尔常量（true或者false），自定义常量（const 常量名称 = 常量取值;）

```javascript
const NUM = 666;
```

### 变量

变量表示一些可以被修改的数据

```javascript
var num;
num = 123;
console.log(num);
```

第一次给变量赋值称为变量的初始化。如果一个变量没有进行初始化，那么变量中储存的就是undefined。可以在定义变量的同时初始化：

```javascript
var num = 123;
```

同时定义多个变量：

```javascript
var num, value;
```

同时初始化多个变量：

```javascript
// 相同的值
var num, value;
num = value = 1;
// 不同的值
var num = 123, value = 666;
```

在老版本之前javascript可以先使用变量再定义变量，并不会报错。于是推出了一种新的定义变量的方式来编写代码：

```javascript
let num = 1;
let num = 2334;  // 重新定义会明确报错而用var不会报错
```

### 关键字和保留字

关键字：被赋予特殊意义的单吃

保留字：预留的关键字

标识符：程序员起的名字就成为标识符

标识符命名规则：

- 只能由26个英文字母的大小写，10个阿拉伯数字，下划线，美元符号组成。
- 不能以数字开头
- 严格区分大小写
- 不可以使用关键字，保留字作为标识符

### 注释

注释是为了方便程序员之间沟通。单行注释：

```javascript
// hello world!
```

多行注释：

```javascript
/*
hello world!
*/
```

## 基本数据类型

JavaScript中基本数据类型分为：Number，String，Boolean，Undefined，Null

### typeof 检测数据类型

```javascript
let res = typeof 123;
```

## 基本数据类型转换

### 字符串类型转换

- Number类型和Boolean类型来说可以通过toString()方法来转换

```javascript
let value = 123;
let str = value.toString(); // 输出“123”
let bool = true;
let str = bool.toString(); // 输出“true”
```

注意点：变量名称.toString是对拷贝的数据进行转换不会影响原有数据

注意点：不能使用常量直接调用toString方法

- 可以通过String(常量or变量);转换为字符串

```javascript
let value = undefined;
let str = String(undefined);  // 输出“undefined”
```

- 还可通过变量or常量+“”转换字符串

### 数值类型转换

- 通过Number（常量or变量）;方式转换

```javascript
let str = "123";
let num = Number(str);
```

注意点：如果字符串中没有数据，那么转换的结果是0

注意点：如果字符串中的数据不仅仅是数值，那么转换的结果是NaN

注意点：如果是布尔类型的true那么转换结果是1，如果是布尔类型的false那么转换结果是0

**注意点**：如果是空类型，那么转换结果是0，如果是未定义类型，那么转换结果是NaN

- 还可以通过数学运算中的加号和减号来转换

```javascript
let str = "123";
let num = +str; // 输出123
let num = -str; // 输出-123
```

- 可以通过parseInt();和parseFloat();进行转换

```javascript
let str = "3.14px";
parseFloat(str)   // 输出3.14
```

注意点：parseInt和parseFloat都会从左至右提取数值，一旦遇到非数值会停止，停止时如果没有提取到数值会返回NaN

注意点：parseInt/parseFloat都会将传入数值当作字符串来处理

```javascript
let bool = true
parseFloat(bool) // 输出NaN
```

### 布尔类型转换

- 在JavaScript中如果想将基本数据类型转换为布尔类型，那么只需要调用Boolean

只要字符串有内容都会转换为true，否则转换为flase

数字是0或者NaN就是flase，其他的都是true

undefined和null都会转换为flase

- 调用双！进行转换

```javascript
let a = "";
console.log(!!a);    // 输出false。
```