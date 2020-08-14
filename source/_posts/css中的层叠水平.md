title: css中的层叠水平
author: coolsong
tags:
  - Css
categories:
  - css
date: 2018-11-19 23:25:00
---
&nbsp;&nbsp;&nbsp;&nbsp;之前呢 对css的层叠水平大概只知道一个z-index，有些时候就会乱用，并且会出现修改z-index无效的效果。然后自己去学习了一下css的层叠水平和层叠上下文。
下面是一些例子
<!--MORE-->
```
首先是比较block和inline
<span style="width:50px;height:100px;background-color: red;margin-top:10px;">xxxxx</span>
<p style="width:100px;height:50px; background-color: blue;margin-top:-10px;">
这里的结果是span元素会盖在p元素上面，并没有遵循后来的会覆盖在前面
在没有其他元素干扰的情况下inline的默认层叠水平高于block
```
![My Pic](/images/z1.png)
```html
 .father{
    position: relative;
    width:200px;
    height:400px;
    background-color: green;
}
.son1{
    position: absolute;
    top:0px;
    width:250px;
    height:120px;
    background-color: antiquewhite;
    z-index:-2;
}
.son2{
    position: absolute;
    top:0px;
    width:220px;
    height:100px;
    background-color: red;
    z-index:-1;
} 
<div class="father">
    <div class="son1"></div>
    <div class="son2"></div>
</div>
这里父元素会盖在子元素上，但是给父元素加上z-index属性之后，子元素又会盖在父元素上。第一种情况是由于父元素与子元素没有建立层叠上下文，遵循层叠顺序所以父元素会显出来，第二种的话由于建立了层叠上下文，子元素就会在夫元素上。
```
![My Pic](/images/z2.png)
![My Pic](/images/z3.png)
```html
 .father1{
    position: absolute;
    left:50px;
    width:300px;
    height:300px;
    background:pink;
    z-index:auto;
}
.father2{
    position: absolute;
    left:100px;
    width:300px;
    height:300px;
    background:red;
    z-index:auto;
}
.son1{
    position: absolute;
    left:0px;
    top:50px;
    width:120px;
    height:100px;
    background-color: blue;
    z-index:2;
}
.son2{
    position: absolute;
    left:50px;
    top:50px;
    width:100px;
    height:100px;
    background-color: yellow;
    z-index:1;
}
 <div class="father1">
    <div class="son1"></div>
</div>
<div class="father2">
    <div class="son2"></div> 
</div> 
这里说明互相比较的时候，如果父与子建立了层叠上下文先看父元素的层叠水平大小，再看子元素的层叠水平 
```
![My Pic](/images/z4.png)
```html
.box {  }
.box > div { background-color: blue;z-index:1; }   
.box > div > img { 
  position: relative; z-index: -1; right: -50px;width:100px;height:100px; 
} 
 <div class="box">
        <div>
            <img src="1.jpg" />
        </div>
</div>
这个例子 当父元素dispaly为flex时，子元素没有定位的z-index也会生效
```
![My Pic](/images/z5.png)
```HTML
.box { background-color: blue;transform: translate(-100px,0);z-index:1;position: relative; }
.box > img {
    position: relative; z-index: -99; right: -50px;width:100px;height:100px; 
}
<div class="box">
        <img src="1.jpg">
</div>
这里说明transform属性会使z-index元素失效
```
