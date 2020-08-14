title: css 进度条
author: coolsong
tags:
  - Css
categories:
  - Css
date: 2019-01-14 22:58:00
---
&nbsp;&nbsp;&nbsp;昨天在掘金上看到了一篇文章，讲得利用css实现滚动条的进度，纯css实现实现。看到里面用的东西其实自己也用过，但是没想到还可以这么玩 哈哈 所以说还是学无止境呀 还是要多多学习。先看看最终的代码吧
<!--more-->
```HTML
<html>
	<head>
		<title>css进度</title>
	</head>
	<style>
		*{
			margin:0;
			padding:0;
		}
		body {
	    background-image: linear-gradient(to right top, #ffcc00 50%, #eee 50%);
	    background-size: 100% calc(100% - 100vh + 5px);
	    background-repeat: no-repeat;
     }
     p{
     	font-size: 20px;
     	margin-top:50px;
     }
	body::after {
    content: "";
    position: fixed;
    top: 5px;
    left: 0;
    bottom: 0;
    right: 0;
    background: #fff;
    z-index: -1;
}

	</style>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<p>hahahahhhahahahhahahhahhahaaahhaha</p>
	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<p>hahahahhhahahahhahahhahhahaaahhaha</p>

	<body>
		
	</body>
</html>
```
其实这段代码主要是利用了css的background-image属性 实现了从屏幕对半分的一个渐变背景图片，当划到最低端是发现渐变颜色已经占满块80%了，在通过调整背景的size使之刚好在滑到底部时刚好占满最上部，再通过使用一个伪元素将多余部分的背景颜色给遮住，在设置z-index：-1，让它不挡住正文的内容。于是就使用css实现了一个滚动的进度条
