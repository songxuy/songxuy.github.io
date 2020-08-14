title: js实现弹窗的拖拽效果
author: coolsong
tags:
  - JavaScript
categories:
  - JavaScript
date: 2019-09-22 23:37:00
---
&nbsp;&nbsp;&nbsp;最近的一个项目中遇到了需要对弹窗进行拖拽操作的需求，隐隐约约记得之前实现过，但现在大概api的名字，于是去查了一下移动端的话主要使用touchstart、touchmove、touchend这三个api来实现。PC端的话主要是利用mousedown、mousemove、mouseup这三个api来实现。下面主要介绍一下PC端弹窗的实现。
<!--more-->
### API

* * *
#### MouseDown
>onmousedown 事件会在鼠标按键被按下时发生。
>与 onmousedown 事件相关连得事件发生次序
>onmousedown->onmouseup->onclick
>触发之后会返回一些相关信息
>
![My Pic](/images/drag1.png)

#### MouseMove
>mousemove 属性在鼠标指针移动到元素上时触发。
>返回的东西和上面的类似
>onmousemove 属性可用于使用 HTML 元素，除了: 

```Html
<base>, <bdo>, <br>, <head>, <html>, <iframe>, <meta>, <param>, <script>, <style>, and <title>.
```

#### MouseUp
>onmouseup 属性在松开鼠标按钮时触发。
>onmouseup 属性不适用于以下元素：

```Html
<base>, <bdo>, <br>, <head>, <html>, <iframe>, <meta>, <param>, <script>, <style>, and <title>.
```

>返回的信息也和上面类似

### 几个距离

* * *

#### clientX OR clientY
>点击位置距离当前body可视区域的x，y坐标

#### pageX OR pageY
>对于整个页面来说，包括了被卷去的body部分的长度

#### screenX OR screenY
>点击位置距离当前电脑屏幕的x，y坐标

#### offsetX OR offsetY
>相对于带有定位的父盒子的x，y坐标

![My Pic](/images/drag2.png)

### 代码实现
>利用这三个api以及clientX和clientY以及offsetX和offsetY实现

```Html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>弹窗拖拽</title>
  <style>
    *{margin:0;padding:0;}
    .box{position: absolute;width: 400px;height: 300px;top:100px;left:100px;border:1px solid #001c67;background: #}
    .move{position: absolute;width: 100px;height: 100px;top:100px;left:150px;border:1px solid #000;}
    .move:hover{cursor: move;}
    .close{position: absolute;width: 30px;height: 30px;top:0px;right:0px;background:red;text-align: center;line-height: 30px;}
  </style>
  <script>
    window.onload=function(){
      var oMove=document.getElementById('move');
      // 拖曳
      oMove.onmousedown=fnDown;
      // 关闭
      var oClose=document.getElementById('close');
      oClose.onclick=function(){
       document.getElementById('box').style.display='none';
      }
    }
    function fnDown(event){
      console.log(event)
      event = event || window.event;
      var oDrag=document.getElementById('box'),
        // 光标按下时光标和面板之间的距离
        disX=event.clientX-oDrag.offsetLeft,
        disY=event.clientY-oDrag.offsetTop;
      // 移动
      document.onmousemove=function(event){
        event = event || window.event;
        var l=event.clientX-disX,
          t=event.clientY-disY,
          // 最大left,top值
          leftMax=(document.documentElement.clientWidth || document.body.clientWidth)-oDrag.offsetWidth,
          topMax=(document.documentElement.clientHeight || document.body.clientHeight)-oDrag.offsetHeight;
        if(l<0) l=0;
        if(l>leftMax) l=leftMax;
        if(t<0) t=0;
        if(t>topMax) t=topMax;
        oDrag.style.left=l+'px';
        oDrag.style.top=t+'px';
      }
      // 释放鼠标
      document.onmouseup=function(){
        document.onmousemove=null;
        document.onmouseup=null;
      }
    }
  </script>
</head>
<body>
  <div class="box" id="box">
    <div class="move" id="move">BUTTON</div>
    <div class="close" id="close">X</div>
  </div>
</body>
</html>
```
![My Pic](/images/drag3.gif)