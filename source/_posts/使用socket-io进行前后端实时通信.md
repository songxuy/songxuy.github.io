title: 使用socket.io进行前后端实时通信
author: coolsong
tags:
  - Vue
  - 实时通信
categories:
  - socket.io
date: 2019-09-08 10:03:00
---
&nbsp;&nbsp;&nbsp;在一个论坛社区项目中有一个需求，就是需要实时推送用户收到的消息如（点赞、评论等）。要实现实时推送可以使用轮询，不断的请求接口查看是否有数据，这种方法比较浪费资源，不能确保每次轮询都会有数据。最好的解决方法应该是后端主动的来告诉我们有数据。这时就要用到websocket。
<!--more-->
### websocket是什么

* * *


>Websocket是HTML5新增的一种全双工通信协议，客户端和服务端基于TCP握手连接成功后，两者之间就可以建立持久性的连接，实现双向数据传输。

* 有哪些优点
>1. 概括地说就是：支持双向通信，更灵活，更高效，可扩展性更好。
>2. 支持双向通信，实时性更强。
>3. 更好的二进制支持。
>4. 较少的控制开销。连接创建后，ws客户端、服务端进行数据交换时，协议控制的数据包头部较小。在不包含头部的情况下，服务端到客户端的包头只有2~10字节（取决于数据包长度），客户端到服务端的的话，需要加上额外的4字节的掩码。而HTTP协议每次通信都需要携带完整的头部。
>5. 支持扩展。ws协议定义了扩展，用户可以扩展协议，或者实现自定义的子协议。（比如支持自定义压缩算法等）

* 基本使用
```JavaScript

const ws = new WebSocket("ws//:xxx.xx", [protocol])

ws.onopen = () => {
    ws.send('hello')
    console.log('send')}

ws.onmessage = (ev) =>{
    console.log(ev.data)
    ws.close()}

ws.onclose = (ev) =>{
    console.log('close')}

ws.onerror = (ev) =>{
    console.log('error')}
```

### socket.io

* * *

>实际应用中，如果需要Websocke进行双向数据通信，Socket.io是一个非常好的选择。其实github上面也有通过JS封装好的Websocket库，ws可用于client和基于node搭建的服务端使用，但是用起来相对繁琐，star相对Socket.io较少，所以不推荐使用。

>Socket.io不是Websocket，它只是将Websocket和轮询 （Polling）机制以及其它的实时通信方式封装成了通用的接口，并且在服务端实现了这些实时机制的相应代码。也就是说，Websocket仅仅是 Socket.io实现实时通信的一个子集。因此Websocket客户端连接不上Socket.io服务端，当然Socket.io客户端也连接不上Websocket服务端。

### socket.io使用demo

* * *

>demo技术构成
>  * 前端vue全家桶
>  * 后端koa2+mongoDb

>最终效果
>当前端没登录时推送一个hello给前端，当登录之后增加该用户的age之后推送当前age给当前登录的用户

#### 前端搭建

* 安装vue-cli之后修改main.js  (加入相应的拦截器 是否登录 以及拼接token)
```JavaScript
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'
import axios from 'axios'
import store from './store'
axios.defaults.withCredentials = true
axios.defaults.crossDomain = true
Vue.config.productionTip = false
axios.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  config.headers.common['Authorization'] = 'Bearer ' + token;
  return config;
})
axios.interceptors.response.use((response)=>{
  if(response.data.code == 401){
    alert('请登录在进行操作！')
    localStorage.removeItem('token')
    router.push('/')
  }else{
    return response.data
  }
}, (error)=>{
  return Promise.reject(error)
})
/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  store,
  components: { App },
  template: '<App/>'
})
```

* 修改HelloWorld.vue (主要是登录)
```Vue
<template>
  <div class="hello">
    <input type="text" v-model="form.username"/>
    <input type="password" v-model="form.password" />
    <button @click="submit">登录</button>
    <span class="i1"></span>
    <i class="i2"></i>
    <i class="i3"></i>
    <i class="i4"></i>
    <i class="i5"></i>
    <i class="i6"></i>
    <i class="i7"></i>
    <i class="i8"></i>
    <i class="i9"></i>
    <i class="i10"></i>
  </div>
</template>

<script>
import axios from 'axios'
axios.defaults.withCredentials = true
axios.defaults.crossDomain = true
export default {
  name: 'HelloWorld',
  data () {
    return {
      form: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    submit () {
      if(!this.form.username || !this.form.password ) {
        alert('输入完整!')
      } else {
        axios.post('/api/login',{
          username: this.form.username,
          password: this.form.password
        }).then(res => {
          if(res.code==200){
            localStorage.setItem('token', res.data);
            this.$store.commit('setSocket')
            alert('登陆成功~')
          }else{
            alert(res.message)
          }
        })
      }
    }
  },
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
}
</script>
```
* 修改about页面 （主要是增加年龄）
```Vue
<template>
  <div class="about">
    <h1>about</h1>
    <button @click="add">ADD AGE</button>
  </div>
</template>

<script>
import axios from 'axios'
axios.defaults.withCredentials = true
axios.defaults.crossDomain = true
export default {
  name: 'HelloWorld',
  data () {
    return {
    }
  },
  methods:{
    add(){
      axios.get('/api/insert')
      .then(res => {
        if(res !== undefined) {
          alert(res.message)
        }
      })
    }
  },
  mounted () {

  }
}
</script>
```
* 安装socket.io-client
```
cnpm install socket.io-client --save
```
* 在store中添加相应的连接的方法
>io方法中第一参数为后端暴露出来的路径  第二个为所携带的参数 更多参数可以参考后面的官方文档链接 on方法监听后端的事件
```JavaScript
import Vue from 'vue'
import Vuex from 'vuex'
import io from 'socket.io-client'
Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    socket: null,
    message: ''
  },
  mutations: {
    setSocket (state) {
      if (!state.socket) {
        state.socket = io(`http://localhost:8888`, {
          transports: ['websocket'],
          query: {
            token: 'Bearer ' + localStorage.getItem('token')
          }
        })
        state.socket.on('message', msg => {
          state.message = msg
        })
      } else {
        state.socket = io(`http://localhost:8888`, {
          transports: ['websocket'],
          query: {
            token: 'Bearer ' + localStorage.getItem('token')
          }
        })
        state.socket.on('message', msg => {
          state.message = msg
        })
      }
    }
  },
  actions: {

  }
})
```

#### 后端搭建

* 安装socket.io
```
cnpm install socket.io --save
```

* 在app中引用并创建服务
```JavaScript
const http = require('http');
let server = http.createServer(app);
//注意，websocket的握手是需要依赖http服务的，所以这里要把server传入进去。
global.io = require('socket.io')(server);
server.listen(8888);
```
* 连接并监听事件和推送数据
>io.on监听前端的事件  io.emit触发事件推送hello给前端
```JavaScript
io.on('connection', function (socket) {
  console.log(socket.id)
  socket.on('message', function (data) {
      console.log('服务端收到 : ', data);
      //注意send()方法其实是发送一个 'message' 事件
      //客户端要通过on('message')来响应
      socket.send('你好客户端, ' + data);
  });
  //发生错误时触发
  socket.on('error', function (err) {
      console.log(err);
  });
  io.emit('message', 'hello')
});
```
* 在指定的情况下推送数据给指定用户
>这里的情况是增加年级时推送给当前用户年纪 handshake.query获取连接时的参数 通过连接时传的token来验证是哪个用户  使用io.socket.connected[id].emit像指定的socketId连接推送数据
```JavaScript
exports.addAge = async (ctx, next) => {
  let token = ctx.headers.authorization
  if (token) {
    var name = await tools.verToken(token)
    await User.updateOneAsync({ _id: name.userid }, { $inc: { age : 1 }, update_date: moment().format('YYYY-MM-DD HH:mm:ss') })
    .then(async res => {
      if(res){
        let data = await User.findOneAsync({ _id: name.userid })
        Object.keys(global.io.sockets.sockets).map(item => {
            if (global.io.sockets.sockets[item].handshake.query.token === token) {
                global.io.sockets.connected[item].emit('message', data.age)
            }
        })
        ctx.body = {
          code: 200,
          message: '添加成功~'
        }
      }else {
        ctx.body = {
          code: 200,
          message: '添加失败~'
        } 
      }
    })
  } else {
    ctx.body = {
      code: -200,
      message: '请登录~'
    }
  }
}
```
#### 验证

* 未登录状态
> 连接之后推送hello
> 
![My Pic](/images/socket1.png)

* 登录之后点击 增加age
![My Pic](/images/socket2.png)
![My Pic](/images/socket3.png)
![My Pic](/images/socket4.png)

* 验证是否只推送给了指定的用户
>重开一个firefox浏览器登录另一个用户 两边分别增加age之后查看推送数据
>
![My Pic](/images/socket5.png)
![My Pic](/images/socket6.png)
![My Pic](/images/socket7.png)

>最终实现进行某种操作之后推送数据给指定用户

>相关链接
>[websocket文档](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)
> [socket.io文档](https://github.com/socketio/socket.io-client/blob/master/docs/API.md)
