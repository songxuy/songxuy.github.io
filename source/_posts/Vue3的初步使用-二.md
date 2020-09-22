title: Vue3的初步使用(二)
author: coolsong
tags:
  - Vue
  - Composition API
categories:
  - Vue
  - Composition API
date: 2020-09-22 14:32:00
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;还是和上一篇一样，继续来介绍Vue3中一些新的特性以及其如何使用。
<!--more-->
>tips: 前几天Vue 官方团队发布了Vue3.0版本 🎉。代号为One Piece。
>[releases docs](https://github.com/vuejs/vue-next/releases/tag/v3.0.0)

#### computed的使用

>计算属性一般是当我们有复杂计算且需要缓存值得时候来使用。Vue3中的使用和之前的并没有太大的区别。

* 一个例子

```JavaScript
<script>
import { computed, reactive, ref, watch } from 'vue';
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
    const age = ref(0);
    const changeName = () => {
      state.name = 'hello, world';
    };
    const changeAge = () => {
      age.value++;
    };
    watch(age, (newValue, oldValue) => {
      console.log(newValue, oldValue);
    });
    const msg = computed(() => {
      return state.name + age.value;
    });
    const obj = {};
    const state1 = reactive(obj);
    // 输出false
    console.log(obj === state1);
    return {
      state,
      changeName,
      age,
      changeAge,
      msg,
    };
  },
};
</script>
```
![Image](/images/usevue32.gif)

>当我们直接传入一个函数即默认设置的getter函数，如果要设置setter的话就需要传一个对象，这样的话创建的就是一个可手动修改的计算状态。demo如下：

```JavaScript
const count = ref(1)
const plusOne = computed({
  get: () => count.value + 1,
  set: (val) => {
    count.value = val - 1
  },})
plusOne.value = 1
console.log(count.value) // 0
```

#### readonly
>传入一个对象（响应式或普通）或 ref，返回一个原始对象的只读代理。一个只读的代理是“深层的”，对象内部任何嵌套的属性也都是只读的。

* 一个例子

```JavaScript
const original = reactive({ count: 0 })
const copy = readonly(original)

watchEffect(() => {
  // 依赖追踪
  console.log(copy.count)
})

// original 上的修改会触发 copy 上的侦听
original.count++

// 无法修改 copy 并会被警告
copy.count++ // warning!
```

#### 使用Vue-Router

##### 初始化
> 在2.0中我们使用vue-router，是通过new VueRouter的方式去创建一个实例，在3.0中创建实例发生了一些变化。

```JavaScript
// router/index.ts
import { createRouter, createWebHashHistory, RouteRecordRaw } from 'vue-router';
import Home from '../views/Home.vue';
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'Home',
    component: Home,
  },
  {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue'),
  },
  {
    path: '/demo',
    name: 'demo',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/demo.vue'),
  },
];
const router = createRouter({
  history: createWebHashHistory(),
  routes,
});
export default router;

// main.js
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";
import store from "./store";
createApp(App)
  .use(store)
  .use(router)
  .mount("#app");
```

##### setup使用
> 同样的在setup中使用和之前$route和$router的使用还是有一些区别的。由于不能直接使用this，需要借助useRouter和useRoute来创建相应的实例。

* 一个例子

```JavaScript
<script>
import { reactive } from 'vue';
import Child from './child';
import { useRoute, useRouter, onBeforeRouteUpdate } from 'vue-router';
export default {
  components: {
    Child,
  },
  beforeRouteEnter(from, to, next) {
    console.log('enter');
    next();
  },
  setup() {
    const route = useRoute();
    // 获取路由实例
    const router = useRouter();
    console.log(route.path);
    console.log(router)
    onBeforeRouteUpdate(() => { 
      // 当当前路由发生变化时，调用回调函数 
    })
    return {
    };
  },
};
</script>
```
>除了上面的使用之外，当我们想要使用router的生命周期函数时，也提供了onBeforeRouteUpdate和onBeforeRouteLeave两个函数。如果还是想向之前那样使用，也可以像原来那样写。

#### 使用Vuex
>vuex在3.0中的使用和之前的大体上差不太多

##### 初始化

* 在store/index.ts中的写法为

```JavaScript
import { createStore } from "vuex";
export default createStore({
  state: {},
  mutations: {},
  actions: {},
  modules: {}
});
```

* 在main.js中的写法

```JavaScript
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";
import store from "./store";
createApp(App)
  .use(store)
  .use(router)
  .mount("#app");
```

##### setup中使用
>同样的也是使用从vuex中引入的方法来操作vuex

```JavaScript
import { useStore } from 'vuex'
export default {
  setup() {
    const store = useStore()
    const roleMenus = store.getters['roleMenus']
    return {
      roleMenus
    }
  }
}
```

#### 生命周期钩子函数
> Vue3.0是兼容一部分Vue2.0的，所以对于Vue2.0的组件写法，生命周期钩子函数并未发生变化，但是如果使用的是composition api，写法上就有一些区别了：
> 1. 取消了beforeCreate和created，其实也就是用setup代替了这两个生命周期钩子函数
> 2. 将生命周期函数名称变为on+XXX。比如onMounted、onUpdated等

* setup中使用的例子

```JavaScript
setup() {
    onMounted(() => {
      console.log('mounted!')
    })
    onUpdated(() => {
      console.log('updated!')
    })
    onUnmounted(() => {
      console.log('unmounted!')
    })
  }
```

#### 总结分享
>这一篇只介绍了一部分的api和特性，下一篇会继续介绍其他的特性
>参考文档：
>[Vue Composition API](https://composition-api.vuejs.org/)
>[Vue3.0来袭，你想学的都在这里](https://juejin.im/post/6869521076771094536)

