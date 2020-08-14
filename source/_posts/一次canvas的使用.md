title: 一次canvas的使用
author: coolsong
tags:
  - JS
categories:
  - Javascript
date: 2018-08-29 21:48:00
---
&nbsp;&nbsp;&nbsp;&nbsp;对于canvas虽然知道它十分的强大并且自己看过，但是在实际的工作中自己还没遇到过需要使用的情况。但是在最近的一个测马拉松成绩的活动中，需要将成绩页面生成图片分享出去。不使用其他的插件的情况下，我首先想到的就是使用canvas来画。最后自己也实现了这个功能，同时发现原来canvas这么好用。下面就是关于canvas的代码以及最后的效果。
<!--more-->
```javascript
try {
var _self=this;
//this.setMum(true);
let feedData={
"name": this.info.user_name,
"mes":'“'+this.wenan+'”',
"time":this.info.chengji,
"img":this.info.user_portrait,//this.info.user_portrait
"process":this.info.process,
"bg":"http://activity-codoon.b0.upaiyun.com/fxd/1533867941041/mls.jpg",
};
//alert(feedData.img)
var mycanvas=document.createElement('canvas');
//document.body.appendChild(mycanvas);
mycanvas.width=750;
mycanvas.height=1300;
var context=mycanvas.getContext('2d');
//画背景
function drawing(){
var img=new Image();
img.crossOrigin = "Anonymous";
// alert('start');
img.onload=function(){
// alert('画背景');
context.drawImage(img,0,0,mycanvas.width,mycanvas.height);
var tou = new Image();
tou.crossOrigin = "Anonymous";
tou.src=feedData.img;
tou.onload=function(){
//alert('画头像')
context.save(); // 保存当前_context的状态
　　　　 context.beginPath();
　　　　 //context.moveTo(mycanvas.width/2,370);
　　　　 //context.lineWidth="20";
　　　　//画出圆
　　　　 context.arc(mycanvas.width/2,155,82,0,2*Math.PI,true);
　　　　//圆有个边框
　　　　 context.lineWidth=20;
　　　　 context.strokeStyle = '#499e87';
　　　　 context.fill();
　　　　 context.stroke();
　　　　//裁剪上面的圆形
　　　　 context.clip();
　　　　// 在刚刚裁剪的园上画图
context.drawImage(tou,mycanvas.width/2-82,155-82,164,164);
　　　　 context.restore();
　　　　 context.stroke();
drawText();
};
}
img.onerror=function (e) {
alert('图片加载过时');
}
img.src=feedData.bg;
}
//画文本
function drawText() {
//头像
// context.drawImage(feedData.img,0,0);

context.font='28px 微软雅黑';
context.fillStyle='#fbf8f4';
context.textAlign = 'center';
//收信人 mycanvas.width/2-70
context.fillText(feedData.name,mycanvas.width/2,290);

//信息内容
context.font='65px 微软雅黑 blod';
context.fillStyle='#ffffff';
context.textAlign = 'right';
//canvasTextAutoLine(feedData.mes,mycanvas,90,500,62);
context.fillText(feedData.time,mycanvas.width-170,490);

//跑力值
context.font='35px 微软雅黑 blod';
context.fillStyle='#ffffff';
context.textAlign = 'right';
//canvasTextAutoLine(feedData.mes,mycanvas,90,500,62);
context.fillText(feedData.process,mycanvas.width-80,590);
//成绩信息
context.font='50px 微软雅黑';
context.fillStyle='#fbd15b';
context.textAlign = 'center';
context.fillText(feedData.mes,mycanvas.width/2,360);
//进度条
context.fillStyle="#ffffff";
context.lineJoin = "round";
context.lineWidth = 30;
context.fillRect(mycanvas.width/2-150, 570, 350, 20);
//进度条2
context.fillStyle="#fbd15b";
context.lineJoin = "round";
context.lineWidth = 30;
context.fillRect(mycanvas.width/2-150, 570, feedData.process*3.5, 20);


var finalimg=mycanvas.toDataURL("image/png");
// _self.setMum(false);
console.log(finalimg)
//document.getElementById('testimg').src=finalimg;

//上传图片
/*_self.$store.dispatch('goLot',{'url':finalimg}).then(()=>{
_self.closeLottc();
_self.$router.push('ucenter');
})*/
//alert('before axios')
axios.post('https://www.codoon.com/activity/v1/api/upload/dataurl',{
data: finalimg,
type: 'image/png'
})
.then(function (response) {
if(response.status){
//that.$store.commit('UPDATEUSERINFO', response.data);
goShareImg(response.data);
console.log(response);
//alert('response')
}else{
alert(response.description)
}
console.log(response)
})
.catch(function (error) {
alert(error);
});
}
//换行
function canvasTextAutoLine(str,canvas,initX,initY,lineHeight){
var ctx = canvas.getContext("2d");
var lineWidth = 0;
var canvasWidth = canvas.width-90;
var lastSubStrIndex= 0;
for(let i=0;i<str.length;i++){
lineWidth+=ctx.measureText(str[i]).width;
if(lineWidth>canvasWidth-initX){//减去initX,防止边界出现的问题
ctx.fillText(str.substring(lastSubStrIndex,i),initX,initY);
initY+=lineHeight;
lineWidth=0;
lastSubStrIndex=i;
}
if(i==str.length-1){
ctx.fillText(str.substring(lastSubStrIndex,i+1),initX,initY);
}
}
}

drawing();

} catch (error) {
alert('catcherror:'+error);
}
```
其中有个坑就是画上去的图片需要加上可以跨域的代码
 ![My Pic](/images/m1.jpg)
  ![My Pic](/images/m2.jpg)