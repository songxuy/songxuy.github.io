title: Jquery中的属性获取
author: coolsong
tags:
  - jquery
  - 属性获取
categories:
  - Javascript
date: 2018-03-17 11:14:00
---

&nbsp;&nbsp;&nbsp;昨天做了一套题，发现自己jquery中好多东西都有点忘了。其中印象比较深的就是prop和attr的区别，当时就蒙逼了，prop感觉自己就没用过，只知道attr是获取属性，设置属性。回来就查了一些资料，下面就总结一下。

# 1.手册上的说明
```Javascript
attr(attributeName)         //获取第一个匹配元素的属性值
attr(properties)   //将一个“名/值”形式的对象设置为所有匹配元素的属性
attr(key,value)    //为所有匹配的元素设置一个属性
attr(key,fn)       //为所有匹配的元素设置一个计算属性

prop(propertyName) //获取匹配元素集中的第一个元素的属性值
prop(key,value)    //为所有匹配的元素设置一个属性
```
<!-- more -->
看了之后，就觉得除了感觉attr方法要多之外，就差不多。还有就是attributeName和propertyName有什么区别
官方文档上的说明是：具有 true 和 false 两个属性的属性，如 checked, selected 或者 disabled 使用prop()，其他的使用 attr()

```Javascript
<input type="checkbox" />
<script>
    $(function() {
        $('input').click(function() {
            alert($(this).attr('checked'));
        });
    });
</script>  //不管选中还是没选中都是undefined

<input type="checkbox" />
<script>
    $(function() {
        $('input').click(function() {
            alert($(this).prop('checked'));
        });
    });
</script>  //返回true，false
```

# 2.网上的总结  [链接](http://blog.csdn.net/qq_35809245/article/details/54389797)
## 2.1 属性的定义:
根据W3C手册所述：属性包括，标准属性：id class style title 语言属性 lang dir以及某些特定的元素的固有的属性，比如 a 的 href target 属性，input元素的 radio checked type alt src disabled value 等 ，img标签的width height src alt 等，不存在的属性叫做新增属性。 
## 2.2 attr():
可以设置元素的属性（也就是给元素新增加一个原来并不存在的属性）也可以获取元素的本来就有的属性以及额外设置的属性。如果要获取的属性没有设置，那么获取到的结果是 undefined; 
## 2.3 prop()：
可以设置元素的属性（HTML固有的属性，可以给元素添加属性）也可以获取元素的固有的属性值，如果是额外设置的其他属性，则无法通过prop（ ）获取。 
## 2.4 设置元素属性： 
attr （“属性名”，“属性值”） 既可以设置元素固有的属性，也可以设置元素本来不存在的属性，比如attr（）可以给下面代码div行设置固有的HTML属性，包括 ttle id class 等，也可以设置原先不存在的属性，也就是造一个新的属性，比如 index aaa 等，任何都行；而 prop( “属性名”，“属性值”)只能设置固有的HTML属性。 
获取元素属性值： 
attr(“属性值“）只能获取已经设置在元素身体上的属性值，包括固有属性和新增属性，没有设置的属性将无法获取到值，结果全部是undefined; 

# 3.举例
```Javascript
<div class="cls1 cls2" id="dv" title="我是一个div"></div>
<script type="text/javascript">
	$("#dv").attr("index","1")
	console.log($("#dv").prop("index"));
	console.log($("#dv").attr("index"));
</script>  //输出结果是:undefined  1
```
说明prop不能获取到非固有属性

```Javascript
<input type="checkbox" value="复选框" id="chk"/>这是一个复选框
<script type="text/javascript">
	console.log($("#chk").prop("value"));
    console.log($("#chk").attr("value"));
    console.log($("#chk").prop("checked"));
    console.log($("#chk").attr("checked"));
</script>
/*
第一次点击输出:复选框 复选框 true undefined
第二次点击输出:复选框 复选框 false undefined
*/
```
说明attr不能获取到没有定义的属性

# 4.用法
1.如果想要通过attr()获取元素的属性值，那么该属性必须显式的设置在HTML代码中或者通过attr新增的属性才能被获取到，如果没有设置，那么将返回undefined
2 如果通过prop（）获取属性值，那么该属性只能是HTMl的固有属性，无论是否显式的设置，都可以获取其对应的属性值，如果是额外增加的属性，那么将无法获取。
对于HTML元素本身就带有的固有属性，在处理时，使用prop方法。
对于HTML元素我们自己自定义的DOM属性，在处理时，使用attr方法。
具有 true 和 false 两个属性的属性，如 checked, selected 或者 disabled 使用prop()