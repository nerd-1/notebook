## 函数定义的方式

函数有三种定义方式，第一种：

```javascript
let f = new Function("a", "b", "return a+b");
```

这种不常用，一般常用的是如下两种方式：

```javascript
function f(a, b){
    return a + b;
}
```

还有一种赋值的方式定义函数：

```javascript
f = function(a, b){
    return a + b;
}
```

### 匿名函数

写法实例如下：

```javascript
(function(a, b){
    return a+b
})(1, 2);
```

## 函数的call和apply调用

使用规则如下：

```
function.call(thisobj, args...)
function.apply(thisobj, [args...])
```

参数thisobj表示绑定的对象，而args表示要传递的参数。call可以接受多个参数列表，而apply只能接受一个数组或者伪类数组，使用实例如下：

```javascript
// call
function f(x, y){
    return x + y;
}
f.call(null, 1, 2)         // 返回3
// apply
function f(x, y){
    return x + y;
}
f.call(null, [1, 2])         // 返回3
```

## 函数argument对象

argument对象表示函数的是实参集合，仅能够在函数体内可见，并可以直接访问。argument有一个属性教callee，这个属性调用后返回arguments对象所在的函数。可以这样使用来判断形参和实参是否相同：

```javascript
function checkArg(){
    if(arguments.length != arguments.callee.length){
        throw new Error("实参和形参不一致")
    }
}
```

## 对象

一类事物中的一个具体的某一个东东

```javascript
// 对象：一组无序的数据，描述一个具体的事物
let obj = {"name":"zhangsan", "age":"30"};
// 可以通过对象.属性的方法活区到对象的属性
console.log(obj.name, obj.age);
// 定义行为或者方法
let obj = {
    "age" : 30,
    "name" : "zhangsan",
    "say" : function(word){
        console.log("hello "+word);
    }
};
// 其中say对应一个值，是一个函数，调用它的方法：
obj.say("world!");
// 设置属性：
obj.age = 40;
// [] 语法
console.log(obj['age']);
// 删除对象的成员
delete obj.age;
```

### this的使用

```javascript
// new Object(); 实例化内置的对象
let obj1 = new Object();
console.log(obj1);         // 得到一个空对象

// 自定义构造函数，构造函数名称，通常首字母大写
function Fn(){
    this.age = 40;
    this.say = function(){
        return 'hello';
    }
}
// 构造函数被实例化时内部函数被自动执行。
let obj2 = new Fn();    // 实例化构造函数fn，得到对象
console.log(obj2, typeof obj2);

// 普通函数中使用this指向一个window对象
function f1(){
    console.log(this);
}
f1();
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

