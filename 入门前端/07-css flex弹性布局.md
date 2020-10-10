## flex布局

设置方式为：display：flex；display: inline-flex;

min-width，min-height，max-width，max-height

## 容器的属性

### flex-direction

排列方式。

row（从左到右，默认），row-reverse（从右到左），column（从上到下），column-reverse（从下到上）

### flex-wrap

是否换行，和换行方式

nowrap（默认，排列不下不换行，内容会被挤压），wrap（排列不下换行到下一层），wrap-reverse（排列不下换行到上一层）

### flex-flow

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

```css
.box {
  flex-flow: <flex-direction> <flex-wrap>;
}
```

### justify-content

flex-start（左对齐，默认），flex-end（右对齐），center（居中对齐），space-between（左右都是项目元素）, space-around（项目与项目的距离是项目与边距离的两倍）, space-evenly（项目与项目的距离等于项目与边之间的距离）

### align-items

- flex-start：交叉轴的起点对齐。
- flex-end：交叉轴的终点对齐。
- center：交叉轴的中点对齐。
- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

### align-content

- flex-start：与交叉轴的起点对齐。
- flex-end：与交叉轴的终点对齐。
- center：与交叉轴的中点对齐。
- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。左右都是项目元素。
- space-around：项目与项目的距离是项目与边距离的两倍。
- space-evenly：项目与项目的距离等于项目与边之间的距离。
- stretch（默认值）：轴线占满整个交叉轴。

## 项目的属性

### order

order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

### flex-grow

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

### flex-shrink

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。如果修改为0则空间不足时不缩小。

### flex-basis

flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

优先级：min-width>flex-basis>width

### flex

flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。

```css
.box {
  flex: <flex-grow> <flex-shrink> <flex-basis>;
}
```

该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

### align-self

align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

- flex-start：交叉轴的起点对齐。
- flex-end：交叉轴的终点对齐。
- center：交叉轴的中点对齐。
- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

### flex的简写属性

flex: 1;（flex-basis：0%，flex-grow：1，flex-shrink：1）

等分布局。

## 小练习

如果觉得自己学的可以了可以做一下这个小练习：[http://flexboxfroggy.com/](http://flexboxfroggy.com/)