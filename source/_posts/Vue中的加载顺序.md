title: Vue中的加载顺序
author: coolsong
tags:
  - Vue
categories: 
  - vue
date: 2019-06-10 17:31:00
---
&nbsp;&nbsp;&nbsp;&nbsp;今天评审的时候，自己才学到了一个新的知识关于Vue的加载顺序。才知道自己之前的某些bug产生的原因呜呜呜呜。然后自己下来也去验证了一下果然是这样的。
<!--more-->
### 1.vue的生命周期
众所周知vue的生命周期包括beforeCreate、created、beforeMount、mounted、beforeUpdate、update、beforeDestory、destoryed以及后面为keep-alive加上的activated、deactivated。执行顺序也都清楚：
```
beforeCreate->created->beforeMount->mounted->beforeDestory->destoryed
```

* 具体每个阶段所做的事，大家也可以去自行查找一下，也挺重要的。由于不是这篇的重点于是就略过啦。

### 2.app.vue与view-router的加载顺序
之前自己的认识是app.vue中加载完成之后才到之后的部分
```JavaScript
//app.vue
<template>
<div id="app">
<router-link to="/">home</router-link><router-link to="/regis">regis</router-link><router-link to="/about">about</router-link>
<!-- <backtop v-if="show"></backtop> -->
<router-view/>
</div>
</template>

beforeCreate(){
  console.log('app beforeCreate')
},
created () {
  console.log('app create')
},
beforeMount () {
  console.log('app beforeMount')
},
mounted () {
  console.log('app mounted')
}

//router-view

beforeCreate(){
  console.log('/ beforeCreate')
},
created () {
  console.log('/ create')
},
beforeMount () {
  console.log('/ beforeMount')
},
mounted () {
  console.log('/ mounted')
}
//最终打印结果是：app beforeCreate -> app create -> app beforeMount -> /  beforeCreate -> / create -> / beforeMount -> / mounted -> app mounted
```

* app.vue中的mounted要等到子组件和router-view中的内容都加载完成之后再执行

### 3. 组件与router-view
```javaScript
//代码顺序

<template>
<div id="app">
<router-link to="/">home</router-link><router-link to="/regis">regis</router-link><router-link to="/about">about</router-link>
<backtop></backtop>
<router-view/>
</div>
</template>
//执行结果：backtop beforeMount -> backtop create -> backtop beforeMount  -> / beforeCreate -> / create -> / beforeMount -> backtop mounted -> / mounted
```

* 交换位置之后二者的执行顺序也反了过来，组件与组件之间的加载顺序也与之类似

总结一下其实就是这样：
父组件beforeCreated ->父组件created ->父组件beforeMounted ->子组件beforeCreated ->子组件created ->子组件beforeMounted ->子组件mounted -> 父组件mounted