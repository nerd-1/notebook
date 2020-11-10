## Symbol场景介绍

Symbol用于防止属性名冲突而产生的，比如向第三方对象中添加属性时。

Symbol 的值是唯一的，独一无二的不会重复的。

可以这么理解：Symbol将生成一个永远不会重复的数字或字符串。

## 声明定义Symbol的几种方式

定义方式如下：

```javascript
let a = Symbol();
```

不要把它当成对象给给它赋值属性，只用把它当作一个特殊的字符串即可。

如果要区分多个Symbol，可以给它添加描述：

```javascript
let a = Symbol("hello");
let b = Symbol("catfish");
cosole.log(a.description);     // 输出hello
```

### Symbol.for

如果需要一个Symbol重复使用，可以使用Symbol.for。使用例子如下：

```javascript
let a = Symbol.for("catfish");
let b = Symbol.for("catfish");
console.log(a === b);    // 输出true
```

当然，如果不采用Symbol.for，而直接使用Symbol，会返回false。例子如下：

```javascript
let a = Symbol("catfish");
let b = Symbol("catfish");
console.log(a === b);     // 输出false
```

### Symbol.keyFor

Symbol.keyFor可以用来用来获取Symbol.for里面的描述。

```javascript
let hd = Symbol.for("后盾人");
console.log(Symbol.keyFor(hd)); //后盾人

let edu = Symbol("houdunren");
console.log(Symbol.keyFor(edu)); //undefined
```

## 实战：使用Symbol解决字符串耦合问题

试想一下这种情况：我们要记录一个班的成绩，这个班有两个叫李四的，会出现如下情况：

```javascript
let stu1 = "李四";
let stu2 = "李四";
let all_student = {
    [stu1] : {js: 100, css:50},
    [stu2] : {js: 40, css:70},
}
console.log(all_student)
```

输出结果如下：

```shell
{李四: {…}}
	李四: {js: 40, css: 70}
	__proto__: Object
```

只有一个李四，第一个李四被覆盖掉了，如何解决这个问题呢？就用到了Symbol了。我们把上面的代码改为如下：

```javascript
let stu1 = {
    name : "李四",
    key : Symbol(),
};
let stu2 = {
	name ; "李四",
    key : Symbol(),
};
let all_student = {
    [stu1.key] : {js: 100, css:50},
    [stu2,key] : {js: 40, css:70},
}
console.log(all_student. all_student[stu1.key]);
```

