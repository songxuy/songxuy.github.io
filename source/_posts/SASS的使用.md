title: SASS的使用
author: coolsong
tags:
  - CSS
categories:
  - css
date: 2019-05-24 21:59:00
---
&nbsp;&nbsp;&nbsp;&nbsp;SASS和LESS都知道并且使用过less，但是在实际的工作中，自己并没有使用到这两种技术。基本上都是纯写的css，但最近自己的css代码被leader看了之后，说写的太冗余了，上面的选择器太长了。建议改成SASS，当时自己心态有点崩，写之前你也没要求这样哇，现在改不是很花时间么，但自己还是要改。最终还是逃不过真香警告，确实改成SASS之后，感觉结构清晰多了，还是挺好用的哈哈。下面就介绍一下自己的使用吧。
<!--more-->
## 1 单文件的使用
自己在尝试的过程中，首先把之前的一个静态页面改成了SASS。下面是具体的使用步骤。
### 1.1 下载
按照官网的步骤，首先要下载Ruby，直接通过官网下载安装包。安装之后就可以使用gem命令下载SASS和COMPASS
```
gem install sass
gem install compass
```
就可以使用sass命令了，下面是一些常用的命令。
```
//单文件转换命令
sass input.scss output.css

//单文件监听命令
sass --watch input.scss:output.css

//如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
sass --watch app/sass:public/stylesheets
```

* SASS有四种编译排版格式
1.nested编译之后会保留之前的嵌套格式,类似这样
```
.box {
  width: 300px;
  height: 400px; }
  .box-title {
    height: 30px;
    line-height: 30px; }
```
2.expanded编译之后和正常我们写的css格式类似
```
.box {
  width: 300px;
  height: 400px;
}
.box-title {
  height: 30px;
  line-height: 30px;
}
```
3.compact编译之后每一条样式都是横向排列
```
.box { width: 300px; height: 400px; }
.box-title { height: 30px; line-height: 30px; }
```
4.compressed通过名字可以知道就是压缩版去掉空格换行

我自己使用的时候不太通过命令行来操作，而是使用可视化工具
Koala，可以从官网下载。[koala](https://www.sass.hk/skill/koala-app.html"%3Ehttps://www.sass.hk/skill/koala-app.html)
下载安装之后就可以直接添加项目，选择生成的路径，使用编译的格式等等。
![sass](/images/sass1.png)


## 2.在vue-cli中使用
其实也很简单，以vue-cli3为例。我们只需要安装相应的loader就可以使用了。
```
npm install --save sass-loader
npm install --save node-sass
```
然后在运行项目就可以使用啦！
![sass](/images/sass2.png)
## 3.SASS中好用的方法
### 3.1 Mixins
这个方法感觉就是使用一个函数，可以大大的增加代码的复用，还可以传递参数，指定默认值。引用之后同样的可以添加自定义的样式。
```scss
@mixin title-style($color, $background: #eee) {
  margin: 0 0 20px 0;
  font-family: $font-serif;
  font-size: 20px;
  font-weight: bold;
  text-transform: uppercase;
  color: $color;
  background: $background;
 }
 
 //使用
 section.main h2 {
    @include title-style(#c63);
 }
  section.main h2 {
    @include title-style(#39c, #333);
 }
```
### 3.2@extend
这个方法有一点继承的赶脚，但是不太建议多用，用多了生成的css就跟我自己写的一样了0.0。
```SCSS
.alert-positive {
 @extend .alert;
 background: #9c3; 
}
//这样就继承了alert类的样式
//编译之后生成的代码

.alert, .alert-positive {
  padding: 15px;
  font-size: 1.2em;
  font-weight: normal;
  text-transform: uppercase;
  line-height: 1;
  letter-spacing: 3px;
  text-align: center;
  color: #fff;
  background: #ea4c89;
  -webkit-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
  moz-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  border-radius: 10px; 
}
.alert-positive { 
  background: #9c3;
}

//就会生成两个样式，当继承的元素多了，就会多生成很多重复的选择器
```
### 3.3 嵌套媒体查询

* 可以直接将媒体查询写到我们希望应用到的元素中，也是很方便了。同样的可以加上变量，混合Mixins，以及循环或者条件语句。
```SCSS
//定义变量
$width-small: 400px; 
$width-medium: 760px; 
$width-large: 1200px; 
//一个带有参数以及条件语句的mixin
@mixin responsive($width) { 
  @if $width == wide-screens { 
    @media only screen and (max-width: $width-large) { 
      @content; 
    } 
  } 
  @else if $width == medium-screens {
     @media only screen and (max-width: $width-medium) {
        @content; 
      } 
    } 
  @else if $width == small-screens { 
    @media only screen and (max-width: $width-small) {
       @content; 
      } 
    } 
  }
#content {
   float: left; 
   width: 70%; 
   @include responsive(wide-screens) {
      width: 80%;
   } 
   @include responsive(medium-screens) {
      width: 50%; 
      font-size: 14px;
   } 
   @include responsive(small-screens) {
      float: none; 
      width: 100%; 
      font-size: 12px;
   } 
}

//编译之后就会生成这样的代码
#content {
  float: left;
  width: 70%;
}
@media only screen and (max-width: 1200px) {
  #content {
    width: 80%;
  }
}
@media only screen and (max-width: 760px) {
  #content {
    width: 50%;
    font-size: 14px;
  }
}
@media only screen and (max-width: 400px) {
  #content {
    float: none;
    width: 100%;
    font-size: 12px;
  }
}
```

