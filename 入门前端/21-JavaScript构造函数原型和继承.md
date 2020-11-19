## 原型链之间的关系

一开始原型链啥也不懂，看了这个图就懂了……

<img src="https://upload-images.jianshu.io/upload_images/18750334-2c491a849f2002c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

## Object静态方法

### getPrototypeOf

获得一个函数的原型对象。

```javascript
Object.getPrototypeOf(hd);  // 等效于 hd.__proto__
```

### create

创建一个没有原型对象的字典：

```javascript
Object.create(null, {})   // 第一个参数写原型，如果想设置没有就写null，第二个参数是对象
```

### setPrototypeOf

把一个对象设置为另一个对象的原型对象。使用方法如下：

```javascript
let hd = {name:"catfish"};
let parent = {name:"catfish1921"};
Object.setPrototypeOf(hd, parent);    // 第一个参数是对象，第二个参数是给第一个参数设置原型对象
console.log(hd);
```

等效于：

```javascript
let hd = {name:"catfish"};
let parent = {name:"catfish1921"};
hd.__proto__ = parent;
console.log(hd);
```

不过更加推荐使用setPrototypeOf来设置原型对象。

## 给原型对象设置多个方法

如果想要给一个原型函数中设置多个方法可以这么写：

```javascript
function User(name){
    this.name = name;
}
User.prototype = {
    constructor: User,
    show(){
        console.log(this.name);
    },
    say(){
        console.log("hello");
    }
}
```

如果只想设置一个方法可以这么写：

```javascript
function User(name){
    this.name = name;
}
User.prototype.show = function(){
    console.log(this.name);
}
```

## 判断一个对象是否在另一个对象的原型链上

表示b对象是否在a对象的原型链上。前一个是原型对象，第二个是对象。

```javascript
let a = {};
let b = {};
console.log(b.isPrototypeOf(a));  // false
console.log(Object.prototype.isPrototypeOf(a));  // true
console.log(b.__proto__.isPrototypeOf(a));    // true，都是object
```

## 判断构造函数是否是对象的原型

使用方法如下：

```javascript
let a = {};
console.log(a instanceof Object);      // 输出 true
console.log(a instanceof String);      // 输出 false
```

注意：右边要有prototype属性，否则会报错。

## 判断该对象中是否有原型方法

使用方法如下：

```javascript
let a = {
    name : "catfish",
};
console.log(a.hasOwnProperty("url"))   // false
console.log(a.hasOwnProperty("name"))  // true
```

hasOwnProperty只能检测当前对象是否含有该属性。如果用in的话会检测当前对象和原型对象是否有该属性：

```javascript
let hd = {name1:"catfish"};
let parent = {name2:"catfish1921"};
Object.setPrototypeOf(hd, parent);
console.log(hd.hasOwnProperty("name1"));  // true
console.log(hd.hasOwnProperty("name2"));  // false
console.log("name1" in hd);               // true
console.log("name2" in hd);               // true
```