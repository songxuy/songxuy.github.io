title: 关于new Date的坑
author: coolsong
tags:
  - Javascript
categories:
  - JS
date: 2019-01-24 22:30:00
---
&nbsp;&nbsp;&nbsp;&nbsp;今天遇到了一个线上的bug，就是活动中在某个时间段会显示宝箱，但是ios用户却没有，Android用户却有，这时就要开始找原因了，之前测的时候应该也是没有问题的。
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;终于在debug中发现了一个问题，在ios中new Date('2019-01-16 20:16:45')这样会返回invalid date 没法识别用-隔开的日期，于是自己查了一下，在Safari上也有这个问题。
![My Pic](/images/time.png)
&nbsp;&nbsp;&nbsp;&nbsp;对于我这个项目的话比较明确后端返回的格式是怎么样的，于是就自己通过replace全局替换了一下。这种问题还是挺难发现的，特别是自己之前不知道的话，算是一种积累吧
