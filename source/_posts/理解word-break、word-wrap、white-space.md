title: 理解word-break、word-wrap、white-space
author: coolsong
tags:
  - Css
categories: 
  - Css
date: 2019-08-11 01:12:13
---
&nbsp;&nbsp;&nbsp;我们在日常的工作中经常会遇到需要对文本显示的格式进行处理，比如控制换行、保留其中的空格和换行等。而与之相关的css属性主要包括了word-break、word-wrap、white-space这三个。下面就简单总结一下这三个属性的一些基本用法：
<!--more-->
### 测试HTMl
* * *
* 主要是一个定宽的box，里边包含了一些换行,空格,以及一个较长的单词

```Html
<div id="box" style="border: 1px dashed #000;position: relative;width:130px;display: inline-block;">
test&nbsp;&nbsp;<br/>
ha ha ha I think i'm the king of the responsibilityworld
</br>
测试&nbsp;&nbsp;<br/>
哈 哈 哈 我认为我是这个有责任的世界的王
</div>
```
>目前的基本样式如下图所示：

![My Pic](/images/word1.png)

>目前可以看到，换行和单个空格已经生效了，而连续的空格会被缩减成一个，换行符也全都无效。句子超过一行后会自动换行，而长度超过一行的单个单词会超出边界。接下来我们看下， 给它上面三个css属性赋值后会出现什么变化。
### write-space

* * *
>这个属性是用来控制空白字符的显示的，同时还能控制是否自动换行。它有五个值：normal | nowrap | pre | pre-wrap | pre-line。因为默认是normal，所以我们主要研究下其它四种值时的展现情况。


* white-space:nowrap
<table><tr><td ><center><img src="/images/word1.png" ></center></td><td ><center><img src="/images/word2.png" ></center></td></tr></table>

>可以看到这个属性作用就是不换行，除非是<br/> 多个空格也同样的会合并为一个

* white-space:pre

<table><tr><td ><center><img src="/images/word1.png" ></center></td><td ><center><img src="/images/word3.png" ></center></td></tr></table>

>可以看到空格和换行符保留下来了，自动换行还是没有了

* white-space:pre-wrap
<table><tr><td ><center><img src="/images/word1.png" ></center></td><td ><center><img src="/images/word4.png" ></center></td></tr></table>

>可以看到其实效果有点像pre + wrap 保留空格和换行符，而且同时自动换行

* white-space:pre-line
<table><tr><td ><center><img src="/images/word1.png" ></center></td><td ><center><img src="/images/word5.png" ></center></td></tr></table>

>空格被合并了，但是换行符可以发挥作用 同时也可以自动换行

根据上面的情况总结为下表

| 是否发挥作用 | 换行符 | 空格 |自动换行  | <br/>, nbsp; |
| --- | --- | --- | --- | --- |
| normal | x | x（合并） | √ | √ |
| nowrap | x | x（合并） | x | √ |
| pre | √ | √  | x | √ |
| pre-wrap | √ | √ | √ | √ |
| pre-line | √ | x | √ | √ |

### word-break

* * *
>这个属性是控制单词如何被拆分换行的。它有三个值：normal | break-all | keep-all。

* word-break:keep-all
<table><tr><td ><center><img src="/images/word1.png" ></center></td><td ><center><img src="/images/word6.png" ></center></td></tr></table>

>所有“单词”一律不拆分换行，注意，我这里的“单词”包括连续的中文字符（还有日文、韩文等），或者可以理解为只有空格可以触发自动换行

* word-break:break-all
<table><tr><td ><center><img src="/images/word1.png" ></center></td><td ><center><img src="/images/word7.png" ></center></td></tr></table>
 
 >所有单词碰到边界一律拆分换行，不管你是一行都显示不下的单词，还是很短的单词，只要碰到边界，都会被强制拆分换行。


### word-wrap（overflow-wrap）

* * *
>这个属性也是控制单词如何被拆分换行的，实际上是作为word-break的互补，它只有两个值：normal | break-word

* word-wrap:break-word
<table><tr><td ><center><img src="/images/word7.png" ></center></td><td ><center><img src="/images/word8.png" ></center></td></tr></table>
>只有当一个单词一整行都显示不下时，才会拆分换行该单词。所以我觉得overflow-wrap更好理解好记一些，overflow，只有长到溢出的单词才会被强制拆分换行！

>[参考链接-掘金](https://juejin.im/post/5b8905456fb9a01a105966b4)
