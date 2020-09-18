title: Vue3的初步使用(-)
author: coolsong
tags:
  - Vue3
  - Composition API
categories:
  - Vue3
  - Composition API
date: 2020-09-18 14:05:00
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;之前就看过了很多关于Vue3的介绍，新特性，Composition API等特性。但是自己一直处于看文档和简单的源码上。虽然Vue3.0至今还是rc版，但是我们已经可以开始动起来了，下面就简单的介绍一下Vue3中一些新特性的使用。
<!--more-->
#### Vue3安装

> 这里有两种方式，一种是通过新版的cli安装，另一种是通过vite创建。

##### Vite

>Vite其实是一个前端构建工具。才出来没多久，也是尤大大搞得。1.0版本已经进入rc版本。主要是和Vue3配合使用。所以我们可以用它来搭建vue3的环境。作为构建工具它也有它很多的优势。
>由于我们这里主要是通过cli去搭建的（vite并没有完善到支撑一个完整项目的地步）这里不做赘述了。
>感兴趣可以看github  [Vite](https://github.com/vitejs/vite)

##### CLI
>我这里使用的版本是4.5.4
>安装过程：

* 命令行新建项目 vue create my-vue3-test
* 选择Manually select features 自定义安装
* 选择下面的这些
![Image](/images/usevue31.png)
* 回车选择3.x(preview)
* Use class-style component syntax?选择n,即输入n然后回车
* Use Babel alongside TypeScript,输入y`
*  Use history mode for router输入n
*  然后css预处理可以任意选择
*  eslint选择ESLint + Prettier
* 然后是Lint on save和In dedicater config files

> 安装好项目之后  ，进入项目启动就可以了

* 由于下面的有些例子会用到一些组件，这里就选择将Vue3.0继承到自家的UI库中的ant-design-vue。

1. 安装依赖以及按需加载
```
yarn add ant-design-vue@2.0.0-beta.6 
yarn add babel-plugin-import -D
```
2. 修改babel.config.js
```JavaScript
module.exports = {
  presets: ["@vue/cli-plugin-babel/preset"],
  plugins: [
    // 按需加载
    [
      "import",
      // style 为 true 加载 less文件
      { libraryName: "ant-design-vue", libraryDirectory: "es", style: "css" }
    ]
  ]
};
```
#### setUp的使用

>setup是Vue3.0提供的一个新的属性，可以在setup中使用Composition API。创建组件实例，然后初始化 props ，紧接着就调用setup 函数。从生命周期钩子的视角来看，它会在 beforeCreate 钩子之前被调用。我们先来看一个例子吧：

```JavaScript
<template>
  <div class="home">
    <About msg="hello"></About>
    <a-form layout="inline" :model="state.form">
      <a-form-item>
        <a-input v-model:value="state.form.user" placeholder="Username">
          <template v-slot:prefix><UserOutlined style="color:rgba(0,0,0,.25)"/></template>
        </a-input>
      </a-form-item>
      <a-form-item>
        <a-input v-model:value="state.form.password" type="password" placeholder="Password">
          <template v-slot:prefix><LockOutlined style="color:rgba(0,0,0,.25)"/></template>
        </a-input>
      </a-form-item>
      <a-form-item>
        <a-button
          type="primary"
          :disabled="state.form.user === '' || state.form.password === ''"
          @click="handleSubmit"
        >
          登录
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>
<script>
import { UserOutlined, LockOutlined } from '@ant-design/icons-vue';
import { Form, Input, Button } from 'ant-design-vue';
import { reactive } from 'vue';
import About from '@/views/About';
export default {
  components: {
    UserOutlined,
    LockOutlined,
    [Form.name]: Form,
    [Form.Item.name]: Form.Item,
    [Input.name]: Input,
    [Button.name]: Button,
    About,
  },
  setup() {
    const state = reactive({
      form: {
        user: '',
        password: '',
      },
    });
    function handleSubmit() {
      console.log(state.form);
    }
    return {
      state,
      handleSubmit,
    };
  },
};
</script>
```
>可以看到， 我们在setup中使用reactive创建了一个响应式对象，和一个方法，然后return出来之后就可以在template中使用了。

##### 参数说明
>setup有两个参数，分别是props和context

* props

>props是setup函数的第一个参数，是组件外部传入进来的属性。可以直接在template中使用

```JavaScript
export default {
  props: {
    name: String,
  },
  setup(props) {
    console.log(props.name)
  },}
```
>需要注意的一点是不要对props使用解构赋值，那样会使其失去响应性。
```JavaScript
export default {
  props: {
    name: String,
  },
  setup({ name }) {
    watchEffect(() => {
      console.log(`name is: ` + name) // Will not be reactive!
    })
  },}
```

* context

>context是setup函数的第二个参数，context是一个对象，里面包含了三个属性，分别是:

>1. attrs: attrs与Vue2.0的this.$attrs是一样的，即外部传入的未在props中定义的属性。对于attrs与props一样，我们不能对attrs使用es6的解构，必须使用attrs.name的写法
>2. slots: slots对应的是组件的插槽，与Vue2.0的this.$slots是对应的,与props和attrs一样，slots也是不能解构的。
>3. emit: emit对应的是Vue2.0的this.$emit, 即对外暴露事件。

##### 返回值
>setup函数一般会返回一个对象，这个对象里面包含了组件模板里面要使用到的data与一些函数或者事件，但是setup也可以返回一个函数，这个函数对应的就是Vue2.0的render函数，可以在这个函数里面使用JSX，对于Vue3.0中使用JSX。

* 官网例子

```JavaScript
import { h, ref, reactive } from 'vue'

export default {
  setup() {
    const count = ref(0)
    const object = reactive({ foo: 'bar' })

    return () => h('div', [count.value, object.foo])
  },}
```

#### Reactive和Ref
>这两个新的api都是和创建响应式数据有关，但它们又有一些不同。

##### reactive
* 它接收一个普通对象然后返回该普通对象的响应式代理。等同于 2.x 的 Vue.observable()。响应式转换是“深层的”：会影响对象内部所有嵌套的属性。基于 ES2015 的 Proxy 实现，返回的代理对象不等于原始对象。建议仅使用代理对象而避免依赖原始对象。

一个例子：
```JavaScript
<template>
  <div class="about">
    <h1 @click="changeName">{{ state.name }}</h1>
    <a-input v-model:value="state.name" placeholder="Basic usage" />
  </div>
</template>
<script>
import { reactive } from 'vue';
import { Button, Input } from 'ant-design-vue';
export default {
  components: {
    [Button.name]: Button,
    [Input.name]: Input,
  },
  setup() {
    const state = reactive({
      name: '子君',
    });
    const changeName = () => {
      state.name = 'hello, world';
    }
    const obj = {};
    const state1 = reactive(obj);
    // 输出false
    console.log(obj === state1);
    return {
      state,
      changeName
    };
  },
};
</script>
```

##### ref

* 和reactive类似，接受一个参数值并返回一个响应式且可改变的 ref 对象。ref 对象拥有一个指向内部值的单一属性 .value。ref更像是定义对象中的单个属性。

一个例子：
```javaScript

<template>
  <div class="about">
    <h4>{{ age }}</h4>
    <a-input v-model:value="age" placeholder="Basic usage" />
  </div>
</template>
<script>
import { ref } from 'vue';
import { Button, Input } from 'ant-design-vue';
export default {
  components: {
    [Button.name]: Button,
    [Input.name]: Input,
  },
  setup() {
    const age = ref(0);
    const changeAge = () => {
      age.value++;
    };
    return {
      age,
      changeAge,
    };
  },
};
</script>
```

##### 区别

* 通过上面的两个例子，我们可以总结出：

1. reactive传入的是一个对象，返回的是一个响应式对象，而ref传入的是一个基本数据类型（其实引用类型也可以），返回的是传入值的响应式值
2. reactive获取或修改属性可以直接通过state.prop来操作，而ref返回值需要通过name.value的方式来修改或者读取数据。但是需要注意的是，在template中并不需要通过.value来获取值，这是因为template中已经做了解套。

#### v-model
>之前我们实现双向数据绑定有两种方式，一种是使用v-mode，而另一种是使用:value.sync+emit('update:value'))。
>v-model一般用于input、select等上，绑定value并监听相应的事件实现双向绑定，在组件上使用v-mode默认会利用名为 value 的 prop 和名为 input 的事件，我们也可以使用model去自定义。但是有一个缺点是只能绑定一个。
>
>使用:value.sync+emit('update:value'))我们就可以绑定多个数据。
>
>在Vue3.0中为了实现统一，实现了让一个组件可以拥有多个v-model，同时删除掉了.sync。在vue3.0中，v-model后面需要跟一个modelValue，即要双向绑定的属性名，Vue3.0就是通过给不同的v-model指定不同的modelValue来实现多个v-model。

一个例子:
child.vue
```JavaScript
<template>
  <div class="child">
    <input :value="value" @input="_handleChangeValue" />
    <input :value="title" @input="_handleChangeTitle" />
    <a-button @click="changeValue('xiuxiu')">change value</a-button>
    <a-button @click="changeTitle('biubiu')">change title</a-button>
  </div>
</template>
<script>
import { Button } from 'ant-design-vue';
export default {
  components: {
    [Button.name]: Button,
  },
  props: {
    value: {
      type: String,
      default: '',
    },
    title: {
      type: String,
      default: '',
    },
  },
  setup(props, { emit }) {
    function _handleChangeValue(e) {
      emit('update:value', e.target.value);
    }
    function _handleChangeTitle(e) {
      emit('update:title', e.target.value);
    }
    function changeValue(str) {
      emit('update:value', str);
    }
    function changeTitle(str) {
      emit('update:title', str);
    }
    return {
      _handleChangeValue,
      _handleChangeTitle,
      changeValue,
      changeTitle,
    };
  },
};
</script>
```
parent.vue
```JavaScript
<template>
  <div class="demo">
    <Child v-model:value="state.value" v-model:title="state.title"></Child>
  </div>
</template>
<script>
import { reactive } from 'vue';
import Child from './child';
export default {
  components: {
    Child,
  },
  setup() {
    /* const count = ref(0);
    const object = reactive({ foo: 'bar' });
    return () => h('div', [count.value, object.foo]); */
    const state = reactive({
      value: '0',
      title: 'test',
    });
    return {
      state,
    };
  },
};
</script>
```
> 可以看到我们在v-model上绑定了两个变量

#### watch的使用
>在Vue3.0中，除了Vue2.0的写法之外，有两个api可以对数据变化进行监听，第一种是watch,第二种是watchEffect。对于watch,其与Vue2.0中的$watch用法基本是一模一样的，而watchEffect是Vue3.0新提供的api。

##### watch

>监听单个值和函数返回
* 一个例子：
```JavaScript
import { watch, ref, reactive } from 'vue'
export default {
  setup() {
    const title = ref('活着')
    const titles = reactive({
      title1: 'book1',
      title2: 'book2'
    })
    watch(name, (newValue, oldValue) => {
      console.log(newValue, oldValue)
    })
    watch(
      () => {
        return titles.title1 + titles.titles
      },
      value => {
        console.log(value)
      }
    )

    setTimeout(() => {
      title.value = 'hello world'
      titles.title1 = 'miss'
    }, 3000)
  }
}
```
>watch除了可以监听单个值和函数返回的变化，还可以监听多个值得变化
```JavaScript
import { watch, ref, reactive } from 'vue'
export default {
  setup() {
    const title1 = ref('活着')
    const title2 = ref('活着2')
    watch([title1, title2], ([title1, title2], [pretitle1, pretitle2]) => {
      console.log(title1, pretitle1)
      console.log(title2, pretitle2)
    })
    setTimeout(() => {
      title1.value = 'hello world'
      title2.value = 'miss you'
    }, 3000)
  }
}
```

##### watchEffect
>会立即执行传入的一个函数，并响应式追踪其依赖，并在其依赖变更时重新运行该函数。

```JavaScript
const count = ref(0)

watchEffect(() => console.log(count.value))// -> 打印出 0

setTimeout(() => {
  count.value++
  // -> 打印出 1
 }, 100)
```

##### 停止侦听
>当 watchEffect 在组件的 setup() 函数或生命周期钩子被调用时， 侦听器会被链接到该组件的生命周期，并在组件卸载时自动停止。在一些情况下，也可以显式调用返回值以停止侦听：

```JavaScript
const stop = watchEffect(() => {
  /* ... */
 })
// 之后
stop()
```

##### 清除副作用
>有时副作用函数会执行一些异步的副作用, 这些响应需要在其失效时清除（即完成之前状态已改变了）。所以侦听副作用传入的函数可以接收一个 onInvalidate 函数作入参, 用来注册清理失效时的回调。当以下情况发生时，这个失效回调会被触发:
>1. 副作用即将重新执行时
>2.侦听器被停止 (如果在 setup() 或 生命周期钩子函数中使用了 watchEffect, 则在卸载组件时)

```JavaScript
function loadData(id, cb) {
  return new Promise(resolve => {
    const timer = setTimeout(() => {
      resolve(
        new Array(10).fill(0).map(() => {
          return id.value + Math.random()
        })
      )
    }, 2000)
    cb(() => {
      clearTimeout(timer)
    })
  })
}

export default {
  setup() {
    // 下拉框1 选中的数据
    const select1Id = ref(0)
    // 下拉框2的数据
    const select2List = ref([])
    watchEffect(onInvalidate => {
      // 模拟请求
      let cancel = undefined
      // 第一个参数是一个回调函数，用于模拟取消请求，关于取消请求，可以了解axios的CancelToken
      loadData(select1Id, cb => {
        cancel = cb
      }).then(data => {
        select2List.value = data
        console.log(data)
      })
      onInvalidate(() => {
        cancel && cancel()
      })
    })
  }
}
```

#### 总结分享
>这一篇只介绍了一部分的api和特性，下一篇会继续介绍其他的特性
>参考文档：
>[Vue Composition API](https://composition-api.vuejs.org/)
>[Vue3.0来袭，你想学的都在这里](https://juejin.im/post/6869521076771094536)