# 正则表达式

在JavaScript中存在正则表达式对象，JavaScript同故宫内置`RegExp`类型支持正则表达式对象，String和`RegExp`都提供了执行正则表达式匹配操作的方法。

## 定义正则表达式

定义正则表达式的很简单，通过构造函数的方式定义一个正则表达式对象。实例如下：

```javascript
let a = new RegExp("aaa");
console.log(a, typeof a);    // 输出/aaa/ "object"
```

此时a就是一个正则表达式的对象了，还有个更简单的 正则表达式定义方式：

```javascript
let a = /aaa/;
console.log(a, typeof a);   // 输出/aaa/ "object"
```

然后这个正则表达式构造函数还可以选择第二个参数，第二个参数有三个取值，分别是g，i，m。g表示全局匹配，如果不写g的话，就只会匹配出现的第一个式子。i表示不区分大小写的匹配，如果不写i，就表示区分大小写的匹配。m表示多行匹配，如果不写m，就会当作是一行来进行匹配。

是不是还很模糊，看看实例就清楚了：

```javascript
let r = /\b\w/gi；     // 或者写成：let r = new RegExp("\\b\\w", "gi");
let s = "Javascript Java"；
a = s.match(r);
console.log(a);       // 输出["J", "J"]
```

match方法不认识？没关系，后面会有解释。然后让我们来先看看m参数的用法，m修饰符规定正则表达式可以执行多行匹配。m修饰符的作用是修改^和$在正则表达式中的作用，让它们分别表示行首和行尾。在默认状态下，一个字符串无论是否换行只有一个开始^和结尾$，如果采用多行匹配，那么每一个行都有一个^和结尾$。简单的说，m标签会给有换行符(\n)的字符串前后都自动加上^和$符号进行匹配。以便于在匹配中使用^和$进行匹配。实例如下：

```javascript
var str="This is an\n antzone good"; 
var reg=/an$/;
console.log(str.match(reg));     // 输出null
```

这个时候无法匹配字符串"an"，因为尽管换行了但是没有开始(^)和结束($)标识符。如果加上m即可匹配到了：

```javascript
var str="This is an\n antzone good"; 
var reg=/an$/m;
console.log(str.match(reg));  // 输出["an", index: 8, input: "This is an↵ antzone good", groups: undefined]
```

现在只管输出结果能匹配到an，其他参数后面会一个一个讲到。

## 字符串对象的相关方法

这是上一节中漏掉了的相关方法，由于涉及到正则表达式，所以我没有在上一章节中讲到。

### search方法

search方法返回匹配字符串的下标。使用方法如下：

```javascript
let s = "https://www.catfish1921.com";
let r = /\./;
console.log(s.search(r));    // 返回11
```

注意：search方法不支持全局匹配，就算加上g参数也只返回匹配到的第一个字符串的下标：

```javascript
let s = "https://www.catfish1921.com";
let r = /\./g;
console.log(s.search(r));    // 返回11
```

如果找不到匹配的字符串，则会返回-1。

### match方法

match方法能找到匹配到的所有字符串，并以数组的形式返回。如果加上g参数就会以数组的形式返回所有匹配到的字符串，如果不加上g参数则只会返回匹配到的第一个字符串。

另外，在非全局匹配的时候（不加参数g），会在数组中添加两个属性：index属性和input属性，其中，index属性表示匹配到的第一个字符串的下标，input属性表示被匹配的字符串，使用用法如下：

```javascript
let str = "https://www.catfish1921.com";
let reg = /w/;
str.match(reg);    // 返回["w", index: 8, input: "https://www.catfish1921.com", groups: undefined]
```

如果是全局匹配，则不会有这两个属性：

```javascript
let str = "https://www.catfish1921.com";
let reg = /w/g;
str.match(reg);    // 返回["w", "w", "w"]
```

如果找不到匹配的字符串，则会返回null.。使用方法如下：

```javascript
let str = "https://www.catfish1921.com";
let reg = /x/;
str.match(reg);    // 返回null
```

match方法还会在非全局匹配中返回所有子表达式的匹配结果，如果全局匹配，则不会返回子表达式的返回结果。所谓子表达式，就是一个表达式中小括号内的表达式称为子表达式。

```javascript
let s = "https://www.catfish1921.com/blog/";
let a = /(\.).*(\.)/;
s.match(a);   //返回 [".catfish1921.", ".", ".", index: 11, input: "https://www.catfish1921.com/blog/", groups: undefined]
```

### replace方法

replace方法也能使用正则表达式，使用方法如下：

```javascript
let str = "https://www.catfish1921.com";
let a = str.replace(/w/, "m");  // 返回https://mww.catfish1921.com
console.log(a);
```

也可以添加全局匹配模式：

```javascript
let str = "https://www.catfish1921.com";
let a = str.replace(/w/g, "m");  // 返回https://mmm.catfish1921.com
console.log(a);
```

### split方法

split方法也能根据正则表达式分割字符串，使用方法如下：

```javascript
let str = "https://www.catfish1921.com";
str.split(/t/);          // 返回 ["h", "", "ps://www.ca", "fish1921.com"]
```

split方法无论是否是全局模式，都会全部拆解字符串。

## 正则表达式对象相关方法

接下来我们将要了解的是正则表达式对象的相关方法。

### exec方法

在非全局匹配的模式下，exec等同于match。

在全局模式下，exec仍然会返回第一个的值，不过它会记录下来，然后第二次再匹配的时候就会从第一次匹配的位置加一开始继续匹配。使用方法如下：

```javascript
let str = "https://www.catfish1921.com";
let reg = /w/g;
reg.exec(str);   // ["w", index: 8, input: "https://www.catfish1921.com", groups: undefined]
reg.exec(str);   // ["w", index: 9, input: "https://www.catfish1921.com", groups: undefined]
```

无论是否全局模式，exec的方法不仅会返回index和input方法，还会返回子表达式。

### test方法

test方法用于检测一个字符串是否包含另一个字符串。使用方法如下：

```javascript
let str = "https://www.catfish1921.com";
let r = /www/
r.test(str)     // 返回true
```

### compile方法

重新编译正则表达式对象。使用方法如下：

```javascript
let a = /www/;
a.compile(/hhh/);   // 返回值为/hhh/
```

等效于对该正则表达式使用`RegExp`函数。

## 正则表达式对象的属性

### global

返回布尔值，检测`RegExp`对象是否有标志g。

### ignoreCase

返回布尔值，检测`RegExp`对象是否有标志i。

### multiline

返回布尔值，检测`RegExp`对象是否有标志m。

### source

返回正则表达式中的源码。使用方法如下：

```javascript
let a = /\b/;
console.log(a.source)  // 输出\b
```

个人觉得这个方法就是把正则表达式对象转换为字符串对象。

## RegExp的静态属性

`RegExp`类型包含一组静态属性，通过`RegExp`对象直接访问，这组属性记录了当前脚本中最新正则表达式的详细信息。

| 长名         | 短名  | 说明                                                         |
| ------------ | ----- | ------------------------------------------------------------ |
| input        | $_    | 返回当前所作用的字符串，初始值为空字符串""                   |
| index        |       | 当前模式匹配的开始位置，从 0 开始计数。初始值为 -1，每次成功匹配时，index 属性值都会随之改变 |
| lastIndex    |       | 当前模式匹配的最后一个字符的下一个字符位置，从 0 开始计数，常被作为继续匹配的起始位置。初始值为 -1，表示从起始位置开始搜索，每次成功匹配时，lastIndex 属性值都会随之改变 |
| lastMatch    | $&    | 最后模式匹配的字符串，初始值为空字符串""。在每次成功匹配时，lastMatch 属性值都会随之改变 |
| lastParen    | $+    | 最后子模式匹配的字符串，如果匹配模式中包含有子模式（包含小括号的子表达式），在最后模式匹配中，最后一个子模式所匹配到的子字符串。初始值为空字符串""。在每次成功匹配时，lastParen属性值都会随之改变 |
| leftContext  | $`    | 在当前所作用的字符串中，最后模式匹配的字符串左边的所有内容。初始值为空字符串""。每次匹配成功时，其属性值都会随之改变 |
| rightContext | $'    | 在当前所作用的字符串中，最后模式匹配的字符串右边的所有内容。初始值为空字符串""。每次匹配成功时，其属性值都会随之改变 |
| $1~$9        | $1~$9 | 只读属性，如果匹配模式中有小括号包含的子字符串，$1~$9 属性值分别是第 1 个到第 9 个子模式所匹配到的内容。如果有超过 9 个以上的子模式，$1~$9 属性分别对应最后的 9 个子模式匹配结果。在一个匹配模式中，可以指定任意多个小括号包含的子模式，但 RegExp 静态属性只能存储最后 9 个子模式匹配的结果。在 RegExp 实例对象的一些方法所返回的结果数组中，可以获得所有圆括号内的子匹配结果 |

使用方法如下：

#### 示例1

下面示例演示了 RegExp 类型静态属性使用，匹配字符串“[Java](http://c.biancheng.net/java/)Script”。

```javascript
var s = "JavaScript, not JavaScript";
var r = /(Java)Script/gi;
var a = r.exec(s);  //执行匹配操作
console.log(RegExp.input);  //返回字符串“JavaScript, not JavaScript”
console.log(RegExp.leftContext);  //返回空字符串，左侧没有内容
console.log(RegExp.rightContext);  //返回字符串“,not JavaScript”
console.log(RegExp.lastMatch);  //返回字符串“JavaScript”
console.log(RegExp.lastParen);  //返回字符串“Java”
```

执行匹配操作后，各个属性的返回值说明如下：

- input 属性记录操作的字符串：“JavaScript, not JavaScript”。
- leftContext 属性记录匹配文本左侧的字符串在第一次匹配操作时，左侧文本为空。而 rightContext 属性记录匹配文本右侧的文本，即为“,not JavaScript”。
- lastMatch 属性记录匹配的字符串，即为“JavaScript”。
- lastParen 属性记录匹配的分组字符串，即为“Java”。


如果匹配模式中包含多个子模式，则最后一个子模式所匹配的字符就是“RegExp.lastParen”。

```JavaScript
var r = /(Java)(Script)/gi;
var a = r.exec(s);  //执行匹配操作console.log(RegExp.lastParen);  
console.log(RegExp.lastParen);//返回字符串“Script”，而不再是“Java”。
```

#### 示例2

针对上面示例也可以使用短名来读取相关信息。

```javascript
var s = "JavaScript, not JavaScript";var r = /(Java)(Script)/gi;var a = r.exec(s);console.log(RegExp.$_);  //返回字符串“JavaScript, not JavaScript”console.log(RegExp["$`"]);  //返回空字符串console.log(RegExp["$'"]);  //返回字符串“,not JavaScript”console.log(RegExp["$&"]);  //返回字符串“JavaScript”console.log(RegExp["$+"]);  //返回字符串“Script”
```

这些属性的值都是动态的，在每次执行匹配操作时，都会被重新设置。

## 相关资料

对于m参数的解释：[https://www.jb51.net/article/101139.htm](https://www.jb51.net/article/101139.htm)

对于RegExp静态属性：[http://c.biancheng.net/view/5621.html](http://c.biancheng.net/view/5621.html)