title: 总结JS的一些小知识
author: coolsong
tags:
  - JavaScript
categories:
  - JavaScript
date: 2019-10-26 22:48:00
---
&nbsp;&nbsp;&nbsp;最近工作一直挺忙的，也没时间学一些东西，而且感觉自己也好久没复习了总结JS的知识了（上次面试之后）加上最近又看到了别人写的一篇blog，发现还是有一些自己没掌握和比较模糊的点，下面就简单总结一下：
<!--more-->
### 1. NULL是对象么？
>看到这个问题的时候自己第一反应感觉不是，虽然typeof null == 'object' 但是要自己说出原因 还是说不太明白

* 答案肯定也是null不是对象

>原因：typeof null 为object是 JS 中存在的一个悠久 Bug。
在 JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象然而 null 表示为全零，所以将它错误的判断为 object 。

### 2. '1'.toString()可以调用么?
>自已最开始是觉得不可以，所以马上试了试结果打脸了，是可以的，之前自己认为这个只能是非字符串类型才能调用（number、object）

>看了官方文档的说明：每个对象都有一个 toString() 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。

>因此这里的'1'.toString()可以分为以下几步  
```JavaScript
var str = new String('1')
str.toString()
str = null
```
*也很容易联想到1.toString()  这里是不能执行的 会报错

>因为在JS中,.点操作符意味着调用Object的属性或者这是一个浮点数。当.跟在一个数字后面就意味着这个数字是一个浮点数，在.后面JS等待着一个数字。
所以在调用.toString()之前，我们需要告诉JS这是就是我们要的数字。需要使用以下的写法
```JavaScript
1..toString() //1.就是1.0就是1
1.0.toString() //与上面同理
(1).toString() //(1)是一个表达式代表这就是一个1
1 .toString()
1//换行，奇葩写法，与上面一样
toString()
```

### 3. Object.is 和 === 区别
>Object.is 自己用的比较少所以先去试了以下它的功能 它和全等类似都是需要值和类型都相等的时候才会为true
>但是自己依稀记得===中会有一些特殊情况
```JavaScript
+0 === -0  //true
+0 === 0 //true
-0 === 0 //true
NaN === NaN // false
```
*Object.is修复了这些问题
```JavaScript
Object.is(NaN, NaN) // true
Object.is(+0, -0) //true
Object.is(+0, 0)// true
Object.is(-0, 0)// false
```
>Object.is的实现
```JavaScript
function is(left, right) {
    if (x === y) {
        return x !== 0 || y !== 0 || 1/x === 1/y
    } else {
        return x !== x && y !== y
    }
}
```
>1/x === 1/y 只有为 0 和 +0 才相等 -Infinity 不等于 Infinity   并且只有NaN不等于其本身

### 4. JS中实现继承
#### 1.利用call和apply
```JavaScript
function parent1 (name) {
    this.name = name
  }
  function child1 (age) {
    parent1.call(this, '小花')
    this.age = age
  }
  console.log(new child1(12))
```
![My Pic](/images/js1.png)
```JavaScript
function parent1 (name) {
    this.name = name
  }
  parent1.prototype.getName = function () {
      console.log(this.name)
  }
  function child1 (age) {
    parent1.call(this, '小花')
    this.age = age
  }
  console.log(new child1(12))
```
![My Pic](/images/js2.png)
>能看到成功的继承到了父元素的属性 但是原型上的方法却无法继承 所以就有了下面的一种方法

#### 2. 利用原型链
```JavaScript
function parent2 (name) {
    this.name = name
  }
  parent2.prototype.getName = function () {
      console.log(this.name)
  }
  function child2 (age) {
    this.age = age
  }
  child2.prototype = new parent2('小花')
  let c2 = new child2(12)
  console.log(c2);
  c2.getName()
```
![My Pic](/images/js3.png)
>可以看到能够继承属性和方法 但是也存在一个问题 每次new的子类其实都是共用的一个父类的原型，导致一些引用类型发生变化之后，所有子类的都会发生变化
![My Pic](/images/js4.png)

#### 3.将前两种组合
```javaScript
function parent3 (name) {
    this.name = name
    this.arr = [1,2,3]
  }
  parent3.prototype.getName = function () {
      console.log(this.name)
  }
  function child3 (age) {
    parent3.call(this, '小花')
    this.age = age
  }
  child3.prototype = new parent3()
  var c3 = new child3();
  var c33 = new child3();
  console.log(c3)
  console.log(c33);
  c3.arr.push(4);
  console.log(c3)
  console.log(c33);
```
![My Pic](/images/js5.png)
> 由于属性都加再了子类上就不会出现上面的问题 基本上就实现了 但是还有可以优化的点（Parent3的构造函数会多执行了一次）

#### 4. 组合继承优化1.0
```JavaScript
child3.prototype = parent3.prototype
```
>只需将赋值改为这种形式 通过在构造函数中打印可以发现次数少了一次赋值时 但是这样赋值之后查看new出来的子类对象可以发现其constructor变为了parent，这肯定不对 于是还需要优化
![My Pic](/images/js6.png)

#### 5. 组合继承优化2.0
```JavaScript
child3.prototype = Object.create(parent3.prototype)
child3.prototype.constructor = child3
```
>这里使用Object.create来创建了一个指向parent.prototype的对象，最重要的是将子类的构造函数指向了子类
>这种方式也叫寄生组合继承
![My Pic](/images/js7.png)