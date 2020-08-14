title: Vue全家桶音乐播放器
author: coolsong
date: 2018-05-27 12:27:59
tags:
	- vue
	- JS
categories:
  - vue.js
---

&nbsp;&nbsp;&nbsp;&nbsp;最近写了一个基于Vue全家桶的音乐播放器，实现了从服务器获取歌曲列表，点击进入播放界面。可以播放、暂停、上一首、下一首。所用到的技术有Vue+VueX+vue-router+express。效果图如下所示：
<!--more-->
![My Pic](/images/mus.png)
![My Pic](/images/mus1.png)
点击播放后 歌词跟着动
![My Pic](/images/mus2.png)
# （一）服务端
## 1.目录结构
&nbsp;&nbsp;&nbsp;&nbsp;通过express应用生成器直接生成的模板，修改app.js中的内容。设置允许不同源可以访问，当路由为某个值时，发送一个对象数组，每一个中包含一首歌的对象。
![My Pic](/images/mus3.png)
## 2.应用生成器搭建
```
//先全局安装express
npm install express-generator -g

//通过express+项目名 生成项目
express myapp

//进入项目 安装依赖
cd myapp 
npm install

//启动
npm start

// http://localhost:3000/ 访问
```
# （二）客户端
## 1.安装脚手架
&nbsp;&nbsp;&nbsp;&nbsp;见我之前的一篇blog中提到过，这里就不赘述了。
## 2.目录结构
![My Pic](/images/mus4.png)


详细代码在github里  想要参考的朋友可以去看[vue-music](https://github.com/songxuy/Vue-music)

