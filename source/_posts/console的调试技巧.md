title: console的调试技巧
author: coolsong
date: 2019-12-20 21:40:11
tags:
  - JavaScript
categories:
  - JavaScript
---
&nbsp;&nbsp;&nbsp;console是我么日常工作和学习中用到的最多的一个调试方法，而我们最常用的仅仅是console.log，在查阅文档和别人的介绍博客之后才知道原来其中还包含了挺多方法的，而且有一些还挺实用的，下面就简单的介绍一下这些方法:
<!--more-->
#### console.log
>这是我们最常用的方法，对我自己来说最常用来打印一个或者多个变量的值，这里就不再赘述了。主要来讲一下其他用法

* 格式化打印
>%s: 字符串占位符
>%d: 整数占位符
>%f: 浮点数占位符
>%o: 对象占位符
>%c: CSS样式占位符

```JavaScript
 // 字符串
  console.log('my name is %s', 'sxb')
  // 整数
  console.log('my age is %d', 12)
  // 浮点数
  console.log('It cost %f', 22.5)
  // 对象
  console.log('my obj is %o', {text: 'hello'})
  // CSS
  console.log('I like eat %c%s', 'color:red', 'cookie')
```
![My Pic](/images/consolelog1.png)

#### console.warn
>用法和console.log基本一致，只是该条打印信息属于警告级别，浏览器会做区别处理，并且会加入到警告信息中背景颜色和文字颜色会和普通的有所区别

```JavaScript
console.warn('It is a warning message')

```
![My Pic](/images/consolelog2.png)

#### console.dir
>在打印结果中和console.log对于对象的返回会有一定的区别。用到最多的就是用它来打印dom对象。
* 普通对象
```JavaScript
let obj = { name: 'sxb', age: 12 }
console.log(obj)
console.dir(obj)
```
![My Pic](/images/consolelog3.png)

* dom对象
```JavaScript
console.log(document.body)
console.dir(document.body)
```
![My Pic](/images/consolelog4.png)

#### console.table
>我们在工作中经常会遇到需要调试后端返回的数组或者比较复杂的对象，这个时候就可以使用console.table，让我们能更加好的展示数据，不用自己去点。

```JavaScript
let obj = [
    { name: 'sxb', age: 12},
    { name: 'sxb', age: 12},
    { name: 'sxb', age: 12},
    { name: 'sxb', age: 12}
  ]
  console.log(obj)
  console.table(obj)
```
![My Pic](/images/consolelog5.png)
* 除此之外 这个方法还能传其他参数，第二个可选参数用于筛选表格需要显示的列，默认为全部列都显示。

```JavaScript
let obj = [
    { name: 'sxb', age: 12},
    { name: 'sxb', age: 12},
    { name: 'sxb', age: 12},
    { name: 'sxb', age: 12}
  ]
  console.log(obj)
  console.table(obj, ['name'])
```
![My Pic](/images/consolelog6.png)

#### console.assert
>assert这个单词相信大家也不会陌生，就断言的意思。这个方法只有当第一个参数表达式为false时才会输出,并且作为错误提示

```JavaScript
console.assert(1 < 2, 'hahahaha you are wrong')
 console.assert(1 > 2, 'hahahaha you are wrong')
```
![My Pic](/images/consolelog7.png)


#### console.trace
>这个方法自己之前还没了解过，查了之后才知道该方法用于在控制台中显示当前代码在堆栈中的调用路径，通过这个调用路径我们可以很容易地在发生错误时找到原始错误点，下面就看一个例子吧

```JavaScript
function foo(data) {
  if (data === null) {
    console.trace();
  }
  return data;
}

function bar1(data) {
  return foo(data);
}

function bar2(data) {
  return foo(data);
}

bar1({a: 1, b: 2}); // -> [1, 2]
bar2(null); // -> []
```
![My Pic](/images/consolelog8.png)

>我们从代码的逻辑看，首先调用的bar1 而bar1中又会调用foo。接着调用bar2中的foo，直到我们console.trace的地方。追踪到的路径就为 foo - bar2

#### console.count
>简单来讲这个方法就是一个计数器.用于记录调用次数，并将记录结果打印到控制台中。其接收一个可选参数console.count(label)，label表示指定标签，该标签会在调用次数之前显示。

```JavaScript
for(let i = 0; i < 10; i++) {
  console.count('label')
}
```
![My Pic](/images/consolelog9.png)

#### console.time 和 console.timeEnd
>这两个是关于时间打印的方法，需要配合起来使用。console.time方法是作为计算的起始时间，console.timeEnd是作为计算的结束时间，并将执行时长显示在控制台。如果一个页面有多个地方需要使用到计算器，则可以为方法传入一个可选参数label来指定标签，该标签会在执行时间之前显示。

```JavaScript
let sum = 0
console.time()
for(let i = 0; i < 1000; i++) {
  sum += sum
}
console.timeEnd()
```
![My Pic](/images/consolelog10.png)

*当没有传递参数时则会默认default参数名


#### 检查DOM元素
>控制台提供了inspect()方法让我们可以直接从控制台中检查一个DOM元素。

>inspect($('selector'))：将检查与选择器匹配的元素，并且会自动跳转到Chrome Developer Tools的Elements选项卡中。例如inspect($('#content'))将检查id为content的元素


![My Pic](/images/consolelog11.gif)



