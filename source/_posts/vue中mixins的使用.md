title: vue中mixins的使用
author: coolsong
tags:
  - Vue
categories:
  -Vue
date: 2019-06-25 22:18:00
---
&nbsp;&nbsp;&nbsp;对于mixins这个属性，自己经常在别人写的一些组件中看到，但是也不太清楚其具体的用法，所以今天抽空网上查了查学习一下它的用法。
<!--more-->
* mixins的基本概念
```
混入 (mixins)： 是一种分发 Vue 组件中可复用功能的非常灵活的方式。混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项。
```

* 那到底是干嘛的？我们该怎么使用它呢？先写的demo
* 我们先定义一个mixins对象
```JavaScript
export const helloMixin = {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {}
  },
  methods: {
    sayHello () {
      console.log('hello' + this.msg)
    }
  },
  mounted () {}
}
```

* 然后再一个组件中引入
```JavaScript
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="sayHello">click</button>
  </div>
</template>
<script>
import { helloMixin } from './mixins/helloMixin'
export default {
  name: 'HelloWorld',
  mixins: [helloMixin]
}
</script>
```

1.  然后我们调用这个组件时，就可以调用mixins中的属性和方法了
2. 还有个比较重要的就是如果mixins中的方法和组件中的方法冲突了那么最后会调用组件中的方法，方法和参数在各组件中不共享，值为函数的选项，如created,mounted等，就会被合并调用，混合对象里的钩子函数在组件里的钩子函数之前调用
3. 主要作用就是提取一些组件中的公共部分来复用，可以定义共用的变量，在每个组件中使用，引入组件中之后，各个变量是相互独立的，值的修改在组件中不会相互影响。

* 还有一个特殊情况，当mixins中含有异步方法时，而我们又需要在组件中使用异步请求函数的返回值时，我们会取不到此返回值
```javascript
methods: {
  sayHello () {
    console.log('hello' + this.msg)
  },
  getData () {
    new Promise((resolve) => {
      let data = 'I am here'
      setTimeout(() => {
        resolve(data)
      }, 1000)
    }).then(v => {
      return v
    })
  }
},
mounted () {
  console.log(this.getData()) //为undefined
}
```

* 解决方案：不要返回结果而是直接返回异步函数
```javascript
methods: {
  sayHello () {
    console.log('hello' + this.msg)
  },
  getData () {
    var fun = new Promise((resolve) => {
    let data = 'I am here'
    setTimeout(() => {
      resolve(data)
    }, 1000)
    })
    return fun
  }
},
mounted () {
  this.getData().then(v => {
    console.log(v) //I am here
  })
}
```

