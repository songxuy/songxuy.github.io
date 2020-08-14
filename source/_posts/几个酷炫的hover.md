title: 几个酷炫的hover
author: coolsong
tags:
  - Css
  - JavaScript
categories:
  - Css
  - JavaScript
date: 2020-07-13 20:55:00
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;今天看到一篇博客介绍一些很酷炫的hover效果和相应的库。于是自己就去学习和了解了一下，自己动手试着还原一下。
<!--more-->
### 悬停图库切割合成大图

* 先来看看最终的效果

![Image](/images/hover1.gif)

>效果就是当我们hover在图片上时，会分割的产生该图片变大之后的效果，分割效果主要是使用的splitting.js这个库去实现的，具体的代码如下：

```Html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<link rel="stylesheet" href="https://unpkg.com/splitting/dist/splitting.css" />
<link rel="stylesheet" href="https://unpkg.com/splitting/dist/splitting-cells.css" />
<link rel="stylesheet" href="./index.css">
<body>
    <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1067" />
      </div>
      
      <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1061" />
      </div>
      
      <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1057" />
      </div>
      
      
      <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1052" />
      </div>
      
      
      <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1043" />
      </div>
      
      
      <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1055" />
      </div>
      
      
      <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1036" />
      </div>
      
      
      <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1037" />
      </div>
      
      
      <div class="tiler">
        <img src="https://picsum.photos/1000/600?image=1039" />
      </div>
</body>
</html>
<script src='https://unpkg.com/splitting@1.0.0/dist/splitting.js'></script>
<script>
    Splitting({
     target: '.tiler',
     by: 'cells',
     rows: 3,
     columns: 3,
     image: true
    });
</script>
```
```Css
.tiler {
  display: inline-block;
  cursor: pointer;
  visibility: hidden;
  width: 33.3%;
  margin: auto; }

.tiler img {
  display: block;
  margin: auto;
  max-width: 100%;
  visibility: visible; }

.tiler .cell-grid {
  background-position: center center;
  margin: auto;
  position: fixed;
  top: 1em;
  bottom: 1em;
  left: 1em;
  right: 1em;
  z-index: 10;
  max-width: 1000px;
  max-height: 600px;
  perspective: 30px; }

.tiler .cell {
  pointer-events: none;
  opacity: 0;
  transform: translateZ(-15px);
  transform-style: preserve-3d;
  transition-property: transform, opacity;
  transition-duration: 0.5s, 0.4s;
  transition-timing-function: cubic-bezier(0.65, 0.01, 0.15, 1.33);
  /* The center character index */
  --center-x: calc((var(--col-total) - 1) / 2);
  --center-y: calc((var(--row-total) - 1) / 2);
  /* Offset from center, positive & negative */
  --offset-x: calc(var(--col-index) - var(--center-x));
  --offset-y: calc(var(--row-index) - var(--center-y));
  /* Absolute distance from center, only positive */
  --distance-x: calc(
    (var(--offset-x) * var(--offset-x)) / var(--center-x)
  );
  /* Absolute distance from center, only positive */
  --distance-y: calc(
    (var(--offset-y) * var(--offset-y)) / var(--center-y)
  );
  transition-delay: calc( 0.1s * var(--distance-y) + 0.1s * var(--distance-x) ); }

.tiler-overlay {
  z-index: 2; }

.tiler:hover .cell {
  transform: scale(1);
  opacity: 1; }

html {
  height: 100%;
  display: flex;
  background: #000; }

body {
  display: flex;
  flex-wrap: wrap;
  max-width: 800px;
  padding: 2em;
  margin: auto; }

/*# sourceMappingURL=index.css.map */
```
>[官方文档](https://splitting.js.org/guide.html)

### 叠加运动悬停效应
* 同样的 我们还是先来看看最终的效果吧

![Image](/images/hover2.gif)

> 效果是hover的时候图片后面会有相应的阴影形成3D堆叠的效果，一些动画效果主要是利用anime.js这个动画库去实现的。具体代码可见[github](https://github.com/codrops/StackMotionHoverEffects/)

### 3D 缩略图悬停效果

* 同样的先来看看最终的效果吧
![Image](/images/hover3.gif)

> 效果就是我们hover时图片的一边会卷起来形成一个3D的效果，同时展示一些其他的信息。主要是利用jquery+css来实现的，我们先来看看js部分的代码：

```JavaScript
( function( $ ) {
	
	$.fn.hoverfold = function( args ) {

		this.each( function() {
		
			$( this ).children( '.view' ).each( function() {
			
				var $item 	= $( this ),
					img		= $item.children( 'img' ).attr( 'src' ),
					struct	= '<div class="slice s1">';
						struct	+='<div class="slice s2">';
							struct	+='<div class="slice s3">';
								struct	+='<div class="slice s4">';
									struct	+='<div class="slice s5">';
									struct	+='</div>';
								struct	+='</div>';
							struct	+='</div>';
						struct	+='</div>';
					struct	+='</div>';
					
				var $struct = $( struct );
				
				$item.find( 'img' ).remove().end().append( $struct ).find( 'div.slice' ).css( 'background-image', 'url(' + img + ')' ).prepend( $( '<span class="overlay" ></span>' ) );
				
			} );
			
		});

	};

} )( jQuery );
```
>然后直接选择相应的dom节点，调用这个方法
>还有就是相关的css代码

```Css
.view {
	-webkit-perspective: 800px;
	-moz-perspective: 800px;
	-o-perspective: 800px;
	-ms-perspective: 800px;
	perspective: 800px;
}
.view:hover .s1{
	-webkit-transition-delay: 200ms;
	-moz-transition-delay: 200ms;
	-o-transition-delay: 200ms;
	-ms-transition-delay: 200ms;
	transition-delay: 200ms;
	
	-webkit-transform: rotate3d(0,1,0,-3deg);
	-moz-transform: rotate3d(0,1,0,-3deg);
	-o-transform: rotate3d(0,1,0,-3deg);
	-ms-transform: rotate3d(0,1,0,-3deg);
	transform: rotate3d(0,1,0,-3deg);
}
.view:hover .s2{
	-webkit-transition-delay: 150ms;
	-moz-transition-delay: 150ms;
	-o-transition-delay: 150ms;
	-ms-transition-delay: 150ms;
	transition-delay: 150ms;
	
	-webkit-transform: translate3d(59px,0,0) rotate3d(0,1,0,-10deg);
	-moz-transform: translate3d(59px,0,0) rotate3d(0,1,0,-10deg);
	-o-transform: translate3d(59px,0,0) rotate3d(0,1,0,-10deg);
	-ms-transform: translate3d(59px,0,0) rotate3d(0,1,0,-10deg);
	transform: translate3d(59px,0,0) rotate3d(0,1,0,-10deg);
}
.view:hover .s3{
	-webkit-transition-delay: 100ms;
	-moz-transition-delay: 100ms;
	-o-transition-delay: 100ms;
	-ms-transition-delay: 100ms;
	transition-delay: 100ms;
	
	-webkit-transform: translate3d(59px,0,0) rotate3d(0,1,0,-16deg);
	-moz-transform: translate3d(59px,0,0) rotate3d(0,1,0,-16deg);
	-o-transform: translate3d(59px,0,0) rotate3d(0,1,0,-16deg);
	-ms-transform: translate3d(59px,0,0) rotate3d(0,1,0,-16deg);
	transform: translate3d(59px,0,0) rotate3d(0,1,0,-16deg);
}
.view:hover .s4{
	-webkit-transition-delay: 50ms;
	-moz-transition-delay: 50ms;
	-o-transition-delay: 50ms;
	-ms-transition-delay: 50ms;
	transition-delay: 50ms;
	
	-webkit-transform: translate3d(59px,0,0) rotate3d(0,1,0,-30deg);
	-moz-transform: translate3d(59px,0,0) rotate3d(0,1,0,-30deg);
	-o-transform: translate3d(59px,0,0) rotate3d(0,1,0,-30deg);
	-ms-transform: translate3d(59px,0,0) rotate3d(0,1,0,-30deg);
	transform: translate3d(59px,0,0) rotate3d(0,1,0,-30deg);
}
.view:hover .s5{
	-webkit-transform: translate3d(60px,0,0) rotate3d(0,1,0,-42deg);
	-moz-transform: translate3d(60px,0,0) rotate3d(0,1,0,-42deg);
	-o-transform: translate3d(60px,0,0) rotate3d(0,1,0,-42deg);
	-ms-transform: translate3d(60px,0,0) rotate3d(0,1,0,-42deg);
	transform: translate3d(60px,0,0) rotate3d(0,1,0,-42deg);
}

.view .s4 > .overlay {
	background: -moz-linear-gradient(right, rgba(0,0,0,0.3) 0%, rgba(0,0,0,0) 100%);
	background: -webkit-linear-gradient(right, rgba(0,0,0,0.3) 0%,rgba(0,0,0,0) 100%);
	background: -o-linear-gradient(right, rgba(0,0,0,0.3) 0%,rgba(0,0,0,0) 100%);
	background: -ms-linear-gradient(right, rgba(0,0,0,0.3) 0%,rgba(0,0,0,0) 100%);
	background: linear-gradient(right, rgba(0,0,0,0.3) 0%,rgba(0,0,0,0) 100%);
}

.view .s5 > .overlay {
	background: -moz-linear-gradient(left, rgba(0,0,0,0.3) 0%, rgba(0,0,0,0) 100%);
	background: -webkit-linear-gradient(left, rgba(0,0,0,0.3) 0%,rgba(0,0,0,0) 100%);
	background: -o-linear-gradient(left, rgba(0,0,0,0.3) 0%,rgba(0,0,0,0) 100%);
	background: -ms-linear-gradient(left, rgba(0,0,0,0.3) 0%,rgba(0,0,0,0) 100%);
	background: linear-gradient(left, rgba(0,0,0,0.3) 0%,rgba(0,0,0,0) 100%);
}

.view div.view-back{
	background: #0a0a0a;
	background: -moz-linear-gradient(left, #0a0a0a 0%, #666666 100%);
	background: -webkit-gradient(linear, left top, right top, color-stop(0%,#0a0a0a), color-stop(100%,#666666));
	background: -webkit-linear-gradient(left, #0a0a0a 0%,#666666 100%);
	background: -o-linear-gradient(left, #0a0a0a 0%,#666666 100%);
	background: -ms-linear-gradient(left, #0a0a0a 0%,#666666 100%);
	background: linear-gradient(left, #0a0a0a 0%,#666666 100%);
}
```
>可以看到主要利用translate+rotate+transtion实现的3d效果






