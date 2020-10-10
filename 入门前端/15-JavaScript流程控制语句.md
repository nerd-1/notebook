## 调试语句

可以给代码加个调试语句来判断出错的地方在哪里，调试语句使用方法如下：

```javascript
debugger;
```

可以用于网站反调试，使用方法如下：

```javascript
setInterval(()=>{
    debugger;
}, 1000);
```

## 条件语句

### if语句

if语句使用方法如下：

```javascript
if(条件表达式A)
{
    // 条件A满足执行的语句
}else if(条件表达式B){
    // 条件B满足执行的语句
}
......
else{
    // 条件不满足执行的语句
}
```

1. 对于非布尔类型的数据，会先转换为布尔类型再判断。

```javascript
if (null){
    console.log("语句A")
}
```

2. 对于==/===判断，将常量写在前面

```javascript
let num = 10;
if (5==num){
    console.log("语句A")
}
```

3. if/else if/else后面的大括号都是可以省略的，但是省略之后必须有紧随其后的语句收到控制。

```javascript
if (false) console.log("语句A");
```

4. JavaScript中;也是一条语句
5. if选择结构可以嵌套使用
6. 当if省略大括号时，else if/else会自动和距离最近没有使用的if匹配

### switch语句

使用方法如下：

```javascript
switch(表达式){
    case 表达式A:
        语句A;
        break;
    case 表达式B:
        语句B;
        break;
    ……
    default:
        前面所有case都不匹配执行的代码;
        break;
}
```

实战：

```javascript
let day = 1;
switch(day){
    case 1:
        console.log("星期一");
        break;
    case 2:
        console.log("星期二");
        break;
    default:
        console.log("Other");
        break;
}
```

注意点：

1. case判断是===，不是==
2. （）中可以是常量也可以是变量还可以是表达式
3. case后面是常量也可以是变量还可以是表达式
4. break的作用是结束整个switch语句，再switch语句中，一旦case或者default被匹配，其他的case和default都会失效
5. default不一定要写在最后，且可以省略。

## 循环语句

### while循环

使用方法如下：

```javascript
while(条件表达式){
    条件满足执行的语句;
}
```

while的注意点：

1. 如果后面只有一条语句可以省略大括号
2. 不能再（）后面写；。
3. 最简单的死循环

```javascript
while(1);
```

4. do... while循环的格式

```javascript
do{
    需要重复执行的代码
}while(条件表达式)
```

### for循环

基本使用如下：

```javascript
for(初始化表达式;条件表达式;循环后增量表达式){
    需要重复执行的代码;
}
```

例子使用如下：

```javascript
for(let num=1; num<=10; num++){
    console.log("发射子弹"+num);
}
```

## 流程控制

### label语句

label语句通常与break和continue语句连用，从而达到代码中指定位置，这种联合使用的情况多发生在循环嵌套的情况下。使用方法如下：

```javascript	
var num = 0;
outermost:
for (var i = 0; i < 10; i++) {
    for (var j = 0; j < 10; j++) {
        if (i == 5 && j == 5) {
            break outermost;
        }else{
            console.log(i,j,88);
        }
        num++;
    }
}
console.log(num); //55
```

然后再看看再和continue使用过程中的label标签：

```javascript
var num = 0;
outermost:
for (var i = 0; i < 10; i++) {
    for (var j = 0; j < 10; j++) {
        if (i == 5 && j == 5) {
            continue outermost;
        }else{
            console.log(i,j,88);
        }
        num++;
    }
}
console.log(num); //95
```

总之，如果label语句和break语句连用，会直接跳出指定层的循环之外。如果label语句和continue语句连用，会直接跳过到相应的循环并开始循环流程。

### break语句

break语句能够结束当前循环语句或者switch语句的执行，注意，break不能使用在if语句中。同时break语句可以接受一个可选的标签名，来决定跳出的结构语句。

### continue语句

continue语句在循环结构内，用于跳出本次循环中的剩余的代码，并在表达式为真时，继续执行下一次循环。它可以接受一个标签名来决定跳出的循环语句。

## 异常处理

基本使用如下：

```javascript
try{
    // 调试代码块
}catch(e){
    // 捕获异常并进行异常处理的代码块
}finally{
    // 后期清理的代码块
}
```

实例如下：

```javascript
try{
    1 = 1;
}catch(error){
    console.log(error.name);
    console.log(error.message);
}finally{
    console.log("1=1");
}
```

### throw语句

throw语句能主动抛出一个异常。

使用方法如下：

```javascript
throw new Error("这是一个错误");
```