## 原型链之间的关系

一开始原型链啥也不懂，看了这个图就懂了……

<img src="https://upload-images.jianshu.io/upload_images/18750334-2c491a849f2002c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

## 继承

继承就是一个函数的原型继承另一个函数的原型，可以这么写：

```javascript
function User(){};

function Admin(){};
Admin.prototype = User.prototype;   // Admin继承User
```

这样做有一个坏处，当你给Admin的原型添加方法时，会改变所有继承User原型的构造函数或者对象。比如如下代码：

```javascript
Admin.prototype.call = function(){
    console.log("hello world!")
}
let a = new Admin();
a.call();               // 输出 hello world!
let b = new User();
b.call();               // 输出 hello world!
```

如果想修改Admin的原型的同时不修改所有继承User.prototype的构造函数和对象怎么办呢？可以采用如下方法继承：

```javascript
function User(){};

function Admin(){};
Admin.prototype = Object.create(User.prototype);  // 这么写原对象会缺失constructor属性，需要补上下一行代码：
Admin.prototype.constructor = Admin;
```

此时再进行调用：

```javascript
Admin.prototype.call = function(){
    console.log("hello world!");
}
let a = new User();
let b = new Admin();

a.call();              // 报错：a.call is not a function...
b.call();              // 输出 hello world!
```

### 方法重写

下面展示的是子类需要重写父类方法的技巧：

```javascript
function Person() {}
Person.prototype.getName = function() {
  console.log("parent method");
};

function User(name) {}
User.prototype = Object.create(Person.prototype);
User.prototype.constructor = User;

User.prototype.getName = function() {
  //调用父级同名方法
  Person.prototype.getName.call(this);
  console.log("child method");
};
let hd = new User();
hd.getName();
```

### 多态

什么是多态？可以将不同的子类当成父类来看待。

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

### defineProperty

使用Object.defineProperty定义来禁止遍历constructor属性：

```javascript
function User(){};
function Admin(name){
    this.name = name;
}

Admin.prototype = Object.create(User.prototype);

Object.defineProperty(Admin.prototype, "constructor", {
    value: Admin,
    enumerable: false // 禁止遍历
})

let hd = new Admin("后盾人");
for(const key in hd){
    console.log(key);
}
```

### assign

`Object.assign`方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

```javascript
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

其他相关内容详情请参考：https://www.jianshu.com/p/d5f572dd3776

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