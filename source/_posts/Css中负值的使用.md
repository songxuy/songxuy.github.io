title: Css中负值的使用
author: coolsong
date: 2019-08-18 00:45:02
tags:
  - Css
categories: 
  - Css
---
&nbsp;&nbsp;&nbsp;今天在网上看到一篇你所不知道的 CSS 负值技巧与细节，看了之后对于一些属性的使用有了新的了解，于是自己就都在本地试了试水，下面就来介绍一下这些属性吧：
<!--more-->
### outline-offset

* * *
>我们都知道outline属性是给box加上一个边框，而outline-offset则是设置边框轮廓的偏移量的属性
```scss
div {
  width: 200px;
  height: 200px;
  position: absolute;
  left:50%;
  top:50%;
  background-color: #eee;
  transform: translate(-50%, -50%);
  outline: 20px solid #424242;
  outline-offset: 10px;
}
```

* 添加正的值则向外偏移，负的值则想内偏移
![My Pic](/images/f1.png)

>但修改 outline-offset 到一个合适的负值 ，那么在恰当的时候，outline 边框就会向内缩进为一个加号。根据作者的尝试这里是118px
```scss
div {
  width: 200px;
  height: 200px;
  position: absolute;
  left:50%;
  top:50%;
  background-color: #eee;
  transform: translate(-50%, -50%);
  outline: 20px solid #424242;
  outline-offset: -118px;
}
```
![My Pic](/images/f2.png)
>添加动画之后可以看到其变化的过程

![My Pic](/images/f3.gif)

>作者也总结了实现这个效果所需要的条件
>要使用负的 outline-offset 生成一个加号有一些简单的限制：容器得是个正方形outline 边框本身的宽度不能太小outline-offset 负值 x 的取值范围为: -(容器宽度的一半 + outline宽度的一半) < x < -(容器宽度的一半 + outline宽度)

### 单侧投影

* * *
>这个问题自己之前也遇到过这个需求，也从网上知道了这个问题的解决问题，先来看看box-shadow的参数
>以 box-shadow: 1px 1px 1px 1px #000 为例，4 个数值的含义分别是，x 方向偏移值、y 方向偏移值 、模糊半径、扩张半径。其中方向偏移值和扩张半径都可以为负值 当我们有实现单侧阴影的需求时 我们可能会这样写：
```scss
div.shadow{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height:200px;
  box-shadow: 0 1px 3px #000;
}
```

* 会发现虽然我们只设置了纵向的阴影但是由于设置了模糊半径其他边也会受到影响

![My Pic](/images/f4.png)

>解决：使用和模糊半径相等的负值扩张半径来抵消 那么我们将看不到任何阴影，因为生成的阴影将被包含在原来的元素之下

```scss
div.shadow{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height:200px;
  box-shadow: 0 3px 5px -5px #000;
}
```
![My Pic](/images/f5.png)

### scale

* * *

>缩放还能是负数么 emmm  好吧 真的可以 看下demo吧

```scss
p.scale{
  position: absolute;
  top:50%;
  left:50%;
  width:300px;
  transform: translate(-50%, -50%) scale(-1) ;
}
```
![My Pic](/images/f6.png)

* 居然结果是翻转了180°

### letter-spacing

* * *

>这个大家应该都用过，是用来控制文本之间距离的，但是它的负值应该大家没试过吧  用它的负值可以实现倒序排列文字

```scss
p.letter_spacing{
  position: absolute;
  width:500px;
  left:50%;
  top: 50%;
  transform: translate(-50%, -50%);
  letter-spacing: 0px;
  animation: move 10s infinite;
}
@keyframes move {
  40% {
    letter-spacing: 36px;
  }
  80% {
    letter-spacing: -72px;
  }
  100% {
    letter-spacing: -72px;
  }
}
```
![My Pic](/images/f7.gif)

>其他的负值还有负的 
>animation-delay 的负值
>负值 margin
>负 text-indent 
>更多的属性参考作者的博客 [参考链接](https://juejin.im/post/5d4b8707f265da03a65302bd)
