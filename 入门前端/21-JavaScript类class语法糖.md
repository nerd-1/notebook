为了和其他语言继承形态一致，JS提供了`class` 关键词用于模拟传统的`class` ，但底层实现机制依然是原型继承。

`class` 只是语法糖为了让类的声明与继承更加简洁清晰。

## 基本

### 定义

定义方式如下：

```js
class User{}
console.log(new User());
```

类方法间不需要加逗号：

```js
class User {
  show() {}
  get() {
    console.log("get method");
  }
}
const hd = new User();
hd.get();
```

### 构造函数

使用 `constructor` 构造函数传递参数，下例中`show`为构造函数方法，`getName`为原型方法：

```js
class User {
  constructor(name) {
    this.name = name;
    this.show = function() {};
  }
  getName() {
    return this.name;
  }
}
const xj = new User("向军大叔");
console.log(xj);
```

## 静态属性

### 静态属性

静态属性即为类设置属性，而不是为生成的对象设置，下面是原理实现

```js
function User(){}
User.username = "catfish1921";
console.log(User.username);   // 输出catfish1921
```

在 `class` 中为属性添加 `static` 关键字即声明为静态属性

可以把为所有对象使用的值定义为静态属性：

```js
class User{
    static username = "catfish1921";
}
console.log(User.username);   // 输出catfish1921
```

### 静态方法

指通过类访问不能使用对象访问的方法。下列是静态方法实现过程：

```js
function User(){}
User.username = function(){
    return "catfish1921";
}
console.log(User.username());   // 输出catfish1921
```

在 `class` 内声明的方法前使用 `static` 定义的方法即是静态方法：

```js
class User{
    static username = function(){
        return "catfish1921";
    };
}
console.log(User.username());   // 输出catfish1921
```

## 访问器get/set

使用访问器可以对对象的属性进行访问控制，下面是使用访问器对私有属性进行管理。

使用访问器可以管控属性，有效的防止属性随意修改。

访问器就是在函数前加上 `get/set`修饰，操作属性时不需要加函数的扩号，直接用函数名。使用如下：

```js
class User{
    constructor(name){
        this._name = name;
    }
    get name(){
        return this._name;
    }
    set name(value){
        if(value.trim() == "") throw new Error("invalid params");
        this._name = value;
    }
}
let obj = new User("catfish1921");
console.log(obj.name);             // 输出catfish1921
obj.name = "catfish";
console.log(obj.name);              // 输出 catfish
obj.name = "";                     // Error:invalid params
```

## 详解继承

### 属性继承

属性继承需要用到super方法。使用代码如下：

```js
class User {
  constructor(name) {
    this.name = name;
  }
}
class Admin extends User {
  constructor(name) {
    super(name);
  }
}
let hd = new Admin("后盾人");
console.log(hd);
```

### 方法继承

使用代码如下：

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  show() {
    return `后盾人会员: ${this.name}`;
  }
}
class User extends Person {
  constructor(name) {
    super(name);
  }
  run() {
    return super.show();     // 方法继承
  }
}
const xj = new User("向军");
console.dir(xj.run());
```

### 方法覆盖

子类存在父类同名方法时使用子类方法。

