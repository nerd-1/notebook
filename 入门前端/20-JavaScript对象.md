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

## 构建函数

### 工厂函数

在函数中返回对象的函数称为工厂函数，工厂函数有以下优点

- 减少重复创建相同类型对象的代码
- 修改工厂函数的方法影响所有同类对象

### 构造函数

和工厂函数相似构造函数也用于创建对象，它的上下文为新的对象实例。使用代码如下：

```javascript
function Student(name){
    this.name = name;
    this.show = function(){
        console.log(this.name);
    }
    // 不需要返回，系统自动返回
    // return this
    // 如果构造函数返回的是对象，那么实例化返回的就是对象
}
const handsome_man = new Student("catfish");
handsome_man.show();
```

### 内置的构造函数

JS中大部分数据类型都是通过构造函数创建的。

```javascript
const num = new Number(99);
console.log(num.valueOf());

const string = new String("后盾人");
console.log(string.valueOf());

const boolean = new Boolean(true);
console.log(boolean.valueOf());

const date = new Date();
console.log(date.valueOf() * 1);

const regexp = new RegExp("\\d+");
console.log(regexp.test(99));

let hd = new Object();
hd.name = "后盾人";
console.log(hd);
```

## 属性特征

### 查看特征

使用 `Object.getOwnPropertyDescriptor`查看对象属性的描述。

```javascript
"use strict";
const user = {
  name: "向军",
  age: 18
};
let desc = Object.getOwnPropertyDescriptor(user, "name");
console.log(JSON.stringify(desc, null, 2));            // 序列化JSON对象
```

使用 `Object.getOwnPropertyDescriptors`查看对象所有属性的描述

```javascript
"use strict";
const user = {
  name: "向军",
  age: 18
};
let desc = Object.getOwnPropertyDescriptors(user);
console.log(JSON.stringify(desc, null, 2));
```

属性包括以下四种特性

| 特性         | 说明                                                   | 默认值    |
| ------------ | ------------------------------------------------------ | --------- |
| configurable | 能否使用delete、能否需改属性特性、或能否修改访问器属性 | true      |
| enumerable   | 对象属性是否可通过for-in循环，或Object.keys() 读取     | true      |
| writable     | 对象属性是否可修改                                     | true      |
| value        | 对象属性的默认值                                       | undefined |

### 设置特征

使用`Object.defineProperty` 方法修改属性特性，通过下面的设置属性name将不能被遍历、删除、修改。

```javascript
"use strict";
const user = {
  name: "向军"
};
Object.defineProperty(user, "name", {
  value: "后盾人",
  writable: false,
  enumerable: false,
  configurable: false
});
```

通过执行以下代码对上面配置进行测试，请分别打开注释进行测试

```javascript
// 不允许修改
// user.name = "向军"; //Error

// 不能遍历
// console.log(Object.keys(user));

//不允许删除
// delete user.name;
// console.log(user);

//不允许配置
// Object.defineProperty(user, "name", {
//   value: "后盾人",
//   writable: true,
//   enumerable: false,
//   configurable: false
// });
```

使用 `Object.defineProperties` 可以一次设置多个属性，具体参数和上面介绍的一样。

```javascript
"use strict";
let user = {};
Object.defineProperties(user, {
  name: { value: "向军", writable: false },
  age: { value: 18 }
});
console.log(user);
user.name = "后盾人"; //TypeError
```

### 禁止添加

`Object.preventExtensions` 禁止向对象添加属性

```javascript
"use strict";
const user = {
  name: "向军"
};
Object.preventExtensions(user);
user.age = 18; //Error
```

`Object.isExtensible` 判断是否能向对象中添加属性

```javascript
"use strict";
const user = {
  name: "向军"
};
Object.preventExtensions(user);
console.log(Object.isExtensible(user)); //false
```

### 封闭对象

`Object.seal()`方法封闭一个对象，阻止添加新属性并将所有现有属性标记为 `configurable: false`

```javascript
"use strict";
const user = {
  name: "后盾人",
  age: 18
};

Object.seal(user);
console.log(
  JSON.stringify(Object.getOwnPropertyDescriptors(user), null, 2)
);

Object.seal(user);
console.log(Object.isSealed(user));
delete user.name; //Error
```

`Object.isSealed` 如果对象是密封的则返回 `true`，属性都具有 `configurable: false`。

```javascript
"use strict";
const user = {
  name: "向军"
};
Object.seal(user);
console.log(Object.isSealed(user)); //true
```

### 冻结对象

`Object.freeze` 冻结对象后不允许添加、删除、修改属性，writable、configurable都标记为`false`

```javascript
"use strict";
const user = {
  name: "向军"
};
Object.freeze(user);
user.name = "后盾人"; //Error
```

`Object.isFrozen()`方法判断一个对象是否被冻结

```javascript
"use strict";
const user = {
  name: "向军"
};
Object.freeze(user);
console.log(Object.isFrozen(user));
```

## 属性访问器

getter方法用于获得属性值，setter方法用于设置属性，这是JS提供的存取器特性即使用函数来管理属性。

主要用于get和set，使用方法如下：

```javascript
const web = {
  name: "catfish",
  url: "catfish1921.com",
  get site() {
    return `${this.name} ${this.url}`;
  },
  set site(value) {
    [this.name, this.url] = value.split(",");
  }
};
web.site = "后盾人,hdcms.com";
console.log(web.site);           // 后盾人 hdcms.com
```

使用语法糖class：

```javascript
"use strict";
const DATA = Symbol();
class User {
  constructor(name, age) {
    this[DATA] = { name, age };
  }
  get name() {
    return this[DATA].name;
  }
  set name(value) {
    if (value.trim() == "") throw new Error("无效的用户名");
    this[DATA].name = value;
  }
  get age() {
    return this[DATA].name;
  }
  set age(value) {
    if (value.trim() == "") throw new Error("无效的用户名");
    this[DATA].name = value;
  }
}
let hd = new User("后盾人", 33);
console.log(hd.name);
hd.name = "向军1";
console.log(hd.name);
console.log(hd);
```

## 代理拦截

代理（拦截器）是对象的访问控制，`setter/getter` 是对单个对象属性的控制，而代理是对整个对象的控制。

- 读写属性时代码更简洁
- 对象的多个属性控制统一交给代理完成
- 严格模式下 `set` 必须返回布尔值

### 使用方法

obj表示该对象，property表示该对象的属性，value指的是设置的值。

```javascript
"use strict";
const hd = { name: "后盾人" };
const proxy = new Proxy(hd, {
  get(obj, property) {
    return obj[property];
  },
  set(obj, property, value) {
    obj[property] = value;
    return true;
  }
});
proxy.age = 10;
console.log(hd);
```

### 代理函数

如果代理以函数方式执行时，会执行代理中定义 `apply` 方法。

- 参数说明：函数，上下文对象，参数

下面使用 `apply` 计算函数执行时间

```javascript
function factorial(num) {
  return num == 1 ? 1 : num * factorial(num - 1);
}
let proxy = new Proxy(factorial, {
  apply(func, obj, args) {
    console.time("run");
    func.apply(obj, args);
    console.timeEnd("run");
  }
});
proxy.apply(this, [1, 2, 3]);
```

## JSON

### 序列化

第一个参数是序列化的对象，第二个参数是指定保存的属性，如果为null则保存所有属性。第三个参数可选，文本添加缩进、空格和换行符，如果 第三个参数是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果 space 大于 10，则文本缩进 10 个空格。

```javascript
let hd = {
  "title": "后盾人",
  "url": "houdunren.com",
  "teacher": {
  	"name": "向军大叔",
  }
}
console.log(JSON.stringify(hd, null, 4));
```

### 反序列化

```javascript
let hd = {
  "title": "后盾人",
  "url": "houdunren.com",
  "teacher": {
  	"name": "向军大叔",
  }
}
let json = JSON.stringify(hd, null, 4);
let str = JSON.parse(json);
console.log(str);
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

