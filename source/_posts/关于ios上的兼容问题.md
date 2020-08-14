title: 关于ios上的兼容问题
author: coolsong
tags:
  - vue
  - webpack
categories:
  - 经验分享
date: 2019-01-04 22:35:00
---
&nbsp;&nbsp;&nbsp;最接近呢？自己开发的一个基于vue的项目上线之后出现了一个很神奇的问题，这个活动在一些ios上会出现白屏的问题，比如ios9和10.但是在Android和PC端都是正常的,查看了自己引用的一些包以及打包之后的文件之后并没有发现什么问题。而且并不知道报了什么错。
<!--MORE-->
&nbsp;&nbsp;&nbsp;于是没有办法，只有通过手机连到mac上看看，连上之后发现控制报了一个错，大概就是不能识别const。但是我明明已经用webpack中配置的babel转译了。我又再打包之后的js中搜了一下，发现真的有const。由于之前使用这套架子没有出现过这个问题，于是想是不是这个活动引入的第三方插件的问题，通过自己一个一个的注释，终于发现了问题的所在。是由于我在vue-cli3的配置中没有对swiper插件进行转译。于是在bebal中自己加上了特别对swiper以及它的一个依赖进行转译的代码，这下在上到服务器上就行了。
```javascript
transpileeDependencies:[
	"swiper",
	"dom7",
	"ssr-windom"
]
```
