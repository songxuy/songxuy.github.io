title: html2canvas的使用
author: coolsong
tags:
  - JS
categories:
  - Javascript
date: 2018-09-17 23:00:00
---
&nbsp;&nbsp;&nbsp;上一次自己通过canvas实现了将网页中的部分内容画在图片上，但是这一次自己遇到的需求需要画的内容太多了而且很多动态的图片。这时就需要使用工具了，没错就是用起来超级方便的html2canvas。不用再去慢慢画，直接能够将网页里的一部分生成图片。不过就是有些时候页面中存在外部图片时，可能会出现跨域的问题，设置了它的一个参数好像还是不行。下面就是自己使用的一个小demo
<!--MORE-->
```javascript
<div class="result">
<img :src="canvasImgUrl" alt="" @load="imgLoaded = true" class="img-view">
<div class="btn" data-html2canvas-ignore="true" >
<img src="../assets/images/result_btn.png" @click="to('/main')"/>
</div>
<div id="posterView" v-show="!imgLoaded" class="poster-canvas">
<img src="../assets/images/ceshihome_logo.png"/>
<div class="peo">
<div class="con-show01">
<div class="con-show02">
<div class="con-show03 bg01">
<img :src="actInfo.userInfo.portrait" class="ico" crossOrigin="anonymous"/>
<!--<img src="../assets/images/liu.jpg" class="ico" crossOrigin="anonymous"/>-->
</div>
</div>
</div>
<div class="info">
<p>{{actInfo.userInfo.nick}}</p>
</div>
</div>
<div class="change">
<img :src='"../../src/assets/images/"+nowtext+"_text.png"' />
<img :src='"../../src/assets/images/"+nowtext+"_des.png"' />
<img :src='"../../src/assets/images/"+now+"_text.png"' />
</div>
</div>
<toast></toast>
</div>
let option = {
useCORS: true,
width: '750',
scale: .7
}
var that =this;
html2canvas(document.getElementById('posterView'), option).then((canvas) => {
let dataURL = canvas.toDataURL("image/jpeg",1)
axios.post('https://www.codoon.com/activity/v1/api/upload/dataurl',{
data: dataURL,
type: 'image/jpeg'
})
.then(function (response) {
if(response.status){
that.isGenerate = false
that.canvasImgUrl = response.data
if(cb){
cb()
}
}else{
alert(response.description)
}
console.log(response)
})
.catch(function (error) {
alert(error);
});
});

```
这里面掉了一个接口 将base64的图片转成了jpg