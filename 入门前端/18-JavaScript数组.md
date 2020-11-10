专门用于存储一组数据的。和我们以前学习Number/String/Boolean/Null/undefined不同，数组是引用数据类型。

## 如何创建一个数组？

基本使用：

```javascript
let 变量名称 = new Array(size);
```

实战：

```javascript
let arr = new Array(3); // 创建一个大小为3的空数组
let arr = new Array();   // 创建一个空数组
let arr = new Array(1,2,3)  // 创建一个数组[1,2,3]
```

一般使用下列构建一个数组：

```javascript
let arr = []   // 创建一个空数组
let arr = [1,2,3]  // 创建一个数组[1,2,3]
```

## 遍历数组

### for循环遍历数组

使用方法如下：

```javascript
let arr = ["a", "b", "c"];
for(let i=0; i<arr.length; i++){
    console.log(arr[i]);
}
```

### for in遍历数组

使用方法如下：

```javascript
let arr = ["a", "b", "c"];
for(let i in ls){
    console.log(arr[i]);
}
```

### for of遍历数组

使用方法如下：

```javascript
let arr = ["a", "b", "c"];
for(let i of ls){
    console.log(i);
}
```

### forEach遍历数组

使用方法如下：

```javascript
let team = ['zhangfei', 'liubei', 'guanyu'];
team.forEach(function(v, k, arr){
    console.log(v, k, arr);
    // v是遍历的值
    // k是遍历的下标
    // arr是被遍历的数组
    // 第二个和第三个参数可以省略
});
```

## 操作数组的方法

接下来介绍一下操作数组的方法。

### push方法

一个或者多个参数值添加到数组的尾部，并返回添加元素后数组的长度。

### pop方法

删除最后一个元素，并返回被删除的元素。

### unshift方法

把一个或多个元素添加到数组的头部，并返回数组长度。

### shift方法

删除第一个元素，并返回删除的元素。

### concat方法

连接两个数组，使用方法如下：

```javascript
let a = [1, 2, 3, 4, 5];
let b = a.concat(6, 7, 8);  // 也可以写成：a.concat([6, 7, 8])
console.log(b);
```

### splice方法

前两个参数不能省略，第一个参数指定删除元素的起始下标，第二个参数指定删除元素的个数。第三个元素或第三个以后的元素被视为插入的元素。

因此splice方法特别强大，可以执行一系列增删改操作。

### slice方法

截取子数组的起始和结束下标。第一个参数是起始下标，第二个参数是结束下标。

### reverse方法

颠倒数组内元素的排列顺序。

### sort方法

可以不传递参数，此时按照字母顺序对数组中的元素进行排序。使用方法如下：

```javascript
let a = ['a', 'e', 'b', 'd', 'c']
a.sort();
console.log(a);             // 返回["a", "b", "c", "d", "e"]
```

传递参数就给它添加一个回调函数，此时该回调函数需要传递两个参数。当回调函数返回值小于零时，回调函数第一个参数放在第二个参数前面；当回调函数值等于0时，说明两个值相等，不改变位置；当回调函数值大于0时，则第一个参数放在第二个参数的后面。使用方法如下：

```javascript
function f(a, b){
    return (a-b);
}
let a = [3, 1, 2];
console.log(a);       // 返回[1, 2, 3]
```

### join方法

join方法可以把数组连接成一个字符串，使用方法如下：

```javascript
let a = [1, 2, 3, 4, 5];
let a = a.join("=");
console.log(a);        // 输出1=2=3=4=5
```

### indexOf方法

返回元素值再数组中从左至右第一个匹配值的下标索引，如果没有则返回负一。第二个参数可选，为开始索引的位置。

### lastIndexOf方法

返回元素值再数组中从右至左第一个匹配值的下标索引，如果没有则返回负一。第二个参数可选，为开始索引的位置。

### every方法

检测指定元素是否满足测试，具体使用如下：

```javascript
function f(value, index, arr){
    return value%2==0 ? true : false;
}
let a = [2, 4, 5, 6, 8];
a.every(f) ? console.log("都是偶数") : console.log("不全为偶数")
```

当所有元素返回为true时，every方法返回为true，否则就返回为false。

### some方法

检测指定元素是否存在满足测试，具体使用如下：

```javascript
function f(value, index, arr){
    return value%2==0 ? true : false;
}
let a = [2, 4, 5, 6, 8];
a.some(f) ? console.log("存在偶数") : console.log("不存在偶数")
```

当有一个元素检测结果为true时，返回true。否则返回false。

### map方法

为数组中每一个元素调用一次回调函数，并返回值，使用方法如下：

```javascript
function f(value, index, array){
    let area = Math.PI*value*value;
    return area.toFixed(0);
}
let a = [10, 20, 30];
let al = a.map(f);
console.log(al);   // 输出["314", "1257", "2827"]
```

### filter方法

对数组中每一个元素调用一次回调函数返回true或者false，根据true和false筛选数组中的元素。该函数也接受三个参数，第一个是值，第二个是下标，第三个是原数组。

使用示例代码如下：

```javascript
let a = ["1", "2", "3", "4", "5", "6", "7", "8", "9"];
let arr = a.filter(function(item){
    return item < 5;
});
console.log(arr);       // 输出 [1, 2, 3, 4]
```

如果想使用箭头语法，可以这么写：

```javascript
let a = ["1", "2", "3", "4", "5", "6", "7", "8", "9"];
let arr = a.filter(item => item < 5);
console.log(arr);       // 输出 [1, 2, 3, 4]
```

### reduce方法

ruduce方法可以对数组中的所有元素调用指定的回调函数。该回调函数的返回值为累计结果，并且此返回值在下一次调用该回调函数时作为参数提供。该回调函数接受四个参数，第一个参数为上一次调用回调函数获得的值，第二个参数为当前元素的值，第三个元素为当前元素的下标，第四个参数为当前数组对象。

```javascript
function f(pre, curr, index, array){
    return pre + "::" + curr
}
let a = ["abc", "def", 123, 456];
let r = a.reduce(f);
console.log(r);        // 输出abc::def::123::456
```

### reduceRight方法

和reduce类似，不过从左至右变为从右至左。使用方法如下：

```javascript
function f(pre, curr, index, array){
    return pre + "::" + curr
}
let a = ["abc", "def", 123, 456];
let r = a.reduceRight(f);
console.log(r);        // 输出456::123::def::abc
```

## Array静态方法

接下来介绍的是Array的静态方法。

### isArray方法

判断一个值是否为数组。使用方法如下：

```javascript
let a = [1, 2, 3];
console.log(Array.isArray(a))  // true
```

### from方法

Array.from() 方法从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。实例代码如下：

```javascript
console.log(Array.from('catfish'));      // 输出 Array ["c", "a", "t", "f", "i", "s", "h"]
console.log(Array.from([1, 2, 3], x => x + x));   // 输出 Array [2, 4, 6]
```

### of方法

Array.of() 方法创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。

```javascript
Array.of(7);       // [7] 
Array.of(1, 2, 3); // [1, 2, 3]

Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]
```

## Array静态属性

### length

返回数组的长度。

