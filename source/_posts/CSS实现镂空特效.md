title: CSS实现镂空特效
author: coolsong
tags:
  - css
categories:
  - css
date: 2018-10-22 22:15:00
---
&nbsp;&nbsp;&nbsp;&nbsp;今天在网上看到了一篇关于css特效的文章，其中有一个属性感觉自己从来都没有看到过，也没有用过。结果实现出来的效果还挺酷的。
首先给大家看一下效果吧
<!--more-->
![My Pic](/images/mask1.png)
中间部分的图片为一个不透明的svg，但是通过css中的一个属性，可以将不透明的图片变成透明。中间的背景反而偷了出来。代码如下：
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>wenzi</title>
</head>
<style>

* {
padding: 0;
margin: 0;
}

html,
body {
height: 100vh;
width: 100vw;
}

body {
display: flex;
justify-content: center;
align-items: center;
flex-direction: column;
font-family: "Open Sans","PingFang SC","Microsoft YaHei","Helvetica Neue","Hiragino Sans GB","WenQuanYi Micro Hei",Arial,sans-serif;
}

@keyframes move {
0% {
background-position: 0 0;
}
50% {
background-position: 100% 0;
}
}

.bg {
background: url(fengjing.jpg);
background-size: cover;
position: fixed;
top: -20px;
left: -20px;
right: -20px;
bottom: -20px;
filter: blur(15px);
z-index: -1;
}

.slogan {
color: white;
margin-top: 24px;
font-size: 36px;
font-weight: 400;
}

.mask {
/* https://sp-webfront.skypixel.com/skypixel/v2/public/website/assets/1535027674204-f6eca6369ec03e70262b58b0e25cda7b */
width: 340px;
height: 200px;
-webkit-animation: move 40s infinite;
animation: move 40s infinite;
background-image: url(fengjing.jpg);
background-size: cover;
-webkit-mask: url(http://static.w3ctrain.com/upload_cae6fcb079f57792a47202cb67bbc04a-dji-seeklogo.com.svg);
mask: url(http://static.w3ctrain.com/upload_cae6fcb079f57792a47202cb67bbc04a-dji-seeklogo.com.svg);
-webkit-mask-size: cover;
mask-size: cover;
/* mask-mode:alpha; */
/* http://static.w3ctrain.com/upload_cae6fcb079f57792a47202cb67bbc04a-dji-seeklogo.com.svg */
}
</style>
<body>
<div class="bg"></div>
<div class="mask"></div>
<h1 class="slogan">How can you be brave if only wonderful things happen to you</h1>
</body>
</html>
```
css的mask属性与backgroud的用法比较相似
```
mask-image 
mask-mode alpha luminance
mask-repeat
mask-position
mask-clip
mask-origin
mask-size  auto cover contain
mask-type
mask-composite

```
兼容性也还不错
![My Pic](/images/mask3.png)

最后再来一张GIF效果的图片
![My Pic](/images/mask2.png)

