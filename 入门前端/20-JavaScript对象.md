## 基本声明

对象表示一组无序的数据，描述一个具体的事物。

下列是介绍对象的两种声明方式：

```javascript
let hd = {};
let houdunren = new Object();
console.log(hd, houdunren);          // {} {}
console.log(hd.constructor);         // Object() { [native code] }
console.log(houdunren.constructor);  // Object() { [native code] }
```

### 引用特性

对象和函数、数组一样是引用类型，即复制只会复制引用地址。

```javascript
let hd = { name: "后盾人" };
let cms = hd;
cms.name = "hdcms";
console.log(hd.name); //hdcms
```

对象做为函数参数使用时也不会产生完全赋值，内外共用一个对象

```javascript
let user = { age: 22 };
function hd(user) {
  user.age += 10;
}
hd(user);
console.log(user.age); //32
```

对多的比较是对内存地址的比较所以使用 `==` 或 `===` 一样

```javascript
let hd = {};
let xj = hd;
let cms = {};
console.log(hd == xj); //true
console.log(hd === xj); //true
console.log(hd === cms); //false
```

### 展开语法

使用`...`可以展示对象的结构，下面是实现对象合并的示例

```JavaScript
let hd = { name: "后盾人", web: "houdurnen.com" };
let info = { ...hd, site: "hdcms" };
console.log(info);
```

## 对象转换

### 基础知识

对象直接参与计算时，系统会根据计算的场景在 `string/number/default` 间转换。

- 如果声明需要字符串类型，调用顺序为 `toString > valueOf`
- 如果场景需要数值类型，调用顺序为 `valueOf > toString`
- 声明不确定时使用 `default` ，大部分对象的 `default` 会当数值使用

下面的数值对象会在数学运算时转换为 `number`

```javascript
let houdunren = new Number(1);
console.log(houdunren + 3); //4
```

如果参数字符串运长时会转换为 `string`

```javascript
let houdunren = new Number(1);
console.log(houdunren + "3"); //13
```

下面当不确定转换声明时使用 `default` ，大部分`default`转换使用 `number` 转换。

```javascript
let houdunren = new Number(1);
console.log(houdunren == "1"); //true
```

### Symbol.toPrimitive

内部自定义`Symbol.toPrimitive`方法用来处理所有的转换场景

```javascript
let hd = {
  num: 1,
  [Symbol.toPrimitive]: function() {
    return this.num;
  }
};
console.log(hd + 3); //4
```

### valueOf/toString

可以自定义`valueOf` 与 `toString` 方法用来转换，转换并不限制返回类型。

```javascript
let hd = {
  name: "后盾人",
  num: 1,
  valueOf: function() {
    console.log("valueOf");
    return this.num;
  },
  toString: function() {
    console.log("toString");
    return this.name;
  }
};
console.log(hd + 3); //valueOf 4
console.log(`${hd}向军`); //toString 后盾人向军
```

## 对象结构赋值

### 基本使用

下面是基本使用语法

```javascript
//对象使用
let info = {name:'后盾人',url:'houdunren.com'};
let {name:n,url:u} = info
console.log(n); // 后盾人

//如果属性名与变量相同可以省略属性定义
let {name,url} = {name:'后盾人',url:'houdunren.com'};
console.log(name); // 后盾人
```

### 函数参数

数组参数的使用

```javascript
function hd([a, b]) {
	console.log(a, b);
}
hd(['后盾人', 'hdcms']);
```

对象参数使用方法

```javascript
function hd({name,url,user='向军大叔'}) {
	console.log(name,url,user);
}
hd({name:'后盾人','url':'houdunren.com'}); //后盾人 houdunren.com 向军大叔
```

对象解构传参

```javascript
function user(name, { sex, age } = {}) {
  console.log(name, sex, age); //向军大叔 男 18
}
user("向军大叔", { sex: "男", age: 18 });
```

### 嵌套结构

使用方法如下：

```javascript
const hd = {
  name:'后盾人',
  lessons:{
    title:'JS'
  }
}
const {name,lessons:{title}}  = hd;
console.log(name,title); //后盾人 JS
```

###  默认值

为变量设置默认值

```text
let [name, site = 'hdcms'] = ['后盾人'];
console.log(site); //hdcms

let {name,url,user='向军大叔'}= {name:'后盾人',url:'houdunren.com'};
console.log(name,user);//向军大叔
```

## 属性管理

### 添加属性

可以为对象添加属性

```javascript
let obj = {name: "后盾人"};
obj.site = "houdunren.com";
console.log(obj);
```

### 删除属性

使用`delete` 可以删除属性（后面介绍的属性特性章节可以保护属性不被删除）

```javascript
let obj = { name: "后盾人" };
delete obj.name;
console.log(obj.name); //undefined
```

### 检测属性

`hasOwnProperty`检测对象自身是否包含指定的属性，不检测原型链上继承的属性。

```javascript
let obj = { name: '后盾人'};
console.log(obj.hasOwnProperty('name')); //true
```

使用 `in` 可以在原型对象上检测

```javascript
let obj = {name: "后盾人"};
let hd = {
  web: "houdunren.com"
};

//设置hd为obj的新原型
Object.setPrototypeOf(obj, hd);
console.log(obj);

console.log("web" in obj); //true
console.log(obj.hasOwnProperty("web")); //false
```

###  assign

以往我们使用类似`jQuery.extend` 等方法设置属性，现在可以使用 `Object.assign` 静态方法

从一个或多个对象复制属性

```javascript
"use strict";
let hd = { a: 1, b: 2 };
hd = Object.assign(hd, { f: 1 }, { m: 9 });
console.log(hd); //{a: 1, b: 2, f: 1, m: 9}
```

## 遍历对象

### 获取内容

使用系统提供的API可以方便获取对象属性与值

```javascript
const hd = {
  name: "后盾人",
  age: 10
};
console.log(Object.keys(hd)); //["name", "age"]
console.log(Object.values(hd)); //["后盾人", 10]
console.table(Object.entries(hd)); //[["name","后盾人"],["age",10]]
```

### for/in

使用`for/in`遍历对象属性

```javascript
const hd = {
  name: "后盾人",
  age: 10
};
for (let key in hd) {
  console.log(key, hd[key]);
}
```

### for/of

`for/of`用于遍历迭代对象，不能直接操作对象。但`Object`对象的`keys/`方法返回的是迭代对象。

```javascript
const hd = {
  name: "后盾人",
  age: 10
};
for (const key of Object.keys(hd)) {
  console.log(key);
}
```

获取所有对象属性

```javascript
const hd = {
  name: "后盾人",
  age: 10
};
for (const key of Object.values(hd)) {
  console.log(key);
}
```

同时获取属性名与值

```javascript
for (const array of Object.entries(hd)) {
  console.log(array);
}
```

使用扩展语法同时获取属性名与值

```javascript
for (const [key, value] of Object.entries(hd)) {
  console.log(key, value);
}
```

添加元素DOM练习

```javascript
let lessons = [
  { name: "js", click: 23 },
  { name: "node", click: 192 }
];
let ul = document.createElement("ul");
for (const val of lessons) {
  let li = document.createElement("li");
  li.innerHTML = `课程:${val.name},点击数:${val.click}`;
  ul.appendChild(li);
}
document.body.appendChild(ul);
```

## 浅拷贝与深拷贝

这个拷贝方法有多种，目前我只一样写一种。

### 浅拷贝

```javascript
let a = {
  name : "catfish",
  gender : "man",
}
let b = a;
b.name = "catfish1921";
console.log(a);         // 输出 {name: "catfish1921", gender: "man"}
console.log(b);         // 输出 {name: "catfish1921", gender: "man"}
```

### 深拷贝

```javascript
let a = {
  name : "catfish",
  gender : "man",
}
let b = {...a}
b.name = "catfish1921";
console.log(a);         // 输出 {name: "catfish", gender: "man"}
console.log(b);         // 输出 {name: "catfish1921", gender: "man"}
```

## 内置对象

### Math对象

```javascript
Math.PI         // 3.141592653589793
Math.random()   // 介于0到1之间的随机数
Math.ceil(6.6)     // 进一法
Math.floor(8.8)    // 去尾法
Math.round(9.9)    // 四舍五入
Math.max(10, 20, 15)  // 取最大值
Math.min(10, 20, 15)  // 取最小值
Math.pow(10,2)       // 10的二次幂
Math.sqrt(100)       // 100的平方根
```

### Date对象

```javascript
let now = new Date()
console.log(now.getTime()); // 获取距离1970年1月1日起的毫秒数
console.log(now.valueOf())  // 同上
let time = new Date(2015, 4, 1)  // 月份从0开始计数（传年月日）
now.getTime()
now.getSeconds()
now.getHours()
now.getDay()
now.getDate()
now.getMonth()
now.getFullYear()
```

实例：返回"2020-09-27 20:50:04"

```javascript
function time(){
    let now = new Date();
    let year = now.getFullYear()
    let month = now.getMonth() + 1;
    let date = now.getDate();
    let hours = now.getHours();
    let minutes = now.getMinutes();
    let seconds = now.getSeconds();
    
    month = month < 10 ? "0" + month : month;
    hours = hours < 10 ? "0" + hours : hours;
    minutes = minutes < 10 ? "0" + minutes : minutes;
    seconds = seconds < 10 ? "0" + seconds : seconds;
    
    return "" + year + "-" + month + "-" + date + " " + hours + ":" + minutes + ":" + seconds;
}
```

