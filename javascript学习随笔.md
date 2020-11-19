## javascript 学习随笔

这个阶段我想看三个视频来学习，嗯，一定要学好！

- 反复是学习之母
- 看视频+手敲练习一遍+做笔记

#### 资源

[后盾人](https://www.bilibili.com/video/BV1NJ411W7wh?from=search&seid=4960592685905598372)

[李南江](https://www.bilibili.com/video/BV1rt4y1Q7wo?from=search&seid=1462485058520994896)

[尚硅谷](https://www.bilibili.com/video/BV1xt41137Ak?from=search&seid=6377666792625428594)

[前端路线图](https://www.bilibili.com/read/cv5650633/)

## Dom对象

浏览器提供一套操作浏览器功能和页面元素的接口（DOM, BOM）

常用的dom操作：

1. 查找界面标签元素
2. 标签增加，修改，删除操作
3. 标签的属性相关操作
4. 给标签元素绑定事件

### 查找节点

```javascript
// 通过id属性值查找
var btn = document.getElementById("div1");
console.log(btn);   // 如果id有重复的只获取第一个

// 通过id名称查找
var divs = document.getElementsByTagName("div");
console.log(divs);   // 返回一个类数组对象

// 通过类名获取
var divs = document.getElementsByClassName("div100");
console.log(divs);   // 返回一个类数组对象

// 通过name属性值获取
var inputs = document.getElementsByName("sex");
console.log(inputs);

// H5提供的方法：根据css选择器获取元素
// document.querySelector()  document.querySelectorAll()
var divs = document.querySelectorAll("div");   // 获取类数组
var div = document.querySelector("div");   // 获取第一个
```

### 事件绑定

```html
<input type="button" name="" onclick="f();">
<input type="button" id="btn" value="js中绑定">
<input type="button" id="btn2" value="事件监听方式">
<!-- 行内绑定事件，如果多次绑定，执行第一个 -->
<script>
	function f(){
    	alert(123);
    	console.log(456);
    }
</script>

<!-- 在js内进行绑定，如果多次绑定，执行第二个 -->
<script>
	document.getElementById("btn").onclick = function(){
        alert("js中通过标签的事件属性");
    }
</script>
<!-- 事件监听，如果多次绑定，依次执行 -->
<script>
document.getElementById('btn2').addEventListener('click', function(){
	alert("事件监听的方式");
})
</script>
```

总结：行内方式不推荐。（`js`和`html`混合在一次，不方便后期的维护）`js`中事件属性方式绑定，自己写简单效果可以使用。事件监听的方式，复杂的`html`页面，推荐使用，可以多次绑定。

事件监听使用的事件的用法：

```javascript
document.getElementById("myBtn").addEventListener("mouseover", myFunction);
document.getElementById("myBtn").addEventListener("click", someOtherFunction);
document.getElementById("myBtn").addEventListener("mouseout", someOtherFunction);
```

### 事件移除

```javascript
// 行内绑定和js事件绑定的直接重新设置值为空字符串即可。
let f = function(){
	alert("事件监听的方式");
}
document.getElementById('btn2').addEventListener('click', f);
document.getElementById('btn2').removeEventListener('click', f);
```

### 事件类型

https://www.w3school.com.cn/jsref/dom_obj_event.asp

### 事件传播：冒泡和捕获

事件传播是一种定义当发生事件时元素次序的方法。假如 <div> 元素内有一个 <p>，然后用户点击了这个 <p> 元素，应该首先处理哪个元素“click”事件？

在冒泡中，最**内侧元素**的事件会首先被处理，然后是更外侧的：首先处理 <p> 元素的点击事件，然后是 <div> 元素的点击事件。

在捕获中，最**外侧元素**的事件会首先被处理，然后是更内侧的：首先处理 <div> 元素的点击事件，然后是 <p> 元素的点击事件。

在 addEventListener() 方法中，你能够通过使用“useCapture”参数来规定传播类型：

```
addEventListener(event, function, useCapture);
```

默认值是 false，将使用冒泡传播，如果该值设置为 true，则事件使用捕获传播。

### 节点操作

```javascript
document.createElement("p");
documnet.createTextNode("这是一段文本");


element.innerHTML
element.innerText

element.attribute
element.setAttribute(attribute, value);
element.getAttribute(attribute);
element.hasAttribute(attribute);
element.removeAttribute(attribute);

element.style.property
element.nodeName;   // 只读属性


element.appendChild(new_element);
parent.insertBefore(new_element, parent.children[0]);   // 或者是parent.firstChild
element.removeChild(remove_element);
element.replaceChile(new_element, old_element);
element.hasChildNodes(element_2);   // 是否有子节点
```

### 事件对象

```javascript
document.getElementById("btn").onclick = function(e){
    // 兼容性问题，ie浏览器要用后者才能获取
    let evt = e || window.event;
    // 输出
    console.log(evt);
}

// 事件代理/事件委托
event.target;    // 获取触发的元素，可能是绑定标签的子元素，this只能是绑定标签

event.bubbles;    // 表示当前事件是否会冒泡

// 阻止默认行为
event.preventDefault();    // 普通浏览器，最新版ie浏览器也兼容

// 阻止事件冒泡（放到触发的元素中，写到里面）
event.stopPrpagation();    
```

### 兼容性写法

阻止默认行为：

```javascript
if(event.preventDefault){
    event.prevntDefault():
}else{
    event.returnValue = false;
}
```

阻止事件在DOM中继续传播：

```js
if(event.stopPrpagation){
    event.stopPrpagation():
}else
    event.cancelBubble = true;
}
```

## BOM

### 对话框

`window.alert` ，`window.confirm`，`window.prompt`

### 页面加载事件

```javascript
window.onload = function(){
    // ...
}
// 等同于：
window.addEventListener("load", function(){
    // ... 
})
```

### 浏览器控制台

`console.clear` ，`console.error`，`console.table`，`console.log`，`console.warn`

`console.time`，`console.timeEnd`使用方法如下：

```javascript
console.time("run");
let a = 1+1;
console.timeEnd("run");   // 输出 run: 0.0078125 ms
```

`console.dir`用来输出查看该对象的原型对象，使用方法如下：

```javascript
let a = {};
console.dir(a);
```

### location对象

```javascript
console.log(window.location);
location.href = "https://www.catfish1921.com";   // 使用js控制界面的跳转
location.reload();    // 刷新界面本身
location.reload(true);   // 强制刷新，页面中的静态文件也会刷新
```

### history对象

```javascript
history.back();   // 返回上一页
history.forward();   // 返回下一页
history.go();      // 上一页传-1，下一页传1
```

### navigator对象

```javascript
// 判断浏览器信息
navigator.userAgent;   // 请求头
navigator.platform;    // 平台
navigator.geolocation;  // 用不上
```

### 定时器

#### setTimeout和clearTimeout

```javascript
let timerId = window.setTimeout(function(){
    console.log("hello world!");
}, 1000);

window.clearTimeout(timerId);   // 定时器的一个编号
```

#### setInterval和clearInterval

```javascript
let timerId = window.setInterval(function(){
    console.log("hello world!");
}, 1000);

window.clearInterval(timerId);   // 定时器的一个编号
```

https://www.bilibili.com/video/BV1gt4y1D7T4?p=97

#### 写一个轮播图出来

- 自动切换图片：

  - 把第一张移动到最后一张

  ```javascript
  setInterval(function(){
      let li = document.querySelector(".img_li");
      let ul = document.querySelector("imgs");
      ul.append(li);
  })
  ```

  - 通过`css`控制图片显示和隐藏

  ```javascript
  let num = 0;
  setInterval(function(){
      num++;
      let img_lis = document.querySelectorAll(".img_li");
      for(let i=0, i<img_lis.length, i++){
          img_lis[i].style.display = "none";
      }
      if(num==img_lis.length){
          num = 0;
      }
      img_lis[num].style.display = "block";
  })
  ```

  - 移动一定的距离（子绝父相，自己实现）

  已实现，详情请查看：https://www.cnblogs.com/catfish1921/p/13760607.html

- 鼠标悬停切换

  ```javascript
  let dot_lis = document.querySelectorAll('.dot');
  for (let i=0; i<dot_lis.length; i++){
      (function(i){     // for循环绑定事件，事件中i丢失问题
          dot_lis[i].addEventLinstener("mouseover", function(){
              let img_lis = document.querySelectorAll(".img_li");
              for(let j=0; j<img_lis.length; j++){
                  img_lis[j].style.display = "none";
                  dot_lis[j].style.backgroundColor = "transparent";
              }
              this.style.backgroundColor = 'white';
              img_lis[i].style.display = "block";
          })
      })(i)
  }
  ```

- 综合效果

```javascript
function change(n){
    let img_lis = document.querySelectorAll(".img_li");
    let dot_lis = document.querySelectorAll(".dot");
    for(let i=0; i<img_lis.length; i++){
        img_lis[i].style.display = "none";
        dot_lis[i].style.backgroundColor = "transparent";
    }
    img_lis[n].style.display = 'block';
    dot_lis[n].style.backgroundColor = 'white';
}

let num = 0;
let timer;

function setTimer(){
    timer = setInterval(function(){
        let img_lis = document.querySelectorAll(".img_li");
        num ++;
        if(num == img_lis.length){
            num = 0;
        }
        change(num);
    }, 1000);
}

setTimer();

let dot_lis = document.querySelectorAll(".dot");
for(let i=0; i<dot_lis.length; i++){
    (function(i){
        dot_lis[i].addEventListener("mouseover", function(){
			clearInterval(timer);
            change(i);
            num = i;
    	})
        dot_lis[i].addEventListener("mouseout", function(){
            setTimer()
        });
    })(i);
}
```

## 其他

### 交换两个变量的值

第一种方式：创建变量

```javascript
let a = 1, b = 2;
let c = a;
a = b;
b = c;
```

第二种方式：加减运算

```javascript
let a = 1, b = 2;
a = a + b;
b = a - b;
a = a - b;
```

第三种方式：数组

```javascript
let a = 1, b = 2;
b = [a, a=b][0];
```

第四种方式：对象

```javascript
let a = 1, b = 2;
b = {"attr1": a, "attr2": a=b}.attr1;
```

## 对象

### 创建对象

创建对象的基本方法：

```javascript
function Person(n, a){
    this.name = n;
    this.age = a;
    this.say = function(){
        console.log("我叫"+this.name);
    }
}
let person = new Person("张三", 30);
```

构造函数特点：通常首字母大写。使用this设定成员。构造函数相当于类型的概念，会得到具体的对象。

但是构造函数也存在一定的问题，构造函数里面的方法，每次都要重新定义一个function，这样浪费内存。可以将构造函数中的方法单独放在外面定义，这样可以一定程度的减少内存的浪费：

```javascript
let fn = function(){
    console.log("我叫"+this.name);
}
function Person(n, a){
    this.name = n;
    this.age = a;
    this.say = fn;
}
let person = new Person("张三", 30);
```

这样依然存在函数命名过多会产生重复的问题。

### 原型对象的概念

js给每一个构造函数都自动分配了一个公共的对象，用来存放这个构造函数实例化的所有实例对象公共的成员，这个公共对象就是原型对象。

```javascript
function Person(n, a){
    this.name = n;
    this.age = a;
}
console.log(Person.prototype);
```

原型对象弥补了上面的问题，可以这样写：

```javascript
function Person(n, a){
    this.name = n;
    this.age = a;
}
Person.prototype.say = function(){
    cosole.log("我叫"+this.name);
}
```

这样既避免了内存问题又避免了函数命名问题。所以，一般采用构造函数和原型对象混合定义的方式来定义对象。

### 原型

构造函数和实例对象都可以获取到原型对象：

```javascript
Person.prototype
person1.__proto__
```

这两个值是相等的。

原型对象上有个属性constructor存的就是它的构造函数，所以，通过原型对象的constructor属性可以取到它的构造函数。

实例对象上也可以通过constructor属性来获得它的构造函数。

```javascript
Person.prototype.constructor  // Person构造函数对象
person1.constructor           // Person构造函数对象
```

这种写法就是替换了原来的原型对象：

```javascript
Person.prototype = {   // 覆盖了原型对象
	"say": function(){
		console.log("我是张三");
	},
}
Person.prototype.constructor   // 获得内置的Object对象
```

这样再取它的构造函数则是Object对象。

如果想避免这个问题，可以这样设置：

```javascript
Person.prototype = { 
    "constructor": Person,
	"say": function(){
		console.log("我是张三");
	},
}
```

所以，不建议直接替换对象。

## 原生对象的原型对象

String，Number，Object，Array，

### Object.prototype里面的方法

hasOwnProperty方法：判断该属性是自己的还是原型对象的方法，如果是自己的返回true，如果不是，返回false。

```javascript
function Person(){
    this.age = 40;
}
Person.prototype.say = function(){
    console.log("我今年" + this.age + "岁");
}
let obj = new Person();
console.log(obj.hasOwnProperty("age"));// true
console.log(obj.hasOwnProperty('say')); // false
```

isPrototypeOf方法：判断一个对象是不是另外一个对象的原型对象。

```javascript
Object.prototype.isPrototypeOf(obj); // true
```

getPrototypeOf方法：获取一个对象的原型对象。这个方法不是原型链中的方法，是Object函数本身的一种方法。构造函数本身的方法，通常叫做静态方法，只能使用函数名.方法名()调用。

```javascript
Object.getPrototypeOf(obj);  // 兼容低版本浏览器
Person.prototype;          // 通过构造函数找
obj.__proto__            // 通过实例对象找
```

### 为内置对象扩展方法

例：给字符串对象String扩展一个方法：

```javascript
String.prototype.daxie = function(){
    console.log("abc");
}
// 然后可以使用：
console.log('hello'.daxie())  // 输出abc
```

这种情况称为猴子补丁，不推荐这么使用。

### 继承

使用方法如下：

```javascript
B.propotype = new A();  // 使用B继承A
```

