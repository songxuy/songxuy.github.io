title: Vue项目中使用TypeScript
author: coolsong
tags:
  - Vue
  - TypeScript
categories:
  - Vue
  - TypeScript
date: 2020-01-03 19:23:00
---
&nbsp;&nbsp;&nbsp;TypeScript也已经出来了蛮久了，不管是项目组还是各种博客论坛中都能看到大家推荐的身影（用过都说好）。但是自己虽然也跟着教程大概看了一遍，但始终没有踏出第一步，但随着vue3即将发布，其next版本已经改为ts编写，也是时候真正的用起来了。下面就给大家介绍一些Vue中使用ts：
<!--more-->
### 介绍
>官网的说明：TypeScript是一个有类型的JavaScript超集，编译成纯JavaScript。任何浏览器。任何主机。任何操作系统。开源的。
>在我看来的话typescript是一个帮助我们管理各种变量参数的工具，能够大大提高代码的质量、健壮性、和可维护性。ts和js最大的区别就是ts多了类型注解功能, 通过名字中的"type"也能看出语言的重点在"类型"上. 这个类型分为基础类型和高级类型, 高级类型就是通过基础类型组成的自定义类型.

#### 原始数据类型
>布尔值、数值、字符串、null、undefined在ts中的使用

```TypeScript
let bool: boolean =  false
let num: number = 6
let str: string = 'Tom'
// 声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null
let unusable: void = undefined
// 与 void 的区别是，undefined 和 null 是所有类型的子类型。也就是说 undefined 类型的变量，可以赋值给 number 类型的变量
let u: undefined = undefined;
let n: null = null;
```
#### Any(任意值)
> 对于不确定类型的变量我们可以赋值为any，但是尽量少用这个类型，用多了其实就失去了我们使用ts的初衷了。

```TypeScript
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
// 在任意值上访问任何属性和方法都是允许的
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
// 变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型
let something;
something = 'seven';
something = 7;
```
#### 联合类型
>当变量的取值可以为一种以上的类型时，我们可以使用联合类型

```TypeScript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

//当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法
function getLength(something: string | number): number {
return something.length;
}
// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
// Property 'length' does not exist on type 'number'.
```

#### 接口
>TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

```TypeScript
//这里就定义了一个Person接口，描述了类中各个属性的类型。赋值的时候，变量的形状必须和接口的形状保持一致。也可以通过？改为非必须属性
interface Person {
   name: string;
   age: number;
}
let tom: Person = {
  name: 'Tom',
  age: 25
};
```
> 关于TypeScript的介绍就到这了，想了解更多可以点击最后的入门教程链接。接下来我们进入正题，如何在Vue项目中使用TS。

### Vue中使用TS

#### 通过脚手架安装项目
>这里直接使用vue-cli安装相应的环境
```code
vue create vuetspro
```
>我选择了这几项
>
![My Pic](/images/ts1.png)
>然后选择自己需要的配置即可，还可以将本次设置保存用于下次创建项目。确认之后就会开始下载相应的依赖。

![My Pic](/images/ts2.png)
>运行项目

![My Pic](/images/ts3.png)

#### 目录结构
>整体上和普通vue项目结构差不太多，会有一些额外的文件

![My Pic](/images/ts4.png)
>会看到有两个文件（shims-tsx.d.ts、shims-vue.d.ts）
>根据它们的名字我们也能大概猜到
>shims-tsx.d.ts：允许项目中出现tsx结尾的文件（即可以使用jsx编写）
>shims-vue.d.ts：允许项目中出现vue结尾的文件（即可以使用vue语法编写）

* 查看HelloWorld.vue
```TypeScript
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
```
>可以看到关于js部分代码已经有了很大的改变了  下面就来具体讲一下

#### 写法
>这里主要介绍的是vue-class-component写法

```TypeScript
import { Component, Vue, Prop } from 'vue-property-decorator'

@Component
export default class Test extends Vue {
  @Prop({ type: Object })
  private test: { value: string }
}
```
>首先我们引入了vue-property-decorator
>vue-property-decorator 是在 vue-class-component 上增强了更多的结合 Vue 特性的装饰器，新增了这 7 个装饰器
* @Emit
* @Inject
* @Model
* @Prop
* @Provide
* @Watch
* @Component

>这里主要介绍我们常用的几个@Prop、@Watch、@Component其他的大家可以参考github [vue-property-decorator](https://github.com/kaorun343/vue-property-decorator)

```JavaScript
import Post from '@/components/post.vue
export default {
    components: {
        Post
    },
    props: {
      msg: {
        type: String
      }
    },
    data () {
        return {
            name: 'xiaobai'
        }
    },
    computed: {
        myname () {
            return 'hahahah';
        }
    },
    watch: {
        'msg': {
          handler: 'onChildChanged',
          immediate: false,
          deep: false
        }
    },
    methods: {
        onChildChanged(val, oldVal) { },
        getAge () { return '123' }
    },
    mounted () {
        console.log('ok')
    }
}
```

> 对应者我们的TS代码如下

```TypeScript
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';

@Component
export default class HelloWorld extends Vue {
  //props
  @Prop() private msg!: string;
  @Prop() private propA!: number;
  @Prop(Number) private propB: number | undefined;
  @Prop({ default: 'default value' }) private propC!: string;

  // data
  private name: string = 'xiaobai';

  // watch
  @Watch('msg')
  onChildChanged(val: string, oldVal: string) { }

  //computed
  get myname () {
    return 'hahahah';
  }

  // methods
  public getAge () {
    return '123'
  }

  //生命周期
  private mounted (): void {
    console.log('ok');
  }
}
```
>这里面有几个我们需要注意的地方
>@Prop() private msg!: string;中的!表示!前面的这个变量一定不是undefined或者null，!叫非空断言操作符。同时我们也可以赋初值以及设置类型。
>当我们需要设置watch中参数时可以在装饰器中加入相应的配置
>@Watch('msg', { immediate: true, deep: true })

#### 添加全局工具
> 同样的，我们也需要修改main.ts
> 这里我们首先先安装 vue国际化工具VueI18n
* npm install vue-i18n --save

```TypeScript
import Vue from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';
// 新模块
import i18n from './i18n';

Vue.config.productionTip = false;

new Vue({
    router, 
    store, 
    i18n, // 新模块
    render: (h) => h(App),
}).$mount('#app');
```
>除此之外，还需要修改shims-vue.d.ts
```TypeScript
declare module 'vue/types/vue' {
  interface Vue {
        readonly $i18n: VueI18Next;
        $t: TranslationFunction;
    }
}
```
>这样就能正常使用了

#### AXIOS封装
>我们在项目中使用axios的时候，一般都会将其封装一层，设置好baseURL、请求、响应、通用错误处理等。在TS中封装其实也差不太多，主要是要加上类型

* 新建service.ts
```TypeScript
import * as axios from 'axios'
import Vue from 'vue'
import { AxiosResponse, AxiosRequestConfig } from 'axios'
const baseURL = '/api'
const service = axios.default.create({
  baseURL,
  timeout: 0,
  maxContentLength: 4000
})
service.interceptors.request.use((config: AxiosRequestConfig) => {
  return config
}, (error: any) => {
  Promise.reject(error)
})
service.interceptors.response.use(
  (response: AxiosResponse) => {
    if (response.status !== 200) {
      Vue.prototype.$message({
        message: '网络错误',
        type: 'error'
      });
    } else {
      return response.data
    }
  },
  (error: any) => {
    Promise.reject(error)
  }
)
export default service
```
>有些地方还习惯将常用的http方法也封装一层 这里就简化了 没进行这一步了

* 新建api.ts

```TypeScript
import service from './request'
export function getSomeThings(params: object) {
  return service({
      url: '/login',
      method: 'post',
      data: params
  });
}

// post
export function postSomeThings(params: object) {
  return service({
    url: '/api/getSomethings',
    method: 'post',
    data: params
  });
}
```
#### 参考文章
>[TypeScript入门教程](https://ts.xcatliu.com/)
>[TypeScript安利指南](https://juejin.im/post/5d8efeace51d45782b0c1bd6)
>[为 Vue3 学点 TypeScript , 体验 TypeScript](https://juejin.im/post/5d19ad6de51d451063431864)
>[一篇朴实的文章带你30分钟捋完TypeScript,方法是正反对比](https://juejin.im/post/5d53a8895188257fad671cbc)
>[Vue官网介绍](https://cn.vuejs.org/v2/guide/typescript.html)


