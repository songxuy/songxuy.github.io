title: Vue中服务端渲染
author: coolsong
date: 2019-08-24 11:20:18
tags:
  - Vue
  - ssr
categories: 
  - Vue
---
&nbsp;&nbsp;&nbsp;&nbsp;最近用vue做的一个项目中，有服务端渲染的需求，然后自己就去看了这方面的一些资料、官方文档、实现方法等。实现方法主要有预渲染（代替服务端渲染只需求部分页面且主要是静态的）、nuxt（服务端渲染框架适合从头开始写的项目）、自己搭建ssr等。下面就主要讲的是通过官网的方式自己搭建服务端渲染：
<!--more-->
### 什么是SSR
>服务端渲染直译就是我们的页面及数据都是在服务端写好之后，再直接返回内容给我们。我们可以查看使用vue或react开发的页面的源码，会发现只有一个app而没有数据，因为数据都是通过js加上的。而服务端渲染之后的网页源码中会有数据。或者通过network中也可以查看不同。
![My Pic](/images/ssr1.png)
![My Pic](/images/ssr2.png)

### 为什么要使用SSR
>类似于像vue、react这类框架开发的spa应用，首屏的时候，因为他要等待资源加载完成，然后再进行渲染，会导致了首屏有白屏，如果是单个页面还好，如果是spa应用 那么 他的加载时间就会变得很长，白屏时间会很影响用户体验，再有就是由于国内的搜索公司 对于spa 应用没有很好的兼容，导致了客户端渲染会对seo非常的不友好，有seo 需求的页面就很迫切的需要服务端渲染。

### 缺点相对于（SPA）
>（1）开发条件所限。浏览器特定的代码，只能在某些生命周期钩子函数(lifecycle hook)中使用；一些外部扩展库(external library)可能需要特殊处理，才能在服务器渲染应用程序中运行。
>（2）涉及构建设置和部署的更多要求。与可以部署在任何静态文件服务器上的完全静态单页面应用程序(SPA)不同，服务器渲染应用程序，需要处于 Node.js server 运行环境。
>（3）更多的服务器端负载。在 Node.js 中渲染完整的应用程序，显然会比仅仅提供静态文件的 server 更加大量占用 CPU 资源(CPU-intensive - CPU 密集)，因此如果你预料在高流量环境(high traffic)下使用，请准备相应的服务器负载，并明智地采用缓存策略。

### 实现一个简单的SSR

* 创建一个空项目 mkdir vue-init && cd vue-init
* npm init 初始化
* 安装依赖  
```node
npm install vue vue-server-renderer --save
/*
推荐使用 Node.js 版本 6+。
vue-server-renderer 和 vue 必须匹配版本。
vue-server-renderer 依赖一些 Node.js 原生模块，因此只能在 Node.js 中使用。我们可能会提供一个更简单的构建，可以在将来在其「JavaScript 运行时(runtime)」运行
*/
```

* 创建index.js代码如下:
```JavaScript
const Vue = require('vue')
const app = new Vue({
  template: `<div>Hello World</div>`
})
// 第 2 步：创建一个 renderer
const renderer = require('vue-server-renderer').createRenderer()
// 第 3 步：将 Vue 实例渲染为 HTML
renderer.renderToString(app, (err, html) => {
  if (err) throw err
    console.log(html)
// => <div data-server-rendered="true">Hello World</div>
})
```

* 运行node index.js 可以看到控制台的输出
![My Pic](/images/ssr3.png)
>我们在将这个结果通过一个服务返给浏览器不就完成服务端渲染了么？

* 安装服务器  npm install express --save
* 创建app.js 代码如下
```JavaScript
const Vue = require('vue')
const server = require('express')()
const renderer = require('vue-server-renderer').createRenderer()
server.get('*', (req, res) => {
const app = new Vue({
  data: {
    url: req.url
  },
  template: `<div>访问的 URL 是： {{ url }}</div>`
})
renderer.renderToString(app, (err, html) => {
if (err) {
  res.status(500).end('Internal Server Error')
  return
}
res.end(`
  <!DOCTYPE html>
  <html lang="en">
  <meta charset="utf-8">
  <head><title>Hello</title></head>
  <body>${html}</body>
  </html>
`)
})
})
server.listen(8080)
```

* 可以看到已经实现了服务端渲染
![My Pic](/images/ssr4.png)

* 我们也可以使用一个页面模板（当你在渲染 Vue 应用程序时，renderer 只从应用程序生成 HTML 标记 (markup)。在这个示例中，我们必须用一个额外的 HTML 页面包裹容器，来包裹生成的 HTML 标记。）创建一个index.template.html 代码如下(注释的地方就是html注入的标记)：
```Html
<!DOCTYPE html>
<html lang="en">
  <meat charset="utf-8">
  <head><title>Hello</title></head>
  <body>
    <!--vue-ssr-outlet--> 
  </body>
</html>
```

* 修改app.js
```JavaScript
const renderer = require('vue-server-renderer').createRenderer({
  template: require('fs').readFileSync('./index.template.html', 'utf-8')
})
//***
renderer.renderToString(app, (err, html) => {
if (err) {
  res.status(500).end('Internal Server Error')
  return
}
res.writeHead(200, {'Content-Type':'text/html;charset=utf-8'});
  res.end(html)
})
```
* 运行结果和上面的相同
* 除此之外，它还支持模板插值操作，修改文件index.template.html，代码如下：
```Html
<html>
  <meta charset="utf-8">
  <head>
  <!-- 使用双花括号(double-mustache)进行 HTML 转义插值(HTML-escaped interpolation) -->
  <title>{{ title }}</title>
  <!-- 使用三花括号(triple-mustache)进行 HTML 不转义插值(non-HTML-escaped interpolation) -->
  {{{ meta }}}
  </head>
  <body>
  <!--vue-ssr-outlet-->
  </body>
</html>
```
修改文件index.js  代码如下
```JavaScript
const Vue = require('vue')
const server = require('express')()
const renderer = require('vue-server-renderer').createRenderer()
server.get('*', (req, res) => {
  const app = new Vue({
  data: {
    url: req.url
  },
    template: `<div>访问的 URL 是： {{ url }}</div>`
  })
  const renderer = require('vue-server-renderer').createRenderer({
    template: require('fs').readFileSync('./index.template.html', 'utf-8')
  })
  const context = {
    title: 'hello vuessr',
    meta: `
    <meta charset="utf-8">
    `
  }
  //***
  renderer.renderToString(app, context, (err, html) => {
  if (err) {
    res.status(500).end('Internal Server Error')
    return
  }
  res.writeHead(200, {'Content-Type':'text/html;charset=utf-8'});
    res.end(html)
  })
})
server.listen(8080)
```
![My Pic](/images/ssr5.png)

> 至此已经实现了一个简单的服务端渲染的demo。但是要使用在实际的项目中还需要解决很多问题。

### vue-cli3中使用ssr

1. 创建一个vue-cli3项目
>vue create myssr

2. 进入文件夹，运行 cnpm run serve；启动项目
3. 安装以下的依赖

>安装 vue-server-renderer
>安装 lodash.merge(lodash中的_.merge方法 合并对象)
>安装 webpack-node-externals（所有的node模块的包 不会被打包）
>安装 cross-env（设置环境变量）

4.在服务器中集成
>在项目根目录下新建一个server.js
>安装koa2
```
cnpm install koa koa-static --save
```
>server.js
```JavaScript
// server.js
const fs = require("fs");
const Koa = require("koa");
const path = require("path");
const koaStatic = require('koa-static')
const app = new Koa();

const resolve = file => path.resolve(__dirname, file);
// 开放dist目录
app.use(koaStatic(resolve('./dist')))

// 第 2 步：获得一个createBundleRenderer
const { createBundleRenderer } = require("vue-server-renderer");
const bundle = require("./dist/vue-ssr-server-bundle.json");
const clientManifest = require("./dist/vue-ssr-client-manifest.json");

const renderer = createBundleRenderer(bundle, {
  runInNewContext: false,
  template: fs.readFileSync(resolve("./src/index.template.html"), "utf-8"),
  clientManifest: clientManifest
});

function renderToString(context) {
  return new Promise((resolve, reject) => {
    renderer.renderToString(context, (err, html) => {
      err ? reject(err) : resolve(html);
    });
  });
}
// 第 3 步：添加一个中间件来处理所有请求
app.use(async (ctx, next) => {
  const context = {
    title: "ssr test",
    url: ctx.url
  };
  // 将 context 数据渲染为 HTML
  const html = await renderToString(context);
  ctx.body = html;
});

const port = 3000;
app.listen(port, function() {
  console.log(`server started at localhost:${port}`);
});
```
5. 改造前端配置，以支持SSR
>在src下新增index.template.html文件
```Html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <!--vue-ssr-outlet-->
  </body>
</html>
```
>在src目录下新建两个文件，一个entry-client.js 仅运行于浏览器 一个entry-server.js 仅运行于服务器
```JavaScript
/* entry-client */
import { createApp } from './main'

// 客户端特定引导逻辑……
const { app } = createApp()

// 这里假定 App.vue 模板中根元素具有 `id="app"`
app.$mount('#app')
```
```JavaScript
/* entry-server */
import { createApp } from "./main";

export default context => {
  // 因为有可能会是异步路由钩子函数或组件，所以我们将返回一个 Promise，
  // 以便服务器能够等待所有的内容在渲染前，
  // 就已经准备就绪。
  return new Promise((resolve, reject) => {
    const { app, router } = createApp();

    // 设置服务器端 router 的位置
    router.push(context.url);

    // 等到 router 将可能的异步组件和钩子函数解析完
    router.onReady(() => {
      const matchedComponents = router.getMatchedComponents();
      // 匹配不到的路由，执行 reject 函数，并返回 404
      if (!matchedComponents.length) {
        return reject({
          code: 404
        });
      }
      // Promise 应该 resolve 应用程序实例，以便它可以渲染
      resolve(app);
    }, reject);
  });
};
```
>修改main.js 
>main.js 是我们应用程序的「通用 entry」。在纯客户端应用程序中，我们将在此文件中创建根 Vue 实例，并直接挂载到 DOM。但是，对于服务器端渲染(SSR)，责任转移到纯客户端 entry 文件。app.js 简单地使用 export 导出一个 createApp 函数：
```JavaScript
// main.js
import Vue from 'vue'
import App from './App.vue'
import { createRouter } from "./router";

// 导出一个工厂函数，用于创建新的
// 应用程序、router 和 store 实例
export function createApp () {
  const router = createRouter();
  const app = new Vue({
    router,
    // 根实例简单的渲染应用程序组件。
    render: h => h(App)
  })
  return { app,router }
}
```
>修改router.js
```JavaScript
import Vue from 'vue'
import Router from 'vue-router'
import Home from './views/Home.vue'

Vue.use(Router)

export function createRouter(){
  return new Router({
    mode: 'history', //一定要是history模式
    routes: [
      {
        path: '/',
        name: 'home',
        component: Home
      },
      {
        path: '/about',
        name: 'about',
        component: () => import(/* webpackChunkName: "about" */ './views/About.vue')
      }
    ]
  })
}

```
6.添加和修改相应的webpack配置，vue-cli3在vue.config.js中配置
>添加vue.config.js
```JavaScript
// vue.config.js

const VueSSRServerPlugin = require("vue-server-renderer/server-plugin");
const VueSSRClientPlugin = require("vue-server-renderer/client-plugin");
const nodeExternals = require("webpack-node-externals");
const merge = require("lodash.merge");
const TARGET_NODE = process.env.WEBPACK_TARGET === "node";
const target = TARGET_NODE ? "server" : "client";


module.exports = {
  css: {
    extract: process.env.NODE_ENV === 'production'
  },
  configureWebpack: () => ({
    // 将 entry 指向应用程序的 server / client 文件
    entry: `./src/entry-${target}.js`,
    // 对 bundle renderer 提供 source map 支持
    devtool: 'source-map',
    target: TARGET_NODE ? "node" : "web",
    node: TARGET_NODE ? undefined : false,
    output: {
      libraryTarget: TARGET_NODE ? "commonjs2" : undefined
    },
    // https://webpack.js.org/configuration/externals/#function
    // https://github.com/liady/webpack-node-externals
    // 外置化应用程序依赖模块。可以使服务器构建速度更快，
    // 并生成较小的 bundle 文件。
    externals: TARGET_NODE
      ? nodeExternals({
          // 不要外置化 webpack 需要处理的依赖模块。
          // 你可以在这里添加更多的文件类型。例如，未处理 *.vue 原始文件，
          // 你还应该将修改 `global`（例如 polyfill）的依赖模块列入白名单
          whitelist: [/\.css$/]
        })
      : undefined,
    optimization: {
          splitChunks: undefined
    },
    plugins: [TARGET_NODE ? new VueSSRServerPlugin() : new VueSSRClientPlugin()]
  }),
  chainWebpack: config => {
    config.module
      .rule("vue")
      .use("vue-loader")
      .tap(options => {
        merge(options, {
          optimizeSSR: false
        });
      });
      
    // fix ssr hot update bug
    if (TARGET_NODE) {
      config.plugins.delete("hmr");
    }
  }
};
```
7. 修改package，新增三个脚本来进行分别的打包
```
"build:client": "vue-cli-service build",
"build:server": "cross-env WEBPACK_TARGET=node vue-cli-service build --mode server",
"build:win": "npm run build:server && move dist\\vue-ssr-server-bundle.json bundle && npm run build:client && move bundle dist\\vue-ssr-server-bundle.json"
```
8. 执行命令
>npm run build:win 生成打包之后的文件
![My Pic](/images/ssr6.png)

9. 启动服务端
> node server.js 
> 访问 localhost:3000，可以看到页面的数据都是又服务端渲染完成后返回的。到这一步已经基本算完成了SSR的构建了。
> 如果没成功可以删除dist中的html试试 避免直接访问到了该html文件。
> 解过实现了ssr
![My Pic](/images/ssr7.png)

10.添加Vuex
>修改store.js
```JavaScript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export function createStore() {
  return new Vuex.Store({
    state: {
        age: 12,
        name: 'sxy'
    },
    mutations: {

    },
    actions: {

    }
  });
}
```
>修改main.js
```JavaScript
import Vue from "vue";
import App from "./App.vue";
import { createRouter } from "@/router";
import { createStore } from "@/store";
export function createApp() {
  const router = createRouter();
  const store = createStore()  // +
  const app = new Vue({
    router,
    store,      // +
    render: h => h(App)
  });
  return { app, router,store };
}
```
>修改home.vue
```Vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" @click="say">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  components: {
    HelloWorld
  },
  methods: {
    say () {
      alert(this.$store.state.age)
    }
  }
}
</script>
```
>点击图片弹出state中的数据
>![My Pic](/images/ssr8.png)

11. 添加异步数据
>在渲染前，要预先获取所有需要的异步数据，然后存到Vuex的store中。
>在后端渲染时，通过Vuex将获取到的数据注入到相应组件中。
>把store中的数据设置到window.__INITIAL_STATE__属性中。
>在浏览器环境中，通过Vuex将window.__INITIAL_STATE__里面的数据注入到相应组件中。

>修改entry-server.js
```JavaScript
/* entry-server */
import { createApp } from "./main";

export default context => {
  // 因为有可能会是异步路由钩子函数或组件，所以我们将返回一个 Promise，
  // 以便服务器能够等待所有的内容在渲染前，
  // 就已经准备就绪。
  return new Promise((resolve, reject) => {
    const { app, router, store } = createApp();

    // 设置服务器端 router 的位置
    router.push(context.url);

    // 等到 router 将可能的异步组件和钩子函数解析完
    router.onReady(() => {
      const matchedComponents = router.getMatchedComponents();
      // 匹配不到的路由，执行 reject 函数，并返回 404
      if (!matchedComponents.length) {
        return reject({
          code: 404
        });
      }
      // Promise 应该 resolve 应用程序实例，以便它可以渲染
      Promise.all(matchedComponents.map( async (component) => {
        if (component.asyncData) {
          await component.asyncData({ store });
          console.log(store.state)
          context.state = store.state;
          console.log(context.state)
          // 返回根组件
          resolve(app);
        }
      }))
    }, reject);
  });
};
```
>修改store.js
```JavaScript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
const fetchBar = function() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(20);
    }, 1000);
  });
};
export function createStore() {
  const store =  new Vuex.Store({
    state: {
      age: 12,
      name: 'sxy'
    },
    mutations: {
      'SET_AGE'(state, data) {
        state.age = data;
      }
    },
    actions: {
      fetchBar({ commit }) {
        return fetchBar().then((data) => {
          console.log(data)
          commit('SET_AGE', data);
        }).catch((err) => {
          console.error(err);
        })
      }
    }
  });
  if (typeof window !== 'undefined' && window.__INITIAL_STATE__) {
    console.log('window.__INITIAL_STATE__', window.__INITIAL_STATE__);
    store.replaceState(window.__INITIAL_STATE__);
  }
  
  return store;
}
```
>修改home.vue
```Vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" @click="say">
    <p>{{age}}</p>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
 const fetchInitialData = async ({ store }) => {
    await store.dispatch('fetchBar');
  };
export default {
  name: 'home',
  asyncData: fetchInitialData,
  components: {
    HelloWorld
  },
  methods: {
    say () {
      alert(this.$store.state.age)
    }
  },
  computed: {
    age () {
      return this.$store.state.age
    }
  }
}
</script>
```
>重新打包之后再启动  可以看到隔了一秒之后加载出了异步数据
![My Pic](/images/ssr9.png)