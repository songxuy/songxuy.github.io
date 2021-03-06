title: Node的一些问题
author: coolsong
tags:
  - NodeJs
categories:
  - NodeJs
date: 2019-03-28 22:56:00
---
&nbsp;&nbsp;&nbsp;&nbsp;最近的几次面试都问到了关于node的一些问题。一个是nodeJS的使用场景有哪些，它的优势有哪些呀。还有一个是node与java等其他后端语言的区别。还有就是node的事件机制与浏览器的事件机制有什么区别。
<!--more-->
## 1.node的使用场景
&nbsp;&nbsp;&nbsp;&nbsp;首先看一下node的特点吧。
```
1. 它是一个Javascript运行环境
2. 依赖于Chrome V8引擎进行代码解释
3. 事件驱动
4. 非阻塞I/O
5. 轻量、可伸缩，适于实时数据交互应用
6. 单进程，单线程
```
优点：
　　1. 高并发（最重要的优点）
　　2. 适合I/O密集型应用
缺点：
　　1. 不适合CPU密集型应用；CPU密集型应用给Node带来的挑战主要是：由于JavaScript单线程的原因，如果有长时间运行的计算（比如大循环），将会导致CPU时间片不能释放，使得后续I/O无法发起；
　　　　解决方案：分解大型运算任务为多个小任务，使得运算能够适时释放，不阻塞I/O调用的发起；
　　2. 只支持单核CPU，不能充分利用CPU
　　3. 可靠性低，一旦代码某个环节崩溃，整个系统都崩溃
　　　　原因：单进程，单线程
　　　　解决方案：（1）Nnigx反向代理，负载均衡，开多个进程，绑定多个端口；
　　　　　　　　　（2）开多个进程监听同一个端口，使用cluster模块；
　　4. 开源组件库质量参差不齐，更新快，向下不兼容
　　5. Debug不方便，错误没有stack trace
所以适用场景大概就是满足这些特点的：在高并发、I/O密集、少量业务逻辑的场景

## 2.node与java的对比
首先就是node是单线程而java是多线程的
（1）node.js比Java更快 ：node.js开发快，运行的效率也算比较高，但是如果项目大了就容易乱，而且javascript不是静态类型的语言，
要到运行时才知道类型错误，所以写的多了之后免不了会出现光知道有错但是找不到哪儿错的情况，所以测试就得些的更好更详细。
     java开发慢，但是如果项目大、复杂的话，用java就不容易乱，管理起来比node.js省。

（2）Node.js 前后端都采用Javascript，代表未来发展的趋势，而java则是现在的最流行的服务器端编程语言。
（3）Node.js和Java EE——一种是解释语言，一种是编译语言.

## 3.node的事件机制
Node 10以前：
执行完一个阶段的所有任务
执行完nextTick队列里面的内容
然后执行完微任务队列的内容

Node 11以后： 和浏览器的行为统一了，都是每执行一个宏任务就执行完微任务队列。