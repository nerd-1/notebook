## 原型链之间的关系

一开始原型链啥也不懂，看了这个图就懂了……

<img src="https://upload-images.jianshu.io/upload_images/18750334-2c491a849f2002c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width="100%">

## 相关方法

### 获得一个函数的原型对象

```javascript
Object.getPrototypeOf(hd);  // 等效于 hd.__proto__
```

### 创建一个没有原型对象的字典

```javascript
Object.create(null, {})
```

### 设置自定义原型对象

使用方法如下：

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

### 给原型对象设置多个方法

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

