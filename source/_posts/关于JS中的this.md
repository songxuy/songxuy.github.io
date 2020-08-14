title: 关于JS中的this
author: coolsong
tags:
  - JS
  - this指向
categories:
  - Javascript
date: 2018-04-04 14:40:00
---

&nbsp;&nbsp;&nbsp;&nbsp;在Js中，this的指向问题一直都是一个很让人头疼的问题，一会指向全局，一会又指向调用它的对象。在网上看了一些资料，和一些大牛的讲解。在这里做一个简单的总结。([参考文章链接](https://www.qdfuns.com/article/47725/e18701ccbbb2c0909bac8e906a42a38f.html))

# 1.默认绑定
在默认绑定的情况下，this会指向window对象
```Javascript
var num = 2;
function showNum(){
	console.log(this.num);
	console.log(this);//window
}
showNum();//2 这里this就指向window对象 但是如果是构造函数情况又会有所不同
```
<!-- more -->
在严格模式下，this会绑定到undefined上
```Javascript
"use strict";
var num = 2;
function showNum(){
	console.log(this.num);
	console.log(this);//
}
showNum();//这里会报错
```

# 2.隐式绑定
this会指向调用它的上一级对象。this 是在运行时被确定，而不是在定义时被确定。(箭头函数相反)
```Javascript
var obj = {
a: 2,
foo: function(){
	console.log(this.a);
}
};
obj.foo();//2 
```
这里obj调用foo方法，所以this指向obj对象
```Javascript
var obj = {
a: 2,
foo: function(){
	console.log(this.a);
}
};
var bar = obj.foo;
var a = 'hello'
bar(); //hello
```
这里bar是函数得别名 是全局函数又因为this是在运行时决定的 所以指向window对象

```Javascript
function foo() {
console.log( this.a );
}
function doFoo(fn) { 
fn(); 
}
var obj = {
a: 2,
foo: foo
};
var a = "hello";
doFoo( obj.foo );//hello
```
这里真正调用，运行foo的doFoo函数 ，因此this指向doFoo，所以指向window对象
```Javascript
function foo() {
console.log( this.a );
}
var obj = {
a: 2,
foo: foo
};
var a = "hello";
setTimeout( obj.foo, 100 );//hello
```
这里跟上面类似，正真调用foo的是一个全局函数 所以this会指向window对象

# 3.改变this指向
改变this指向通常有这几种方法,call、bind、apply、以及new方法
前面三个都比较类似，只是参数和返回值有点区别，就举一个例子
```Javascript
function foo() {
console.log( this.a );
}
var obj = {
a: 2,
foo: foo
};
var a = "hello";
setTimeout(obj.foo.call(obj),100 );//2
```
这里使用call方法将this指向obj对象
```Javascript
function Person (name) {
    console.log(this) // window
    this.name = name;
    console.log(this) //windiw
}
Person('inwe');
console.log(window.name) //inwe
```
这里相当于普通函数 里面的this指向window对象
```Javascript
function Person (name) {
	console.log(this) //pepole
      this.name = name
      console.log(this) //people
      self = this
  }
  var people = new Person('iwen')
  console.log(this.self) //people
```
这里使用new改变了this指向，将this由window指向Person的实例对象people
```Javascript
示例代码一:

function fn()
{
this.user = '追梦子';
return {};
}
var a = new fn;
console.log(a.user); //undefined
示例代码二:

function fn()
{
this.user = '追梦子';
return function(){};
}
var a = new fn;
console.log(a.user); //undefined
示例代码三:

function fn()
{
this.user = '追梦子';
return 1;
}
var a = new fn;
console.log(a.user); //追梦子
示例代码四:

function fn()
{
this.user = '追梦子';
return undefined;
}
var a = new fn;
console.log(a.user); //追梦子
```
这里表明 如果构造函数中返回值是一个对象，那么 this 指向的就是那个返回的对象，如果返回值不是一个对象那么 this 还是指向函数的实例

# 4.箭头函数中的this
箭头函数中的this是定义时就决定，而非运行时。
（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

```Javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });//42
```
这个箭头函数的定义生效是在foo函数生成时，而它的真正执行要等到 100 毫秒后。如果是普通函数，执行时this应该指向全局对象window，这时应该输出21。但是，箭头函数导致this总是指向函数定义生效时所在的对象（本例是{id: 42}），所以输出的是42。
```Javascript
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);
  // 普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);//3
setTimeout(() => console.log('s2: ', timer.s2), 3100);//0
```
这里箭头函数指向定义时的作用域，即Timer。而普通函数指向window对象
