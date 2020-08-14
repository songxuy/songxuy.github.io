title: Js中作用域和闭包
author: coolsong
tags:
  - JS
  - 作用域
  - 闭包
categories:
  - Javascript
date: 2018-02-25 18:03:00
---
&nbsp;&nbsp;&nbsp;&nbsp;最近看了一些前端的面试题，发现自己的基础不是很扎实。这两天恶补了关于js中作用域和闭包的知识。下面分享几个例子。
<h2 style="color:#6795B5;">1.局部变量和全局变量</h2>
```javascript
<script type="text/javascript">
	var num=10;
		function get(){
			alert(num);
		}
		get();//答案是:10
</script>
```
<!-- more-->
这里在最外层var num=10相当于定义了一个全局变量，都可以访问。相反在函数内部使用var定义的变量，外部访问时就会报错。
```javascript
<script type="text/javascript">
	var num=10;
		function get(){
			alert(num);
			var num =20;
			alert(num);
		}
		get();//答案是:undefined  20
	var num=10;
		function get(){
			alert(num);
			num =20;
			alert(num);
		}
		get();//答案是:10 20
</script>
```
在js中函数内部申明变量会先进行预编译，所以第一个里并没有访问到外部的变量，而是返回undefined(注意不是报错)。第二个中并没有申明而是直接重新赋值。
```javascript
<script type="text/javascript">
	var func=function(){
			alert('调用外部函数');
		};
	var foo=function(){
		func();
		var func=function(){
		alert('调用内部函数');
		}
		func();
		};
		foo();//答案会报错
</script>
```
在js中匿名函数不会预编译 其他的与上面同理
<h2 style="color:#6795B5;">2.闭包</h2>
```javascript
<script type="text/javascript">
	function getnumber(){
			var number=10;
			var show=function(){
				number++;
				alert(number);
			}
			return show;
		}
	var shonumber=getnumber();
	var shonumber1=getnumber();
	shonumber();
	shonumber();
	shonumber1();//答案是:11 12 11
</script>
```
这是一个经典的闭包。我们先看一看闭包的作用：（1）可以获取本函数外部的变量（2）这些变量一直保存在内存中，页面关闭释放
因此number变量保存在内存中，所以为 11 12 11
```javascript
<script type="text/javascript">
	var name = "outer";
　　var object = {
　　　　name : "inner",
　　　　agetNameFunc : function(){
　　　　　　return function(){
　　　　　　　　return this.name;
　　　　　　};
　　　　}
　　};
　　alert(object.getNameFunc()());//答案是:outer
</script>
```
this对象是在运行时基于函数的执行环境绑定的：在全局函数中，this等于window，而当函数被作为某个对象调用时，this等于那个对象。不过，匿名函数具有全局性，因此this对象同常指向window