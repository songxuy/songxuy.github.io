title: grid布局
author: coolsong
tags:
  - Css
categories: 
  - Css
date: 2019-08-04 00:19:00
---
&nbsp;&nbsp;&nbsp;一直知道css中存在这个属性，以及它能够很方便的布局，但是感觉自己使用flex基本上也够用了，就没去仔细的学习。但是今天突然想看看grid的庐山真面目。
<!--more-->
### 1.容器属性

>容器我也就理解成父元素（里面必须要有标签包裹的子元素）
```scss
div{
display: grid;
}
//默认情况下容器都是块级元素 但是也可以通过属性设置为行内元素
div{
display: inline-grid
}
```

>注意，设为网格布局以后，容器子元素（项目）的float、display: inline-block、display: table-cell、vertical-align和column-等设置都将失效。

#### grid-template-columns 属性，grid-template-rows 属性
```scss
div{
    display: grid;
    grid-template-column: 100px 100px 100px;
    grid-template-rows: 100px 100px 100px;
}
//容器指定了网格布局以后，接着就要划分行和列。grid-template-columns属性定义每一列的列宽，grid-template-rows属性定义每一行的行高。也可以使用百分比。同时可以使用repeat() 第一个参数是数量 第二个是重复的值 repeat(3, 33.3%) repeat(2, 100px 20px 80px
//repeat(auto-fill, 100px)
//有时，单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用auto-fill关键字表示自动填充)

div{
    display: grid;
    grid-template-column: 1fr  1fr;
}
//会生成同样宽的两列  代表一个比例
//fr可以与绝对长度的单位结合使用，这时会非常方便。grid-template-column: 150px 1fr 1fr;

div{
    display: grid;
    grid-template-columns: 1fr 1fr minmax(100px, 1fr)
}
//minmax()函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值  上面代码中，minmax(100px, 1fr)表示列宽不小于100px，不大于1fr

//除此之外 还有 auto（浏览器自己决定长度） 还可以为自己划分的行和列命名 

.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
  }
```
#### grid-row-gap 属性，grid-column-gap 属性，grid-gap 属性
> 用来设置行间距和列间距
```scss
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
  }
```

#### grid-auto-flow 
>划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行，即下图数字的顺序。
这个顺序由grid-auto-flow属性决定，默认值是row，即"先行后列"。也可以将它设成column，变成"先列后行"
```scss
div{
    display:grid;
    -auto-flow: column;
}
//
设为row dense，表示"先行后列"，并且尽可能紧密填满，尽量不出现空格。
div{
    display:grid;
    -auto-flow: row dense;
}
```

#### justify-items 属性，align-items 属性，place-items 属性
> 是控制单元格的水平方向和垂直方向的对齐方式
```scss
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
 }
```
>start：对齐单元格的起始边缘。
>end：对齐单元格的结束边缘。
>center：单元格内部居中。
>stretch：拉伸，占满单元格的整个宽度（默认值）。

####  justify-content 属性，align-content 属性，place-content 属性
> 前两个和flex中的类似一个是控制整个内容区域水平方向和垂直方向的对齐方式
```scss
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
//place-content属性是align-content属性和justify-content属性的合并简写形式。place-content: <align-content> <justify-content>
```

#### grid-auto-columns 属性，grid-auto-rows 属性
> 有时候，一些项目的指定位置，在现有网格的外部。比如网格只有3列，但是某一个项目指定在第5行。这时，浏览器会自动生成多余的网格，以便放置项目。
```scss
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-auto-rows: 50px; 
}
// 这样就生成了5行
```
### 2.项目属性

#### grid-column-start 属性，grid-column-end 属性，grid-row-start 属性，grid-row-end 属性
```scss
// grid-column-start属性：左边框所在的垂直网格线
// rid-column-end属性：右边框所在的垂直网格线
// grid-row-start属性：上边框所在的水平网格线
// grid-row-end属性：下边框所在的水平网格线
.item-1 {
  grid-column-start: 2;
  grid-column-end: 4;
 }
 // 指定，1号项目的左边框是第二根垂直网格线，右边框是第四根垂直网格线。
 //这四个属性的值还可以使用span关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。grid-column-start: span 2;
```

####  grid-column 属性，grid-row 属性
>grid-column属性是grid-column-start和grid-column-end的合并简写形式，grid-row属性是grid-row-start属性和grid-row-end的合并简写形式。用/分开

```scss
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 2;
}
  /* 等同于 */
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 2;
}
/*这两个属性之中，也可以使用span关键字，表示跨越多少个网格。*/
.item-1 {
  background: #b03532;
  grid-column: 1 / span 2;
  grid-row: 1 / span 1;}
```
#### grid-area 属性
>grid-area属性指定项目放在哪一个区域。

```scss
.item-1 {
  grid-area: e;
}

.item {
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
```
#### justify-self 属性，align-self 属性，place-self 属性
>justify-self属性设置单元格内容的水平位置（左中右），跟justify-items属性的用法完全一致，但只作用于单个项目。align-self属性设置单元格内容的垂直位置（上中下），跟align-items属性的用法完全一致，也是只作用于单个项目。

```scss
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```
>start：对齐单元格的起始边缘。
>end：对齐单元格的结束边缘。
>center：单元格内部居中。
>stretch：拉伸，占满单元格的整个宽度（默认值）。

>[CSS Grid网格布局教程](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)