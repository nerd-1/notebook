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
let f = function(a, b){
    return a + b;
}
```

还可以使用点语法来定义多个参数：

```javascript
function sum(...args) {
  return args.reduce((a, b) => a + b);
}
console.log(sum(1, 2, 3));
```

### 匿名函数

写法实例如下：

```javascript
(function(a, b){
    return a+b
})(1, 2);
```

### 定义函数时的注意点

如果是以下面的方式定义一个函数：

```javascript
function hello() {
  console.log("catifsh");
}
console.log(hello()); // 输出 catfish
```

然后，如果这样定义会出现一个问题：函数hello会出现在全局window中：

```javascript
window.hello()    // 输出 catfish
```

如果取的函数名与全局window的内置函数相同时，会覆盖全局window的相关函数，此时需要使用let来定义一个函数：

```javascript
let hello = function(){
    console.log("catfish");
}
```

此时定义的函数将不会出现在全局window中。

### 箭头函数

箭头函数是函数声明的简写形式，在使用递归调用、构造函数、事件处理器时不建议使用箭头函数。

使用用法如下：

```javascript
let sum = () => {
	return 1 + 3;
}
console.log(sum()); //4
```

函数体为单一表达式时不需要 `return` 返回处理，系统会自动返回表达式计算结果。

```javascript
let sum = () => 1 + 3;
console.log(sum()); //4
```

## 函数argument对象

argument对象表示函数的是实参集合，仅能够在函数体内可见，并可以直接访问。

argument有一个属性教callee，这个属性调用后返回arguments对象所在的函数。可以这样使用来判断形参和实参是否相同：

```javascript
function checkArg(){
    if(arguments.length != arguments.callee.length){
        throw new Error("实参和形参不一致")
    }
}
```

同时当传入的形参和实参不相同时：

- 形参数量大于实参时，没有传参的形参值为 undefined
- 实参数量大于形参时，多于的实参将忽略并不会报错

## 函数解析字面量

函数可以直接解析字面量。第一个参数是字符串值的数组，其余的参数为标签变量。使用过程如下：

```javascript
function hd(str, ...values) {
  console.log(str); //["站点", "-", "", raw: Array(3)]
  console.log(values); //["后盾人", "houdunren.com"]
}
let name = '后盾人',url = 'houdunren.com';
hd `站点${name}-${url}`;
```

## this

全局环境下`this`就是window对象的引用。使用严格模式时在全局函数内`this`为`undefined`。

### 构造函数

函数当被 `new` 时即为构造函数，一般构造函数中包含属性与方法。函数中的上下文指向到实例对象。构造函数主要用来生成对象，里面的this默认就是当前对象。使用代码如下：

```javascript
function User() {
  this.name = "后盾人";
  this.say = function() {
    console.log(this); //User {name: "后盾人", say: ƒ}
    return this.name;
  };
}
let hd = new User();
console.log(hd.say()); //后盾人
```

### 对象中使用this

- 下例中的hd函数不属于对象方法所以指向`window`
- show属于对象方法执向 `obj`**对象**

```javascript
let obj = {
  site: "后盾人",
  show() {
    console.log(this.site); //后盾人
    console.log(`this in show method: ${this}`); //this in show method: [object Object]
    function hd() {
      console.log(typeof this.site); //undefined
      console.log(`this in hd function: ${this}`); //this in hd function: [object Window]
    }
    hd();
  }
};
obj.show();
```

### 箭头函数中的this

箭头函数没有`this`, 也可以理解为箭头函数中的`this` 会继承定义函数时的上下文，可以理解为和外层函数指向同一个this。对于上面的例子，如果我们想将hd函数中的this指向该对象，这时候，我们就可以使用箭头函数：

```javascript
let obj = {
  site: "后盾人",
  show() {
    console.log(this.site); //后盾人
    console.log(`this in show method: ${this}`); //this in show method: [object Object]
    let hd = ()=>{
      console.log(typeof this.site); //undefined
      console.log(`this in hd function: ${this}`); //this in hd function: [object Object]
    }
    hd();
  }
};
obj.show();
```

这时候它的显示就是当前对象，而不是全局对象window。

### DOM中的this

DOM中使用this会指向当前节点对象，使用方法如下：

```html
<body>
  <button desc="hdcms">button</button>
</body>
<script>
  let Dom = {
    site: "后盾人",
    bind() {
      const button = document.querySelector("button");
      button.addEventListener("click", function() {
        alert(this.getAttribute("desc"));
      });
    }
  };
  Dom.bind();
</script>
```

此时this代表的就是button节点。如果函数使用箭头函数的话，那么这个this就是最外层的Dom对象，你可以自己尝试下。

### this与call/apply连用绑定对象

call与apply 用于显示的设置函数的上下文，两个方法作用一样都是将对象绑定到this，只是在传递参数上有所不同。

- apply 用数组传参
- call 需要分别传参

语法使用介绍

```javascript
function show(title) {
    alert(`${title+this.name}`);
}
let lisi = {
    name: '李四'
};
let wangwu = {
    name: '王五'
};
show.call(lisi, '后盾人');
show.apply(wangwu, ['HDCMS']);
```

这两个方法可以这么理解，就是相当于给函数加了个this对象，并且调用了一次。

使用 `call` 设置函数上下文：

```html
<body>
    <button message="后盾人">button</button>
    <button message="hdcms">button</button>
</body>
<script>
    function show() {
        alert(this.getAttribute('message'));
    }
    let bts = document.getElementsByTagName('button');
    for (let i = 0; i < bts.length; i++) {
        bts[i].addEventListener('click', () => show.call(bts[i]));
    }
</script>
```

找数组中的数值最大值

```javascript
let arr = [1, 3, 2, 8];
console.log(Math.max(arr)); //NaN
console.log(Math.max.apply(Math, arr)); //8
 console.log(Math.max(...arr)); //8
```

### this中bind的使用

- 与 call/apply 不同bind不会立即执行
- bind 是复制函数形为会返回新函数

bind第一个参数是传入对象，第二个参数及以后的参数就是函数传入的参数，这个与call相似。

使用例子如下：

```javascript
function hd(a, b) {
  return this.f + a + b;
}

//使用bind会生成新函数
let newFunc = hd.bind({ f: 1 }, 3);

//1+3+2 参数2赋值给b即 a=3,b=2
console.log(newFunc(2));
```

使用实例如下：

```html
<style>
  * {
    padding: 0;
    margin: 0;
  }

  body {
    width: 100vw;
    height: 100vh;
    font-size: 3em;
    padding: 30px;
    transition: 2s;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #34495e;
    color: #34495e;
  }
</style>
<body>
  houdunren.com
</body>
<script>
  function Color(elem) {
    this.elem = elem;
    this.colors = ["#74b9ff", "#ffeaa7", "#fab1a0", "#fd79a8"];
    this.run = function() {
      setInterval(
        function() {
          let pos = Math.floor(Math.random() * this.colors.length);
          this.elem.style.background = this.colors[pos];
        }.bind(this),
        1000
      );
    };
  }
  let obj = new Color(document.body);
  obj.run();
</script>
```

