## Set场景介绍

- Set只能储存唯一值，如果有多个相同值会变成一个值：

```js
let set = new Set(["hello", "hello", "world", "catfish"])  // 返回 Set(3) {"hello", "world", "catfish"}
```

- 只能保存值，没有键名。

## Set的声明方式

声明方式如下：

```javascript
// 参数放入数组：
let set = new Set(["hello", "world"]);  // 返回Set(2) {"hello", "world"}
// 参数放入字符串：
let set = new Set("hello");     // 返回 Set(4) {"h", "e", "l", "o"}
// 如果放入多个字符串：
let set = new Set("catfish", "catfish1921");    // 返回 Set(7) {"c", "a", "t", "f", "i", "s"," h"} 只返回第一个参数，第二个参数不做返回
```

如果放入数字会报错，提示数字类型不可迭代：

```text
> let set = new Set(3124);
Uncaught TypeError: number 12341234 is not iterable (cannot read property Symbol(Symbol.iterator))
    at new Set (<anonymous>)
    at <anonymous>:1:11
```

## Set对象的相关属性

### size

返回set对象中元素的个数。

## Set对象的相关方法

### add

从最后添加对象：

```javascript
let set = new Set();
set.add("catfish");
set.add("catfish1921");
console.log(set);           // 返回 Set(2) {"catfish", "catfish1921"}
```

### has

判断Set对象中是否含有该元素：

```javascript
let set = new Set(["catfish",]);
set.has("catfish");              // 返回true
set.has("catfish1921");          // 返回false
```

### delete

从Set对象中删除一个指定元素，如果Set对象中有这个元素，那么会删除这个元素并返回true，如果没用这个元素，会不做处理返回false。

```javascript
let set = new Set(["catfish",]);
set.delete("catfish1921");        // 返回 false
console.log(set);                 // 输出 Set(1) {"catfish"}
set.delete("catfish");            // 返回 true
console.log(set);                 // 输出 Set(0) {}
```

### values

返回该Set对象的值。

```javascript
let set = new Set(["catfish", "catfish1921"]);
console.log(set.values());         // 输出 SetIterator {"catfish", "catfish1921"} 
```

### clear

清空Set对象。

```javascript
let set = new Set(["catfish", "catfish1921"]);
set.clear();
console.log(set);           // 输出 Set(0) {}
```

## Set和数组直接的转换

转换方法如下：

```javascript
let set = new Set(["catfish", "catfish1921"]);
console.log(Array.from(set));   // 第一种方式转换
console.log([...set]);          // 第二种方式转换 
```

同时，我们也可以使用Set的去重性质来将数组内元素去重：

```javascript
let array = [1, 2, 3, 4, 5, 1, 2, 3];
console.log([... new Set(array)]);   // 输出 [1, 2, 3, 4, 5]
```