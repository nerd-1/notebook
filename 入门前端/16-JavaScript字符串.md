这一节主要讲的是如何操作字符串。

## 字符串的连接

### concat方法

类似于+运算符的作用，使用如下：

```javascript
let a = "abc"
console.log(a.concat("d", "e", "f"))  // 输出abcdef
```

## 字符串的查找

### charAt方法

根据字符串下标返回值。

### charCodeAt方法

根据字符串下标返回值得Unicode编码值。

### indexOf方法

从左至右根据值返回下标，如果没有返回-1。第二个参数为可选值，表示开始查找的位置。

### lastIndexOf方法

从右至左根据值返回下标，如果没有返回-1。第二个参数为可选值，表示开始查找的位置。

## 字符串的截取

### substr方法

第一个参数表示起始下标，第二个参数表示截取的长度。

### slice方法

第一个参数为起始下标，第二个参数为结果下标。

### substring方法

和slice方法一样。区别就是如果第一个参数值比第二个参数大，substring方法能够事先交换两个参数，而对于slice方法来说则视为无效，并返回空的字符串。

## 字符串的大小转换

### toLowerCase

全部转换为小写。

### toUpperCase

全部转换为大写

## 字符串转换为数组

### split方法

使用方法如下：

```javascript
let a = "JavaScript";
let b = a.split("");
console.log(b);      // 输出["J", "a", "v", "a", "S", "c", "r", "i", "p", "t"]
```

## 清空两侧空字符

### trim方法

使用如下：

```javascript
let a = "      javascript       ";
console.log(a.trim());    // 输出javascript
```

## 字符串的编码与解码

### Unicode编码——escape和unescape

escape会将所有ASCII以外的字符都进行编码，unescape是解码。使用方法如下：

```javascript
let s = "javascript中国";
escape(s);           // 返回"javascript%u4E2D%u56FD"
```

### Unicode编码——encodeURI和decodeURI

推荐使用encodeURI代替escape，用decodeURI代替unescape。但是这两者编码方式不同。

### Unicode编码——encodeURIComponent和decodeURIComponent

区别就是encodeURIComponnent和decodeURIComponent会对标点符号进行编码。而encodeURI和decodeURI不会。

### base64编码——btoa和atob

btoa是编码，atob是解码。不过btoa不能对中文进行编码，如果想对中文进行编码，则需要先把中文进行Unicode编码后再进行base64编码。