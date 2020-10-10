## 函数一览

### attr()

返回选择元素的属性值，使用方法如下：

```css
a:after {
    content: " (" attr(href) ")";
}
```

### calc()

计算元素属性值，使用方法如下：

```css
#div1 {
    position: absolute;
    left: 50px;
    width: calc(100% - 100px);
    border: 1px solid black;
    background-color: yellow;
    padding: 5px;
    text-align: center;
}
```

### cubic-bezier()

表示贝塞尔曲线，表示运动函数的速度分布，如果想查询贝塞尔曲线的值可以查询这个网站：[点击进入](https://cubic-bezier.com/)

然后选择一个合适的速度曲线，再将相关值放进去即可，使用方法如下：

```css
div {
  width: 100px;
  height: 100px;
  background: red;
  transition: width 2s;
  transition-timing-function: cubic-bezier(0.1, 0.7, 1.0, 0.1);
}
```

### hsl()

hsl() 函数使用色相、饱和度、亮度来定义颜色。

### hsla()

hsla() 函数使用色相、饱和度、亮度、透明度来定义颜色。

### linear-gradient()

创建一个线性渐变的图像

使用方法如下：

从头部开始线性渐变，从蓝色开始，转为红色，再到蓝色。

```css
div{
    background-image: linear-gradient(blue, red, yellow)
}
```

从左到右开始线性渐变。

```css
div{
    background-image: linear-gradient(to right, red, yellow)
}
```

由左上角到右下角的线性渐变：

```css
div {
  background-image: linear-gradient(to bottom right, red, yellow);
}
```

指定一个角度线性渐变：

```css
div {
  background-image: linear-gradient(180deg, red, yellow);
}
```

### radial-gradient()

第一个参数有两个取值：ellipse（椭圆，默认），circle（圆），这个取值效果只有第三个参数设置为top和bottom才会生效，如果是center则都为圆形。

第二个参数有四个取值：

farthest-corner（默认）: 中心点在图片中间，较大

closest-side：没有中心点

closest-corner: 没有中心点

farthest-side: 中心点在图片中间，较小

所以，如果想要有渐变效果不妨设置farthest-corner和farthest-side属性。

第三个参数取值为：top，bottom和center（默认）

使用方法如下：

```css
div{
    width: 100px;
    height: 100px;
    background-image: radial-gradient(ellipse farthest-side at top, red, blue, yellow);
}
```

### rgb()

rgb() 函数使用红(R)、绿(G)、蓝(B)三个颜色的叠加来生成各式各样的颜色。

### rgba()

rgba() 函数使用红(R)、绿(G)、蓝(B)、透明度(A)的叠加来生成各式各样的颜色。

### var()

var() 函数用于插入自定义的属性值，如果一个属性值在多处被使用，该方法就很有用。

使用方法如下：

```css
:root {
  --main-bg-color: coral;
}
 
#div1 {
  background-color: var(--main-bg-color);
}
 
#div2 {
  background-color: var(--main-bg-color);
}
```