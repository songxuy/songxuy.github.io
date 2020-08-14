title: 初探VUE3
author: coolsong
tags:
  - Vue
categories:
  - Vue
date: 2020-01-10 21:22:00
---
&nbsp;&nbsp;&nbsp;VUE3的第一版也已经出来好几个月了，自己一直说要去看看，结果一直拖到了现在。目前依然是pre-alpha状态，但主要的架构改进、优化和新功能都已经完成，剩下的主要是完成一些Vue2现有功能的移植（尤大大原话）。下面就通过代码为大家初探一下Vue3：
<!--more-->
### 拉取源代码
>GIthub源码：[vue-next](https://github.com/vuejs/vue-next)

```
git pull https://github.com/vuejs/vue-next.git
cd vue-next
```

### 相对于之前的改变
* 双向数据绑定的实现（由Obejct.defineProperty->Proxy）
>这一点是众所周知的，使用代理解决了之前数组没实现的问题（使用之前的实现消耗太大）
```JavaScript
const observer = new Proxy(data, {
    get(obj, prop) { ... },
    set(obj, prop, newVal) { ... },
    deleteProperty() {
        //invoked when property from source data object is deleted
    }})
```
* 源码几乎都是使用TypeScript编写，ts的优势就不用多说了吧
* 重构了虚拟DOM
* OptionApi => Composition API

### 走进源代码
#### 项目结构
> 我们主要需要关注package文件夹，里面包含了功能的实现
> 
![My Pic](/images/vue31.png)
* compiler-core 目录: 平台无关的编译器. 它既包含可扩展的基础功能，也包含所有平台无关的插件。
* compiler-dom 目录: 针对浏览器而写的编译器。
* reactivity 目录：数据响应式系统，这是一个单独的系统，可以与任何框架配合使用。
* runtime-core 目录：与平台无关的运行时。其实现的功能有虚拟 DOM 渲染器、Vue 组件和 Vue 的各种API，我们可以利用这个 runtime 实现针对某个具体平台的高阶 runtime，比如自定义渲染器。
* runtime-dom 目录: 针对浏览器的 runtime。其功能包括处理原生 DOM API、DOM 事件和 DOM 属性等。
* runtime-test 目录: 一个专门为了测试而写的轻量级 runtime。由于这个 rumtime 「渲染」出的 DOM 树其实是一个 JS 对象，所以这个 runtime 可以用在所有 JS 环境里。你可以用它来测试渲染是否正确。它还可以用于序列化 DOM、触发 DOM 事件，以及记录某次更新中的 DOM 操作。
* server-renderer 目录: 用于 SSR。尚未实现。
* shared 目录: 没有暴露任何 API，主要包含了一些平台无关的内部帮助方法。
* vue 目录: 用于构建「完整构建」版本，引用了上面提到的 runtime 和 compiler。


#### runtime-core 模块
>这部分包含了Vue中各种Api的实现以及生命周期等功能，我们可以在这个模块看看相应的方法的具体实现。
>apiCreateComponent中是关于createComponent的实现
>apiInject中是关于provide和inject的实现
>apiLifecycle中是关于生命周期的实现
>其他的大家可以自己去看看，这里就不再赘述了
>
![My Pic](/images/vue32.png)

#### reactivity 模块
>这个文件主要是数据响应模块的实现。其暴露的主要 API 有 ref（数据容器）、reactive（基于 Proxy 实现的响应式数据）、computed（计算数据）、effect（副作用） 等几部分。
>
![My Pic](/images/vue33.png)
>下面是一个源码实现的一个方法createReactiveObject,可以看到里面已经是使用Proxy了
```TypeScript
function createReactiveObject(
  target: any,
  toProxy: WeakMap<any, any>,
  toRaw: WeakMap<any, any>,
  baseHandlers: ProxyHandler<any>,
  collectionHandlers: ProxyHandler<any>
) {
  if (!isObject(target)) {
    if (__DEV__) {
      console.warn(`value cannot be made reactive: ${String(target)}`)
    }
    return target
  }
  // target already has corresponding Proxy
  let observed = toProxy.get(target)
  if (observed !== void 0) {
    return observed
  }
  // target is already a Proxy
  if (toRaw.has(target)) {
    return target
  }
  // only a whitelist of value types can be observed.
  if (!canObserve(target)) {
    return target
  }
  const handlers = collectionTypes.has(target.constructor)
    ? collectionHandlers
    : baseHandlers
  observed = new Proxy(target, handlers)
  toProxy.set(target, observed)
  toRaw.set(observed, target)
  if (!targetMap.has(target)) {
    targetMap.set(target, new Map())
  }
  return observed
}
```

#### runtime-dom 模块
>这部分主要是写的浏览器上的 runtime，主要功能是适配了浏览器环境下节点和节点属性的增删改查。它暴露了两个重要 API：render 和 createApp，并声明了一个 ComponentPublicInstance 接口。

```TypeScript
import { createRenderer } from '@vue/runtime-core'
import { nodeOps } from './nodeOps'
import { patchProp } from './patchProp'

const { render, createApp } = createRenderer<Node, Element>({
  patchProp,
  ...nodeOps
})

export { render, createApp }

// re-export everything from core
// h, Component, reactivity API, nextTick, flags & types
export * from '@vue/runtime-core'

export interface ComponentPublicInstance {
  $el: Element
}
```

### 结尾
>这次就先介绍这几个模块了，大家还是可以自己去看看。之后还会给大家介绍，相应的demo的编写以及运行其中编写好的测试demo。






