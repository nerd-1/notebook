## Map介绍

Map和对象类似，不过对象中键名都会被转换为字符串。而Map则不会。

首先让我们来看看对象：

```javascript
let a = {
    1 : 2,
    "1" : 3,
}
console.log(a);    // 输出 {1: 3}
```

由于对象会将键转换为字符串，所以后面那个会覆盖前面那个。

然后我们看看Map：

```javascript
let a = new Map([[1, 2], ["1", 3]]);
console.log(a);                  // 输出 Map(2) {1 => 2, "1" => 3}
```

## Map的声明方式

声明方式就如之前的代码，传入几个二元数组：

```javascript
let m = new Map([
    ["catfish1921", "catfish"],
    ['hello', "world"],
])
```

## Map对象的相关属性

### size

获取键值对的数量。

## Map对象的相关方法

### set

set方法可以给Map中添加键值对。使用方法如下：

```javascript
let m = new Map();
m.set("catfish", "catfish1921");
console.log(m);        // 输出 Map(1) {"catfish" => "catfish1921"}
```

### has

has方法需要传入一个键的名字，然后返回一个布尔值，检测该键是否在对象中存在。使用方法如下：

```javascript
let m = new Map();
m.set("catfish", "catfish1921");
console.log(m.has("catfish"));     // 输出 true
```

### get

用来根据键取值：

```javascript
let m = new Map();
m.set("catfish", "catfish1921");
console.log(m.get("catfish"));  // 输出 "catfish1921"
```

### delete

指定键名，用来删除Map对象中指定的键值对。使用方法如下：

```javascript
let m = new Map();
m.set("catfish", "catfish1921");
m.delete("catfish");
console.log(m);        // 输出 Map(0) {}
```

### clear

清楚Map中所有元素。使用方法如下：

```javascript
let m = new Map();
m.set("catfish", "catfish1921");
m.set("hello", "world");
m.delete();
console.log(m);  // 输出 Map(0) {}
```

### keys & values & entries

返回相关的键，值，键值对。使用方法如下：

```javascript
let m = new Map([["catfish", "catfish1921"], ["hello", "world"]]);
console.log(m.keys());       //输出 {"catfish", "hello"}
console.log(m.values());    // 输出 {"catfish1921", "world"}
console.log(m.entries());    // 输出 {"catfish" => "catfish1921", "hello" => "world"}

```

可以根据这个属性然后使用for of和forEach遍历：

```javascript
// for of 遍历
let m = new Map([["catfish", "catfish1921"], ["hello", "world"]]);
for (const [key, value] of m){
    console.log(`${key}=>${value}`);
}
// forEach 遍历
let m = new Map([["catfish", "catfish1921"], ["hello", "world"]]);
m.forEach((value, key) => {
    console.log(`${key}=>${value}`);
});
```

## WeakMap

**WeakMap** 对象是一组键/值对的集

- 键名必须是对象
- WeaMap对键名是弱引用的，键值是正常引用

- 垃圾回收不考虑WeaMap的键名，不会改变引用计数器，键在其他地方不被引用时即删除
- 因为WeakMap 是弱引用，由于其他地方操作成员可能会不存在，所以不可以进行`forEach( )`遍历等操作
- 也是因为弱引用，WeaMap 结构没有keys( )，values( )，entries( )等方法和 size 属性
- 当键的外部引用删除时，希望自动删除数据时使用 `WeakMap`

