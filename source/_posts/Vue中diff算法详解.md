title: Vue中diff算法详解
author: coolsong
tags:
  - Vue
categories:
  - Vue
date: 2020-08-06 21:21:00
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最近看到了一篇关于Vue diff算法的详解的文章，写的真的挺好的，解答了很多之前自己对一些概念和一些比较过程的过程中模糊的地方。自己也来总结一下，理下思路：
<!--more-->
#### 前言
* 数据发生变化时，vue是怎么更新视图的？
>我们应该都知道vue使用了虚拟dom来减轻频繁的操作修改真实的dom元素带来的性能的消耗。在更新视图时，vDom不会进行频繁修改，然后一次性比较并修改真实的dom中需要修改的dom。其本质vDom映射到真实的dom元素。其主要需要经历vnode的create、diff、patch。

* * *

* VDom和真实Dom的区别
>VDom本质上是从真实的Dom的数据中抽出来形成的一个类似于对象的结构，而真实的Dom对应的就是相应的Dom节点。VDom不会立马进行排版和重绘操作，同时可以有效降低大面积真实Dom的重绘和排版，可以只渲染局部。

* * *

* vue2.x和Vue3.x渲染器的diff算法
>总的来说，diff算法有一下的过程：
>* 同级比较，再比较子节点
>* 先判断一方有子节点一方没有子节点的情况(如果新的children没有子节点，将旧的子节点移除)
>* 比较都有子节点的情况(核心diff)
>* 递归比较子节点
>
>正常Diff两个树的时间复杂度是**O(n^3)**，但实际情况下我们很少会进行**跨层级的移动DOM**，所以Vue将Diff进行了优化，从**O(n^3) -> O(n)**，只有当新旧children都为多个子节点时才需要用核心的Diff算法进行同层级比较。
>Vue2的核心Diff算法采用了**双端比较**的算法，同时从新旧children的两端开始进行比较，借助key值找到可复用的节点，再进行相关操作。相比React的Diff算法，同样情况下可以减少移动节点次数，减少不必要的性能损耗，更加的优雅。
>Vue3.x借鉴了 **ivi算法**和 **inferno算法**在创建VNode时就确定其类型，以及在mount/patch的过程中采用位运算来判断一个VNode的类型，在这个基础之上再配合核心的Diff算法，使得性能上较Vue2.x有了提升。(实际的实现可以结合Vue3.x源码看。)

#### DIff流程图
>总的来说就是触发更新时，比较两个节点是否相同，不同则直接返回新的，相同再去比较子节点。

![Image](/images/diff.png)

#### 详细分析
##### Diff
* 初次比较两个原始节点的过程（主要是核心部分）首先传入新节点和之前的旧节点

```JavaScript
function diff (oldVnode, newnode) {
    // some code
    if (sameVnode(oldVnode, newnode)) {
    	patchVnode(oldVnode, newnode)
    } else {
    	const oEl = oldVnode.el // 当前oldVnode对应的真实元素节点
    	let parentEle = api.parentNode(oEl)  // 父元素
    	createEle(newnode)  // 根据Vnode生成新元素
    	if (parentEle !== null) {
            api.insertBefore(parentEle, newnode.el, api.nextSibling(oEl)) // 将新元素添加进父元素
            api.removeChild(parentEle, oldVnode.el)  // 移除以前的旧元素节点
            oldVnode = null
    	}
    }
    // some code 
    return newnode
}
```
##### isSameNode
* 之后判断两个节点是否值得比较（是否相同），相同则进行下一步的比较（深入检查其子节点），不同则直接进行替换。
```JavaScript
function isSameNode (a, b) {
  return (
    a.key === b.key &&  // key值
    a.tag === b.tag &&  // 标签名
    a.isComment === b.isComment &&  // 是否为注释节点
    // 是否都定义了data，data包含一些具体信息，例如onclick , style
    isDef(a.data) === isDef(b.data) &&  
    sameInputType(a, b) // 当标签是<input>的时候，type必须相同
  )
}
```
##### patchNode
>对子节点进行深入的比较，会进行以下的操作
* 找到真实的dom设为el
* 判断oldVnode和vnode是否指向同一个对象，如果是 则返回。
* 如果它们两个都有文本节点且不相等，则将真实dom的文本节点设为vnode的文本节点
* 如果oldVnode有子节点而Vnode没有，则删除el的子节点
* 如果oldVnode没有子节点而Vnode有，则将Vnode的子节点真实化之后添加到el
* 如果两者都有子节点，则执行updateChildren函数比较子节点，这一步很重要（即核心diff）

```JavaScript
patchVnode (oldVnode, vnode) {
    const el = vnode.el = oldVnode.el
    let i, oldCh = oldVnode.children, ch = vnode.children
    if (oldVnode === vnode) return
    if (oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text) {
        api.setTextContent(el, vnode.text)
    }else {
        updateEle(el, vnode, oldVnode)
    	if (oldCh && ch && oldCh !== ch) {
            updateChildren(el, oldCh, ch)
    	}else if (ch){
            createEle(vnode) //create el's children dom
    	}else if (oldCh){
            api.removeChildren(el)
    	}
    }
}
```

##### updateChildren
>这个核心diff算法会比较复杂一点，我们先来看看具体的代码吧

```JavaScript
updateChildren (parentElm, oldCh, newCh) {
    let oldStartIdx = 0, newStartIdx = 0
    let oldEndIdx = oldCh.length - 1
    let oldStartVnode = oldCh[0]
    let oldEndVnode = oldCh[oldEndIdx]
    let newEndIdx = newCh.length - 1
    let newStartVnode = newCh[0]
    let newEndVnode = newCh[newEndIdx]
    let oldKeyToIdx
    let idxInOld
    let elmToMove
    let before
    while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
        if (oldStartVnode == null) {   // 对于vnode.key的比较，会把oldVnode = null
            oldStartVnode = oldCh[++oldStartIdx] 
        }else if (oldEndVnode == null) {
            oldEndVnode = oldCh[--oldEndIdx]
        }else if (newStartVnode == null) {
            newStartVnode = newCh[++newStartIdx]
        }else if (newEndVnode == null) {
            newEndVnode = newCh[--newEndIdx]
        }else if (sameVnode(oldStartVnode, newStartVnode)) {
            patchVnode(oldStartVnode, newStartVnode)
            oldStartVnode = oldCh[++oldStartIdx]
            newStartVnode = newCh[++newStartIdx]
        }else if (sameVnode(oldEndVnode, newEndVnode)) {
            patchVnode(oldEndVnode, newEndVnode)
            oldEndVnode = oldCh[--oldEndIdx]
            newEndVnode = newCh[--newEndIdx]
        }else if (sameVnode(oldStartVnode, newEndVnode)) {
            patchVnode(oldStartVnode, newEndVnode)
            api.insertBefore(parentElm, oldStartVnode.el, api.nextSibling(oldEndVnode.el))
            oldStartVnode = oldCh[++oldStartIdx]
            newEndVnode = newCh[--newEndIdx]
        }else if (sameVnode(oldEndVnode, newStartVnode)) {
            patchVnode(oldEndVnode, newStartVnode)
            api.insertBefore(parentElm, oldEndVnode.el, oldStartVnode.el)
            oldEndVnode = oldCh[--oldEndIdx]
            newStartVnode = newCh[++newStartIdx]
        }else {
           // 使用key时的比较
            if (oldKeyToIdx === undefined) {
                oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx) // 有key生成index表
            }
            idxInOld = oldKeyToIdx[newStartVnode.key]
            if (!idxInOld) {
                api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
                newStartVnode = newCh[++newStartIdx]
            }
            else {
                elmToMove = oldCh[idxInOld]
                if (elmToMove.sel !== newStartVnode.sel) {
                    api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
                }else {
                    patchVnode(elmToMove, newStartVnode)
                    oldCh[idxInOld] = null
                    api.insertBefore(parentElm, elmToMove.el, oldStartVnode.el)
                }
                newStartVnode = newCh[++newStartIdx]
            }
        }
    }
    if (oldStartIdx > oldEndIdx) {
        before = newCh[newEndIdx + 1] == null ? null : newCh[newEndIdx + 1].el
        addVnodes(parentElm, before, newCh, newStartIdx, newEndIdx)
    }else if (newStartIdx > newEndIdx) {
        removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx)
    }
}
```
> 这里采用的是双端比较，即分别设置一头一尾两个变量指向oldvnode和vnode。然后开始循环比较4个节点（4种比较的方式oldstart，start、oldstart，end、oldend，start、oldend，end）。循环结束的条件oldstart>oldend或者start>end。
> 如果oldstart和end匹配上了，则真实dom中第一个节点会移到最后。oldstart向后移，end向前移
> 如果oldend和start匹配上了，则真实dom中最后一个节点会移到最前面，匹配上的两个指针向中间移动
> 如果四种匹配没有一对是成功的，分为两种情况
> > 如果新旧子节点都存在key，那么会根据oldChild的key生成一张hash表，用S的key与hash表做匹配，匹配成功就判断S和匹配节点是否为sameNode，如果是，就在真实dom中将成功的节点移到最前面，否则，将S生成对应的节点插入到dom中对应的oldS位置，S指针向中间移动，被匹配old中的节点置为null。
> > 
> > 如果没有key,则直接将S生成新的节点插入真实DOM（ps：这下可以解释为什么v-for的时候需要设置key了，如果没有key那么就只会做四种匹配，就算指针中间有可复用的节点都不能被复用了）



#### 参考文章

[详解vue的diff算法](https://juejin.im/post/6844903607913938951)