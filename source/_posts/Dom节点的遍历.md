title: Dom节点的遍历
author: coolsong
tags:
  - JavaScript
categories: 
  - JS
date: 2019-07-09 22:20:00
---
&nbsp;&nbsp;&nbsp;在网上看到了一道关于dom节点遍历的题，自己也有点忘记了，所以决定重新写一个遍历函数，顺便复习一下广度遍历和深度遍历。下面就是两类实现dom节点遍历的方法：
<!--more-->
## 1.基本DOM结构
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>js_question</title>
</head>
<body>
  <div class="parent">
    <div class="child-1">
      <div class="child-1-1">
        <div class="child-1-1-1">
          a
        </div>
      </div>
      <div class="child-1-2">
        <div class="child-1-2-1">b</div>
      </div>
      <div class="child-1-3">c</div>
    </div>
    <div class="child-2">
      <div class="child-2-1">
        d
      </div>
      <div class="child-2-2">e</div>
    </div>
    <div class="child-3">
      <div class="child-3-1">f</div>
    </div>
  </div>
</body>
</html>
```
## 2.广度优先遍历

* 主要是通过队列来模拟实现的
```JavaScript
let node = document.querySelector('.parent')
let widthTraversal2 = (node) => {
  let nodes = []
  let stack = []
  if (node) {
    stack.push(node)
    while (stack.length) {
      let item = stack.shift()
      let children = item.children
      nodes.push(item)
      for (let i = 0; i < children.length; i++) {
        stack.push(children[i])
      }
    }
  }
  return nodes
}
console.log(widthTraversal2(node))
// stack数组中的变化  先进先出
/*
[parent]
[child1, child2, child3]
[child2,child3,child1-1,child1-2,child1-3]
[child3,child1-1,child1-2,child1-3,child2-1]
......
*/
```

* 最终结果
![My Pic](/images/dom1.png)

## 3.深度优先遍历

* 主要是通过栈来模拟实现
```javascript
let node = document.querySelector('.parent')
let deepTraversal1 = (node, nodeList = []) => {
  if (node !== null) {
    nodeList.push(node)
    let children = node.children
    for (let i = 0; i < children.length; i++) {
      deepTraversal1(children[i], nodeList)
    }
  }
  return nodeList
}
let deepTraversal2 = (node) => {
    let nodes = []
    if (node !== null) {
      nodes.push(node)
      let children = node.children
      for (let i = 0; i < children.length; i++) {
        nodes = nodes.concat(deepTraversal2(children[i]))
      }
    }
    return nodes
  }
// 非递归
let deepTraversal3 = (node) => {
  let stack = []
  let nodes = []
  if (node) {
    // 推入当前处理的node
    stack.push(node)
    while (stack.length) {
      let item = stack.pop()
      let children = item.children
      nodes.push(item)
      for (let i = children.length - 1; i >= 0; i--) {
        stack.push(children[i])
      }
    }
  }
  return nodes
}
console.log(deepTraversal3(node))
//stack数组中的变化   先进后出
/*
[parent]
[child3,child2,child1]
[child3,child2,child1-3,child1-2,child1-1]
......
*/
```

* 最终结果
![My Pic](/images/dom2.png)