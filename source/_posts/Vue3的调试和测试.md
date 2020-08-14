title: Vue3的调试和测试
author: coolsong
tags:
  - Vue
categories:
  - Vue
date: 2020-01-17 22:02:00
---
&nbsp;&nbsp;&nbsp;上一篇中我们简单的介绍了一下Vue3源码的基本目录结构，以及相应的目录对应的功能的实现，和部分新功能在源码中的实现样例。这篇文章主要介绍一下如何使用vue3写一些小demo，将新的特性用起来。除此之外，如何测试源码中的测试样例，并且加入自己的测试：
<!--more-->
### 如何调试
>首先拉取vue-next的源码，上一篇文章已经介绍过了，这里就不再赘述了
>然后修改根目录中的两个文件  tsconfig,json  rollup.config.js方便调试

![My Pic](/images/Vue31a.png)
![My Pic](/images/Vue32a.png)
>在运行npm run dev

![My Pic](/images/Vue33a.png)

>在项目根目录上新建一个demo文件夹，并在里面创建一个index.html和index.js
```Html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>vue3 demo</title>
    <script src="../packages/vue/dist/vue.global.js"></script>
</head>

<body>
    <!-- <div id="app"></div> -->
</body>
<script src="./index.js"></script>
</html>
```
* packages/vue目录中是用于构建「完整构建」版本，引用了runtime 和 compiler。我们就可以直接使用其特性了。
```JavaScript
const { reactive } = Vue
var App = {
  template: `
    <div class="container">
         {{msg}}
    </div>`,
  setup() {
  	const state = reactive({msg: "Hello World!!!"})
	return state
  }
}
Vue.createApp().mount(App, '#app')
```
![My Pic](/images/Vue34.png)
>这个例子里面可能reactive和setup大家还不是很熟悉。从名字就可以看出reactive把对象变为响应式的方法，和之前我们写在data中的数据一样，Vue3中同样也支持data的写法。
>setup实际上也是一个生命周期函数等价于beforeCreate和created

* 为了更加方便的编写demo调试，我们可以本地起一个服务。

```
npm install -g serve
```
>然后就可以直接运行 serve 就会在根目录启动服务 在进入相应的demo目录就能看到结果

![My Pic](/images/Vue35.png)

* 接下来我们再看一个例子

```JavaScript
const { reactive } = Vue
let Input = {
  template: `
  <input type="text" v-model="word"/>
  `,
  setup(props, context) {
    console.log(props, context)
    const state = reactive({ word: props.title})
    return state
  }
}
let App = {
  template: `
    <div class="msg">
        {{state.value}}
        <Input title="init data" />
    </div>`,
  components: {
    Input
  },
  setup() {
    const state = reactive({ value: 'hello world' })
    return { state }
  }
}
Vue.createApp().mount(App, '#app')
```
![My Pic](/images/Vue36.png)
>这个例子主要是一个父子组件之间的信息传递。可以看到setup方法接受两个参数一个是props（也就是父组件传递的值），另一个是context。

* 再来看看ref 和 toRefs

```JavaScript
const { reactive, ref, computed, onMounted } = Vue
let App = {
  template: `
  <button @click="increment">
  Count is: {{ count }}, double is {{ double }}, click to increment.
  </button>`,
  setup() {
    console.log('setup')
    const count = ref(0)
    console.log(count)
    const double = computed(() => count.value * 2)

    function increment() {
      count.value++
    }

    onMounted(() => console.log('component mounted!'))

    return {
      count,
      double,
      increment
    }
  }
}
Vue.createApp().mount(App, '#app')
```
![My Pic](/images/Vue37.gif)
>ref可以创建单个的响应式的对象 返回之后可以直接访问。通过打印出的值也可以看到其中包含了一个value键和_isRef键。我们通过reactive创建一个对象也能实现同样的效果。使用ref调用时会更加简单一点。如果使用reactive要想实现同样的效果，就需要使用toRefs将创建的对象拆成单个的响应式对象。

* 在看看数据双向绑定

>我们都知道3中的数据响应使用的代理来实现的。我们来看一个例子吧。

```JavaScript
const { reactive, watch, effect, toRefs } = Vue
let App = {
  template: `
    <div class="msg">
        count: {{count}}
        <button @click="handlerCountAdd"> Click ++ </button>
    </div>`,
  setup() {
    const state = reactive({ count: 0, value: 1 })
    const handlerCountAdd = () => {
      state.count++
      state.value++
    }
    watch(
      () => state.count,
      val => {
        console.log('watch', state.count)
        console.log('watch', state.value)
      }
    )
    effect(() => {
      console.log('effect', state.count)
      console.log('effect', state.value)
    })
    return { ...toRefs(state), handlerCountAdd }
  }
}
Vue.createApp().mount(App, '#app')
```
![My Pic](/images/Vue38.gif)
>effect和watch都可以监听到咱们数据的变化,它们的区别是effect 在响应式数据变化的时候就会执行，执行次数根据响应式数据的个数来决定。watch则只会触发一次。

### 测试
>我们在源码的功能目录可看到每个功能文件夹下面都对应着一个__test__文件夹里面就是写的一些测试用例。这里使用的Jest测试框架来进行测试的，相应的测试方法参数可以参考官网[Jest](https://jestjs.io/)
>
![My Pic](/images/Vue39.png)

>可以在命令行中直接运行npm run test 就可以对整个pakeage目录中的测试文件进行测试。可以看到测试通过和失败的个数。

![My Pic](/images/Vue310.png)

* 我们如何测试单个文件呢？ 这就需要修改jest.config.js，将
testRegex设置为我们想要测试的文件
```TypeScript
import {
  onBeforeMount,
  h,
  nodeOps,
  render,
  serializeInner,
  onMounted,
  ref,
  onBeforeUpdate,
  nextTick,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onRenderTracked,
  reactive,
  TrackOpTypes,
  onRenderTriggered
} from '@vue/runtime-test'
import { ITERATE_KEY, DebuggerEvent, TriggerOpTypes } from '@vue/reactivity'

// reference: https://vue-composition-api-rfc.netlify.com/api.html#lifecycle-hooks

describe('api: lifecycle hooks', () => {
  it('onBeforeMount', () => {
    const root = nodeOps.createElement('div')
    const fn = jest.fn(() => {
      // should be called before inner div is rendered
      expect(serializeInner(root)).toBe(``)
    })

    const Comp = {
      setup() {
        onBeforeMount(fn)
        return () => h('div')
      }
    }
    render(h(Comp), root)
    expect(fn).toHaveBeenCalledTimes(1)
  })

  it('onMounted', () => {
    const root = nodeOps.createElement('div')
    const fn = jest.fn(() => {
      // should be called after inner div is rendered
      expect(serializeInner(root)).toBe(`<div></div>`)
    })

    const Comp = {
      setup() {
        onMounted(fn)
        return () => h('div')
      }
    }
    render(h(Comp), root)
    expect(fn).toHaveBeenCalledTimes(1)
  })

  it('onBeforeUpdate', async () => {
    const count = ref(0)
    const root = nodeOps.createElement('div')
    const fn = jest.fn(() => {
      // should be called before inner div is updated
      expect(serializeInner(root)).toBe(`<div>0</div>`)
    })

    const Comp = {
      setup() {
        onBeforeUpdate(fn)
        return () => h('div', count.value)
      }
    }
    render(h(Comp), root)

    count.value++
    await nextTick()
    expect(fn).toHaveBeenCalledTimes(1)
  })

  it('onUpdated', async () => {
    const count = ref(0)
    const root = nodeOps.createElement('div')
    const fn = jest.fn(() => {
      // should be called after inner div is updated
      expect(serializeInner(root)).toBe(`<div>1</div>`)
    })

    const Comp = {
      setup() {
        onUpdated(fn)
        return () => h('div', count.value)
      }
    }
    render(h(Comp), root)

    count.value++
    await nextTick()
    expect(fn).toHaveBeenCalledTimes(1)
  })

  it('onBeforeUnmount', async () => {
    const toggle = ref(true)
    const root = nodeOps.createElement('div')
    const fn = jest.fn(() => {
      // should be called before inner div is removed
      expect(serializeInner(root)).toBe(`<div></div>`)
    })

    const Comp = {
      setup() {
        return () => (toggle.value ? h(Child) : null)
      }
    }

    const Child = {
      setup() {
        onBeforeUnmount(fn)
        return () => h('div')
      }
    }

    render(h(Comp), root)

    toggle.value = false
    await nextTick()
    expect(fn).toHaveBeenCalledTimes(1)
  })

  it('onUnmounted', async () => {
    const toggle = ref(true)
    const root = nodeOps.createElement('div')
    const fn = jest.fn(() => {
      // should be called after inner div is removed
      expect(serializeInner(root)).toBe(`<!---->`)
    })

    const Comp = {
      setup() {
        return () => (toggle.value ? h(Child) : null)
      }
    }

    const Child = {
      setup() {
        onUnmounted(fn)
        return () => h('div')
      }
    }

    render(h(Comp), root)

    toggle.value = false
    await nextTick()
    expect(fn).toHaveBeenCalledTimes(1)
  })

  it('onBeforeUnmount in onMounted', async () => {
    const toggle = ref(true)
    const root = nodeOps.createElement('div')
    const fn = jest.fn(() => {
      // should be called before inner div is removed
      expect(serializeInner(root)).toBe(`<div></div>`)
    })

    const Comp = {
      setup() {
        return () => (toggle.value ? h(Child) : null)
      }
    }

    const Child = {
      setup() {
        onMounted(() => {
          onBeforeUnmount(fn)
        })
        return () => h('div')
      }
    }

    render(h(Comp), root)

    toggle.value = false
    await nextTick()
    expect(fn).toHaveBeenCalledTimes(1)
  })

  it('lifecycle call order', async () => {
    const count = ref(0)
    const root = nodeOps.createElement('div')
    const calls: string[] = []

    const Root = {
      setup() {
        onBeforeMount(() => calls.push('root onBeforeMount'))
        onMounted(() => calls.push('root onMounted'))
        onBeforeUpdate(() => calls.push('root onBeforeUpdate'))
        onUpdated(() => calls.push('root onUpdated'))
        onBeforeUnmount(() => calls.push('root onBeforeUnmount'))
        onUnmounted(() => calls.push('root onUnmounted'))
        return () => h(Mid, { count: count.value })
      }
    }

    const Mid = {
      setup(props: any) {
        onBeforeMount(() => calls.push('mid onBeforeMount'))
        onMounted(() => calls.push('mid onMounted'))
        onBeforeUpdate(() => calls.push('mid onBeforeUpdate'))
        onUpdated(() => calls.push('mid onUpdated'))
        onBeforeUnmount(() => calls.push('mid onBeforeUnmount'))
        onUnmounted(() => calls.push('mid onUnmounted'))
        return () => h(Child, { count: props.count })
      }
    }

    const Child = {
      setup(props: any) {
        onBeforeMount(() => calls.push('child onBeforeMount'))
        onMounted(() => calls.push('child onMounted'))
        onBeforeUpdate(() => calls.push('child onBeforeUpdate'))
        onUpdated(() => calls.push('child onUpdated'))
        onBeforeUnmount(() => calls.push('child onBeforeUnmount'))
        onUnmounted(() => calls.push('child onUnmounted'))
        return () => h('div', props.count)
      }
    }

    // mount
    render(h(Root), root)
    expect(calls).toEqual([
      'root onBeforeMount',
      'mid onBeforeMount',
      'child onBeforeMount',
      'child onMounted',
      'mid onMounted',
      'root onMounted'
    ])

    calls.length = 0

    // update
    count.value++
    await nextTick()
    expect(calls).toEqual([
      'root onBeforeUpdate',
      'mid onBeforeUpdate',
      'child onBeforeUpdate',
      'child onUpdated',
      'mid onUpdated',
      'root onUpdated'
    ])

    calls.length = 0

    // unmount
    render(null, root)
    expect(calls).toEqual([
      'root onBeforeUnmount',
      'mid onBeforeUnmount',
      'child onBeforeUnmount',
      'child onUnmounted',
      'mid onUnmounted',
      'root onUnmounted'
    ])
  })

  it('onRenderTracked', () => {
    const events: DebuggerEvent[] = []
    const onTrack = jest.fn((e: DebuggerEvent) => {
      events.push(e)
    })
    const obj = reactive({ foo: 1, bar: 2 })

    const Comp = {
      setup() {
        onRenderTracked(onTrack)
        return () =>
          h('div', [obj.foo, 'bar' in obj, Object.keys(obj).join('')])
      }
    }

    render(h(Comp), nodeOps.createElement('div'))
    expect(onTrack).toHaveBeenCalledTimes(3)
    expect(events).toMatchObject([
      {
        target: obj,
        type: TrackOpTypes.GET,
        key: 'foo'
      },
      {
        target: obj,
        type: TrackOpTypes.HAS,
        key: 'bar'
      },
      {
        target: obj,
        type: TrackOpTypes.ITERATE,
        key: ITERATE_KEY
      }
    ])
  })

  it('onRenderTriggered', async () => {
    const events: DebuggerEvent[] = []
    const onTrigger = jest.fn((e: DebuggerEvent) => {
      events.push(e)
    })
    const obj = reactive({ foo: 1, bar: 2 })

    const Comp = {
      setup() {
        onRenderTriggered(onTrigger)
        return () =>
          h('div', [obj.foo, 'bar' in obj, Object.keys(obj).join('')])
      }
    }

    render(h(Comp), nodeOps.createElement('div'))

    obj.foo++
    await nextTick()
    expect(onTrigger).toHaveBeenCalledTimes(1)
    expect(events[0]).toMatchObject({
      type: TriggerOpTypes.SET,
      key: 'foo',
      oldValue: 1,
      newValue: 2
    })

    delete obj.bar
    await nextTick()
    expect(onTrigger).toHaveBeenCalledTimes(2)
    expect(events[1]).toMatchObject({
      type: TriggerOpTypes.DELETE,
      key: 'bar',
      oldValue: 2
    })
    ;(obj as any).baz = 3
    await nextTick()
    expect(onTrigger).toHaveBeenCalledTimes(3)
    expect(events[2]).toMatchObject({
      type: TriggerOpTypes.ADD,
      key: 'baz',
      newValue: 3
    })
  })
})
```
>再次运行npm run test
>
![My Pic](/images/Vue311.png)

>这里可以看到我们编写的测试用例的名称，以及是否通过。












